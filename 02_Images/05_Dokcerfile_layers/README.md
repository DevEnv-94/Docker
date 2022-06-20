
Default 190MB wrong
```Dockerfile
FROM ubuntu:20.04

ENV testenv1=env1

RUN groupadd --gid 2000 user && useradd --uid 2000 --gid 2000 --shell /bin/bash --create-home user

RUN apt-get update -y && apt-get install nginx -y
RUN rm -rf /var/lib/apt/lists/*

COPY testfile .
RUN chown user:user testfile

USER user

CMD ["sleep infinity"]
```

Optimized 143MB
```Dockerfile
FROM ubuntu:20.04

ENV testenv1=env1

RUN groupadd --gid 2000 user && useradd --uid 2000 --gid 2000 --shell /bin/bash --create-home user

RUN apt-get update -y && apt-get install nginx -y && rm -rf /var/lib/apt/lists/*

COPY --chown=user testfile .

USER user

CMD ["sleep infinity"]
```