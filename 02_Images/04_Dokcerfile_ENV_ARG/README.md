

Dokcerfile
```Dockerfile

ARG NG_VERSION #impact to FROM

FROM nginx:$NG_VERSION

ARG NG_VERSION #impact to ENV

ENV NG_VERSION="$NG_VERSION"

ARG ARG_FILE #impact to RUN

RUN touch /opt/$ARG_FILE


```

```bash


user@server:~$ sudo docker build . -t nginx:rbm-dkr-10 --build-arg NG_VERSION=stable --build-arg ARG_FILE=rbm-dkr-10

user@server:~$ sudo docker run -d --name rbm-dkr-10 --env REBRAINME=DKR10 nginx:rbm-dkr-10


user@server:~$ sudo docker exec rbm-dkr-10 env
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=efdbecf34c8f
REBRAINME=DKR10
NGINX_VERSION=1.22.0
NJS_VERSION=0.7.4
PKG_RELEASE=1~bullseye
NG_VERSION=stable
HOME=/root

user@server:~$ sudo docker exec rbm-dkr-10 ls /opt
rbm-dkr-10

```