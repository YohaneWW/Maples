---
layout: post
title: NextCloud 搭建
categories: [Development Notes]
series: CentOS Notes
excerpt: Built NextCloud.
---
* Index
{:toc}

`postgresql` 的 Docker 鏡像使用上有些問題，於是選擇 `mysql` 來作為數據庫。

{% highlight shell_session %}
# docker pull nextcloud:apache
# docker pull mysql:5 #高版本的 mysql 會出現問題
{% endhighlight %}

{% highlight shell_session %}
# docker run -d --name="mysql" \
	-e MYSQL_ROOT_PASSWORD=password \
	-v /var/mysql:/var/lib/mysql \ #配置分離
    --restart=always mysql:5
{% endhighlight %}

## 登入 mysql 添加表

{% highlight shell_session %}
# docker exec -it mysql mysql -u root -p
> passwd: ....
mysql> CREATE DATABASE nextcloud;
mysql> exit;
{% endhighlight %}

{% highlight shell_session %}
# docker run -d --name="nextcloud" \
    -p 35235:80 \ #如果需要使用反代 https，此句可忽略
    -v /var/nextcloud:/var/www/html \
    -v /path:/path \ #掛載需要添加的外部目錄
    --link mysql:mysql \
    --restart=always nextcloud
{% endhighlight %}

## 設定文字編碼

{% highlight shell_session %}
# docker exec -it nextcloud env LANG=C.UTF-8 /bin/bash
{% endhighlight %}

## 設定文字編碼來支持漢語字符

{% highlight shell_session %}
# vim /var/nextcloud/config/config.php
'trusted_domains' =>
  array (
    0 => 'nextcloud.mydomain.com',
  ),
{% endhighlight %}

## ＊設定重寫

如果需要使用反代服務器支持 SSL，還需要設定重寫規則。

	!/var/nextcloud/config/config.php
	
	  'overwritehost' => 'butwonts.top:33663',
	  'overwriteprotocol' => 'https',
	  'overwrite.cli.url' => 'https://butwonts.top',

## 開啟插件

- Talk
- Markdown Editor

## 已知的問題

- `NextCloud Talk` 通話功能無法正常使用
