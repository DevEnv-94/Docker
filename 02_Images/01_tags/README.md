
```bash

user@server:~$ sudo docker pull nginx:stable-alpine

user@server:~$ sudo docker tag nginx:stable-alpine nginx:rbm-dkr-07

user@server:~$ sudo docker images
REPOSITORY   TAG             IMAGE ID       CREATED       SIZE
nginx        rbm-dkr-07      4341472ddfe8   3 weeks ago   23.4MB
nginx        stable-alpine   4341472ddfe8   3 weeks ago   23.4MB

user@server:~$ sudo docker run -d nginx:rbm-dkr-07

user@server:~$ sudo docker ps
CONTAINER ID   IMAGE              COMMAND                  CREATED          STATUS          PORTS     NAMES
bd2a586cbd1e   nginx:rbm-dkr-07   "/docker-entrypoint.…"   29 seconds ago   Up 27 seconds   80/tcp    xenodochial_brattain

```