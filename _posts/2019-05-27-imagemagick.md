---
layout: post
title: ImageMagick 簡易操作
categories: [Development Notes]
excerpt: ImageMagick tutorial.
---
* Index
{:toc}

## convert

usage:

{% highlight shell_session %}
$ convert <source> [options] <output>
{% endhighlight %}

注：

- 可以使用格式化字串`%d`
- conver 工具只適合處理少數文檔，大量文檔的時候效能十分著急。且會大量空讀寫硬盤和佔用內存。

常用選項：

- -monochrome 黑白
- -adaptive-sharpen radius[xsigma] 自適應鋭化 useful: 12
- -colorspace 調整色彩空間
- -resize 調整大小
- -quality 品質

## mogrify

usage:

{% highlight shell_session %}
$ mogrify [options] <source>
{% endhighlight %}

注：

- 批處理代替 `conver`，能達到最大效能

常用選項：

- -path 指定輸出文件夾，默認會覆蓋文件
- -format 轉換類型
