🚀 Guia de Configuração do Docker com NGINX e SSL
Introdução
🌐 Gerar certificados SSL com Certbot e NGINX
🔄 Iniciar React com Proxy Reverso do NGINX
⚙️ Iniciar API e MongoDB com Proxy Reverso
Prática
1. 🌐 Gerar certificados SSL com Certbot e NGINX (obrigatório)
Primeiramente, baixe a base disponibilizada no link:

Link do GitLab
Passos:
🚀 Construa a imagem do NGINX:

sh
Copiar código
docker compose build
🖥️ Verifique as stacks do Docker Swarm (inicialize o Docker Swarm, se necessário):

sh
Copiar código
docker swarm init
docker stack ls
🗑️ Remova qualquer serviço existente:

sh
Copiar código
docker stack rm {nome-do-servico}
🌐 Crie a rede obrigatória:

sh
Copiar código
docker network create -d overlay --attachable net_swarm
📜 Utilize o script sh-up.sh para construir as imagens:

sh
Copiar código
sh sh-up.sh
Arquivo docker-swarm.yml
yaml
Copiar código
version: '3'
services:
    nginx:
        build:
            context: ./
            args:
                NGINX_VERSION: '1.25.2'
                # https://hub.docker.com/_/nginx
        image: yourusernamedockerhub/nginx
        ports:
            - '80:80'
            - '443:443'
        volumes:
            - ./nginx/nginx.conf:/etc/nginx/nginx.conf
            - ./nginx/sites-available:/etc/nginx/sites-available
            - ./nginx/conf.d:/etc/nginx/conf.d
            - ./nginx/letsencrypt:/etc/letsencrypt
            - ./public:/var/www/public
        networks:
            - net_local

networks:
    net_local:
        driver: bridge
🔄 Execute o script sh-up.sh no terminal:

sh
Copiar código
sh sh-up.sh
📊 Verifique os contêineres em execução:

sh
Copiar código
docker stats
🔍 Entre no contêiner do NGINX:

sh
Copiar código
docker exec -it {id-container} bash
🔐 Dentro do contêiner, execute o Certbot:

sh
Copiar código
certbot --nginx
Digite as credenciais solicitadas e verifique o status de sucesso. Certifique-se de que seu arquivo estático está funcionando com SSL corretamente.

Nos próximos passos, aprenderemos a tornar a aplicação dinâmica com o React.

