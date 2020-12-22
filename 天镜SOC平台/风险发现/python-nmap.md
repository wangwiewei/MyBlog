# python-nmap 使用

### 基本用法

``` python
 nm = nmap.PortScanner()    #创建端口扫描对象
 nm.scan(hosts=hosts, arguments=' -v -sS -p '+port) 
except nmap.PortScannerError:
    print('Nmap not found', sys.exc_info()[0])
    sys.exit(0)
except:
    print("Unexpected error:", sys.exc_info()[0])
    sys.exit(0)
    
```

