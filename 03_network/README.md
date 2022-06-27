```bash
user@server:~$ sudo docker network create -d bridge rbm-25-bridge
9ae062f2717ddda4eb849d8f7242f9c73cd532d8a8b1e9aa8f470dc30ae98fdd

user@server:~$ sudo docker network list
NETWORK ID     NAME            DRIVER    SCOPE
4014518d4ffb   bridge          bridge    local
5f27884c01f1   host            host      local
3ac5934a7279   none            null      local
9ae062f2717d   rbm-25-bridge   bridge    local


user@server:~$ sudo docker run -d --network rbm-25-bridge --name rbm-dkr-25-nginx nginx:stable


user@server:~$ sudo docker run -it --network rbm-25-bridge --name rbm-dkr-25-pinger alpine:3.10
/ # apk add curl --no-cache

/ # curl rbm-dkr-25-nginx
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
exit


user@server:~$ sudo docker inspect rbm-dkr-25-nginx | grep IPAddress
            "SecondaryIPAddresses": null,
            "IPAddress": "",
                    "IPAddress": "172.18.0.2",

user@server:~$ sudo docker run -it --network bridge --name another_network alpine:3.10
/ # apk add curl --no-cache
/ # curl rbm-dkr-25-nginx
curl: (6) Could not resolve host: rbm-dkr-25-nginx
/ # curl 172.18.0.2
curl: (6) Could not resolve host: 172.18.0.2
```