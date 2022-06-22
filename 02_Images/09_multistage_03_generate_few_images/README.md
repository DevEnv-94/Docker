

The most common situation in which such a scheme is used is to remove the burden of returning static from the main application. That is, when we put a caching proxy (for example, nginx) in front of our application, which gives up all the static, and dynamic requests - REST requests - are proxied to the backend behind it.

```bash
user@server:~$ git clone https://github.com/grafana/grafana.git

user@server:~/grafana$ sudo docker build --target app -t grafana:app .

user@server:~/grafana$ sudo docker images
REPOSITORY   TAG              IMAGE ID       CREATED              SIZE
grafana      static           96e0cd198f38   About a minute ago   58.6MB
grafana      app              26049f1e676b   6 minutes ago        246MB
```

This Dockerfile just example how its could be, only on Dockerfile side without nginx.conf
```Dockerfile
# Golang build container
FROM golang:1.12.9-stretch

WORKDIR $GOPATH/src/github.com/grafana/grafana

COPY go.mod go.sum ./
COPY vendor vendor

RUN go mod verify

COPY pkg pkg
COPY build.go build.go
COPY package.json package.json

RUN go run build.go build

# Node build container
FROM node:10.14.2

WORKDIR /usr/src/app/

COPY package.json yarn.lock ./
COPY packages packages

RUN yarn install --pure-lockfile --no-progress

COPY Gruntfile.js tsconfig.json tslint.json ./
COPY public public
COPY scripts scripts
COPY emails emails

ENV NODE_ENV production
RUN ./node_modules/.bin/grunt build

# APP container
FROM ubuntu:18.04

LABEL maintainer="Grafana team <hello@grafana.com>"

ARG GF_UID="472"
ARG GF_GID="472"

ENV PATH=/usr/share/grafana/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin \
    GF_PATHS_CONFIG="/etc/grafana/grafana.ini" \
    GF_PATHS_DATA="/var/lib/grafana" \
    GF_PATHS_HOME="/usr/share/grafana" \
    GF_PATHS_LOGS="/var/log/grafana" \
    GF_PATHS_PLUGINS="/var/lib/grafana/plugins" \
    GF_PATHS_PROVISIONING="/etc/grafana/provisioning"

WORKDIR $GF_PATHS_HOME

RUN apt-get update && apt-get upgrade -y && \
    apt-get install -qq -y libfontconfig1 ca-certificates && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/*

COPY conf ./conf

RUN mkdir -p "$GF_PATHS_HOME/.aws" && \
    groupadd -r -g $GF_GID grafana && \
    useradd -r -u $GF_UID -g grafana grafana && \
    mkdir -p "$GF_PATHS_PROVISIONING/datasources" \
             "$GF_PATHS_PROVISIONING/dashboards" \
             "$GF_PATHS_PROVISIONING/notifiers" \
             "$GF_PATHS_LOGS" \
             "$GF_PATHS_PLUGINS" \
             "$GF_PATHS_DATA" && \
    cp "$GF_PATHS_HOME/conf/sample.ini" "$GF_PATHS_CONFIG" && \
    cp "$GF_PATHS_HOME/conf/ldap.toml" /etc/grafana/ldap.toml && \
    chown -R grafana:grafana "$GF_PATHS_DATA" "$GF_PATHS_HOME/.aws" "$GF_PATHS_LOGS" "$GF_PATHS_PLUGINS" "$GF_PATHS_PROVISIONING" && \
    chmod 777 -R "$GF_PATHS_DATA" "$GF_PATHS_HOME/.aws" "$GF_PATHS_LOGS" "$GF_PATHS_PLUGINS" "$GF_PATHS_PROVISIONING"

COPY --from=0 /go/src/github.com/grafana/grafana/bin/linux-amd64/grafana-server /go/src/github.com/grafana/grafana/bin/linux-amd64/grafana-cli ./bin/
COPY --from=1 /usr/src/app/public ./public
COPY --from=1 /usr/src/app/tools ./tools
COPY tools/phantomjs/render.js ./tools/phantomjs/render.js

EXPOSE 3000

COPY ./packaging/docker/run.sh /run.sh

USER grafana
ENTRYPOINT [ "/run.sh" ]

# Static container
FROM nginx:alpine AS static

COPY --from=app /usr/share/grafana/public /public

CMD ["nginx" "-g" "daemon off;"]
```

```bash

```