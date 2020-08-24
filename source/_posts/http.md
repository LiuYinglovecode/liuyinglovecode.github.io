---
title: userinfo_interface
date: 2019-9-28 06:31:09
tags:
- GET
- POST
- HTTP
categories:
- web
---

## HTTP请求包
Request包分为3部分，依次为Request line,Request header, body.
HTTP协议与服务器交互的请求方法：GET,POST,PUT,DELETE.查增改删。
### fiddler
GET方法
![](https://raw.githubusercontent.com/astaxie/build-web-application-with-golang/master/zh/images/3.1.http.png)

POST方法
![](https://raw.githubusercontent.com/astaxie/build-web-application-with-golang/master/zh/images/3.1.httpPOST.png)

### HTTP响应包（服务器信息）
``` bash
HTTP/1.1 200 OK						//状态行
Server: nginx/1.0.8					//服务器使用的WEB软件名及版本
Date:Date: Tue, 30 Oct 2012 04:14:25 GMT		//发送时间
Content-Type: text/html				//服务器发送信息的类型
Transfer-Encoding: chunked			//表示发送HTTP包是分段发的
Connection: keep-alive				//保持连接状态
Content-Length: 90					//主体内容长度
//空行 用来分割消息头和主体
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"... //消息体
```
Response包中的第一行叫做状态行，由HTTP协议版本号， 状态码， 状态消息 三部分组成。

状态码用来告诉HTTP客户端,HTTP服务器是否产生了预期的Response。HTTP/1.1协议中定义了5类状态码， 状态码由三位数字组成，第一个数字定义了响应的类别

1. 1XX 提示信息 - 表示请求已被成功接收，继续处理
2. 2XX 成功 - 表示请求已被成功接收，理解，接受
3. 3XX 重定向 - 要完成请求必须进行更进一步的处理
4. 4XX 客户端错误 - 请求有语法错误或请求无法实现
5. 5XX 服务器端错误 - 服务器未能实现合法的请求

### HTTP协议是无状态的和Connection: keep-alive的区别
无状态是指协议对于事务处理没有记忆能力，服务器不知道客户端是什么状态。从另一方面讲，打开一个服务器上的网页和你之前打开这个服务器上的网页之间没有任何联系。

HTTP是一个无状态的面向连接的协议，无状态不代表HTTP不能保持TCP连接，更不能代表HTTP使用的是UDP协议（面对无连接）。

从HTTP/1.1起，默认都开启了Keep-Alive保持连接特性，简单地说，当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的TCP连接不会关闭，如果客户端再次访问这个服务器上的网页，会继续使用这一条已经建立的TCP连接。

Keep-Alive不会永久保持连接，它有一个保持时间，可以在不同服务器软件（如Apache）中设置这个时间。
