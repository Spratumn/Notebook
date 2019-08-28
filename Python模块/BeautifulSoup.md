## 1. BeautifulSoup对象的类型
Beautiful Soup将复杂HTML文档转换成一个复杂的树形结构，每个节点都是Python对象。所有对象可以归纳为4种类型: Tag , NavigableString , BeautifulSoup , Comment 。
### 1.1 Tag
find()方法返回的类型就是Tag

find-all()返回的是多个该对象的集合，可以用for循环遍历

返回标签之后，还可以提取标签中的信息。

提取标签的名字：tag.name

提取标签的属性：tag['attribute']

```python3
from bs4 import BeautifulSoup

html_doc = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
"""
soup = BeautifulSoup(html_doc, 'lxml')  #声明BeautifulSoup对象
find = soup.find('p')  #使用find方法查到第一个p标签
print("find's return type is ", type(find))  #输出返回值类型
print("find's content is", find)  #输出find获取的值
print("find's Tag Name is ", find.name)  #输出标签的名字
print("find's Attribute(class) is ", find['class'])  #输出标签的class属性值
```
### 1.2 NavigableString
NavigableString就是标签中的文本内容（不包含标签）。

提取标签的文本：tag.string
```python3
print('NavigableString is：', find.string)
```
### 1.3 BeautifulSoup
BeautifulSoup对象表示一个文档的全部内容。支持遍历文档树和搜索文档树。
### 1.4 Comment
这个对象其实就是HTML和XML中的注释。

XML的注释格式和HTML一样,都是以 \<!--注释内容--> 作为注释方式。

XML中有一些特殊的规定，如： 
1. 在注释文本中不能出现字符 "- "或字符串 "-- "
2. 不要把注释文本放在标记之中，类似地，不要把注释文本放在实体声明之中或之前。
3. 注释不能被嵌套。


```python3
markup = "<b><!--Hey, buddy. Want to buy a used parser?--></b>"
soup = BeautifulSoup(markup)
comment = soup.b.string
type(comment)
# <class 'bs4.element.Comment'>
```
## 2. BeautifulSoup遍历方法
### 2.1 节点和标签名
使用子节点、父节点、 及标签名的方式遍历：

```python3
>>> from bs4 import BeautifulSoup
>>> html_doc = """
<html><head><title>The Dormouse's story</title></head>
    <body>
<p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
"""
>>> soup = BeautifulSoup(html_doc, 'html.parser')
>>> soup.head
<head><title>The Dormouse's story</title></head>
>>> soup.title
<title>The Dormouse's story</title>
>>> soup.body.b
<b>The Dormouse's story</b>
>>> soup.a
<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>
>>> soup.find_all('a')
[<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
>>> soup.body.b.parent
<p class="title"><b>The Dormouse's story</b></p>
```
实例：

```python3
import requests
from bs4 import BeautifulSoup
import os
#获取html
f = requests.get('http://tieba.baidu.com/p/2166231880').text
#用BS解析html
s = BeautifulSoup(f,'lxml')
s_imgs = s.find_all('img',pic_type='0')
#逐个将图片保存到本地
i = 0
os.makedirs(os.getcwd()+'\\'+'picture')
for s_img in s_imgs:
    img_url = s_img['src']
    img_content = requests.get(img_url).content
    file_name = str(i) + '.jpg'
    file = open(os.getcwd()+'\\'+'picture'+'\\'+file_name, 'wb')
    file.write(img_content)
    file.close()
    i = i + 1

```
实例2：

```python3
import requests
from bs4 import BeautifulSoup
import os


textget = requests.get('http://news.baidu.com/').text
# 用BS解析html
soup = BeautifulSoup(textget, 'lxml')
news = soup.find('div', attrs={'class': 'mod-tab-content'})
news = '<html>'+ str(news.contents[1])
# print(news)

soup1 = BeautifulSoup(news, 'lxml')
news1 = soup1.find_all('a','target'=='_blank')
for new in news1:
    string = new.string
    print(string)
    file = open(os.getcwd()+'\\' + 'news.txt', 'a')
    file.write(string+'\n')
    file.close()
```

```python3
# coding=utf-8
# -*- coding=utf-8 -*-
import requests
from bs4 import BeautifulSoup
import os
import re

weatherget = requests.get('http://www.weather.com.cn/weather/101270101.shtml')
# 用BS解析html
if weatherget.encoding == 'ISO-8859-1':
    encodings = requests.utils.get_encodings_from_content(weatherget.text)
    if encodings:
        encoding = encodings[0]
    else:
        encoding = weatherget.apparent_encoding
encode_content = weatherget.content.decode(encoding, 'replace').encode('utf-8', 'replace')
soup = BeautifulSoup(encode_content, 'lxml')
# weathers = soup.find_all('li', {'class':{'sky skyid lv3','sky skyid lv3 on'}} )
weathers = soup.find_all('li',attrs={"class":re.compile(r'^sky skyid lv3')} )
# print(len(wethers))
file = open(os.getcwd() + '\\' + 'weather.txt', 'a')
for weather in weathers:
    date = weather.h1.string
    weatherstring = weather.p.string
    temptag = weather.select('p[class="tem"]')
    tem = temptag[0].i.string
    file.write(date + '  ')
    file.write(weatherstring + '  ')
    file.write(tem + '\n')
file.close()

```
