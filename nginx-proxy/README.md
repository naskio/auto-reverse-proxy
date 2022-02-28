# nginx reverse proxy

Reverse proxy using nginx.

This container will listen on port `80` and `443` and forward requests to other containers. it is also responsible for
setting up the certificates.

## Current Setup Overview

- `nginx.tmpl` is not being used.
- `conf.d/rate_limit.conf` is used to define rate limits.
- `host.d/default_location` is used to define the default location and set up the rate limit.