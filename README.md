# SpiderFrame
基本爬虫框架
## 基本功能  
1.多线程发送请求  
2.设置代理  
3.网页信息匹配（xpath,jsonpath,re,BeautifulSoup）  
4.多线程存储数据(txt,csv,excel,mysql,monogdb)  

# 实例  
a=Spider('知乎')  
package=[]  
#构造package实例  
for i in range(29999900,30000000):  
    package.append({'url':'https://www.zhihu.com/question/'+str(i),'params':None,'data':None})  
len(package)    
#调用方法  
a.multithreads_requests('get',package,10)  
