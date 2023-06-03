---
title : 03-Request基础模块
date : 2021-6-20
tags: 网络爬虫
---



# Request模块



概念：python中原生的一款基于网络请求的模块。
特点：功能强大，简单便捷，效率极高。
作用：模拟浏览器发请求。
使用方式：（requests	模块的编码流程）
				  -指定url
				  -发起请求
				  -获取响应数据
				  -持久化存储

环境安装：
	pip install requests



#### 实战代码



```python
#爬取搜狗首页的页面数据
import requests
if __name__ == "__main__":
    # step1:指定url
    url = 'https://www.sogou.com/'
    # step2:发起请求，get方法会返回一个响应对象
    response = requests.get(url=url)
    # step3:获取响应数据，text返回字符串形式的响应数据
    page_text = response.text
    print(page_text)
    # step 4:持久化存储
    with open('./sogou.html', 'w', encoding='utf-8') as fp:
        fp.write(page_text)
    print('爬取数据结束！！！')
```



```python
#爬取搜狗指定词条对应的搜索结果页面（简易网页采集器）
import requests
if __name__ == "__main__":
    headers={
       'User-Agent':'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.25 Safari/537.36 Core/1.70.3861.400 QQBrowser/10.7.4313.400'
    }
    url='https://www.sogou.com/web'
    #处理url携带的参数：封装到字典中
    kw=input('enter a word:')
    param={
        'query':kw
    }
    response = requests.get(url=url,params=param,headers=headers);
    page_text = response.text
    filename = kw+'.html'
    with open(filename,'w',encoding='utf-8') as fp:
        fp.write(page_text)
    print(filename,'保存成功！！！')
```



```python
#爬取豆瓣电影分类排行榜
import requests
import json
if __name__ == "__main__":
    url='https://movie.douban.com/j/chart/top_list'
    param={
        'type':'24',
        'interval_id':'100:90',
        'action':'',
        'start':'1',#从库中的第几部电影去取
        'limit':'20',#一次取出的个数
    }
    headers={
        'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.146 Safari/537.36'
    }
    response=requests.get(url=url,params=param,headers=headers)
    list_data=response.json()
    fp=open('./douban.json','w',encoding='utf-8')
    json.dump(list_data,fp=fp,ensure_ascii=False)
    print('over!!!')
```



```python
#需求：爬取肯德基餐厅查询
import requests
if __name__=="__main__":    url='http://www.kfc.com.cn/kfccda/ashx/GetStoreList.ashx?op=keyword'
    params={
        'cname':'',
        'pid':'',
        'keyword':'',
        'pageIndex':'1',
        'pageSize': '10'
    }
    headers={
        'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.146 Safari/537.36'
    }
    response=requests.get(url=url,params=params,headers=headers)
    text_list=response.text
    fp=open('./kfc.html','w',encoding='utf-8')
    fp.write(text_list)
    print(text_list)
    print('over!!!')
```



##### 动态加载数据

-首页对应的企业信息数据是通过ajax动态请求到的
-通过对详细页url的观察发现：
①url的域名都是一样的，只有携带的参数（id）不一样
②id值可以从首页对应的ajax请求到的json串中获取
③域名的id拼接处一个完整的企业对应的详细页的Url

-详细页的企业详细数据也是动态加载出来的
观察后发现
所有的post请求的Url都是一样的，只有参数的id是不同的
如果我们可以批量获取多家企业的id后，就可以将id和url形成一个完整的详细页对应信息

```python
#需求：爬取国家药品监督管理总局中基于中华人民共和国化妆品生产许可证相关数据
import requests
import json
if __name__=='__main__':
    #批量获取不同企业的Id值
    url='http://scxk.nmpa.gov.cn:81/xk/itownet/portalAction.do?method=getXkzsList'
    headers={
        'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.146 Safari/537.36'
    }
    for page in range(1,6):
        page=str(page)
        data={
            'on': 'true',
            'page': page,
            'pageSize': '15',
            'productName':'',
            'conditionType': '1',
            'applyname':'',
        }
        id_list=[] #存储企业的id
        all_data_list=[]#存储所有的企业详细信息
        json_ids=requests.post(url=url,headers=headers,data=data).json()
        for dic in json_ids['list']:
            id_list.append(dic['ID'])
    #获取企业详情数据
    post_url = 'http://scxk.nmpa.gov.cn:81/xk/itownet/portalAction.do?method=getXkzsById'
    for id in id_list:
        data={
            'id':id
        }
        detail_json=requests.post(url=post_url,headers=headers,data=data).json()
        print(detail_json)
        all_data_list.append(detail_json)
    fp=open('./allData.json','a',encoding='utf-8')
    json.dump(all_data_list,fp=fp,ensure_ascii=False)
    print('end!!')
```

