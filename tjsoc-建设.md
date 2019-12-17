## tjsoc()安全审计监控平台

### 1数据库结构

1. 数据表
   1. IP资产表-记录公司IP和主机资产
      1. id（自动添加无需记录）
      2. ip = models.IPAddressField(protocol='IPv4', verbose_name="IP地址") # 通信地址
      3. status = models.CharField(max_length=20， verbose_name="主机状态") #：IP启用是active，未启用是inactive
      4. ip_type：区分ip类别：VIP，跳板机，蜜罐，防火墙，办公，服务器，安全服务器
      5. hostname = models.CharField(max_length=128, null = True) #：主机名和域名：vip给一个统一的名称
      6. os：系统类型：win，linux，虚拟，其他
      7. net_area：安全区域划分（外部和内部，内部-dmz-生产-测试-开发-office结合安全开发确认）
      8. APP：应用：默认开放为全端口，或者使用防护墙指定端口
      9. owner：记录最终的使用人
      10. sec_level:IP安全等级（10：为黑名单，1：为白名单）
      11. 备注
   2. 应用表-记录使用的应用
      1. id
      2. ip
      3. hostname
      4. APP
      5. FTP：端口
      6. port，
   3. 漏洞跟进
      1. id
      2. IP\hostname
      3. port
      4. vuln_type：漏洞种类共总结出21个攻击类型（攻击点，面临风险），SQL注入,Windows漏洞,XSS,bash漏洞,不安全方法,信息泄露,其他,反序列化,命令执行,弱口令,文件上传,未授权访问,跨站请求伪造,后台暴露,DOS攻击,IP伪造,目录遍历,越权,XXE,任意文件下载
      5. vuln_levle：漏洞级别在后台计算统一使用10以内的整数
      6. net_area：安全区域划分（外部和内部，内部-dmz-生产-测试-开发-office结合安全开发确认）
      7. task_type：漏洞发现类型（外部上报，扫描发现）
      8. vuln_type：漏洞处理进度（待通知，已通知，已整改，已备案，误报，延期）
      9. vuln_desc：漏洞描述
      10. process_owner：漏洞跟进人
      11. APP：所属应用 
      12. vuln_owner：漏洞整改责任人
      13. department_1：跟进描述
      14. department_2：跟进描述
      15. vuln_org_nu:漏洞组织编码
      16. discover_time：漏洞发现时间
      17. crent_time：漏洞创建时间
      18. request_time：漏洞整改请求时间
      19. close_time：漏洞闭环时间
      20. remark：备注
   4. 
   5. 表
2. 安全等级计算
   1. 参考arcsight安全手册
3. 





SQL注入	
Windows漏洞	高危漏洞
XSS	
bash漏洞	高危漏洞
不安全方法	
信息泄露	中危漏洞
其他	
反序列化	高危漏洞
命令执行	
弱口令	高危漏洞
文件上传	
未授权访问	
跨站请求伪造	
后台暴露	
DOS攻击	
IP伪造	
目录遍历	
越权	
XXE	
任意文件下载	
高危端口	