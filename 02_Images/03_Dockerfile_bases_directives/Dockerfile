FROM ubuntu:18.04

RUN apt-get update && apt-get install nginx -y

WORKDIR /etc/nginx

COPY . .

ENTRYPOINT ["/usr/sbin/nginx"]

CMD [ "-g daemon off;" ]

EXPOSE 80

VOLUME [ "/var/lib/nginx" ]