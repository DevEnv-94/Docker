The most important case is the assembly of images in an environment limited from everything.
If we collect the image inside another container, we can get an environment that contains the necessary tools before starting work, no secret data, and most importantly, after its work, it can be destroyed, freeing the resources necessary on the host, leaving no garbage behind.
This approach is used in almost every modern CI / CD - GitLab CI , Circle CI, Travis CI, etc.


Historical
docker run --privileged -d docker:dind
```bash
user@server:~$ sudo docker run d --privileged docker:dind

user@server:~$ sudo docker exec ad271c50097e docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

user@server:~$ sudo docker exec ad271c50097e docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE

user@server:~$ sudo docker exec ad271c50097e docker pull redis:5

user@server:~$ sudo docker exec ad271c50097e docker run -d --name rbm-dkr-26 redis:5

user@server:~$ sudo docker exec ad271c50097e docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS      NAMES
dd77942e9ca6   redis:5   "docker-entrypoint.s…"   37 seconds ago   Up 37 seconds   6379/tcp   rbm-dkr-26

user@server:~$ sudo docker exec ad271c50097e docker rm --force rbm-dkr-26
rbm-dkr-26

```


More safe
docker run -v /var/run/docker.sock:/var/run/docker.sock docker
```bash
user@server:~$ sudo docker run -it -v /var/run/docker.sock:/var/run/docker.sock docker

/ # docker ps
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS          PORTS           NAMES
6cd2ce19a8af   docker        "docker-entrypoint.s…"   20 seconds ago   Up 18 seconds                   focused_kilby
ad271c50097e   docker:dind   "dockerd-entrypoint.…"   5 minutes ago    Up 5 minutes    2375-2376/tcp   compassionate_feistel

/ # docker pull redis:5

/ # docker run -d --name rbm-dkr-26 redis:5
19a619f45229f0ee424397c896a92954ec110a8ebad19b6436ff324f0b2d7e23

/ # docker ps
CONTAINER ID   IMAGE         COMMAND                  CREATED         STATUS         PORTS           NAMES
19a619f45229   redis:5       "docker-entrypoint.s…"   7 seconds ago   Up 6 seconds   6379/tcp        rbm-dkr-26
6cd2ce19a8af   docker        "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes                   focused_kilby
ad271c50097e   docker:dind   "dockerd-entrypoint.…"   7 minutes ago   Up 7 minutes   2375-2376/tcp   compassionate_feistel

/ # docker rm --force rbm-dkr-26
rbm-dkr-26
exit

```