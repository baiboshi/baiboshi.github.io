---
layout: post
title: 'pyhon 爬取网页上得mp3和文本信息'
tags: [daima]
---
```python
##python爬取网页上得mp3文件和文本信息。
#!/usr/bin/env python
# -*- coding: utf-8 -*-
import urllib.request
import re
from bs4 import BeautifulSoup
from urllib.request import urlopen
#import webbrowser
import wget
BASE_URL = 'http://portuguese.cri.cn/audioonline/index.html' #输入自己爬取网页的url地址
BOOK_LINK_PATTERN = 'href="(.*.html)">'    #匹配网页中所有得html连接。
req = urllib.request.Request(BASE_URL)   #urllib.request模块的Requests()是Python中的HTTP客户端库。
html = urllib.request.urlopen(req)   #用urllib.request模块的urlopen（）获取页面。
doc = html.read().decode('utf8')  #把所有html转换为utf-8格式。
url_list = list(set(re.findall(BOOK_LINK_PATTERN, doc))) #set() 函数创建一个无序不重复元素集,re.findall搜索匹配并返回正确的字符串。用列表显示出来。
for ailab in url_list:      #循环网页中的html 连接。
    #print(ailab)
    ailab1 = urllib.request.Request(ailab)
    htmll = urllib.request.urlopen(ailab1,timeout=1).read()   #用urllib.request模块的urlopen（）获取页面，read()方法用于从文件读取指定的字节数。
    soup = BeautifulSoup(htmll, features='lxml')    #把html里的html传递给BeautifulSoup，用于处理Python语言中的XML和HTML。
    cese = (soup.h1)    #匹配出网页中（h1）的标签
    #print(cese)
    for tes in soup.find_all("audio"):          #过滤网页中audio开头得标签
        accpe=tes['_url']        #匹配（_url）字段得音频文件
        if str(accpe).endswith('mp3'):   #判断有MP3文件文件为True
            print(ailab)    #打印有mp3文件得url
            print(accpe)    #打印MP3音频文件的连接
            wget.download(accpe)   #下载所有的MP3文件
            ded=accpe.split('/')   #把每个http连接以/分割
            #print(ded)
            #print(ded[-1])
            bced=ded[-1]   #匹配/分割的最后一串数
            tces = bced.split('.')  #把字符串以点分割
            tcesd = tces[0]   #匹配点前面的数
            html2 = urlopen(ailab).read()  #urlopen只是代表了一种访问控制协议 如：FTP,FILE,HTTP和HTTP 等等，read() 方法用于从文件读取指定的字节数。
            soupa = BeautifulSoup(html2, features='lxml')   #把html2里的html传递给BeautifulSoup，用于处理Python语言中的XML和HTML。
            cesed = (soupa.h1)    #配置html中的(hl)的标签。
            print(cesed)   #打印匹配的html（h1)标签。
            for tesd in soupa.find_all(re.compile("^p")):  #筛选网页中所有的p标签。
                if tesd.string:    #if判断（p）标签里有文本的为True。
                    with open(tcesd + ".txt", 'a', encoding='utf-8') as f:    #打开一个文件，以MP3文件名来命名。
                        f.writelines(tesd.string  + '\n')    #把匹配出的文本写到文件里，换行写入。
                    f.close()     #把打开的文件关闭。
```

