
nginx.conf
```bash
...
...
    server {
        listen 80;

        location = / {
            add_header Content-Type text/plain always;
            return 200 'Welcome to the Docker!\n';
        }
    }
...
...

```

Dokckerfile
```Dockerfile

FROM ubuntu:18.04

RUN apt-get update && apt-get install nginx -y

WORKDIR /etc/nginx

COPY . .

ENTRYPOINT ["/usr/sbin/nginx"]

CMD [ "-g daemon off;" ]

EXPOSE 80

VOLUME [ "/var/lib/nginx" ]

```

```bash

user@server:~/nginx$ sudo docker build . -t nginx:rbm-dkr-09

user@server:~/nginx$ sudo docker images
REPOSITORY   TAG          IMAGE ID       CREATED         SIZE
nginx        rbm-dkr-09   485e294f73c2   7 seconds ago   165MB
ubuntu       18.04        ad080923604a   13 days ago     63.1MB

user@server:~/nginx$ sudo docker run -d -p 127.0.0.1:8901:80 nginx:rbm-dkr-09

user@server:~/nginx$ curl 127.0.0.1:8901
Welcome to the Docker!

```
