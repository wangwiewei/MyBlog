### install  Docker

``` shell
wget -qO- https://get.docker.com | sh
```

[ 官方文档 ](https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface)

### Centos 8 安装Docker

1. 修改更新源为阿里云

   ```shell
   # 下面两条命令选择一个
   curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-8.repo
   wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-8.repo
   ```

2. 安装ssh服务

   ``` shell
   # 安装ssh服务
   dnf install openssh-server
   # 设置ssh服务开机启动
   systemctl enable sshd
   ```

3. 安装Docker

   ``` shell
   # 添加Docker 更新源
   dnf config-manager --add-repo=https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
   # 查看已添加的Docker 源
   dnf list docker-ce
   Docker CE Stable - x86_64                                                             
   docker-ce.x86_64  3:19.03.13-3.el7
   # 安装Docker 依赖包
   dnf install device-mapper-persistent-data lvm2
   dnf install https://download.docker.com/linux/fedora/30/x86_64/stable/Packages/containerd.io-1.2.6-3.3.fc30.x86_64.rpm
   # 安装Docker
   dnf install -y docker-ce --nobest
   # 设置 Docker开机启动
   systemctl enable docker
   # 启动Docker
   systemctl start docker
   # 设置Docker 镜像源 -- ustc
   
   # 添加以下内容保持退出 - 需要重启Docker
   
   ```

4. 设置Docker 镜像源

   ``` shell
   vim /etc/docker/daemon.json
   ```

   添加以下内容

   ``` json
   {
     "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn/"]
   }
   ```

   重启 Docker

   ``` shell
   systemctl restart docker
   ```

5. Docker 镜像

   ``` shell
   # 查找镜像
   docker search centos
   # 拉取镜像
   docker pull centos
   ```

6. 构建Docker 镜像

   ```shell
   # 构建镜像
   docker commit
   # 
   dcoker build 构建dockerfile 文件
   ```

7. 删除镜像

   ``` shell
   docker rmi $IMAGE_ID
   ```

8. 创建运行容器

   ``` shell
   # 创建容器
   docker run 
   -i 		交互模式运行容器
   -t		容器启动后进入命令行
   --name 	容器命名
   -v		创建目录映射，前者是宿主机目录，后者是映射到宿主机的目录。可以使用多个-v
   -d		守护模式运行
   -p		端口映射，前者是宿主机端口，后者是容器内的端口
   ```

9. 登录容器的方式

   ``` shell
   # 直接启动进入容器
   docker -i -t --name=myname image /bin/bahs
   # 守护式启动容器
   docker -d -i -t --name=myname image /bin/bash
   # 附着守护式容器 - 此方式退出容器时，容器会停止
   docker attach myname
   # 此方式登录容器退出后容器不会停止
   docker exec -i -t myname /bin/bash
   ```

10. 查看正在运行的docker

    ``` shell
    docker ps
    -a		历史上启动过的dockers 容器
    -l		查看最后一次运行的容器
    ```

    

11. 停止运行的容器

    ``` shell
    docker stop container_name[ID]
    ```

12. 启动停止的容器

    ``` shell
    docker start container_name[ID]
    ```

13. 深入容器内部

    ``` shell
    docker inspect container_name[ID]
    -f --fromat 查看选定内容+
    ```

14. Docker 文件拷贝

    ``` shell
    docker cp file container_name:/路径
    ```

    

15. Docker 镜像导出

16. Docker 镜像导入

### 常用命令 [Docker-学习.md](Docker-学习.md) 

``` shell
# 万能命令
docker help
# 列出Docker 镜像
docker images
# 获取Docker镜像 - 获取centos镜像
dcoker pull centos
# 查看Docker
docker version
# 查看Docker存储位置
docker info
# 启动Docker服务
service docker start
# 查看运行的docker

```

