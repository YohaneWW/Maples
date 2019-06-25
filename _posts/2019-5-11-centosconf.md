---
layout: post
title: CentOS 使用筆記
categories: [Development Notes]
series: CentOS Notes
excerpt: CentOS Config.
---
* Index
{:toc}

## Enviroment

- CentOS Linux release 7.6.1810 (Core) 

## 基礎設置

### ssh

ssh 是實現遠程連接的基石，應該在第一時間配置妥當，並確保完無一失。

### 網卡設置

CentOS 最小化安裝後，默認不帶 `net-tools`，可用 `ip addr` 命令來代替 `ifconfig`。

在較新的 Linux 之中，已經取代了傳統的類似 `eth0` 的網卡命名。CentOS 默認會使用 `NetworkManager` 管理，而如需手動管理，需要禁用 `NetworkManager`。

{% highlight shell_session %}
# systemctl stop NetworkManager
# systemctl disable NetworkManager
{% endhighlight %}

網卡的配置文件在 `/etc/sysconfig/network-scripts` 以相應網卡的名稱命名，如果新增網卡的配置文件不存在，往往需要手動複製一份。

	BOOTPROTO=static # 靜態 ip
	IPADDR=0.0.0.0 # ip 位址
	NETMASK=255.255.255.0 # 路由器遮罩
	GATEWAY:0.0.0.0 # 網關地址
	DNS1=8.8.8.8 # DNS1

確認無誤後，重啟網路。

{% highlight shell_session %}
$ systemctl restart network
$ ping 1.1.1.1 -c 4
> PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=53 time=233 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=53 time=229 ms
64 bytes from 1.1.1.1: icmp_seq=4 ttl=53 time=232 ms

--- 1.1.1.1 ping statistics ---
4 packets transmitted, 3 received, 25% packet loss, time 3001ms
rtt min/avg/max/mdev = 229.373/231.699/233.061/1.652 ms
$ ping z.cn -c 4
> 64 bytes from 54.222.60.252 (54.222.60.252): icmp_seq=1 ttl=238 time=57.4 ms
64 bytes from 54.222.60.252 (54.222.60.252): icmp_seq=2 ttl=238 time=58.0 ms
64 bytes from 54.222.60.252 (54.222.60.252): icmp_seq=3 ttl=238 time=57.9 ms
64 bytes from 54.222.60.252 (54.222.60.252): icmp_seq=4 ttl=238 time=57.4 ms

--- z.cn ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3003ms
rtt min/avg/max/mdev = 57.404/57.713/58.060/0.338 ms
{% endhighlight %}

網路調試工具：`traceroute` `bind-utils`

### Firewall

#### 查看状态

{% highlight shell_session %}
firewall-cmd --state //running
firewall-cmd --list-all
{% endhighlight %}

#### 启用某个服务

{% highlight shell_session %}
# firewall-cmd --permanent --zone=public --add-service=https
{% endhighlight %}

#### 开启某个端口

{% highlight shell_session %}
# firewall-cmd --permanent --zone=public --add-port=8080-8081/tcp
{% endhighlight %}

#### 重新加載

{% highlight shell_session %}
# firewall-cmd --reload
{% endhighlight %}

#### 删除上面设置的规则

{% highlight shell_session %}
# firewall-cmd --permanent --zone=public --remove-service=http
{% endhighlight %}

### 更新時間

{% highlight shell_session %}
# date;hwclock -r
> 日  1月 11 05:21:56 CST 2015
西元2015年01月11日 (週日) 05時21分25秒  -0.520968 秒
# yum install -y ntp
# ntpdate pool.ntp.org && hwclock -w
{% endhighlight %}

### 對磁盤進行分區與掛載

{% highlight shell_session %}
# fdisk -l
# fdisk /dev/sd*
g
n
w
# mkfs.ext4 /dev/sdb
# mkdir /mnt/sd*
# mount /dev/sdb /mnt/sd*
# cd /mnt/sd*
# ls
> lost+found
# umount
# vim /etc/fstab
# mount -a
# df -h
{% endhighlight %}

### 配置第三方源

{% highlight shell_session %}
# yum localinstall --nogpgcheck http://mirrors.aliyun.com/epel/beta/7/x86_64/epel-release-7-0.2.noarch.rpm
# yum localinstall --nogpgcheck http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
# yum localinstall --nogpgcheck http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el7.rf.x86_64.rpm

# rpm --import http://elrepo.org/RPM-GPG-KEY-elrepo.org
# rpm -Uvh https://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm
{% endhighlight %}

### 安裝 ntfs-3g

由於 CentOS yum 官方源默認沒有 ntfs-3g，所以需要添加源解決。

{% highlight shell_session %}
# yum install -y wget
# wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
# yum install -y ntfs-3g
# vim /etc/fstab
{% endhighlight %}

如果出錯

	=> ntfsprogs

### 關閉 selinux

{% highlight shell_session %}
# setenforce 0
# vim /etc/selinux/config
{% endhighlight %}

## 服務搭建

### Docker

- Docker 是一個虛擬化容器，可以讓很多事變得非常簡單。
- 配置 ali 鏡像加速器以提升 docker hub 的拉取速度。
- 在关闭宿主机时务必需要停止 Docker，否则会导致不可预料的错误发生。
