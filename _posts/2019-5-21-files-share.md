---
layout: post
title: 配置 Linux 下文件共享服務
categories: [Development Notes]
series: CentOS Notes
excerpt: File shares on Linux.
---
* Index
{:toc}

## Samba

有[報導](https://www.v2ex.com/t/352813)稱 `smb` 协议在 macOS 下有效能问题，建议使用 `WebDAV` `nfs` `afp` `ftp` 替代。（注：實測在 macOS 下 WebDAV 的上傳仍舊存在效能問題，如果沒有通用性需求建議使用 afp 協議。）

{% highlight shell_session %}
# chmod 770 -R /mnt #使掛載目錄具有正確的權限
# useradd share -d /mnt -s /usr/sbin/nologin
# vim /etc/samba/smb.conf
# testparm #檢查配置合法性
# pdbedit -a share #添加 smb 帳號，必須存在此 Linux 用戶
# smbpasswd -a <username>
# firewall-cmd --permanent --zone=public --add-service=samba #配置防火牆
# systemctl restart firewalld
# systemctl enable smb.service
# systemctl restart smb.service
{% endhighlight %}

### smb.conf

	!/etc/samba/smb.conf
	
	[global]
	netbios name        = htpc #顯示名稱
	workgroup           = workgroup
	load printers       = no
	security            = user #根據 Linux 用戶進行權限設置
	log level           = 8
	log file            = /var/log/samba/samba.log
	max log size        = 50
	unix charset        = utf8
	passdb backend      = tdbsam
	map hidden          = no
	[share]
	comment             = share
	path                = /mnt #路徑
	browseable          = yes #列出目錄，如果為 no 則必須手動輸入路徑
	writable            = yes
	read only           = no
	inherit acls        = Yes
	create mask         = 0777
	directory mask      = 0777
	valid users         = share,@htpc
	write list          = share,@htpc

## 打印機服務

打印服務需要 `cups` 支持

{% highlight shell_session %}
# yum -y install cups
# vim /etc/cups/cupsd.conf
# systemctl enable cups
# systemctl start cups
# smbcontrol all reload-config
{% endhighlight %}

### cupsd.conf

	!/etc/cups/cupsd.conf
	
	Listen 0.0.0.0:631 #監聽所有 ip
	Order deny,allow #允許遠程主機訪問
	Order deny,allow #配置 admin

配置為 share 後在 macOS 下可直接發現使用，也可通過配置 `smb` 來實現。另注意配置防火牆開放端口。

## WebDAV

為使維護與安裝更加快捷簡便，仍舊使用 `Docker`。仍舊需要共享目錄的權限問題。

	docker run -d --name="webdav" \
	    --publish port:80 \
	    -v /path:/var/lib/dav/data \
	    -e USERNAME=name -e PASSWORD=password \
	    --restart always bytemark/webdav
