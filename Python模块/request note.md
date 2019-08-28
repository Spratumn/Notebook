## 0.预备知识
###  0.1什么是 HTTP？

超文本传输协议（HTTP）的设计目的是保证客户机与服务器之间的通信。

HTTP 的工作方式是客户机与服务器之间的请求-应答协议。

客户端（浏览器）向服务器提交HTTP请求；服务器向客户端返回响应。响应包含关于请求的状态信息以及可能被请求的内容。

在客户机和服务器之间进行请求-响应时，两种最常被用到的方法是：GET 和 POST。

- GET - 从指定的资源请求数据。
- POST - 向指定的资源提交要被处理的数据

### 0.2. 什么是URL？
全称是UniformResourceLocator, 中文叫统一资源定位符,是互联网上用来标识某一处资源的地址。以下面这个URL为例，介绍下普通URL的各部分组成：http://www.aspxfans.com:8080/news/index.asp?boardID=5&ID=24618&page=1#name
#### 0.2.1协议
该URL的协议部分为“http：”，这代表网页使用的是HTTP协议。

在Internet中可以使用多种协议，如HTTP，FTP等等.
#### 0.2.2 域名
该URL的域名部分为“www.aspxfans.com”。一个URL中，也可以使用IP地址作为域名使用
#### 0.2.3 端口
跟在域名后面的是端口，域名和端口之间使用“:”作为分隔符。
#### 0.2.4 虚拟目录
从域名后的第一个“/”开始到最后一个“/”为止，是虚拟目录部分。
#### 0.2.5 文件名
从域名后的最后一个“/”开始到“？”为止，是文件名部分，如果没有“?”,则是从域名后的最后一个“/”开始到“#”为止，是文件部分，如果没有“？”和“#”，那么从域名后的最后一个“/”开始到结束，都是文件名部分。本例中的文件名是“index.asp”。
#### 0.2.6锚
从“#”开始到最后，都是锚部分。本例中的锚部分是“name”。
#### 0.2.7 参数
从“？”开始到“#”为止之间的部分为参数部分，又称搜索部分、查询部分。本例中的参数部分为“boardID=5&ID=24618&page=1”。参数可以允许有多个参数，参数与参数之间用“&”作为分隔符。
### 0.3. HTTP状态码
 Response 消息中的第一行叫做状态行，由HTTP协议版本号， 状态码， 状态消息 三部分组成。
 
 状态码由三位数字组成，第一个数字定义了响应的类别
- 1XX  提示信息 - 表示请求已被成功接收，继续处理
- 2XX  成功 - 表示请求已被成功接收，理解，接受
- 3XX  重定向 - 要完成请求必须进行更进一步的处理
- 4XX  客户端错误 -  请求有语法错误或请求无法实现
- 5XX  服务器端错误 -   服务器未能实现合法的请求

常见状态码：
- 200 OK                       //客户端请求成功
- 400 Bad Request             //客户端请求有语法错误，不能被服务器所理解
- 401 Unauthorized              //请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用
- 403 Forbidden                //服务器收到请求，但是拒绝提供服务
- 404 Not Found                //请求资源不存在，eg：输入了错误的URL
- 500 Internal Server Error     //服务器发生不可预期的错误
- 503 Server Unavailable      //服务器当前不能处理客户端的请求，一段时间后可能恢复正常

### 0.4 什么是HTTP Headers?
#### 0.4.1 Content-Type
这个是文档的mime-type, 浏览器根据此参数来决定如何对文档进行解析. 例如一个html页面返回值就是:'text/html; charset=utf-8'; 

/前的'text'表示文档类型, /后面的'html'表示文档的子类型;

如果是图片,则返回: 'image/jpeg', 表示目标是一个图像(image), 具体是一个jpeg图像.

如果是pdf文档, 则返回'application/pdf'
#### 0.4.2 Cache-Control
缓存控制字段用于指定所有缓存机制在整个请求/响应中必须服从的指令. 常见的取值有: private(默认), no-cache, max-age, must-revalidate

#### 0.4.3 User-Agent
作用：告诉HTTP服务器，客户端使用的操作系统和浏览器的名称和版本.

## 1.安装 Requests
pip install requests
## 2.用法
get() 简单实例：
```python3
import requests
 
r = requests.get(url='http://www.itwhy.org')    # 最基本的GET请求
print(r.status_code)    # 获取返回状态
r = requests.get(url='http://dict.baidu.com/s', params={'wd':'python'})   #带参数的GET请求
print(r.url)
print(r.text)   #打印解码后的返回数据
```
post() 简单实例

```python3
import requests
import json
 
r = requests.post('https://api.github.com/some/endpoint', data=json.dumps({'some': 'data'}))
print(r.json())
```
定制header：

```python3
import requests
import json
 
data = {'some': 'data'}
headers = {'content-type': 'application/json',
           'User-Agent': 'Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:22.0) Gecko/20100101 Firefox/22.0'}
 
r = requests.post('https://api.github.com/some/endpoint', data=data, headers=headers)
print(r.text)
```
## 3. Response对象
使用requests方法后，会返回一个response对象，其存储了服务器响应的内容，如上实例中已经提到的 r.text、r.status_code……

```python3
r = requests.get('http://www.itwhy.org')
print(r.text, '\n{}\n'.format('*'*79), r.encoding)
r.encoding = 'GBK'
print(r.text, '\n{}\n'.format('*'*79), r.encoding)
```
## 4. 超时与异常
我们可以通过timeout属性设置超时时间，一旦超过这个时间还没获得响应内容，就会提示错误。

```python3
>>> requests.get('http://github.com', timeout=0.001)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
requests.exceptions.Timeout: HTTPConnectionPool(host='github.com', port=80): Request timed out. (timeout=0.001)
```
