# Docker

---

[toc]





### Terminology

Register, 注册服务器, 用于管理 image 仓库. 

Repository, image 仓库, 用于存储具体的 docker image, 起到的是仓库存储作用. Docker 存储库是一个具有相同名称(repository)的不同 image 的集合, 这些 image 具有不同的标记(tag), 通俗的理解就是 docker repository一般存放的是一类镜像, 这一类镜像只不过是 tag 版本不同. 在一个 repository 中一个镜像(image)的 tag 就是一个 alphanumeric 标识.

> 实际上注册服务器是管理仓库的具体服务器, 每个服务器上可以有多个仓库, 而每个仓库下面有多个 image. 
>
> 从这方面来说, 仓库可以被认为是一个具体的项目或目录.
>
> 例如: 对于仓库地址 `docker.io/ubuntu` 来说, `docker.io` 是注册服务器地址, `ubuntu` 是仓库名.
>
> 我理解:
>
> Register 是一个服务, 用户管理逻辑概念的 repository, 而 repository 对于 register 是逻辑概念, 其自身实际是 register 服务管理的目录. 
>
> 换句话说, 当提到 register 时是指管理 repository 的服务. 而提到 repository 时是说具体 image 存放的目录. 二者在一个逻辑服务器上.

Docker Hub, Docker 官方的公共仓库, 可以免费注册 Docker 账号来使用. docker login 交互方式登录. docker logout 退出. docker hub 上的 image 分两类, 

1) 基础 image 或根 image, 比如, centos, 这些基础 image 由 Docker 公司创建、验证、支持、提供. 这样的 image 往往使用单个单词作为名字.
1) 比如: `ansible/centos7-ansible` image, 它是由 Docker Hub 的注册用户创建并维护的, 往往带有用户名称前缀. 可以通过前缀 `username/` 来指定使用某个用户提供的镜像, 比如 ansible 用户. 

另外, 在查找的时候通过 `--filter=stars=N` 参数可以指定仅显示收藏数量为 `N` 以上的 image.



### 先决条件

#### OS 必须条件

. CentOS 7 或者 CentOS 8

. CentOS 的 centos-extras 仓库必须开启. https://wiki.centos.org/AdditionalResources/Repositories

. `overlay2` 存储驱动

#### https://docs.docker.com/engine/install/centos/

#### 卸载老版本

```shell
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
如果 yum 报告没有安装这些软件包，那也没关系。
保留 /var/lib/docker/ 的内容，包括图像、容器、卷和网络。 Docker Engine 包现在称为 docker-ce。
```



#### 安装方法

不同安装方法:

. 大多数人从 Docker 仓库安装, 这是推荐的.

. RPM 包手动安装, 通常用于没网的情况.

. 使用自动化的便捷脚本来安装 Docker, 通常用于测试或者开发环境.

##### 使用仓库安装

在新主机上首次安装 Docker Engine 之前，您需要设置yum 的  Docker 仓库。之后，您可以从仓库安装和更新 Docker。

###### 设置仓库
安装 yum-utils 包（提供 yum-config-manager 实用程序）并设置稳定版仓库。

```shell
$ sudo yum install -y yum-utils
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

> 选项: 启用 nightly 夜间或 test 测试仓库。
> 
> 这些仓库包含在上面的 docker.repo 文件中，但默认情况下是禁用的。您可以在stable稳定仓库旁边启用它们。以下命令启用nightly夜间仓库。
>
> $ sudo yum-config-manager --enable docker-ce-nightly
>
> 要启用测试通道channel，请运行以下命令：
> 
> $ sudo yum-config-manager --enable docker-ce-test
>
> 您可以通过运行带有 --disable 标志的 yum-config-manager 命令来禁用nightly夜间或测试仓库。要重新启用它，请使用 --enable 标志。以下命令禁用nightly夜间仓库。
>
> $ sudo yum-config-manager --disable docker-ce-nightly
> 了解nightly夜间channel频道和测试channel频道。

###### 安装 Docker Engine

1. 安装 Docker Engine 和 containerd 最后的版本

```shell
$ sudo yum install docker-ce docker-ce-cli containerd.io
```

此命令会安装 Docker，但不会启动 Docker。它还创建了一个 docker 组，但是，默认情况下它不会向该组添加任何用户。

2. To install a *specific version* of Docker Engine, list the available versions in the repo, then select and install:

   a. List and sort the versions available in your repo. This example sorts results by version number, highest to lowest, and is truncated:

   若要安装 Docker 引擎的*特定版本*，请在存储库中列出可用版本，然后选择并安装： 一个。列出存储库中可用的版本并对其进行排序。此示例按版本号从最高到最低对结果进行排序，并被截断：

   ```
   $ yum list docker-ce --showduplicates | sort -r
   
   docker-ce.x86_64  3:18.09.1-3.el7                     docker-ce-stable
   docker-ce.x86_64  3:18.09.0-3.el7                     docker-ce-stable
   docker-ce.x86_64  18.06.1.ce-3.el7                    docker-ce-stable
   docker-ce.x86_64  18.06.0.ce-3.el7                    docker-ce-stable
   ```

   The list returned depends on which repositories are enabled, and is specific to your version of CentOS (indicated by the `.el7` suffix in this example).

   b. Install a specific version by its fully qualified package name, which is the package name (`docker-ce`) plus the version string (2nd column) starting at the first colon (`:`), up to the first hyphen, separated by a hyphen (`-`). For example, `docker-ce-18.09.1`.

   返回的列表取决于启用的存储库，并特定于您的 CentOS 版本（在本例中由“.el7”后缀表示）。 

   b.通过其完全限定的软件包名称安装特定版本，该名称是软件包名称 （'docker-ce'） 加上从第一个冒号 （'：'） 开始的版本字符串（第 2 列），直到第一个连字符，用连字符 （'-'） 分隔。例如，'docker-ce-18.09.1'。

   ```
   $ sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
   ```

   This command installs Docker, but it doesn’t start Docker. It also creates a `docker` group, however, it doesn’t add any users to the group by default.

   此命令安装 Docker，但不启动 Docker。它还会创建一个“docker”组，但是，默认情况下不会向该组添加任何用户。

3. Start Docker.

   ```
   $ sudo systemctl start docker
   ```

4. Verify that Docker Engine is installed correctly by running the `hello-world` image.

   ```
   $ sudo docker run hello-world
   ```

   This command downloads a test image and runs it in a container. When the container runs, it prints a message and exits.

This installs and runs Docker Engine. Use `sudo` to run Docker commands. Continue to [Linux postinstall](https://docs.docker.com/engine/install/linux-postinstall/) to allow non-privileged users to run Docker commands and for other optional configuration steps.

https://docs.docker.com/engine/install/centos/



### Uninstall Docker Engine

1. Uninstall the Docker Engine, CLI, and Containerd packages:

   ```
   $ sudo yum remove docker-ce docker-ce-cli containerd.io
   ```

2. Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

   ```
   $ sudo rm -rf /var/lib/docker
   $ sudo rm -rf /var/lib/containerd
   ```

You must delete any edited configuration files manually.



Linux 的安装后步骤







```shell
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

cd /etc/yum.repos.d/docker-ce.repo

sudo yum install docker-ce docker-ce-cli containerd.io
sudo systemctl start docker
systemctl status docker
chkconfig --list
systemctl status docker
systemctl enable docker
sudo systemctl enable docker
systemctl status docker

mkdir docker
touch app.py
touch Dockerfile

sudo docker build --tag py .

sudo docker image ls
sudo docker images

sudo docker run py
sudo docker images
sudo docker run c5e9de8c315c
sudo docker run -d c5e9de8c315c

sudo docker ps
sudo docker ps -a

sudo docker stop b249de0d65e6
sudo docker rm edea8185e3c3

sudo docker run -d -p 8889:8888 -p 1111:333 c5e9de8c315c
从外:往里
netstat -ntlp

sudo docker volume ls

sudo docker run -d -p 8889:8888 -p 1111:333 -v v1:/app c5e9de8c315c

sudo docker ps
sudo docker volume ls
sudo docker volume inspect
sudo docker volume inspect V1
sudo ls /var/lib/docker/volumes/V1/_data

sudo docker volume rm v1
sudo docker rm a2ef0a86e4e4 98f1d29930af 475176db48bb 0e4d2706eece b249de0d65e6 ba24fe5c542a 06a455c6179f

sudo docker image prune

sudo docker run -d -p 8889:8888 -v `pwd`:/app py

sudo docker exec -it 800705c96724 bash



```





### Dockerfile
该指令不区分大小写. 然而, 约定是它们是大写的, 以便更容易地将它们与参数区分开来.

```dockerfile
格式:
大写指令 参数
```

Dockerfile 是个纯文本文件, 从 From 指令开始按顺序执行指令, 在 FROM 指令之前只能会存在一个或者多个 ARG 指令参数, 同时不能再有命令. 这些 ARG 指令是 FROM 行中要用到的参数.







#### ARG, 构建时变量, 在构建时使用

```dockerfile
里:
ARG <name>[=<default value>]

外:
docker build --build-arg <varname>=<value>

报错:
[Warning] One or more build-args [foo] were not consumed.
```

ARG 定义一个变量, 这个变量是在 Dockerfile 文件里定义的构建时从命令行传入的构建参数. 命令行 docker build 使用的 --build-arg <varname>=<value> 参数. 如果里外对应不上, 会在 docker build 时报一个 warning.

密码之类的不要用于构建时变量, 因为其他image的用户通过 docker history 命令可以看到.

```dockerfile
ARG user1=someuser
```

ARG 默认值, 如果构建时没有传递, 那么就自动使用默认值.





FROM 指定了将要构建的 父image.

```
```



CMD, 在 DOckerfile 中只能有一条 CMD 命令, 如果设置了多条 CMD, 只有最后一条 CMD 会生效.

VOLUME, 指定容器挂载点到宿主机自动生成的目录或其他容器, 一般的使用场景为需要持久化存储数据时. 一般不会在 Dockerfile 中用到, 更常见的还是在 docker run 的时候通过 -v 指定数据卷.

```dockerfile
# 容器的 /var/lib/mysql 目录会在运行时自动挂载为匿名卷，匿名卷在宿主机的 /var/lib/docker/volumes 目录下
VOLUME ["/var/lib/mysql"]
```

RUN, 有两种形式, 1)是shell形式, 2)是数组形式(双引号). 运行在当前 image 上面的一个新层里(RUN 会增加一个新层).  CMD, 有三种形式, 1)数组形式(双引号), 2)数组形式*ENTRYPOINT*的默认参数, 3)shell命令行形式. 整个 Dockerfile 中只有一个 CMD 有效, 多个 CMD 中, 只有最后一个 CMD 生效.

LABEL, 将原数据加入到 image. 一个 image 可以有多个 LABEL. 多个 LABEL 可以连写. 对个多个 FROM 中的 LABEL 都会又最后的 image 继承下来, 并且出现重名 LABEL 的话, 最后以最后的为准覆盖之前的.

ENV, 设置环境变量. 也会继承. 如果环境变量不是为了用于最终 image 中, 而是仅在构建期间, 这时用 RUN 或者 ARG, 而不是 ENV.



#### CLI

```shell
Usage:  docker build [OPTIONS] PATH | URL | -
从一个 Dockerfile 文件构建一个本地 image

以下二者是一摸一样的:
sudo docker build --help
sudo docker image build --help

-f, 指定 Dockerfile 文件名称, 默认为要构建目录的根目录下的 Dockerfile 文件名. 如: 'PATH/Dockerfile', 这里 PATH 是用例中的 PATH, 是要被构建的目录. 
-t, 设置名字和可选的 tag 名. 格式: name:tag
-q, 安静, 抑制生成输出并在成功时打印 image ID
--rm, 在成功构建后删除中间容器(默认值为 true)

sudo docker build -f ./Dockerfile -t kk .
按照用例规定 build 的必选是要指定的目录. 即, 对目录(目录根含有 Dockerfile 文件)构建

[chenchen@grpc01 tefd]$ sudo docker images
REPOSITORY   TAG               IMAGE ID       CREATED         SIZE
py           latest            6278d941ad4a   13 months ago   123MB
kk           latest            6278d941ad4a   13 months ago   123MB
python       3.6-slim-buster   eb32e0d643ed   13 months ago   112MB
[chenchen@grpc01 tefd]$ sudo docker build -t rr:v1 .
Sending build context to Docker daemon  3.072kB
Step 1/5 : FROM python:3.6-slim-buster
 ---> eb32e0d643ed
Step 2/5 : WORKDIR /app
 ---> Using cache
 ---> e8c7a0ead52a
Step 3/5 : RUN pip install flask
 ---> Using cache
 ---> 9f0836dbf81b
Step 4/5 : EXPOSE 8888
 ---> Using cache
 ---> 3b87ca271313
Step 5/5 : CMD ["python", "app.py"]
 ---> Using cache
 ---> 6278d941ad4a
Successfully built 6278d941ad4a
Successfully tagged rr:v1
[chenchen@grpc01 tefd]$ sudo docker images
REPOSITORY   TAG               IMAGE ID       CREATED         SIZE
kk           latest            6278d941ad4a   13 months ago   123MB
py           latest            6278d941ad4a   13 months ago   123MB
rr           v1                6278d941ad4a   13 months ago   123MB
python       3.6-slim-buster   eb32e0d643ed   13 months ago   112MB

⚠️ IMAGE ID 有重复, 这表示是同一个 image, 但 TAG 不同, REPOSITORY 也不同. 
自己总结: 
IMAGE ID, 就理解为 image 的唯一标识, 可以用在后续的 docker 指令中, 但对人不友好.
TAG, 理解为给 image 起个利于人读的名字, 等同于 image 别名理解. 即, 一个 image 可以有多个别名 tag, 而这些别名 tag 都可以有自己的仓库 REPOSITORY. 
REPOSITORY, 仓库, 不同 tag 可以有相同的仓库. 仓库是聚合 tag 用的逻辑概念, 并和注册服务的存储对应.

# 必须带上结尾的路径, 哪怕是点(.)
[chenchen@grpc01 tefd]$ sudo docker build -t registry.nevis.sina.com.cn/tiyu/rr:v5
"docker build" requires exactly 1 argument.
[chenchen@grpc01 tefd]$ sudo docker build -t registry.nevis.sina.com.cn/tiyu/rr:v5 .
Sending build context to Docker daemon  3.072kB
Step 1/5 : FROM python:3.6-slim-buster
 ---> eb32e0d643ed
Step 2/5 : WORKDIR /app
 ---> Using cache
 ---> e8c7a0ead52a
Step 3/5 : RUN pip install flask
 ---> Using cache
 ---> 9f0836dbf81b
Step 4/5 : EXPOSE 8888
 ---> Using cache
 ---> 3b87ca271313
Step 5/5 : CMD ["python", "app.py"]
 ---> Using cache
 ---> 6278d941ad4a
Successfully built 6278d941ad4a
Successfully tagged registry.nevis.sina.com.cn/tiyu/rr:v5
```

```shell
Usage:  docker images [OPTIONS] [REPOSITORY[:TAG]]
列出 images

-a， --all, 显示所有图像（默认隐藏中间图像） 
--degests, 显示摘要 
-f, --filter filter, 根据提供的条件过滤输出 
--format string, 使用 Go 模板漂亮打印图像 
--no-trunc, 不截断输出 
-q， --quiet 仅显示映像 ID

[chenchen@grpc01 tefd]$ sudo docker images --digests=true
[chenchen@grpc01 tefd]$ sudo docker images --digests

诊断步骤:
如果使用 --digests=true 选项(未标记的条目具有 Docker 内容信任签名摘要), 则可以看到这两个条目的差异:

拉取签名 image 后, "docker image" 输出中有一个 "extra" 条目(带标签<none>). 这使得很难对 image 进行 rmi (您必须强制它, 否则首先删除正确 tag 的条目, 或通过摘要删除.

```



```shell
Usage:  docker image COMMAND
管理 image
Usage:  docker image rm [OPTIONS] IMAGE [IMAGE...]
删除一个或者多个 image
别名: docker rmi, remove
Usage:  docker image history [OPTIONS] IMAGE
显示 image 的历史
Usage:  docker image inspect [OPTIONS] IMAGE [IMAGE...]
显示一个或者多个 image 的详细信息
Usage:  docker image push [OPTIONS] NAME[:TAG]
Usage:  docker push [OPTIONS] NAME[:TAG]
以上两个应该是同一个意思
Usage:  docker image pull [OPTIONS] NAME[:TAG|@DIGEST]
Usage:  docker pull [OPTIONS] NAME[:TAG|@DIGEST]
以上两个应该是同一个意思

ls, 列出本地 image
pull, 从 registry 拉 image 或者 repository 
push, 推 image 或者 repository 到 registry
rm, 删除一个或者多个 image
tag, 等同于直接 docker tag
build, 等同于直接 docker build
history, 显示 image 历史

[chenchen@grpc01 tefd]$ sudo docker image ls
REPOSITORY   TAG               IMAGE ID       CREATED         SIZE
kk           latest            6278d941ad4a   13 months ago   123MB
py           latest            6278d941ad4a   13 months ago   123MB
rr           v1                6278d941ad4a   13 months ago   123MB
rr           v2                6278d941ad4a   13 months ago   123MB
python       3.6-slim-buster   eb32e0d643ed   13 months ago   112MB

[chenchen@grpc01 tefd]$ sudo docker rm rr:v1
Error: No such container: rr:v1
[chenchen@grpc01 tefd]$ sudo docker image rm rr:v1
Untagged: rr:v1
[chenchen@grpc01 tefd]$ sudo docker image rm 6278d941ad4a
Error response from daemon: conflict: unable to delete 6278d941ad4a (must be forced) - image is referenced in multiple repositories
[chenchen@grpc01 tefd]$ sudo docker rmi rr:v2
Untagged: rr:v2

[chenchen@grpc01 tefd]$ sudo docker image history  6278d941ad4a
[chenchen@grpc01 tefd]$ sudo docker image history --no-trunc 6278d941ad4a

[chenchen@grpc01 tefd]$ sudo docker image inspect -f '' 6278d941ad4a
[chenchen@grpc01 tefd]$ sudo docker image inspect -f='' 6278d941ad4a
[chenchen@grpc01 tefd]$ sudo docker image inspect --format='' 6278d941ad4a
[chenchen@grpc01 tefd]$ sudo docker image inspect -f '' rr:v4

[chenchen@grpc01 tefd]$ sudo docker login registry.nevis.sina.com.cn
Authenticating with existing credentials...
Stored credentials invalid or expired
Username (chenchen): chenchen
Password: 
WARNING! Your password will be stored unencrypted in /home/chenchen/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
[chenchen@grpc01 tefd]$ sudo docker image push registry.nevis.sina.com.cn/tiyu/rr:v5
The push refers to repository [registry.nevis.sina.com.cn/tiyu/rr]
26455d666b04: Pushed 
76346ef86489: Pushed 
58c164828582: Pushed 
f9a763779f41: Pushed 
eac46588d645: Pushed 
ca44cfdf2cf5: Pushed 
77b323d4ec74: Pushed 

v5: digest: sha256:4b58d3d272e067c9169ae76b8b28eca4499cc2bb9984e0ae82b161f35a1f1c53 size: 1787
[chenchen@grpc01 tefd]$

[chenchen@grpc01 tefd]$ sudo docker pull registry.nevis.sina.com.cn/tiyu/rr:v7
v7: Pulling from tiyu/rr
Digest: sha256:4b58d3d272e067c9169ae76b8b28eca4499cc2bb9984e0ae82b161f35a1f1c53
Status: Downloaded newer image for registry.nevis.sina.com.cn/tiyu/rr:v7
registry.nevis.sina.com.cn/tiyu/rr:v7
[chenchen@grpc01 tefd]$ sudo docker images
REPOSITORY                           TAG               IMAGE ID       CREATED          SIZE
registry.nevis.sina.com.cn/tiyu/rr   v6                c957dd44cd57   30 minutes ago   123MB
py                                   latest            6278d941ad4a   13 months ago    123MB
rr                                   v3                6278d941ad4a   13 months ago    123MB
rr                                   v4                6278d941ad4a   13 months ago    123MB
registry.nevis.sina.com.cn/tiyu/rr   v7                6278d941ad4a   13 months ago    123MB
kk                                   latest            6278d941ad4a   13 months ago    123MB
python                               3.6-slim-buster   eb32e0d643ed   13 months ago    112MB

[chenchen@dev3_10.211.21.18 sports.weibo.com]$ sudo docker pull registry.nevis.sina.com.cn/tiyu/env.sports.weibo.com@sha256:794f18aa827a172d73eeeb1a5644e2c1cb23fc486023940376411d7ef9e2e1f1
registry.nevis.sina.com.cn/tiyu/env.sports.weibo.com@sha256:794f18aa827a172d73eeeb1a5644e2c1cb23fc486023940376411d7ef9e2e1f1: Pulling from tiyu/env.sports.weibo.com
Digest: sha256:794f18aa827a172d73eeeb1a5644e2c1cb23fc486023940376411d7ef9e2e1f1
Status: Image is up to date for registry.nevis.sina.com.cn/tiyu/env.sports.weibo.com@sha256:794f18aa827a172d73eeeb1a5644e2c1cb23fc486023940376411d7ef9e2e1f1
registry.nevis.sina.com.cn/tiyu/env.sports.weibo.com@sha256:794f18aa827a172d73eeeb1a5644e2c1cb23fc486023940376411d7ef9e2e1f1
```

```shell
Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
在一个新容器里运行一个命令.

sudo docker run -d c5e9de8c315c

-a, attach list, 结合|连接列表, 连接到 STDIN, STDOUT 或 STDERR
-d, detach, 分离, 在后台运行容器, 并且打印出容器ID
-i, 交互式, 即使未连接, 也能保持 STDIN 是打开的
-p, 将容器的端口发布到主机
-t, 分配一个伪 tty
-v, 绑定装载一个卷
-w, 设置容器内部的工作目录
-h, 设置容器主机名
-u, 设置用户名或者用户UID, 格式: <name|uid>[:<group|gid>], 即 chenchen:chenchen 或者 32323:323231
--name string, 给容器分配一个名字, 不能重名
--network network, 把一个容器连接到网络上
--rm, 当容器 exit 时, 自动删除此容器

[chenchen@grpc01 tefd]$  sudo docker run -it python:3.6-slim-buster
[chenchen@grpc01 tefd]$  sudo docker run -it --name t01 python:3.6-slim-buster
[chenchen@grpc01 tefd]$  sudo docker run -it --name t02 --rm python:3.6-slim-buster

sudo docker run -it --rm -d -p 8088:80 -v /usr/home/chenchen/htdocs/sports.weibo.com:/data1/www/htdocs/sports.weibo.com --name chenchen.env.sports.weibo.com registry.nevis.sina.com.cn/tiyu/env.sports.weibo.com

[chenchen@dev3_10.211.21.18 _docker-env]$ sudo docker run -it --rm -d -p 8088:80 -v /usr/home/chenchen/htdocs/sports.weibo.com:/data1/www/htdocs/sports.weibo.com --name chenchen registry.nevis.sina.com.cn/tiyu/env.sports.weibo.com8d2ebabd56cf51640acc35c525d1b9b114810be30280de748687ddfede3ac142

[chenchen@dev3_10.211.21.18 _docker-env]$ sudo docker run --rm -d -p 8089:80 -v /usr/home/chenchen/htdocs/sports.weibo.com:/data1/www/htdocs/sports.weibo.com --name chenchen01 fromimage
5c1866c770430eb4a5e07b0f10368658df167060081af5917f94dbcc43320f2b
[chenchen@dev3_10.211.21.18 _docker-env]$ sudo docker ps
CONTAINER ID   IMAGE                                                  COMMAND                  CREATED          STATUS          PORTS                   NAMES
5c1866c77043   fromimage                                              "/etc/dAppCluster/st…"   3 minutes ago    Up 3 minutes    0.0.0.0:8089->80/tcp    chenchen01
8d2ebabd56cf   registry.nevis.sina.com.cn/tiyu/env.sports.weibo.com   "/etc/dAppCluster/st…"   21 minutes ago   Up 21 minutes   0.0.0.0:8088->80/tcp    chenchen
90c415a55723   sports.weibo.com-env                                   "/etc/dAppCluster/st…"   3 days ago       Up 3 days       0.0.0.0:8080->80/tcp    wenqiang1
5746fece6316   sports.weibo.com-env                                   "/etc/dAppCluster/st…"   2 weeks ago      Up 2 weeks      0.0.0.0:21576->80/tcp   runyan1
[chenchen@dev3_10.211.21.18 _docker-env]$ sudo docker exec -it 5c1866c77043 /bin/bash
[root@5c1866c77043 /]# cd ~
```

```shell
Usage:  docker ps [OPTIONS]
查看容器

sudo docker ps
sudo docker ps -a

不带参数表示只显示正在运行的容器
-a, 显示所有容器

[chenchen@grpc01 tefd]$ sudo docker ps -a
CONTAINER ID   IMAGE       COMMAND           CREATED         STATUS                       PORTS      NAMES
4436ad1e31de   kk:latest   "python app.py"   2 minutes ago   Exited (2) 2 minutes ago                t01
9e457558a189   kk:latest   "--name t01"      2 minutes ago   Created                      8888/tcp   goofy_shockley
800705c96724   py          "python app.py"   13 months ago   Exited (137) 13 months ago              keen_fermat
```

```shell
Usage:  docker top CONTAINER [ps OPTIONS]
显示一个容器里正在运行的进程
```

```shell
Usage:  docker stop [OPTIONS] CONTAINER [CONTAINER...]
停止一个或者多个正在运行的容器

sudo docker stop b249de0d65e6
sudo docker stop b249de0d65e6 -t 1

不带参数, 默认是10s
-t, 停之前的等待时间(秒)

[chenchen@grpc01 tefd]$ sudo docker stop 800705c96724
800705c96724
[chenchen@dev3_10.211.21.18 _docker-env]$ sudo docker stop 5c1866c77043 8d2ebabd56cf -t 1
5c1866c77043
8d2ebabd56cf
```

```shell
Usage:  docker kill [OPTIONS] CONTAINER [CONTAINER...]
杀掉容器

-s, --signal string   Signal to send to the container (default "KILL")

sudo docker kill 800705c96724
```

```shell
Usage:  docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
在指定的容器ID上运行一个命令

  -d, --detach               Detached mode: run command in the background
      --detach-keys string   Override the key sequence for detaching a container
  -e, --env list             Set environment variables
      --env-file list        Read in a file of environment variables
  -i, --interactive          Keep STDIN open even if not attached
      --privileged           Give extended privileges to the command
  -t, --tty                  Allocate a pseudo-TTY
  -u, --user string          Username or UID (format: <name|uid>[:<group|gid>])
  -w, --workdir string       Working directory inside the container

sudo docker exec -it 07228ec25756 /bin/bash
```

```shell
Usage:  docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
从容器的变化来创建新镜像

  -a, --author string    Author (e.g., "John Hannibal Smith <hannibal@a-team.com>")
  -c, --change list      Apply Dockerfile instruction to the created image
  -m, --message string   Commit message
  -p, --pause            Pause container during commit (default true)

sudo docker commit -m 'openssh' -a 'Docker for ssh' ffe81683c404 ssh_box

[chenchen@dev3_10.211.21.18 _docker-env]$ sudo docker commit -m 'yocc' -a 'for yocc' 8d2ebabd56cf fromimage
sha256:3b4c5efbc6338a0fd0a1cb6f18cca94e09fadb129ebc6284ab4d345e73ca6a7a
[chenchen@dev3_10.211.21.18 _docker-env]$ sudo docker images
REPOSITORY                                             TAG       IMAGE ID       CREATED         SIZE
fromimage                                              latest    3b4c5efbc633   2 minutes ago   855MB
sports.weibo.com-env                                   latest    b4eaf9315a8d   2 weeks ago     853MB
registry.nevis.sina.com.cn/tiyu/env.sports.weibo.com   latest    b4eaf9315a8d   2 weeks ago     853MB
registry.api.weibo.com/csmonitor/php_base.php71        1.7       362185d8bf02   3 months ago    2.2GB
registry.dpool.sina.com.cn/dpool/dpool-web5            v33       c8a990b8d391   10 months ago   852MB
```



```shell
Usage:  docker rmi [OPTIONS] IMAGE [IMAGE...]
删除一个或者多个 image

-f, --force      Force removal of the image
    --no-prune   Do not delete untagged parents
    
[chenchen@grpc01 tefd]$ sudo docker images --digests=true
REPOSITORY                           TAG               DIGEST                                                                    IMAGE ID       CREATED          SIZE
registry.nevis.sina.com.cn/tiyu/rr   v6                sha256:1291ffbdf8f5926649e94e857807eb9f7617c81093ffdef439c640829188dab6   c957dd44cd57   45 minutes ago   123MB
kk                                   latest            <none>                                                                    6278d941ad4a   13 months ago    123MB
py                                   latest            <none>                                                                    6278d941ad4a   13 months ago    123MB
rr                                   v3                <none>                                                                    6278d941ad4a   13 months ago    123MB
rr                                   v4                <none>                                                                    6278d941ad4a   13 months ago    123MB
registry.nevis.sina.com.cn/tiyu/rr   <none>            sha256:4b58d3d272e067c9169ae76b8b28eca4499cc2bb9984e0ae82b161f35a1f1c53   6278d941ad4a   13 months ago    123MB
python                               3.6-slim-buster   sha256:a19c4a23d697b9cbb499de7e2760d0221f4bedb2d661e9ee51db7e5be43c6179   eb32e0d643ed   13 months ago    112MB
# docker rmi rep@DIGEST
[chenchen@grpc01 tefd]$ sudo docker rmi registry.nevis.sina.com.cn/tiyu/rr@sha256:4b58d3d272e067c9169ae76b8b28eca4499cc2bb9984e0ae82b161f35a1f1c53
Untagged: registry.nevis.sina.com.cn/tiyu/rr@sha256:4b58d3d272e067c9169ae76b8b28eca4499cc2bb9984e0ae82b161f35a1f1c53
```



```shell
Usage:  docker rm [OPTIONS] CONTAINER [CONTAINER...]
删除一个或者多个容器
本命令不能删除 image, 要删除 image, 用 docker image rm rep:tag 或者 imageid(所有同id都删光, 慎用), 或者用 docker rmi rep:tag
说明, docker rm 是删容器, docker rmi 是删 image. 如果同一个 image 有多个 tag, 那么 docker rmi -f imageid, 只要是这个 imageid 的都强制删除光(谨慎).

sudo docker rm edea8185e3c3

-v, 删除与指定容器相关联的 volumes

[chenchen@grpc01 tefd]$ sudo docker rm 9e457558a189
9e457558a189

```

```shell
Usage:  docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
创建一个 tag, 这个 tag 指向原 image.
标记本地 image, 将其归入某一仓库.
我觉得是 docker tag image repository:tag

[chenchen@grpc01 tefd]$ sudo docker tag 6278d941ad4a rr:v3
[chenchen@grpc01 tefd]$ sudo docker images
REPOSITORY   TAG               IMAGE ID       CREATED         SIZE
rr           v1                6278d941ad4a   13 months ago   123MB
rr           v2                6278d941ad4a   13 months ago   123MB
rr           v3                6278d941ad4a   13 months ago   123MB
kk           latest            6278d941ad4a   13 months ago   123MB
py           latest            6278d941ad4a   13 months ago   123MB
python       3.6-slim-buster   eb32e0d643ed   13 months ago   112MB
[chenchen@grpc01 tefd]$ sudo docker tag py:latest rr:v4
[chenchen@grpc01 tefd]$ sudo docker images
REPOSITORY   TAG               IMAGE ID       CREATED         SIZE
kk           latest            6278d941ad4a   13 months ago   123MB
py           latest            6278d941ad4a   13 months ago   123MB
rr           v1                6278d941ad4a   13 months ago   123MB
rr           v2                6278d941ad4a   13 months ago   123MB
rr           v3                6278d941ad4a   13 months ago   123MB
rr           v4                6278d941ad4a   13 months ago   123MB
python       3.6-slim-buster   eb32e0d643ed   13 months ago   112MB

```

```shell
Usage:  docker login [OPTIONS] [SERVER]
登录进一个 register

-u, 用户名
-p, 密码文件

```

```shell
Usage:  docker inspect [OPTIONS] NAME|ID [NAME|ID...]
返回 docker 对象低级别信息

-f '', -f='', --format=''
-s, 显示容器大小尺寸
--type container, ID 类型, 既可以是 image id, 也可以是 container id, 还可以是其他对象的名字和id


LABEL org.opencontainers.image.authors="SvenDowideit@home.org.au"

docker image inspect --format='' myimage
[chenchen@grpc01 tefd]$ sudo docker inspect -s 6278d941ad4a
[chenchen@grpc01 tefd]$ sudo docker inspect -s rr:v4
[chenchen@grpc01 tefd]$ sudo docker inspect -s --type container 11280031f06e
[chenchen@grpc01 tefd]$ sudo docker inspect -s --type=container t01
```

### 通过已有容器得到镜像, container -> image

```sh
1. 停容器
sudo docker stop <容器名|容器ID>
$ sudo docker stop v801

2. 将容器打包成镜像
sudo commit <容器名|容器ID> <镜像名>
$ sudo docker commit v801 centos:802

3. 用新镜像开启新容器
$ sudo docker run -itd --rm -p 5173:5173 -name <新容器名> <镜像名>
$ sudo docker run -itd --rm -p 5173:5173 --name v802 centos:802
```











### FAQ

#### ENV, RUN, ARG, LABEL 区别

ARG, 类似C的宏, 对 Dockerfile 文件文本进行替换.



#### Docker, Docker-Compose, Docker Swarm, Kubernets 区别

Dcoker 容器引擎, 构建 image, 创建 container

Docker-Compose 只能管理单机上的 container 的工具

Docker Swarm 可以管理多台机器上的 container 的工具

Kubernets 角色定位是和 Docker Swarm 一样.



#### Register, Repository 区别

公共: 

私有:

1. 对于 docker hub 中私人 repository, 建议采用 docker hub 用户名/repository:tagname 这样的形式
2. 对于私有 repository 建议采用 ip:port/repository:tagname 这样的形式, 或者 domain/repository:tag

![img](https://upload-images.jianshu.io/upload_images/6087133-5b8b1f00d2e8cec9.png?imageMogr2/auto-orient/strip|imageView2/2/w/815/format/webp)



#### Volumes, Bind mounts, tmpfs 区别

Volumes 和 Bind mounts 相同之处: 都是持久化容器的机制, 都能持久化容器.

Volumes 和 Bind mounts 不同之处: volume 是由 docker 来进行管理的, 而 bind mount 完全是依赖于主机的目录结构和操作系统. `bind mounts 功能较为有限. 创建一个 bind mount 之后, Host 上的一个文件/文件夹就相当于被[挂载](https://so.csdn.net/so/search?q=挂载&spm=1001.2101.3001.7020)到容器里了, 该文件/文件夹就被容器通过绝对路径引用了, **意味着这个文件/文件夹必须实现存在于 Host 上** . 而使用使用`volume 时, 会在 Host 主机上新建一个 **Docker 存储文件夹**，文件夹里的内容由 Docker 管理. `volume`映射的文件/文件夹不需要存在于 Host 上, 它们是按需生成的, 即可以被自动创建, 不需要实现在 Host 上创建. **虽然`bind mounts`很高效, 但是它依赖 Host 上存在特定的目录结构**, 这对于部署到新主机上显然不是一件好事, 所以推荐新应用使用 `volume`.

volume 相对于 bind mount的 优点:

* volume更加容易进行备份和迁移

* 可以通过docker客户端命令或者docker api来管理volume (比如：docker volume命令)

* volume可以在linux和windows容器中运行

* volume可以更加安全的在多个容器之间进行共享

* volume驱动程序允许在远程主机或云提供商上存储卷，以加密volume的内容或添加其他功能

* 新volume的内容可以由容器预先填充

* Docker Desktop上的卷比Mac和Windows主机上的绑定挂载具有更高的性能。

  此外, 与将数据持久化到容器的可写层相比, volume通常是更好的选择, 因为volume不会增加使用它的容器的大小, 而且volume的内容存在于给定容器的生命周期之外. 也就是说, 当容器被移除了之后, volume中的内容还是可以独立存在的.

tmpfs:

若容器产生的数据不需要持久化, 可以考虑使用 tmpfs 来避免持久化数据和写入容器层(writable layer), 从而提高性能 
tmpfs是将文件写到内存中, 可以避免写数据到容器层增加容器大小.

但tmpfs有两个缺点: 

1) 只能在 Linux Host 上使用
2) 不能在多个容器间共享

![img](https://img-blog.csdnimg.cn/img_convert/e01fb3864d4801eeaa80bb35c497be9d.png)



#### WORKDIR

为 每一个 RUN, CMD, ENTRYPOINT, COPY 和 ADD 指令及其后续指令设置工作目录. 如果 WORKDIR 不存在, 即使它未在任何后续的 Dockerfile 指令中使用, 也会创建它. 指定工作目录(或者称为当前目录), 以后各层的当前目录就被改为指定的目录, 如该目录不存在, WORKDIR 会帮你建立目录.

WORKDIR 指令可以在 Dockerfile 中多次使用. 如果提供了相对路径, 则该路径将相对于上一个 WORKDIR 指令的路径. 

```dockerfile
WORKDIR /a
WORKDIR b
WORKDIR cs
RUN pwd

# /a/b/c
```

WORKDIR 指令可以解析(变量替换)以前使用 ENV 设置的环境变量. 您只能使用 Dockerfile 中显式设置的环境变量.

```dockerfile
ENV DIRPATH=/path
WORKDIR $DIRPATH/$DIRNAME
RUN pwd

# /path/$DIRNAME
```

如果未指定, 则默认工作目录为 /. 实际上, 如果您不是从头开始(从头开始 FROM scratch)构建 Dockerfile, 则 WORKDIR 可能由您使用的基本 image 设置. 因此, 为避免在未知目录中执行意外操作, 最佳做法是显式设置 WORKDIR.



#### # syntax=docker/dockerfile:1, FROM alpine, FROM scratch



#### ENTRYPOINT, CMD 区别

ENTRYPOINT, 切入点, 入口点. 有两种形式: 1)是数组形式(双引号)(被推荐使用), 2)是shell形式.

ENTRYPOINT 相当于 CMD, 都是配置容器后的一个指令, 但是他不会被 docker run 提供的参数覆盖, 每个 Dockerfile 只能有一个ENTRYPOINT, 如果指定了多个, 只有最后一个被执行, 而且一定会被执行.

在 ENTRYPOINT 的数组形式下, 在创建容器实例执行 docker run 命令时, 设置的任何命令参数或 CMD 指令的命令, 都将作为 ENTRYPOINT 指令的命令参数, 追加到 ENTRYPOINT 指令的命令之后. 并且 ENTRYPOINT 是容器启动后执行的第一条完整命令, 系统进程ID为1, 即 PID=1. 也就是 docker run 后面的参数都会追加在 ENTRYPOINT 数组形式元素指令的后面. 并且覆盖所有 CMD 的命令.

docker run --entrypoint 覆盖 Dockerfile 文件里的 ENTRYPOINT 指令.

docker run \<image\> 命令的命令行参数将附加到 数组形式 ENTRYPOINT 的数组中的所有元素之后, 并将覆盖使用 CMD 指定的所有元素. 这允许将参数传递到入口点, 即 docker run -d  会将 -d 参数传递给入口点. 您可以使用 docker run --entrypoint 标志覆盖 ENTRYPOINT 指令.

 shell 形式阻止使用任何 CMD 或 run 命令行参数(即, shell 形式不能再往 ENTRYPOINT 指令的后面追加任何参数. shell 形式是什么样就是什么样了), 但缺点是 ENTRYPOINT 将作为 /bin/sh -c 的子命令启动(也就是在容器系统启动之后的shell里执行的进程), 它不传递信号(这种在容器系统环境里的进程不能将信号传递到容器外部, 所以才有后面说的 docker stop 问题). 这意味着可执行文件不会是容器的PID 1 - 并且不会接收Unix信号 - 因此您的可执行文件不会从 docker stop 接收 SIGTERM. 

只有 Dockerfile 中的最后一个 ENTRYPOINT 指令才会起作用。



```shell
1) 数组形式用例:
您可以使用 ENTRYPOINT 的 数组形式来设置相当稳定的 默认命令 和 参数, 然后使用任一形式的 CMD 来设置更有可能更改的其他默认值.

以下是 Dockerfile:
FROM ubuntu
ENTRYPOINT ["top", "-b"]
CMD ["-c"]

运行容器时, 可以看到 top 是唯一的进程:
docker run -it --rm --name test  top -H

top - 08:25:00 up  7:27,  0 users,  load average: 0.00, 0.01, 0.05
Threads:   1 total,   1 running,   0 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.1 us,  0.1 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem:   2056668 total,  1616832 used,   439836 free,    99352 buffers
KiB Swap:  1441840 total,        0 used,  1441840 free.  1324440 cached Mem

  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
    1 root      20   0   19744   2336   2080 R  0.0  0.1   0:00.04 top

要进一步检查结果, 您可以使用 docker exec:
docker exec -it test ps aux

USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  2.6  0.1  19752  2352 ?        Ss+  08:24   0:00 top -b -H
root         7  0.0  0.1  15572  2164 ?        R+   08:25   0:00 ps aux

您可以使用 docker stop 测试优雅地请求 top 关闭. 

以下 Dockerfile 显示了使用 ENTRYPOINT 在前台运行 Apache (即作为 PID 1):
FROM debian:stable
RUN apt-get update && apt-get install -y --force-yes apache2
EXPOSE 80 443
VOLUME ["/var/www", "/var/log/apache2", "/etc/apache2"]
ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]

如果需要为单个可执行文件编写 starter 脚本, 则可以使用 exec 和 gosu 命令确保最终可执行文件接收 Unix 信号:
#!/usr/bin/env bash
set -e

if [ "$1" = 'postgres' ]; then
    chown -R postgres "$PGDATA"

    if [ -z "$(ls -A "$PGDATA")" ]; then
        gosu postgres initdb
    fi

    exec gosu postgres "$@"
fi

exec "$@"

最后, 如果您需要在关闭时执行一些额外的清理(或与其他容器通信), 或者要协调多个可执行文件, 则可能需要确保 ENTRYPOINT 脚本接收 Unix 信号, 传递它们, 然后执行更多工作:
#!/bin/sh
# Note: I've written this using sh so it works in the busybox container too

# USE the trap if you need to also do manual cleanup after the service is stopped,
#     or need to start multiple services in the one container
trap "echo TRAPed signal" HUP INT QUIT TERM

# start service in background here
/usr/sbin/apachectl start

echo "[hit enter key to exit] or run 'docker stop <container>'"
read

# stop service and clean up here
echo "stopping apache"
/usr/sbin/apachectl stop

echo "exited $0"

如果使用 docker run -it --rm -p 80：80 --name test Apache 运行此映像, 则可以使用 docker exec 或 docker top 检查容器的进程, 然后要求脚本停止 Apache:
docker exec -it test ps aux

USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.1  0.0   4448   692 ?        Ss+  00:42   0:00 /bin/sh /run.sh 123 cmd cmd2
root        19  0.0  0.2  71304  4440 ?        Ss   00:42   0:00 /usr/sbin/apache2 -k start
www-data    20  0.2  0.2 360468  6004 ?        Sl   00:42   0:00 /usr/sbin/apache2 -k start
www-data    21  0.2  0.2 360468  6000 ?        Sl   00:42   0:00 /usr/sbin/apache2 -k start
root        81  0.0  0.1  15572  2140 ?        R+   00:44   0:00 ps aux
docker top test

PID                 USER                COMMAND
10035               root                {run.sh} /bin/sh /run.sh 123 cmd cmd2
10054               root                /usr/sbin/apache2 -k start
10055               33                  /usr/sbin/apache2 -k start
10056               33                  /usr/sbin/apache2 -k start
/usr/bin/time docker stop test

test
real	0m 0.27s
user	0m 0.03s
sys	0m 0.03s

您可以使用 --entrypoint 覆盖 ENTRYPOINT 设置, 但这只能将二进制文件设置为 exec(不会使用 sh -c)
数组形式解析为 JSON 数组, 这意味着您必须在单词周围使用双引号 (") 而不是单引号 (').

与 shell 形式不同, 数组形式不调用容器的 shell. 这意味着不会发生正常的 shell 处理. 
例如, ENTRYPOINT [ "echo", "$HOME" ] 不会对 $HOME 进行变量替换. 
如果你想要 shell 处理, 那么要么使用 shell 形式, 要么直接执行 shell, 
例如: ENTRYPOINT [ "sh",  "-c", "echo $HOME" ]. 当使用数组并直接执行 shell 时, 就像 shell 形式一样, 是 shell 在执行环境变量扩展, 而不是 docker.
```

```shell
2) shell 形式用例:
您可以为 ENTRYPOINT 指定一个纯字符串, 它将在 /bin/sh -c 中执行. 此形式将使用 shell 处理来替换 shell 环境变量, 并将忽略任何 CMD 或 docker 运行命令行参数. 为了确保 docker stop 将正确发出任何长时间运行的 ENTRYPOINT 可执行文件的信号, 您需要记住使用 exec 启动它:

以下是 Dockerfile 文件:
FROM ubuntu
ENTRYPOINT exec top -b

运行此 image 时, 你将看到单个 PID 1 进程：
docker run -it --rm --name test top

Mem: 1704520K used, 352148K free, 0K shrd, 0K buff, 140368121167873K cached
CPU:   5% usr   0% sys   0% nic  94% idle   0% io   0% irq   0% sirq
Load average: 0.08 0.03 0.05 2/98 6
  PID  PPID USER     STAT   VSZ %VSZ %CPU COMMAND
    1     0 root     R     3164   0%   0% top -b

在 docker stop 时干净地退出:
/usr/bin/time docker stop test

test
real	0m 0.20s
user	0m 0.02s
sys	0m 0.04s

如果您忘记将 exec 添加到入口点的开头:
FROM ubuntu
ENTRYPOINT top -b
CMD -- --ignored-param1

然后, 您可以运行它(为下一步给他起个名字):
docker run -it --name test top --ignored-param2

top - 13:58:24 up 17 min,  0 users,  load average: 0.00, 0.00, 0.00
Tasks:   2 total,   1 running,   1 sleeping,   0 stopped,   0 zombie
%Cpu(s): 16.7 us, 33.3 sy,  0.0 ni, 50.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   1990.8 total,   1354.6 free,    231.4 used,    404.7 buff/cache
MiB Swap:   1024.0 total,   1024.0 free,      0.0 used.   1639.8 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
    1 root      20   0    2612    604    536 S   0.0   0.0   0:00.02 sh
    6 root      20   0    5956   3188   2768 R   0.0   0.2   0:00.00 top

您可以从 top 的输出中看到指定的入口点不是 PID 1. 如果随后运行 docker stop test, 容器将不会完全退出 - stop 命令将在超时后强制发送 SIGKILL:
docker exec -it test ps waux

USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.4  0.0   2612   604 pts/0    Ss+  13:58   0:00 /bin/sh -c top -b --ignored-param2
root         6  0.0  0.1   5956  3188 pts/0    S+   13:58   0:00 top -b
root         7  0.0  0.1   5884  2816 pts/1    Rs+  13:58   0:00 ps waux
/usr/bin/time docker stop test

test
real	0m 10.19s
user	0m 0.04s
sys	0m 0.03s
```



了解 CMD 和  ENTRYPOINT 如何交互 

CMD 和 ENTRYPOINT 指令都定义了运行容器时执行的命令. 描述他们合作的规则很少.  

1. Dockerfile 应至少指定一个 CMD 或 ENTRYPOINT 命令. 
2. 将容器用作可执行文件时, 应定义 ENTRYPOINT. 
3. CMD 应该用作定义 ENTRYPOINT 命令的默认参数或在容器中执行临时命令的一种方式.  
4. 使用替代参数(docker run image params)运行容器时, CMD 将被覆盖.



下表显示了针对不同的 ENTRYPOINT / CMD 组合执行的命令:

|                                | No ENTRYPOINT              | ENTRYPOINT exec_entry p1_entry | ENTRYPOINT [“exec_entry”, “p1_entry”]          |
| :----------------------------- | :------------------------- | :----------------------------- | :--------------------------------------------- |
| **No CMD**                     | *error, not allowed*       | /bin/sh -c exec_entry p1_entry | exec_entry p1_entry                            |
| **CMD [“exec_cmd”, “p1_cmd”]** | exec_cmd p1_cmd            | /bin/sh -c exec_entry p1_entry | exec_entry p1_entry exec_cmd p1_cmd            |
| **CMD exec_cmd p1_cmd**        | /bin/sh -c exec_cmd p1_cmd | /bin/sh -c exec_entry p1_entry | exec_entry p1_entry /bin/sh -c exec_cmd p1_cmd |

> 如果从基础 image 定义 CMD, 则设置 ENTRYPOINT 会将 CMD 重置为空值. 在这种情况下, 必须在当前 image 中定义 CMD 才能具有值.



#### /bin/sh -c

因为命令解释器 (这里是/bin/sh) 有两种工作模式: 交互模式和非交互模式.

1. 交互模式

   用终端远程连接服务器, 在终端上敲入命令就可以显示对应结果. 这样与终端进行交互执行命令的方式就称为交互模式.

   ```shell
   [chenchen@grpc01 tefd]$ ls -l
   total 8
   -rwxrwxr--. 1 chenchen chenchen 214 Jan  9 15:16 app.py
   -rw-rw-r--. 1 chenchen chenchen 145 Jan 10 11:42 Dockerfile
   ```

1. 非交互模式

   非交互模式就是调用 bash 解释器, 通过 bash -c 后接命令字符串的形式来解释执行命令.

   ```shell
   [chenchen@grpc01 tefd]$ whereis bash
   bash: /usr/bin/bash /usr/share/man/man1/bash.1.gz
   [chenchen@grpc01 tefd]$ /usr/bin/bash -c 'ls -l'
   total 8
   -rwxrwxr--. 1 chenchen chenchen 214 Jan  9 15:16 app.py
   -rw-rw-r--. 1 chenchen chenchen 145 Jan 10 11:42 Dockerfile
   ```

   /usr/bin/bash -c help

   ```shell
   [chenchen@grpc01 tefd]$ /usr/bin/bash --help
   GNU bash, version 4.2.46(2)-release-(x86_64-redhat-linux-gnu)
   Usage:  /usr/bin/bash [GNU long option] [option] ...
           /usr/bin/bash [GNU long option] [option] script-file ...
   GNU long options:
           --debug
           --debugger
           --dump-po-strings
           --dump-strings
           --help
           --init-file
           --login
           --noediting
           --noprofile
           --norc
           --posix
           --protected
           --rcfile
           --rpm-requires
           --restricted
           --verbose
           --version
   Shell options:
           -irsD or -c command or -O shopt_option          (invocation only)
           -abefhkmnptuvxBCHP or -o option
   Type `/usr/bin/bash -c "help set"' for more information about shell options.
   Type `/usr/bin/bash -c help' for more information about shell builtin commands.
   [chenchen@grpc01 tefd]$
   [chenchen@grpc01 tefd]$ /usr/bin/bash -c help
   GNU bash, version 4.2.46(2)-release (x86_64-redhat-linux-gnu)
   These shell commands are defined internally.  Type `help' to see this list.
   Type `help name' to find out more about the function `name'.
   Use `info bash' to find out more about the shell in general.
   Use `man -k' or `info' to find out more about commands not in this list.
   
   A star (*) next to a name means that the command is disabled.
   
    job_spec [&]                            history [-c] [-d offset] [n] or hist>
    (( expression ))                        if COMMANDS; then COMMANDS; [ elif C>
    . filename [arguments]                  jobs [-lnprs] [jobspec ...] or jobs >
    :                                       kill [-s sigspec | -n signum | -sigs>
    [ arg... ]                              let arg [arg ...]
    [[ expression ]]                        local [option] name[=value] ...
    alias [-p] [name[=value] ... ]          logout [n]
    bg [job_spec ...]                       mapfile [-n count] [-O origin] [-s c>
    bind [-lpvsPVS] [-m keymap] [-f filen>  popd [-n] [+N | -N]
    break [n]                               printf [-v var] format [arguments]
    builtin [shell-builtin [arg ...]]       pushd [-n] [+N | -N | dir]
    caller [expr]                           pwd [-LP]
    case WORD in [PATTERN [| PATTERN]...)>  read [-ers] [-a array] [-d delim] [->
    cd [-L|[-P [-e]]] [dir]                 readarray [-n count] [-O origin] [-s>
    command [-pVv] command [arg ...]        readonly [-aAf] [name[=value] ...] o>
    compgen [-abcdefgjksuv] [-o option]  >  return [n]
    complete [-abcdefgjksuv] [-pr] [-DE] >  select NAME [in WORDS ... ;] do COMM>
    compopt [-o|+o option] [-DE] [name ..>  set [-abefhkmnptuvxBCHP] [-o option->
    continue [n]                            shift [n]
    coproc [NAME] command [redirections]    shopt [-pqsu] [-o] [optname ...]
    declare [-aAfFgilrtux] [-p] [name[=va>  source filename [arguments]
    dirs [-clpv] [+N] [-N]                  suspend [-f]
    disown [-h] [-ar] [jobspec ...]         test [expr]
    echo [-neE] [arg ...]                   time [-p] pipeline
    enable [-a] [-dnps] [-f filename] [na>  times
    eval [arg ...]                          trap [-lp] [[arg] signal_spec ...]
    exec [-cl] [-a name] [command [argume>  true
    exit [n]                                type [-afptP] name [name ...]
    export [-fn] [name[=value] ...] or ex>  typeset [-aAfFgilrtux] [-p] name[=va>
    false                                   ulimit [-SHacdefilmnpqrstuvx] [limit>
    fc [-e ename] [-lnr] [first] [last] o>  umask [-p] [-S] [mode]
    fg [job_spec]                           unalias [-a] name [name ...]
    for NAME [in WORDS ... ] ; do COMMAND>  unset [-f] [-v] [name ...]
    for (( exp1; exp2; exp3 )); do COMMAN>  until COMMANDS; do COMMANDS; done
    function name { COMMANDS ; } or name >  variables - Names and meanings of so>
    getopts optstring name [arg]            wait [id]
    hash [-lr] [-p pathname] [-dt] [name >  while COMMANDS; do COMMANDS; done
    help [-dms] [pattern ...]               { COMMANDS ; }
   [chenchen@grpc01 tefd]$ 
   ```

/bin/sh -c 其他作用

```shell
$ sudo chown root.root test.csv
$ echo "信息" >> test.csv						# 提示 permission denied
$ sudo echo "信息" >> test.csv			# 依然提示 permission denied
# 这是因为 >> 重定向符号也是 bash 命令, sudo 只是让 echo 具有了 root 权限, 并没有让 >> 重定向符号命令具有 root 权限, 所以提示没有权限.

# 解决1: 
$ sudo /bin/sh -c 'echo "hahah" >> test.asc'
将 'echo "hahah" >> test.asc' 作为一个字符串整体, 直接 sudo /bin/sh -c 解决.

# 解决2:
$ echo "hahah" | sudo tee -a test.asc
使用管道, 通过管道, 将 echo 输出的结果(无需 root 权限), 直接作为输入, 仅对需要 root 的 tee sudo 即可.
```



#### \<none\>, rmi

```shell
# 本地 image 中含有 <none> tag 的 image, 要删除这个 image, 如何操作
[chenchen@grpc01 tefd]$ sudo docker images
REPOSITORY                           TAG               IMAGE ID       CREATED          SIZE
registry.nevis.sina.com.cn/tiyu/rr   v6                c957dd44cd57   38 minutes ago   123MB
kk                                   latest            6278d941ad4a   13 months ago    123MB
py                                   latest            6278d941ad4a   13 months ago    123MB
rr                                   v3                6278d941ad4a   13 months ago    123MB
rr                                   v4                6278d941ad4a   13 months ago    123MB
registry.nevis.sina.com.cn/tiyu/rr   <none>            6278d941ad4a   13 months ago    123MB
python                               3.6-slim-buster   eb32e0d643ed   13 months ago    112MB

# 
[chenchen@grpc01 tefd]$ sudo docker images --digests=true
REPOSITORY                           TAG               DIGEST                                                                    IMAGE ID       CREATED          SIZE
registry.nevis.sina.com.cn/tiyu/rr   v6                sha256:1291ffbdf8f5926649e94e857807eb9f7617c81093ffdef439c640829188dab6   c957dd44cd57   45 minutes ago   123MB
kk                                   latest            <none>                                                                    6278d941ad4a   13 months ago    123MB
py                                   latest            <none>                                                                    6278d941ad4a   13 months ago    123MB
rr                                   v3                <none>                                                                    6278d941ad4a   13 months ago    123MB
rr                                   v4                <none>                                                                    6278d941ad4a   13 months ago    123MB
registry.nevis.sina.com.cn/tiyu/rr   <none>            sha256:4b58d3d272e067c9169ae76b8b28eca4499cc2bb9984e0ae82b161f35a1f1c53   6278d941ad4a   13 months ago    123MB
python                               3.6-slim-buster   sha256:a19c4a23d697b9cbb499de7e2760d0221f4bedb2d661e9ee51db7e5be43c6179   eb32e0d643ed   13 months ago    112MB

```

#### docker login, docker logout

```shell
[chenchen@grpc01 tefd]$ sudo docker logout
Removing login credentials for https://index.docker.io/v1/
[chenchen@grpc01 tefd]$ sudo docker login registry.nevis.sina.com.cn
Authenticating with existing credentials...
Stored credentials invalid or expired
Username (chenchen): chenchen
Password: 
WARNING! Your password will be stored unencrypted in /home/chenchen/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
[chenchen@grpc01 tefd]$ 


# /home/chenchen/.docker/config.json
[chenchen@grpc01 tefd]$ sudo less /home/chenchen/.docker/config.json
{
        "auths": {
                "registry.nevis.sina.com.cn": {
                        "auth": "Y2hlbmNoZW46NTY3ODkwMjg3MjAy"
                }
        }
}
[chenchen@grpc01 tefd]$ echo 'Y2hlbmNoZW46NTY3ODkwMjg3MjAy' | base64 --decode
chenchen:567890287202
[chenchen@grpc01 tefd]$ 

```

### login 官方 hub.docker.io

```shell


# 以下是 登录 hub.docker.com
# 如果未指定镜像仓库地址, 默认为官方仓库 Docker Hub.
[chenchen@grpc01 dc8]$ sudo docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: yocczr@gmail.com
Password: 
Error response from daemon: Get "https://registry-1.docker.io/v2/": unauthorized: incorrect username or password
[chenchen@grpc01 dc8]$ sudo docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: yocc
Password: 
WARNING! Your password will be stored unencrypted in /home/chenchen/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
[chenchen@grpc01 dc8]$ 

# 登录成功后, Docker 会将 token 存储在 ~/.docker/config.json 文件中, 从而作为拉取私有镜像的凭证.
我们使用 cat 命令, 查看 token 凭证信息. 通过 $ echo 'Y2hlbmNoZW46NTY3ODkwMjg3MjAy' | base64 --decode

~/.docker/config.json
{
        "auths": {
                "https://index.docker.io/v1/": {
                        "auth": "eW9jYzp0amNlbnRlaw=="
                },
                "registry.nevis.sina.com.cn": {
                        "auth": "Y2hlbmNoZW46NTY3ODkwMjg3MjAy"
                }
        }
}
```

### docker pull centos:8

```sh
[chenchen@grpc01 dc8]$ sudo docker pull centos:8
8: Pulling from library/centos
a1d0c7532777: Pulling fs layer 
error pulling image configuration: Get "https://production.cloudflare.docker.com/registry-v2/docker/registry/v2/blobs/sha256/5d/5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6/data?verify=1676282218-p5yDzl%2Fkr3Umi66ejuygwVU4wmQ%3D": dial tcp 104.18.122.25:443: i/o timeout
# 本地物理存放位置, image 和 container, root 权限
[root@grpc01 docker]# pwd
/var/lib/docker
```

### 代理网络环境下配置Docker联网拉取镜像

```sh
问题:
系统已经配置代理, 但 docker pull 依然很慢
[chenchen@grpc01 dc8]$ sudo docker pull centos:8
8: Pulling from library/centos
a1d0c7532777: Pulling fs layer 
error pulling image configuration: Get "https://production.cloudflare.docker.com/registry-v2/docker/registry/v2/blobs/sha256/5d/5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6/data?verify=1676282218-p5yDzl%2Fkr3Umi66ejuygwVU4wmQ%3D": dial tcp 104.18.122.25:443: i/o timeout

原因:
事实与真相: 
Docker这个程序只是一个控制台程序，用于attach，真正操作docker的是运行在后台的docker daemon，也就是我们需要通过systemctl start docker来启动docker daemon。所以说即使我们设置了环境变量http_proxy，那么也只是针对前台docker console使用，而真正访问pull镜像的确是后台的daemon，因此，需要设置daemon访问proxy。

解决: 
1. 给 systemctl 服务做配置, 让所有 daemon 服务进程走代理
sudo mkdir -p /etc/systemd/system/docker.service.d/		# 创建目录
sudo vim /etc/systemd/system/docker.service.d/http-proxy.conf		# 创建并编辑文件
文件内容如下:
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:7890"
Environment="HTTPS_PROXY=http://127.0.0.1:7890"
#Environment="ALL_PROXY=socks5://127.0.0.1:7891"
#Environment="NO_PROXY=localhost,127.0.0.1,docker-registry.example.com,.corp,.docker.io,.docker.com"
⚠️ 要注释掉 所有代理和 不需要代理域名, 也就是让所有都走代理
2. 重启 systemctl 和 docker 服务
~# systemctl daemon-reload
~# systemctl restart docker
3. 验证配置是否生效
~# docker info
4. 拉镜像
[root@grpc01 dc8]# docker pull centos:8
Error response from daemon: Get "https://registry-1.docker.io/v2/": read tcp 127.0.0.1:52246->127.0.0.1:7890: read: connection reset by peer
[root@grpc01 dc8]# docker pull centos:8
8: Pulling from library/centos
a1d0c7532777: Pull complete 
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:8
docker.io/library/centos:8
[root@grpc01 dc8]# docker images
REPOSITORY                           TAG               IMAGE ID       CREATED         SIZE
registry.nevis.sina.com.cn/tiyu/rr   v6                c957dd44cd57   4 weeks ago     123MB
rr                                   v3                6278d941ad4a   14 months ago   123MB
rr                                   v4                6278d941ad4a   14 months ago   123MB
kk                                   latest            6278d941ad4a   14 months ago   123MB
pp                                   latest            6278d941ad4a   14 months ago   123MB
pp                                   v2                6278d941ad4a   14 months ago   123MB
py                                   latest            6278d941ad4a   14 months ago   123MB
python                               3.6-slim-buster   eb32e0d643ed   14 months ago   112MB
centos                               8                 5d0da3dc9764   17 months ago   231MB
5. 通过镜像创建容器, --name <容器名>, --it 交互终端, centos:8 镜像名
[root@grpc01 dc8]# docker create --name v801 -it centos:8
sudo docker create -it --name 容器名 镜像名
[chenchen@grpc01 dc8]$ sudo docker ps -a
CONTAINER ID   IMAGE                    COMMAND       CREATED         STATUS                   PORTS     NAMES
28cdf5453e1e   centos:8                 "/bin/bash"   3 minutes ago   Created                            v801
11280031f06e   python:3.6-slim-buster   "python3"     4 weeks ago     Exited (0) 4 weeks ago             t01
8a49bb362e5d   python:3.6-slim-buster   "python3"     4 weeks ago     Exited (0) 4 weeks ago             peaceful_swartz
容器状态:
created	已创建
restarting	重启中
running	运行中
removing	迁移中
paused	暂停
exited	停止
dead	死亡
6. 启动容器
sudo docker start <容器ID 或者 容器名>
此时容器状态为UP，Exited，Created
[chenchen@grpc01 dc8]$ sudo docker start v801
v801
[chenchen@grpc01 dc8]$ sudo docker ps -a
CONTAINER ID   IMAGE                    COMMAND       CREATED         STATUS                   PORTS     NAMES
28cdf5453e1e   centos:8                 "/bin/bash"   6 minutes ago   Up 52 seconds                      v801
11280031f06e   python:3.6-slim-buster   "python3"     4 weeks ago     Exited (0) 4 weeks ago             t01
8a49bb362e5d   python:3.6-slim-buster   "python3"     4 weeks ago     Exited (0) 4 weeks ago             peaceful_swartz
7. 停止容器
sudo docker stop <容器ID 或者 容器名>
[chenchen@grpc01 ~]$ sudo docker stop v801
v801
8. 进入容器
$ docker exec -it 容器ID/容器NAME /bin/bash
注：只有在容器启动的时候即UP状态才能进入容器终端
[chenchen@grpc01 ~]$ sudo docker exec -it v801 /bin/bash
9. 新建容器 -> 启动容器 -> 进入容器
$ docker run -it centos:8 /bin/bash
10. 指定端口号
注：将宿主机的7001端口隐射给容器的8001端口，"-p"是小写，如果是"-P"Docker将会随机映射一个49000到49900的端口
$ docker run -it -p 7001:8001 centos:8 /bin/bash
11. 守护状态
更多的时候，需要让 Docker 容器在后台以守护态（ Daemonized ）形式运行 此时，可以通过添加-d参数来实现
$ docker run -it -d --name v801 centos:8
12. 退出容器
退出时如果想继续运行容器：按顺序按【ctrl+p】, 【ctrl+q】
如果不想继续运行：按【ctrl+d】或输入exit
13. 宿主机与容器之间复制文件
$ docker cp 宿主机绝对路径  容器id:路径
14. 删除容器
$ docker rm 容器ID
注：加"-f"参数便可删除已启动的容器

```









### See Also

https://www.docker.com/

https://hub.docker.com/, Hub

Docker Desktop, 连接到 Hub



https://docs.docker.com/reference/ Dockerfile 文档

https://docs.docker.com/engine/reference/builder/ Dockerfile reference



https://blog.csdn.net/chenguihua5/article/details/102705106?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-2-102705106-blog-127785843.pc_relevant_recovery_v2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-2-102705106-blog-127785843.pc_relevant_recovery_v2&utm_relevant_index=4 签名, harbor



https://m.geekku.com/spec/docker/1524.html

https://www.jianshu.com/p/5b499ee4d5af?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation



https://yeasy.gitbook.io/docker_practice/ docker实践

https://yeasy.gitbook.io/docker_practice/repository/registry_auth 

https://stackoverflow.com/questions/33913020/docker-remove-none-tag-images/33913711?r=SearchResults#33913711 \<none\>

## test



docker run -rm -tl lmp bash

bash 覆盖掉 lmp 里面的 最后 命令, 也就是进入lmp系统后, 启动bash, 得到命令行

rm是退出时, 销毁容器, 

容器是镜像的实例, 默认常驻 引擎, 在引擎里能看到



dock流程

通过 dockerfile, build 出 image, 通过 image run 出 container, 

dockerfile, from, run copy cmd

iamge 包括 name 和 tag 两部分  centos:8



docker login 自建hubharbor

docker tag, 给image 起别名, hub路径/tiyu名/image名 

docker push <tagname>

小提示：

（4）推送镜像到本地仓库，首先我们使用`docker tag`命令把 tomcat 镜像"重命名"为`localhost:8000/mytomcat`

> docker tag tomcat localhost:8000/mytomcat

（5）使用`docker push`推送镜像到本地仓库

> docker push localhost:8000/mytomcat

（6）查看本地仓库镜像 ，并拉取镜像

> docker pull localhost:8000/mytomcat





sudo docker run --rm -d -p 8088:80 -p 2222:22 -v /usr/home/chenchen/htdocs/sports.weibo.com:/data1/www/htdocs/sports.weibo.com --name chenchen.env.sports.weibo.com registry.nevis.sina.com.cn/tiyu/env.sports.weibo.com



sudo docker run --rm -d -p 8088:80 -p 2222:22 -v /usr/home/chenchen/htdocs/sports.weibo.com:/data1/www/htdocs/sports.weibo.com --name chenchen.env.sports.weibo.com registry.nevis.sina.com.cn/tiyu/env.sports.weibo.com /bin/bash



sudo docker run -it --rm -d -p 8088:80 -v /usr/home/chenchen/htdocs/sports.weibo.com:/data1/www/htdocs/sports.weibo.com --name chenchen.env.sports.weibo.com registry.nevis.sina.com.cn/tiyu/env.sports.weibo.com /bin/bash



docker exec -it 8b1de32984a1 /bin/bash



[chenchen@dev3_10.211.21.18 _docker-env]$ sudo docker ps -a
CONTAINER ID   IMAGE                                                  COMMAND                  CREATED         STATUS         PORTS                   NAMES
ebc5ce0efc81   registry.nevis.sina.com.cn/tiyu/env.sports.weibo.com   "/etc/dAppCluster/st…"   6 minutes ago   Up 6 minutes   0.0.0.0:8088->80/tcp    chenchen.env.sports.weibo.com
90c415a55723   sports.weibo.com-env                                   "/etc/dAppCluster/st…"   3 days ago      Up 3 days      0.0.0.0:8080->80/tcp    wenqiang1
5746fece6316   sports.weibo.com-env                                   "/etc/dAppCluster/st…"   2 weeks ago     Up 2 weeks     0.0.0.0:21576->80/tcp   runyan1
[chenchen@dev3_10.211.21.18 _docker-env]$ sudo docker stop ebc5ce0efc81

























































































































