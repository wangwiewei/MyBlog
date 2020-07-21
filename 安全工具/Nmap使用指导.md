# Nmap使用指导

1. 扫描大量机器

   ``` nmap
   nmap -sS -Pn -n --open --min-hostgroup 4 --min-parallelism 1024 --host-timeout 30 -T4 -v
   -sS: 使用SYN方式扫描，默认用的是-sT方式，即TCP方式，需要完成三次握手，比较费时。SYN比较快一些
   -Pn: 禁用PING检测，省去PING探测的时间
   -n: 禁止DNS反向解析，只扫描IP地址时使用
   --open: 只输出检测状态为open的端口
   --min-hostgroup4: 调整并行扫描组的大小
   --min-parallelism 1024: 调整探测报文的并行度
   --host-timeout 30: 检测超时时间
   -T4: 总共有T0-T5探测深度
   -v: 打印详细扫描过程
   -oG: 输出人性化
   -iL: 载入IP文件
   ```

   

2. 的