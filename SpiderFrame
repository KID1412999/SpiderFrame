#基本爬虫框架
import requests
import threading
from lxml import etree
import re
from bs4 import BeautifulSoup
import random

class Spider:
    id=0
    def get_ip_list(headers):
        url= 'http://www.xicidaili.com/nn/'#代理IP获取网址
        web_data = requests.get(url, headers=headers)
        soup = BeautifulSoup(web_data.text, 'lxml')
        ips = soup.find_all('tr')
        proxy_list = []
        for i in range(1, len(ips)):
            ip_info = ips[i]
            tds = ip_info.find_all('td')
            proxy_list.append({'http': 'http://' + tds[1].text + ':' + tds[2].text})
        return proxy_list
    def __init__(self,name):
        self.name=name
        Spider.id+=1
        self.show_information()
        self.head={"User-Agent":"Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.140 Safari/537.36"}
        self.head.update({'cookie':'_zap=7189e847-70da-45b6-9bb9-4faa9fa4eb10; _xsrf=2WSo461hFc9R8IKQ5eo7p535MN2DqDoR; d_c0="AEDoJ5Fwew6PTvoGYUQXas9-0PVF4rDD8HU=|1541582067"; __gads=ID=7077444a52f6f0d1:T=1541599985:S=ALNI_MYv-vGtpO2j78BLvnHWk71AHAv8zw; z_c0="2|1:0|10:1542801953|4:z_c0|92:Mi4xVHBTRUF3QUFBQUFBUU9nbmtYQjdEaVlBQUFCZ0FsVk5JWmppWEFCYXRVeFhtLXNsR0s4aEM5dDlXLVBLcktkWjh3|f10e39c65ae553c4dbfeeb034f7a78ffb3f42d3c2b587798dad4853160ca02fd"; q_c1=fa0e0d8c87744ae5856cf5ecc989adbe|1544261715000|1541582073000; tst=r; __utmz=155987696.1545744610.1.1.utmcsr=(direct)|utmccn=(direct)|utmcmd=(none); __utma=155987696.1977676472.1545744610.1545785239.1545792128.3; tgw_l7_route=56f3b730f2eb8b75242a8095a22206f8'})
        self.urls=[]
        self.usedurl=[]
        self.response=[]
        self.data=[]
        self.ip=Spider.get_ip_list(self.head)#代理ip
    def show_information(self):
        print("我是 %s 爬虫\n编号%s"%(self.name,self.id))
    def requests(self,mathod,url,data=None,params=None,encode='utf-8'):
        for i in url:
            if i not in self.usedurl:
                if mathod=='get':
                    html=requests.get(i,headers=self.head,params=params,proxies=random.choice(self.ip),verify=True)
                    html.encoding=encode
                    if html.status_code==200:
                        self.response.append(html)
                else:
                    html=requests.post(i,headers=self.head,data=data)
                    html.encoding=encode
                    if html.status_code==200:
                        self.response.append(html)
                self.usedurl.append(i)
    def multithreads_requests(self,mathod,urls,k,data=None,params=None,encode='utf-8'):#多线程发送请求
        threads = []#存放线程的数组，相当于线程池
        p=0
        k=k#间隔
        while p+k<=len(urls):
            thread =threading.Thread(target=self.requests,args=(mathod,urls[p:p+k],data,params,encode))#指定线程i的执行函数为myThread
            p+=k
            threads.append(thread)#先讲这个线程放到线程threads
        if p!=len(urls):
            thread =threading.Thread(target=self.requests,args=(mathod,urls[p:len(urls)],data,params,encode))
            threads.append(thread)
        for t in threads:#让线程池中的所有数组开始
            t.start()
        for t in threads:#让线程池中的所有数组开始 
            t.join()
    def search(self,mode='/html',mathod='xpath'):#匹配信息
        if mathod=='xpath':
            for i in self.response:
                txt=etree.HTML(i.text)
                self.data.append(txt.xpath(mode))
