# Portainer

Portainer is a web-based management and monitoring tool for Docker containers.

> default port: 9000

-------------------------------------------------------------------------------

# Setup in production environment

In production, we use `nginxproxy/nginx-proxy` to proxy the container from `CONTAINER_PORT` to the internet.

## Prerequisites

- Reverse proxy network:
  - auto-reverse-proxy network `auto-reverse-proxy-global-network`.

-------------------------------------------------------------------------------

# Setup in development environment

In development mode, the container is exposed to localhost at `HOST_PORT`.

> No need for service networks since all containers are part of the shared network `l_shared-network`.