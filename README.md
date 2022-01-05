# docker-nginx-auto-reverse-proxy

Deploy multiple containerized web apps in the same Server (VPS, ...) using Docker and NGINX as a reverse proxy. This
will automatically route the requests to the containers and manage (create, generate and renew) the Let's encrypt SSL
certificates.

# Step-by-step guide

1- Install Docker in your server [(Official docs)](https://docs.docker.com/engine/install/ubuntu/)

2- Make docker run at startup of the OS

```
sudo systemctl enable docker
```

3- Install docker-compose [(Official Docs)](https://docs.docker.com/compose/install/)

4- Create a Docker network

```
docker network create auto-reverse-proxy-global-network
```

5- Start the reverse-proxy

```
cd ./nginx-proxy
docker-compose up -d
```

to stop the reverse-proxy

```
cd ./nginx-proxy
docker-compose down
```

# Credit

Based on [nginx-proxy](https://github.com/nginx-proxy/nginx-proxy)
and [acme-companion](https://github.com/nginx-proxy/acme-companion).