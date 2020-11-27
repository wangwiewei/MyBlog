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

   

8. 的

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
```

