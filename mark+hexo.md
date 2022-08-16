## 安装node

安装node方法1

``` shell
cd ~/soft
wget https://nodejs.org/dist/v16.16.0/node-v16.16.0-linux-x64.tar.xz
xz -d node-v16.16.0-linux-x64.tar.xz
tar -xf node-v16.16.0-linux-x64.tar
mv node-v16.16.0-linux-x64 /usr/share/node
cd /usr/share/node
echo "export NODE_HOME=/opt/node-v12.16.1-linux-x64" >> /etc/profile
echo "export PATH=\$NODE_HOME/bin:$PATH" >> /etc/profile
source /etc/profile
```

yum方式安装

```shell
curl -sL https://rpm.nodesource.com/setup_12.x | bash -
yum install nodejs -y
```

[NPM 相关命令，报错 node-gyp... 的解决方法_班纳的博客-CSDN博客_node-gyp nvm](https://blog.csdn.net/sinat_36112136/article/details/125216695)

