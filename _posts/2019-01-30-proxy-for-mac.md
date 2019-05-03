---
layout: post
title: 設置 macOS shell 代理
categories: [Development Notes]
series: macOS Notes
excerpt: Set the proxy for macOS shell.
---
* Index
{:toc}

## cd 到根目录，查看所有文件（包括隐藏文件）

{% highlight shell_session %}
$ ls -al
{% endhighlight %}

找到 .bash\_profile 文件，如果没有（macOS 默认是没有这个文件的），则创建一个。

## 创建 .bash\_profile 文件

{% highlight shell_session %}
$ touch .bash_profile
{% endhighlight %}

使用 vim 编辑器打开 .bash\_profile 文件，添加以下代码。

    alias setproxy="export http_proxy=http://127.0.0.1:1080; export https_proxy=$http_proxy; echo 'HTTP Proxy on';"
    alias unsetproxy="unset http_proxy; unset https_proxy; echo 'HTTP Proxy off';"

最后使用以下命令来让 .bash\_profile 文件立即生效，或者重启电脑。

{% highlight shell_session %}
$ source ~/.bash_profile
{% endhighlight %}

## 开启代理

{% highlight shell_session %}
$ setproxy    
> HTTP Proxy on
{% endhighlight %}

## 关闭代理

{% highlight shell_session %}
$ unsetproxy  
> HTTP Proxy off
{% endhighlight %}