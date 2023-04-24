# Docker超入門

## Docker入門②

- アカウント作成
- 「Docker Desktop for mac」ダウンロード
- 「Dockerfile」>>「Docker image」>>「コンテナ」

```bash
# Dockerにログイン
$ docker login

Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: ginmaru0docker
Password:
Login Succeeded

Logging in with your password grants your terminal complete access to your account.
For better security, log in with a limited-privilege personal access token. Learn more at https://docs.docker.com/go/access-tokens/
```

```bash
# pullコマンド
# docker pull <image-name:tag-name>
$ docker pull hello-world:latest

latest: Pulling from library/hello-world
2db29710123e: Pull complete
Digest: sha256:53f1bbee2f52c39e41682ee1d388285290c5c8a76cc92b42687eecf38e0af3f0
Status: Downloaded newer image for hello-world:latest
docker.io/library/hello-world:latest
```

- [Docker Hub Library](https://hub.docker.com/u/library/)

```bash
# Docker imageのリスト
$ docker images

REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
alpine/git    latest    22a2874e1112   11 days ago     39.5MB
hello-world   latest    feb5d9fea6a5   10 months ago   13.3kB
```

```bash
# imageからコンテナ作成
# docker run <image-name:tag-name>
$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

```bash
# コンテナの一覧
$ docker ps -a

CONTAINER ID   IMAGE         COMMAND                  CREATED             STATUS                         PORTS     NAMES
e2f9bff88d26   hello-world   "/hello"                 9 minutes ago       Exited (0) 9 minutes ago                 stoic_northcutt
cbb3db81c719   alpine/git    "git clone https://g…"   About an hour ago   Exited (0) About an hour ago             repo
```

```bash
# imageからコンテナを作り中に入る
# docker run -it <image-name:tag-name> bash
$ docker run -it hello-world bash

docker: Error response from daemon: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: exec: "bash": executable file not found in $PATH: unknown.
```

## Docker入門③

```bash
$ docker run -it ubuntu bash

Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
405f018f9d1d: Pull complete
Digest: sha256:b6b83d3c331794420340093eb706a6f152d9c1fa51b262d9bf34594887c2c7ac
Status: Downloaded newer image for ubuntu:latest
root@81c8b4c6b278:/#
```

```bash
root@81c8b4c6b278:/# pwd
/

root@81c8b4c6b278:/# ls
bin   dev  home  lib32  libx32  mnt  proc  run   srv  tmp  var
boot  etc  lib   lib64  media   opt  root  sbin  sys  usr
```

```bash
# ファイル作成
root@81c8b4c6b278:/# touch test
```

```bash
# コンテナからexit
root@81c8b4c6b278:/# exit
```

```bash
$ docker ps -a

CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                          PORTS     NAMES
81c8b4c6b278   ubuntu        "bash"                   7 minutes ago    Exited (0) About a minute ago             flamboyant_wozniak
8009ced12d17   hello-world   "bash"                   13 minutes ago   Created                                   nostalgic_snyder
e2f9bff88d26   hello-world   "/hello"                 32 minutes ago   Exited (0) 32 minutes ago                 stoic_northcutt
cbb3db81c719   alpine/git    "git clone https://g…"   2 hours ago      Exited (0) 2 hours ago                    repo
```

```bash
# コンテナのrestart
$ docker restart flamboyant_wozniak
flamboyant_wozniak

$ docker ps -a
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                      PORTS     NAMES
81c8b4c6b278   ubuntu        "bash"                   10 minutes ago   Up 16 seconds                         flamboyant_wozniak
8009ced12d17   hello-world   "bash"                   17 minutes ago   Created                               nostalgic_snyder
e2f9bff88d26   hello-world   "/hello"                 36 minutes ago   Exited (0) 36 minutes ago             stoic_northcutt
cbb3db81c719   alpine/git    "git clone https://g…"   2 hours ago      Exited (0) 2 hours ago                repo
```

```bash
# すでにあるコンテナに入る
# docker exec <container-name> -it bash
$ docker exec -it flamboyant_wozniak bash
root@81c8b4c6b278:/#

root@81c8b4c6b278:/# ls
bin   dev  home  lib32  libx32  mnt  proc  run   srv  test  usr
boot  etc  lib   lib64  media   opt  root  sbin  sys  tmp   var
```

- コンテナからdetachする
  - ctrl + p + q
  - 動かしたままにする
  - 止めない
  - コンテナを起動したままにする

```bash
root@81c8b4c6b278:/# read escape sequence
```

```bash
$ docker ps -a
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                      PORTS     NAMES
81c8b4c6b278   ubuntu        "bash"                   18 minutes ago   Up 8 minutes                          flamboyant_wozniak
8009ced12d17   hello-world   "bash"                   25 minutes ago   Created                               nostalgic_snyder
e2f9bff88d26   hello-world   "/hello"                 44 minutes ago   Exited (0) 44 minutes ago             stoic_northcutt
cbb3db81c719   alpine/git    "git clone https://g…"   2 hours ago      Exited (0) 2 hours ago                repo
```

```bash
# 動いているコンテナに入る
# docker exec it <container-name> bash
$ docker exec -it flamboyant_wozniak bash
root@81c8b4c6b278:/# ls
bin   dev  home  lib32  libx32  mnt  proc  run   srv  test  usr
boot  etc  lib   lib64  media   opt  root  sbin  sys  tmp   var
```

## Docker入門④

```bash
$ docker ps -a
CONTAINER ID   IMAGE         COMMAND                  CREATED             STATUS                   PORTS     NAMES
81c8b4c6b278   ubuntu        "bash"                   About an hour ago   Up About an hour                   flamboyant_wozniak
8009ced12d17   hello-world   "bash"                   About an hour ago   Created                            nostalgic_snyder
e2f9bff88d26   hello-world   "/hello"                 2 hours ago         Exited (0) 2 hours ago             stoic_northcutt
cbb3db81c719   alpine/git    "git clone https://g…"   3 hours ago         Exited (0) 3 hours ago             repo
```

```bash
$ docker exec -it flamboyant_wozniak bash
root@81c8b4c6b278:/# ls
bin   dev  home  lib32  libx32  mnt  proc  run   srv  test  usr
boot  etc  lib   lib64  media   opt  root  sbin  sys  tmp   var
```

```bash
# Docker imageとして保存する
# docker commit <container-name> <new image-name>
root@81c8b4c6b278:/# read escape sequence
$ docker commit flamboyant_wozniak ubuntu:updated
sha256:1cddae41d8c0d5b9227e9d253c90b82e918cb972e7d9b0f96d67aa3a42a5cab7
```

```bash
$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
ubuntu        updated   1cddae41d8c0   23 seconds ago   77.8MB
alpine/git    latest    22a2874e1112   11 days ago      39.5MB
ubuntu        latest    27941809078c   7 weeks ago      77.8MB
hello-world   latest    feb5d9fea6a5   10 months ago    13.3kB
```

```bash
$ docker run -it ubuntu:updated bash
root@c1e4b94a689d:/# ls
bin   dev  home  lib32  libx32  mnt  proc  run   srv  test  usr
boot  etc  lib   lib64  media   opt  root  sbin  sys  tmp   var
```

- Docker Hubにリポジトリを作成


