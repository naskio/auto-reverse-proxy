# Customize nginx reverse proxy

## Replacing default proxy settings

mount here `/etc/nginx/proxy.conf`

```
# HTTP 1.1 support
proxy_http_version 1.1;
proxy_buffering off;
proxy_set_header Host $http_host;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection $proxy_connection;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $proxy_x_forwarded_proto;
proxy_set_header X-Forwarded-Ssl $proxy_x_forwarded_ssl;
proxy_set_header X-Forwarded-Port $proxy_x_forwarded_port;

# Mitigate httpoxy attack (see README for details)
proxy_set_header Proxy "";# HTTP 1.1 support
```

NOTE: If you provide this file it will replace the defaults; you may want to check the .tmpl file to make sure you have
all of the needed options.

## Proxy-wide

To add settings on a proxy-wide basis, add your configuration file under /etc/nginx/conf.d using a name ending in .conf.

```
FROM jwilder/nginx-proxy
RUN { \
      echo 'server_tokens off;'; \
      echo 'client_max_body_size 100m;'; \
    } > /etc/nginx/conf.d/my_proxy.conf
```

## Per-VIRTUAL_HOST

To add settings on a per-VIRTUAL_HOST basis, add your configuration file under /etc/nginx/vhost.d. Unlike in the
proxy-wide case, which allows multiple config files with any name ending in .conf, the per-VIRTUAL_HOST file must be
named exactly after the VIRTUAL_HOST.

if you have a virtual host named app.example.com

/path/to/vhost.d/app.example.com

```
docker run -d -p 80:80 -p 443:443 -v /path/to/vhost.d:/etc/nginx/vhost.d:ro -v /var/run/docker.sock:/tmp/docker.sock:ro jwilder/nginx-proxy
$ { echo 'server_tokens off;'; echo 'client_max_body_size 100m;'; } > /path/to/vhost.d/app.example.com
```

If you are using multiple hostnames for a single container (e.g. VIRTUAL_HOST=example.com,www.example.com), the virtual
host configuration file must exist for each hostname.

```
$ { echo 'server_tokens off;'; echo 'client_max_body_size 100m;'; } > /path/to/vhost.d/www.example.com
$ ln -s /path/to/vhost.d/www.example.com /path/to/vhost.d/example.com
```

## Per-VIRTUAL_HOST default configuration

If you want most of your virtual hosts to use a default single configuration and then override on a few specific ones,
add those settings to the /etc/nginx/vhost.d/default file. This file will be used on any virtual host which does not
have a /etc/nginx/vhost.d/{VIRTUAL_HOST} file associated with it.

## Per-VIRTUAL_HOST location configuration

To add settings to the "location" block on a per-VIRTUAL_HOST basis, add your configuration file under
/etc/nginx/vhost.d just like the previous section except with the suffix _location.

For example, if you have a virtual host named app.example.com and you have configured a proxy_cache my-cache in another
custom file, you could tell it to use a proxy cache as follows:

$ docker run -d -p 80:80 -p 443:443 -v /path/to/vhost.d:/etc/nginx/vhost.d:ro -v /var/run/docker.sock:/tmp/docker.sock:
ro jwilder/nginx-proxy $ { echo 'proxy_cache my-cache;'; echo 'proxy_cache_valid 200 302 60m;'; echo 'proxy_cache_valid
404 1m;' } > /path/to/vhost.d/app.example.com_location If you are using multiple hostnames for a single container (e.g.
VIRTUAL_HOST=example.com,www.example.com), the virtual host configuration file must exist for each hostname. If you
would like to use the same configuration for multiple virtual host names, you can use a symlink:

```
$ { echo 'proxy_cache my-cache;'; echo 'proxy_cache_valid  200 302  60m;'; echo 'proxy_cache_valid  404 1m;' } > /path/to/vhost.d/app.example.com_location
$ ln -s /path/to/vhost.d/www.example.com /path/to/vhost.d/example.com
```

You can also provide a default location: `/etc/nginx/vhost.d/default_location`
