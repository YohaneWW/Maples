---
layout: post
title: "設置 macOS Shell 代理"
categories: "macOS Notes"
---
Set the proxy for macOS shell.

## cd 到根目录，查看所有文件（包括隐藏文件）

	$ ls -al

找到 .bash\_profile 文件，如果没有（macOS 默认是没有这个文件的），则创建一个。

## 创建 .bash\_profile 文件

	$ touch .bash\_profile

使用 vim 编辑器打开 .bash\_profile 文件，添加以下代码。

	alias setproxy="export http\_proxy=http://127.0.0.1:1080; export https\_proxy=$http\_proxy; echo 'HTTP Proxy on';"
	alias unsetproxy="unset http\_proxy; unset https\_proxy; echo 'HTTP Proxy off';"

最后使用以下命令来让 .bash\_profile 文件立即生效，或者重启电脑。

	$ source \~/.bash\_profile

# 开启代理

	$ setproxy    
	HTTP Proxy on

# 关闭代理

	$ unsetproxy  
	HTTP Proxy off