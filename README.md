## Go to docker folder and copy default.config.env.dist into default.config.env

## Generate a new secret key to be shared by all sentry containers. This value will then be used as the SENTRY_SECRET_KEY environment variable.

`$ docker run --rm sentry config generate-secret-key`

## Put secret into docker/default.config.env

## Run docker-compose

`$ docker-compose up -d`

## Upgrade sentry

`$ docker-compose exec sentry sentry upgrade`

## Nginx config

```
server {
    client_max_body_size 15M;
    server_name sentry.domain.com;

    location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-NginX-Proxy true;
        proxy_set_header X-Forwarded-Proto https;
        proxy_pass http://127.0.0.1:9000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
