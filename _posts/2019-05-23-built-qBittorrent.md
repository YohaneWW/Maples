---
layout: post
title: qBittorrent 搭建
categories: [Development Notes]
series: CentOS Notes
excerpt: Built headless qBittorrent.
---
* Index
{:toc}

為使搭建更加簡單，使用 Docker。

{% highlight shell_session %}
# docker pull emmercm/qbittorrent
{% endhighlight %}

為使遷移與維護更加放便，並充分發揮虛擬化優勢，採用配置文件分離的方式進行安裝。

在 `/etc` 下新建 `qBittorrent` 文件夾以存放配置文件。

{% highlight shell_session %}
docker run -d --name="qbittorrent" \
    --publish 8080:8080 \ #webUI 端口
    --publish 6881:6881/tcp \ #通訊端口
    --publish 6881:6881/udp \
    --volume "/etc/qBittorrent/config:/config" \
    --volume "/etc/qBittorrent/data:/data" \
    --volume "$PWD/downloads:/downloads" \ #默認下載目錄
    --volume "$PWD/incomplete:/incomplete" \ #默認臨時存放目錄
    --restart=always emmercm/qbittorrent
{% endhighlight %}

注意開放端口，之後通過 `web-ui` 與默認用戶 `admin` 與密碼 `adminadmin` 訪問。
