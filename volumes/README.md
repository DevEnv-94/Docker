
```bash

user@server:~$ cat nginx1
...
...
    access_log  /var/log/nginx/external/access.log  main;
...
...

```

```bash

user@server:~$ sudo docker volume create rbm-dkr-04-volume
rbm-dkr-04-volume

user@server:~$ sudo docker run -d  -v /home/user/nginx1:/etc/nginx/nginx.conf -v rbm-dkr-04-volume:/var/log/nginx/external -d -p 127.0.0.1:8891:80 --name rbm-dkr-04 nginx:stable

user@server:~$ curl 127.0.0.1:8891
Welcome to Docker!

user@server:~$ docker volume ls
DRIVER    VOLUME NAME
local     rbm-dkr-04-volume


user@server:~$ docker volume inspect rbm-dkr-04-volume
[
    {
        "CreatedAt": "2022-06-17T18:07:52Z",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/rbm-dkr-04-volume/_data",
        "Name": "rbm-dkr-04-volume",
        "Options": {},
        "Scope": "local"
    }
]


user@server:~$ sudo ls -la /var/lib/docker/volumes/rbm-dkr-04-volume/_data
total 8
drwxr-xr-x 2 root root 4096 Jun 17 18:07 .
drwx-----x 3 root root 4096 Jun 17 18:03 ..
lrwxrwxrwx 1 root root   11 May 28 05:41 access.log -> /dev/stdout
lrwxrwxrwx 1 root root   11 May 28 05:41 error.log -> /dev/stderr


```