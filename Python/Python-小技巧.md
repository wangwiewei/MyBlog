# Python 小技巧

### 字符格式时间转换

``` python
ver = "2021-01-10 17:06:00"
dd = int(time.mktime(time.strptime($ver,"%y-%M-%d %H%M%S")))
dd = time.strftime("%Y/%m/%d",time.localtime(dd))
```

