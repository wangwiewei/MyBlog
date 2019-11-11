# Django 学习记录

### 环境部署

vulnerable

### 环境准备

#### Anaconda 创建虚拟环境

```python
conda create -n djconda python=3.7.3
```

#### 启动Django

```python
python3 manage.py runserver 0.0.0.0:8000
```

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