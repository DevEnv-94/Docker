ARG SECRET=secret

FROM golang:1.16.5-alpine

ARG SECRET

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

COPY --from=0 /app /app

ARG SECRET

RUN echo $SECRET > /file

CMD ["/app"]

