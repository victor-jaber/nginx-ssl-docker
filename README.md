# 🚀 Guia de Configuração do Docker com NGINX e SSL

## Introdução 

1. 🌐 **Gerar certificados SSL com Certbot e NGINX**
2. 🔄 **Iniciar React com Proxy Reverso do NGINX**
3. ⚙️ **Iniciar API e MongoDB com Proxy Reverso**

## Prática

### 1. 🌐 Gerar certificados SSL com Certbot e NGINX (obrigatório)

Primeiramente, baixe a base disponibilizada no link:
- [Link do GitLab](#)

### Passos:

1. 🚀 **Construa a imagem do NGINX**:
    ```sh
    docker compose build
    ```

2. 🖥️ **Verifique as stacks do Docker Swarm** (inicialize o Docker Swarm, se necessário):
    ```sh
    docker swarm init
    docker stack ls
    ```

3. 🗑️ **Remova qualquer serviço existente**:
    ```sh
    docker stack rm {nome-do-servico}
    ```

4. 🌐 **Crie a rede obrigatória**:
    ```sh
    docker network create -d overlay --attachable net_swarm
    ```

5. 📜 **Utilize o script `sh-up.sh` para construir as imagens**:
    ```sh
    sh sh-up.sh
    ```

### Arquivo `docker-swarm.yml`

```yaml
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
