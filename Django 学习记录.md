# Django 学习记录

### 环境部署

Linux 安装Django

环境准备

#### Linux 环境准备

[anaconda官方安装指导-64位系统]:https://docs.anaconda.com/anaconda/install/linux/	"官方指导"

[ djnago 2.2 ]: https://docs.djangoproject.com/zh-hans/2.2/ref/models/fields/#registering-and-fetching-lookups 官方指导

*资料*

``` python
https://docs.djangoproject.com/zh-hans/2.2/ref/models/fields/#registering-and-fetching-lookups
```



``` shell
conda install -n mydjango mysql
pip install pymysql
pip install https://github.com/darklow/django-suit/tarball/v2
```

#### Anaconda 创建虚拟环境

```python
# 查看conda的环境
conda list
# 显示安装的虚拟环境列表
conda  env list
# 创建conda虚拟环境
conda create -n mydjango python=3.7.3
pip install https://github.com/darklow/django-suit/tarball/v2
# 虚拟环境安装mysql库
conda install -n mydjango mysqlclient
# 查看虚拟环境安装了那些库
conda list -n mydjango
# 激活虚拟环境
source activate mydjango
conda activate mydjango
# 激活后安装django
pip install django==2.2
```

#### 创建Django工程

``` python
django-admin startproject tjsoc
```

#### 启动Django

```python
python3 manage.py runserver 0.0.0.0:8000
```

#### 取消Django访问控制

```python
ALLOWED_HOSTS = ['*']
```

#### Django添加MySQL数据库

编辑项目目录/settings.py找到DATABASES按照如下格式修改

 ``` python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',  # 或者使用 mysql.connector.django
        'NAME': 'test',
        'USER': 'test',
        'PASSWORD': 'test123',
        'HOST':'localhost',
        'PORT':'3306',
    }
}
 ```

#### 启动 django

```python
python manage.py runserver 0.0.0.0:8000
```

#### 创建apps

```python
# 创建一个app
django-admin.py startapp tjsoc

# 添加Django admin 超级管理员账号
python manage.py createsuperuser# 添加Django admin 超级管理员账号
python manage.py createsuperuser

# 迁移模型
python manage.py makemigrations tjsoc
python manage.py migrate tjsoc

# 使用现有数据库自动生成模型文件
python manage.py inspectdb
python manage.py inspectdb > tjsoc/models.py

# 使用SQL自动生成models模型文件
python manage.py inspectdb
```

#### URL文件

```python
url(r'hello/$', view.hello),
# 变量参数?P表示参数,<参数名称>
url(r'xxxurl/(?P<year>[0-9]{4})/(?P<month>[0,1][1-9])',函数.类),
```

#### views

```python
#掉URL参数
def hanshu(request,year,month):
    return HttpResponse("这里打印年{0}月{1}".fromat(year,month))
```

#### 更新model

```python
python manage.py makemigrations
python manage.py migrate
```

*如果此次更新有新增字段并且不允许字段为空会报错。选择1指定默认值，然后再输入timezone.now 继续*

#### 查找方法

- gt:大于

- gte:大于等于

- lt: 小于

- lte:小于等于

- range:范围

- year:年份

- isnull:是否为空

  ##### 模糊查询

- 

- contains:包含

### admin

#### 1.mysql install

``` shell
# mysql5.6添加更新源
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
mysql_install_db --user=mysql  --datadir=/data/mysql &
service mysql start
mysqladmin -u root password '密码'


yum install mysql-community-server

/mysql_install_db --user=mysql  --datadir=/data/mysql &
mysqladmin -u root password 'new-password'

# 初始化，则情况数据目录（默认为/var/lib/mysql）
mysqld --initialize --user=mysql --cosole
# 查看默认密码
grep 'temporary password' /var/log/mysqld.log
cat ~/.mysql_secert
# 查看授权用户
select host, user from mysql.user;
SELECT DISTINCT CONCAT('User: ''',user,'''@''',host,''';') AS query FROM mysql.user;
# 修改数据库默认密码
alter user 'root'@'localhost' identified by 'wei=61722';
# 新建数据指定utf8编码
CREATE DATABASE `tjsoc` CHARACTER SET utf8 COLLATE utf8_general_ci;
# 添加一个远程可以登录数据库的用户,%表示任何地方
create user "tjsoc"@"%" identified by "tjsoc@2020";
# 为用户授权数据库访问和操作
grant all privileges on `tjsoc`.* to 'tjsoc'@'%' identified by 'tjsoc@2020';
grant all privileges on `tjsoc`.* to 'tjsoc'@'%';
#skip_grate_table 登录后修改root密码

# mysql 修改密码
update user set Password =password('wei=61722') where User='root';
update user set password=password('tjsoc@2020') where user='tjsoc'; 
update user set plugin="mysql_native_password"; 
# mysql => 5.7
update user set authentication_string=password("你的密码") where user='root'; 

# 刷新权限表
flush privileges;
# 查看用户权限
show grants;
show grants for tjsocdb;
```

#### 3. 安装Django-suit

1. 安装Django-suit

   ``` python
   pip install https://github.com/darklow/django-suit/tarball/v2
   ```

2. 创建suitconfig

   ``` python
   # 修改assetmgr/apps.py添加如下内容
   from suit.apps import DjangoSuitConfig
   
   class SuitConfig(DjangoSuitConfig):
       layout = 'horizontal'
   # 修改tjsoc/settings.py 找到APPS 添加下面两行
   INSTALLED_APPS = (
       ...
       'assetmgr.apps.SuitConfig',
       'django.contrib.admin',
   )
   ```

3. 创建diango 超级管理员

   ``` python
   # 添加Django admin 超级管理员账号
   python manage.py createsuperuser
   # user:tjadmin  pass: managertjsoc长度
   ```

4. 测试





[https://docs.anaconda.com/anaconda/install/linux/"官方安装指导（64位系统）"]: 
[https://docs.anaconda.com/anaconda/install/linux/]: 