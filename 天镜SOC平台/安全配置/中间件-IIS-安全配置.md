# IIS 安全配置

### 账号权限设置



```
修改IIS的MIME映射增加支持flv格式视频播放支持.flvvideo/x-flv增加支持7z压缩格式下载支持.7zapplication/x-7z-compress删除access数据库mdb格式，不让下载.mdb文件，安全考虑。.mdbapplication/x-msaccess
```

![iis11.jpg](https://img.jbzj.com/file_images/article/201101/2011011400443434.jpg)
![iis12.jpg](https://img.jbzj.com/file_images/article/201101/2011011400443435.jpg)

```
Web站点权限设定（建议） 脚本源访问 不允许 读取 允许 写入 不允许 目录浏览 建议关闭 记录访问 建议关闭　网站开启日志记录时使用 索引资源 建议关闭 执行权限 纯脚本
```

![iis2.jpg](https://img.jbzj.com/file_images/article/201101/2011011400443436.jpg)
![iis3.jpg](https://img.jbzj.com/file_images/article/201101/2011011400443437.jpg)

```
在IIS管理器中删除必须之外的任何没有用到的映射（保留asa、asp、php、等必要映射即可） 
```

![iis4.jpg](https://img.jbzj.com/file_images/article/201101/2011011400443438.jpg)
启用父级路径
![iis5.jpg](https://img.jbzj.com/file_images/article/201101/2011011400443439.jpg)
HTTP404 Not Found出错页面定制
![iis8.jpg](https://img.jbzj.com/file_images/article/201101/2011011400443440.jpg)

```
如果选择向客户端发送下列文本错误消息，就屏蔽了asp程序出错信息。
```

![iis6.jpg](https://img.jbzj.com/file_images/article/201101/2011011400443441.jpg)

```
启用默认文档 
 建议不使用索引，占磁盘空间和系统资源。 建议不使用日志，除非你不兴趣每天审查日志。 如果启用，最好不要使用缺省的目录，建议更换一个记日志的路径， 同时设置目录访问权限加上网络服务或者IIS工作组修改权限。 
```

![iis10.jpg](https://img.jbzj.com/file_images/article/201101/2011011400443443.jpg)
在扩展中启用ASP支持
![iis9.jpg](https://img.jbzj.com/file_images/article/201101/2011011400443444.jpg)
编辑权限设置批处理

### 删除实例

文件安全配置要求：删除可能带来风险的实例文件。
操作：进入相应目录，删除实例文件
IIS   c:\inetpub\iissamples
Admin Scripts     c:\inetpub\scripts
Admin Samples %systemroot%\system32\inetsrv\adminsamples
IISADMPWD %systemroot%\system32\inetsrv\iisadmpwd
IISADMIN       %systemroot%\system32\inetsrv\iisadmin
Data access   c:\Program Files\Common Files\System\msadc\Samples
MSADC c:\program files\common files\system\msadc
检测：进入c:\inetpub；c:\Program Files\Common Files\System\msadc\Samples 查看是否删除可能带来风险的实例文件。
判定：删除可能带来风险的实例文件。



### apk文件下载

IIS服务器不能下载.apk文件的解决步骤：

打开IIS服务管理器，找到服务器，右键-属性，打开IIS服务属性；
单击MIME类型下的“MIME类型”按钮，打开MIME类型设置窗口；
单击“新建”，建立新的MIME类型；
扩展名中填写“.apk”,
MIME类型中填写apk的MIME类型“ application/vnd.android.package-archive ”

![img](https://www.jb51.net/upload/201204/20120421162953888.gif)

或者

在"MIME 类型"框中，键入application/octet-stream

![img](https://img.jbzj.com/file_images/article/202001/20200117165946.png)

单击“确定”保存设置。
重启IIS，使设置生效。
现在使用IIS服务器的网站就可以下载.apk文件了。

**.ipa无法下载**

解决步骤：
1）、打开IIS服务管理器，找到服务器，右键-属性，打开IIS服务属性；
2、单击MIME类型下的“MIME类型”按钮，打开MIME类型设置窗口；
3）、单击“新建”，建立新的MIME类型；
扩展名是：.apk MIMI类型是：application/vnd.android.package-archive
扩展名是：.ipa MIMI类型是：application/iphone
4）、单击“确定”保存设置。重启IIS，使设置生效。
如此操作之后，使用IIS服务器的网站便可以下载.apk文件、.ipa文件了。

**【IIS6】**

1）打开IIS服务管理器，找到服务器，右键-属性，打开IIS服务属性；
2）单击MIME类型下的“MIME类型”按钮，打开MIME类型设置窗口；
3）单击“新建”，建立新的MIME类型；
4）扩展名中填写".apk",MIME类型中填写"application/vnd.android.package-archive"
5）单击“确定”保存设置。
6）重启IIS，使设置生效。

**【IIS7、IIS7.5】**

1）打开IIS服务管理器，左边点到计算机（也可设置特定网站）
2）右边功能项中找到MIME类型，双击打开。
3）右键“添加”
4）扩展名中填写".apk",MIME类型中填写"application/vnd.android.package-archive"
5）确定后重启IIS生效。