
Dockerfile
```Dockerfile
FROM alpine:latest
ARG MYARG
RUN apk update && apk add build-base
```

```bash

docker build . --cache-from "ID or name"

user@server:~$ sudo time docker build . -t cache:1
Successfully built 16930fa4f26a
Successfully tagged cache:1
0.03user 0.03system 0:12.59elapsed 0%CPU (0avgtext+0avgdata 58284maxresident)k
0inputs+0outputs (0major+7624minor)pagefaults 0swaps



user@server:~$ sudo time docker build . -t cache:2
Successfully built 16930fa4f26a
Successfully tagged cache:2
0.05user 0.03system 0:00.23elapsed 40%CPU (0avgtext+0avgdata 57988maxresident)k
112inputs+0outputs (0major+7395minor)pagefaults 0swaps


user@server:~$ sudo time docker build . -t cache:2 --no-cache
Successfully built 5c7b07338f3d
Successfully tagged cache:2
0.04user 0.02system 0:09.74elapsed 0%CPU (0avgtext+0avgdata 58480maxresident)k
0inputs+0outputs (0major+7805minor)pagefaults 0swaps



user@server:~$ sudo time docker build . -t cache:3 --build-arg MYARG=3
Successfully built 929a79348c38
Successfully tagged cache:3
0.03user 0.03system 0:08.40elapsed 0%CPU (0avgtext+0avgdata 58416maxresident)k
8inputs+0outputs (0major+7617minor)pagefaults 0swaps


user@server:~$ sudo time docker build . -t cache:4 --build-arg MYARG=4
Successfully built 810362cc5c7d
Successfully tagged cache:3
0.03user 0.03system 0:08.58elapsed 0%CPU (0avgtext+0avgdata 58672maxresident)k
0inputs+0outputs (0major+7585minor)pagefaults 0swaps

```