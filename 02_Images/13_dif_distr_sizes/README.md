
```bash
user@server:~$ git clone https://github.com/apache/zookeeper.git

user@server:~/zookeeper$ git checkout  remotes/origin/branch-3.7

user@server:~/zookeeper$ vim Dockerfile.alpine

user@server:~/zookeeper$ sudo docker build . --file Dockerfile.alpine  -t zookeeper-alpine:latest

user@server:~/zookeeper$ sudo docker images
REPOSITORY         TAG            IMAGE ID       CREATED          SIZE
zookeeper-alpine   latest         3ec74a04cce0   10 seconds ago   518MB
```

Dockerfile.alpine
```Dockerfile
FROM openjdk:8-jdk-alpine

WORKDIR /apache-zookeeper-3.7.1-bin

COPY .	.

RUN apk add --no-cache curl && \
	curl https://dlcdn.apache.org/maven/maven-3/3.8.6/binaries/apache-maven-3.8.6-bin.tar.gz -o maven.tar && \
	tar -xf maven.tar && \
	apache-maven-3.8.6/bin/mvn clean install -DskipTests && \
	apk del curl && \
	rm -rf maven.tar apache-maven-3.8.6
```


```bash
user@server:~/zookeeper$ vim Dockerfile.buster

user@server:~/zookeeper$ sudo docker build . --file Dockerfile.buster  -t zookeeper-buster:latest

user@server:~/zookeeper$ sudo docker images
REPOSITORY         TAG            IMAGE ID       CREATED          SIZE
zookeeper-buster   latest         4dda7d32cbc0   13 seconds ago   927MB
```

Dockerfile.buster
```Dockerfile
FROM openjdk:8-jdk-buster

WORKDIR /apache-zookeeper-3.7.1-bin

COPY .	.

RUN curl https://dlcdn.apache.org/maven/maven-3/3.8.6/binaries/apache-maven-3.8.6-bin.tar.gz -o maven.tar && \
	tar -xf maven.tar && \
	apache-maven-3.8.6/bin/mvn clean install -DskipTests && \
	rm -rf maven.tar apache-maven-3.8.6
```