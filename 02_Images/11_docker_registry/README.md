
```bash
user@server:~$ git clone https://gitlab.rebrainme.com/docker_users_repos/3018/dkr-14-gocalc.git

user@server:~/dkr-14-gocalc$ sudo build -t gocalc:latest .

user@server:~$ sudo docker login registry.rebrainme.com

user@server:~/dkr-14-gocalc$ sudo docker image tag gocalc:latest registry.rebrainme.com/docker_users_repos/3018/dkr-19-gocalc/gocalc:latest

user@server:~$ sudo docker image push registry.rebrainme.com/docker_users_repos/3018/dkr-19-gocalc/gocalc:latest
The push refers to repository [registry.rebrainme.com/docker_users_repos/3018/dkr-19-gocalc/gocalc]
200c708e60aa: Pushed
77cae8ab23bf: Pushed
latest: digest: sha256:e26096ebf766d3e553b2eb2c0217a8287be290e3d53104e762aeab672f294ef8 size: 739
```