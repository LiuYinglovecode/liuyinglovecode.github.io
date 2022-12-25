---
title: go get cmd
date: 2020-1-13 09:56:35
tags:
- go get
- cmd
categories:
- Golang
---
### go get cmd
1. go get
下载导入路径指定的包及其依赖项，然后安装命名包，即执行go install命令。
用法：go get [-d] [-f] [-t] [-u] [-fix] [-insecure] [build flags] [packages]

2. 标记名称	                 描述
-d	让命令程序只执行下载动作，而不执行安装动作。
-f	仅在使用-u标记时有效,该标记会让命令程序忽略掉对已下载代码包的导入路径的检查。如下载并安装的代码包所属的项目是Fork的。
-fix	让命令程序在下载代码包后先执行修正动作，而后再进行编译和安装。
-insecure	使用非安全scheme（如HTTP）去下载指定的代码包。如果用的代码仓库（如公司内部的Gitlab）没有HTTPS支持，可以添加此标记。
-t	让命令程序同时下载并安装指定的代码包中的测试源码文件中依赖的代码包。
-u	让命令利用网络来更新已有代码包及其依赖包。默认情况下，该命令只会从网络上下载本地不存在的代码包，而不会更新已有的代码包。
-v	打印出被构建的代码包的名字
-x	打印出用到的命令
3. go install
使用：go install [-i] [build flags] [packages]。
和go build命令比较相似，go build命令会编译包及其依赖，生成的文件存放在当前目录下。而且go build只对main包有效，其他包不起作用。而go install对于非main包会生成静态文件放在$GOPATH/pkg目录下，文件扩展名为a。如果为main包，则会在$GOPATH/bin下生成一个和给定包名相同的可执行二进制文件。

综上： go get命令会下载指定的包，并将下载的包进行编译，然后安装到特定目录。
