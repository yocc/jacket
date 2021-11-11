# Docker

### 先决条件

#### OS 必须条件

. CentOS 7 或者 CentOS 8

. CentOS 的 centos-extras 仓库必须开启. https://wiki.centos.org/AdditionalResources/Repositories

. `overlay2` 存储驱动

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

在新主机上首次安装 Docker Engine 之前，您需要设置 Docker 仓库。之后，您可以从仓库安装和更新 Docker。

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

   ```
   $ yum list docker-ce --showduplicates | sort -r
   
   docker-ce.x86_64  3:18.09.1-3.el7                     docker-ce-stable
   docker-ce.x86_64  3:18.09.0-3.el7                     docker-ce-stable
   docker-ce.x86_64  18.06.1.ce-3.el7                    docker-ce-stable
   docker-ce.x86_64  18.06.0.ce-3.el7                    docker-ce-stable
   ```

   The list returned depends on which repositories are enabled, and is specific to your version of CentOS (indicated by the `.el7` suffix in this example).

   b. Install a specific version by its fully qualified package name, which is the package name (`docker-ce`) plus the version string (2nd column) starting at the first colon (`:`), up to the first hyphen, separated by a hyphen (`-`). For example, `docker-ce-18.09.1`.

   ```
   $ sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
   ```

   This command installs Docker, but it doesn’t start Docker. It also creates a `docker` group, however, it doesn’t add any users to the group by default.

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

## Uninstall Docker Engine

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































































































































































