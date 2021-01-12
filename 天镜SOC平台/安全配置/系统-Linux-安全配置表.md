# Linux 安全配置检查表

| 账号口令 |          |      |      |      |      |      |      |
| -------- | -------- | ---- | ---- | ---- | ---- | ---- | ---- |
|          | 特殊账号 |      |      |      |      |      |      |



```mermaid
graph LR
F[账号权限-安全脑图]
user(账号权限) -->userB1(特权账号)
user(账号权限) -->userB2(密码策略)
userB1 --> userC1(特权账号)
userB1 --> userC2(普通账号)
userC1 --> userD1(su权限分配)
userD1 --> userE1(vi /etc/pam.d/su auth required pam_wheel.so group=test)
userC1 --> userD2(账号登录)
userD2 --> userE2(登录超时)
userE2 --> userG1(vi /etc/profile  TMOUT=180)
userC2 --> userD3(账号清理)
userC2 --> userD2(账号登录)
userC2 --> userD4(账号锁定)
userB2 --> userC3(密码时效)
userB2 --> userC4(密码长度)
userB2 --> userC5(密码复杂度)
```



``` mermaid
graph LR
F[服务进程-安全脑图]
prs(服务进程) --> prsB1(服务管理)
prs(服务进程) --> prsB2(SSH服务)
prsB1 -->prsC1(关闭无用服务)
prsB2 -->prsC2(密码错误次数)
prsB2 -->prsC3(禁止root用户登录)
prsC1 -->prsD1(systemctl disable <服务名>)
F1[文件系统--安全脑图]
fls(文件系统) --> flsB1(umask)
flsB1 --> flsC1(vi /etc/profile)
f2[系统日志--安全脑图]
logA --> logB1(系统日志)
logA --> logB2(cron日志)
logA --> logB3(安全日志)
logB1 --> logC1(/var/log/messages)
logB2 --> logC2(/var/log/cron)
logB3 --> logC3(/var/log/secure)
logC3 --> logD1(vim /etc/profile)
```

