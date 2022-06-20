
```bash

user@server:~$ sudo docker run -d --name rbm-dkr-05-run-$(cat /dev/urandom | tr -cd 'a-f0-9' | head -c 10) nginx:stable
ed449bce0b628056366218b41b56be0c5ba935ba23c634577173a9db09c9ab62

user@server:~$ sudo docker run -d --name rbm-dkr-05-run-$(cat /dev/urandom | tr -cd 'a-f0-9' | head -c 10) nginx:stable
ed449bce0b628056366218b41b56be0c5ba935ba23c634577173a9db09c9ab62

user@server:~$ sudo docker run -d --name rbm-dkr-05-stop nginx:stable
1eee5b2ef15a6f189aeec4a5c465531f88c0924a05bed7db75780630ad33efe4


```

```bash

user@server:~$ sudo docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED              STATUS              PORTS     NAMES
1eee5b2ef15a   nginx:stable   "/docker-entrypoint.…"   26 seconds ago       Up 25 seconds       80/tcp    rbm-dkr-05-stop
ed449bce0b62   nginx:stable   "/docker-entrypoint.…"   About a minute ago   Up About a minute   80/tcp    rbm-dkr-05-run-fb620de1b7
e6eabd423bc6   nginx:stable   "/docker-entrypoint.…"   About a minute ago   Up About a minute   80/tcp    rbm-dkr-05-run-8aad49e64a

```

```bash 

user@server:~$ sudo docker stop rbm-dkr-05-stop
rbm-dkr-05-stop

user@server:~$ sudo docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS     NAMES
ed449bce0b62   nginx:stable   "/docker-entrypoint.…"   3 minutes ago   Up 3 minutes   80/tcp    rbm-dkr-05-run-fb620de1b7
e6eabd423bc6   nginx:stable   "/docker-entrypoint.…"   3 minutes ago   Up 3 minutes   80/tcp    rbm-dkr-05-run-8aad49e64a

```

```bash

user@server:~$ sudo docker stop $(sudo docker ps -q)
ed449bce0b62
e6eabd423bc6

user@server:~$ sudo docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

user@server:~$ sudo docker rm $(sudo docker ps -qa)
1eee5b2ef15a
ed449bce0b62
e6eabd423bc6

user@server:~$ sudo docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```