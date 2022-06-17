
logging to syslog on host node
```bash

docker run --rm --log-driver syslog --log-opt syslog-address=unix:///tmp/syslog.sock nginx:stable

```


file to change default log driver
```bash

/etc/docker/daemon.json

```

```bash

user@server:~$ sudo docker run -d --log-driver local --log-opt max-size=10m --name rbm-dkr-06-local -p 127.0.0.1:8892:80 nginx:stable

user@server:~$ curl --silent http://127.0.0.1:8892 > /dev/null

root@server:~# cat /var/lib/docker/containers/7909db154686fbeba9a3c273e35792f6b96a38e5c0a99583ae8871c650400336/local-logs/container.log
t
stdout��������`/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configurationt]
stdout�����I/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/]i
stdout�������U/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.shiq
stdout��������]10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.confqs
stdout��������_10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.confse
stdout�������Q/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.shee
stdout袘�����Q/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sheU
stdoutƔ������A/docker-entrypoint.sh: Configuration complete; ready for start upUT
stderr��������@2022/06/17 19:12:58 [notice] 1#1: using the "epoll" event methodTB
stderr��������.2022/06/17 19:12:58 [notice] 1#1: nginx/1.22.0Be
stderr��������Q2022/06/17 19:12:58 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) eQ
stderr�������=2022/06/17 19:12:58 [notice] 1#1: OS: Linux 5.4.0-117-genericQ_
stderr��������K2022/06/17 19:12:58 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576_L
stderr��������82022/06/17 19:12:58 [notice] 1#1: start worker processesLM
stderr��������92022/06/17 19:12:58 [notice] 1#1: start worker process 31Mn
stdout�������Z172.17.0.1 - - [17/Jun/2022:19:13:00 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.68.0" "-"n

```



```bash

user@server:~$ cat /etc/docker/daemon.json
{
  "log-driver": "local",
  "log-opts": {
    "max-size": "10m"
  }
}

user@server:~$ sudo systemctl restart docker

user@server:~$ sudo docker run -d --name rbm-dkr-06-global -p 127.0.0.1:8893:80 nginx:stable

root@server:~# curl --silent http://127.0.0.1:8893 > /dev/null
root@server:~# curl --silent http://127.0.0.1:8893 > /dev/null

root@server:~# cat /var/lib/docker/containers/7fca53fdc37d3fb98b4fa11b05c723e0ae2d2518a104fd515a41ac43f6c72fd3/local-logs/container.log
t
stdout���ߨ���`/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configurationt]
stdout���ߨ���I/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/]i
stdout������U/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.shiq
stdout�������]10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.confqs
stdout�������_10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.confse
stdout�ˠ����Q/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.shee
stdout�������Q/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sheU
stdout�������A/docker-entrypoint.sh: Configuration complete; ready for start upUT
stderr��ހ����@2022/06/17 19:17:51 [notice] 1#1: using the "epoll" event methodTB
stderr��߀����.2022/06/17 19:17:51 [notice] 1#1: nginx/1.22.0Be
stderr��������Q2022/06/17 19:17:51 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6) eQ
stderr��������=2022/06/17 19:17:51 [notice] 1#1: OS: Linux 5.4.0-117-genericQ_
stderr��������K2022/06/17 19:17:51 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576_L
stderr��������82022/06/17 19:17:51 [notice] 1#1: start worker processesLM
stderr��������92022/06/17 19:17:51 [notice] 1#1: start worker process 31Mn
stdout�۹�����Z172.17.0.1 - - [17/Jun/2022:19:19:02 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.68.0" "-"nn
stdout��������Z172.17.0.1 - - [17/Jun/2022:19:19:06 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.68.0" "-"n

```