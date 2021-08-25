# Django-学习笔记

### 将anaconda环境分享给所有用户

``` shell
# 首先进入root用户安装anaconda至/opt/anaconda
groupadd anaconda # 创建anaconda组
adduser <username> anaconda # 将需要的用户添加至anaconda组
chgrp -R anaconda /opt/anaconda # 移交目录管理权
chmod 770 -R /opt/anaconda # 设置读写权限
chmod g+s /opt/anaconda # 设置组继承
chmod g+s `find /opt/anaconda/ -type d` # 设置子目录组继承
chmod g-w /opt/anaconda/envs # 关闭共享环境的写入权限
source /opt/anaconda/bin/activate # root用户下启动anaconda环境
conda create -n share3.7 python=3.7 # 创建共享环境
```

设置之后：

- 由root用户创建的环境会保存在`/opt/anaconda/envs`中，所有anaconda组成员都可以访问。
- 用户自己创建的环境则会保存至 ~/.conda/envs

