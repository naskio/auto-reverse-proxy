# auto-reverse-proxy

Deploy multiple containerized web apps in the same Server (VPS, ...) using Docker and NGINX as a reverse proxy. This
will automatically route the requests to the containers and manage (create, generate and renew) the Let's encrypt SSL
certificates.

## Overview

- `nginx-proxy` is a containerized NGINX reverse proxy.
- `portainer`is a web-based tool to manage containers.
- `demo` is a web example.

## Setup and Configuration

1. Install Docker in your server [(Official docs)](https://docs.docker.com/engine/install/ubuntu/)

2. Make docker run at startup of the OS
   ```shell
   sudo systemctl enable docker
   ```

3. Install docker-compose [(Official Docs)](https://docs.docker.com/compose/install/)

4. Create a Docker network
   ```shell
   docker network create auto-reverse-proxy-global-network
   ```

5. Configure env variables in each folder
   ```shell
   cd $FOLDER_NAME
   cp .env.example .env
   nano .env
   ```

6. Start the reverse-proxy
   ```shell
   cd ./nginx-proxy
   docker-compose up -d
   ```
   to stop the reverse-proxy run
   ```shell
   cd ./nginx-proxy
   docker-compose down
   ```

7. Run portainer
   ```shell
   cd  ./portainer
   docker-compose up -d
   ```
   to stop portainer run
   ```shell
   cd  ./portainer
   docker-compose down
   ```

## Customize nginx reverse proxy

See [Customize nginx reverse proxy](./CUSTOMIZE.md) for more details.

-------------------------------------------------------------------------------------------

## Contribute

Pull requests are welcome. For any bug report, please create an issue
on [GitHub](https://github.com/naskio/auto-reverse-proxy).

## Acknowledgements

Based on [nginx-proxy](https://github.com/nginx-proxy/nginx-proxy)
, [acme-companion](https://github.com/nginx-proxy/acme-companion)
and [portainer](https://docs.portainer.io/v/ce-2.11/advanced/reverse-proxy/nginx)
.

## License

[GNU General Public License v3.0](./LICENSE)