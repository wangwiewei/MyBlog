## Centos系统更新

#### Centos系统安全更新汇总

``` shell
# 检测可用补丁
yum --security check-update
# 列出可升级补丁
yum list-security
# 查看升级包信息
yum info-security
# 升级所有安全补丁
yum -y --security upgrade

```

#### 自动安装安全补丁

``` shell
# 安装yum自动更新工具
yum -y install yum-cron
```

##### 修改yum-cron配置

``` shell
# Don't install, just check (valid: yes|no)
CHECK_ONLY=yes
# Check to see if you can reach the repos before updating (valid: yes|no)
CHECK_FIRST=yes
# Don't install, just check and download (valid: yes|no)
# Implies CHECK_ONLY=yes (gotta check first to see what to download)
DOWNLOAD_ONLY=yes
# 启动
service yum-cron start
# 设置自动更新服务开机启动
chkconfig yum-cron on
# 检查自动更新服务是否启动
chkconfig yum-cron --list
```

#### upgrade 和 update的差别

Linux升级命令有两个分别是yum upgrade和yum update,:

yum -y update

升级所有包同时也升级软件和系统内核

yum -y upgrade

只升级所有包，不升级软件和系统内核

