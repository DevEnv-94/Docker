
Some editors change file inode,but Docker mount files with indoe and when you change file with some editors for instance vim this change file inode of this file, but docker mounted to old inode and dont see changes. Only after restart a container you can see changes because docker now mount same file inode.

https://medium.com/@jonsbun/why-need-to-be-careful-when-mounting-single-files-into-a-docker-container-4f929340834

```bash

user@server:~$ sudo vim nginx1.conf
...
...
            add_header Content-Type text/plain always;
            return 200 'Welcome to the Nginx in Docker!';
...
...

user@server:~$ sudo docker run -p 127.0.0.1:8890:80 -v /home/user/nginx1.conf:/etc/nginx/nginx.conf --name rbm-dkr-03 -d nginx:stable

user@server:~$ curl 127.0.0.1:8890
Welcome to the Nginx in Docker!


user@server:~$ sudo docker exec -ti rbm-dkr-03 md5sum /etc/nginx/nginx.conf
cde71f355e6770b80deb0065e85fa254  /etc/nginx/nginx.conf

```


```bash

user@server:~$ sudo vim nginx1.conf
...
...
            add_header Content-Type text/plain always;
            return 200 'Welcome to the Nginx in Docker! Again!';
...
...

user@server:~$ sudo docker exec -it rbm-dkr-03 nginx -s reload
2022/06/17 11:52:20 [notice] 75#75: signal process started

user@server:~$ sudo docker exec -ti rbm-dkr-03 md5sum /etc/nginx/nginx.conf
cde71f355e6770b80deb0065e85fa254  /etc/nginx/nginx.conf

user@server:~$ curl 127.0.0.1:8890
Welcome to the Nginx in Docker!

```



```bash

user@server:~$ sudo docker restart rbm-dkr-03
rbm-dkr-03

user@server:~$ sudo docker exec -ti rbm-dkr-03 md5sum /etc/nginx/nginx.conf
87283d198ce47de77dfc7f9de0653485  /etc/nginx/nginx.conf

user@server:~$ curl 127.0.0.1:8890
Welcome to the Nginx in Docker! Again!

```