
nginx.conf
```bash

user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    tcp_nopush     on;

    keepalive_timeout  65;

    gzip  on;


    server {
        listen 80;

        location = / {
            add_header Content-Type text/plain always;
            return 200 'Welcome to the Docker!\n';
        }
    }

}

```

Dockerfile 
```Dockerfile

FROM nginx:stable

WORKDIR /etc/nginx

COPY . .

```


```bash

user@server:~/nginx$ sudo docker build . -t nginx:rbm-dkr-08

user@server:~/nginx$ sudo docker run -d -p 127.0.0.1:8900:80 nginx:rbm-dkr-08

user@server:~/nginx$ sudo docker ps
CONTAINER ID   IMAGE              COMMAND                  CREATED          STATUS          PORTS                    NAMES
feea6a98788b   nginx:rbm-dkr-08   "/docker-entrypoint.…"   36 seconds ago   Up 35 seconds   127.0.0.1:8900->80/tcp   stoic_satoshi

user@server:~/nginx$ curl 127.0.0.1:8900
Welcome to the Docker!

```