

```Dockerfile
FROM $BUILD_IMAGE AS build

FROM $RUNTIME_IMAGE

COPY --from=build $SRC_COMPILED_PATH $DEST_COMPILED_PATH
```

```Dockerfile
FROM golang:1.16.5-alpine as build

ENV GO111MODULE=auto GOPATH=/go

WORKDIR /go/application/

COPY . .

RUN apk add --update git && \
    rm -rf /var/cache/apk/*/apk/* && \
    go mod init module && \
    go mod tidy && \
    go get ./... && \
    go build -o /app .


FROM alpine:3.10.3

COPY --from=build /app /app

CMD ["/app"]

```bash
user@server:~$ git clone https://gitlab.rebrainme.com/docker-course-students/gocalc.git

user@server:~/gocalc$ sudo docker build . -t dkr-14-gocalc
Successfully built 7b7550d49f7d
Successfully tagged dkr-14-gocalc:latest

user@server:~/gocalc$ git pull https://gitlab.rebrainme.com/docker_users_repos/3018/dkr-14-gocalc
user@server:~/gocalc$ git add Dockerfile
user@server:~/gocalc$ git push https://gitlab.rebrainme.com/docker_users_repos/3018/dkr-14-gocalc
```