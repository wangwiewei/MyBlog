# Django 学习记录

### 环境部署

Linux 安装Django

##### Linux 环境准备

[anaconda官方安装指导][https://docs.anaconda.com/anaconda/install/linux/"官方安装指导"]

### 环境准备

#### Anaconda 创建虚拟环境

```python
# 查看conda的环境
conda list
# 显示安装的虚拟环境列表
conda  env list
# 创建conda虚拟环境
conda create -n djconda python=3.7.3
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

### Django添加MySQL数据库

 

#### 调试Django

```python
python manage.py shell
```

#### 使用模型迁移数据

```python
python manage.py makemigrations TestModel
python manage.py migrate TestModel
```

#### 使用现有数据库自动生成模型文件

```python
自动生成models模型文件
python manage.py inspectdb
#创建一个app
django-admin.py startapp app
#导入数据
python manage.py inspectdb > app/models.py
```

#### URL文件

```python
	url(r'hello/$', view.hello),
	#变量参数?P表示参数,<参数名称>
    url(r'xxxurl/(?P<year>[0-9]{4})/(?P<month>[0,1][1-9])',函数.类),
```

#### views

```python
#掉URL参数
def hanshu(request,year,month):
    return HttpResponse("这里打印年{0}月{1}".fromat(year,month))
```

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
yum install mysql-community-server
# 如果提示目录存在数据，则情况数据目录（默认为/var/lib/mysql）
mysqld --initialize --user=mysql --cosole
# 查看默认密码
grep 'temporary password' /var/log/mysqld.log
cat ~/.mysql_secert
# 修改数据库默认密码
alter user 'root'@'localhost' identified by 'Vi@qf4J6SD';、
# 添加一个远程可以登录数据库的用户
create user "tjsocdb"@"%" identified by "tj@soc2019";
# 为用户授权数据库访问和操作
grant all privileges on `tjsoc`.* to 'tjsocdb'@'%';
# 刷新权限表
flush privileges;
# 查看用户权限
show grants;
```



#### 2. 启用APP



#### 3. 安装Django-suit

2. 创建suitconfig

   ```python
   # my_project_app/apps.py
   from suit.apps import DjangoSuitConfig
   
   class SuitConfig(DjangoSuitConfig):
       layout = 'horizontal'
   ```

   ```python
   INSTALLED_APPS = (
       ...
       'my_project_app.apps.SuitConfig',
       'django.contrib.admin',
   )
   ```

3. 长度

```python
pip install https://github.com/darklow/django-suit/tarball/v2
```

