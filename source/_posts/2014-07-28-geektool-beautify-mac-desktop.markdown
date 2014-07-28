---
layout: post
title: "使用GeekTool来美化Mac的桌面"
date: 2014-07-28 21:00:57 +0800
comments: true
categories: Mac
tags: Mac
---
> 偶然间在论坛中看到有这么一个美化(拓展)桌面的应用。
于是边好好研究了一番。。

由于GeekTool的教程百度一搜一大把，所以我也就不写成教学向的，
主要还是写我当前的桌面是如何定制的好了。。  
整体效果预览：
{%img center /images/post/140728/preview.png %}
<!-- more -->

首先要先下载我们这个应用[GeekTool],
美国地区的[App Store]也是有提供下载的。

{%img center /images/post/140728/GeekTool_icon.icns 100px %}

打开后的界面如下：

{%img center /images/post/140728/mainface.png %}

主要分成3各区域，左边的Geeklets，提供三种选项，
文件、图像还有shell脚本。
右上方的Groups，是通过将建立的Geeklets放在不通组，然后快速管理。
下面的就是基本控制，Enable就是总开关了。

{%img right half /images/post/140728/properties.png 240px %}
通过将Geeklets拖拽到桌面来新建一个，
然后每一个都会有其对应的属性：

属性包括内容有：  
基本信息：* 名称(可选)* 位置* 最前* 阴影

分组：勾选所在的组，便于管理

命令：shell代码(核心部分)、刷新时间

返回状态：对于执行失败的反馈，便于调试

样式：* 字体* 背景* 对齐等

对于文件和图片的属性内容更简单，
选择要显示的图片就可以了。这里就不显示了。
<br /><br /><br />

首先是构建左上角的日期和天气显示：   
效果如图所示：  
{%img center /images/post/140728/date_and_weather.png %}

日期
{% codeblock lang:bash %}
date +"%d"
{% endcodeblock %}
月份(B小写则显示缩写,如:Jul)
{% codeblock lang:bash %}
date +"%B"
{% endcodeblock %}
星期(A小写则显示缩写,如:Mon)
{% codeblock lang:bash %}
date +"%A"
{% endcodeblock %}
图片：
{% codeblock lang:bash %}
URL:file:///tmp/weather4.png
{% endcodeblock %}
辅助脚本，用于获取天气图片到/tmp/weather4.png,其中CHXX0008这个表示的是北京城市，
可以修改成你所在城市，获取方法上[Weather]搜索。
{% codeblock lang:bash %}
if [ -f /tmp/weather4.png ];then
rm /tmp/weather4.png
fi
curl --silent "http://www.weather.com/weather/today/CHXX0008"|grep -E 'wxicon/120/'|head -1|sed s/'<img src="'//|awk '{print $1}'|sed s/'"'//|xargs curl --silent -o /tmp/weather4.png

osascript -e 'tell application "GeekTool Helper"
				  refresh image geeklet "w_image"
			  end tell'
{% endcodeblock %}
显示当前气温和天气状况。
{% codeblock lang:bash %}
curl --silent "http://www.weather.com/weather/today/CHXX0008"|awk '/Right Now/{getline; getline; getline; print}'|awk '{print int(($14-32)*5/9) "°C " $15 " " $16 " " $17}'|sed s/';'/""/|sed s/','/" "/|sed s/'">'//|awk '{getline; print}'
{% endcodeblock %}
显示降雨可信度，方向和湿度
{% codeblock lang:bash %}
curl --silent "http://www.weather.com/weather/today/CHXX0008"|grep "raindrop"|awk -F'>' '{print "Chance of rain " $4}'|sed 's/\<\/div//g'|head -1
curl --silent "http://www.weather.com/weather/today/CHXX0008"|grep -i "wind-label"|head -1|awk -F">" '{print "Wind  " $2}'|sed -e 's/\<\/div//g'
curl -s 'http://www.weather.com/weather/today/CHXX0008' | grep 'humidity' | head -1 | awk -F '>' '{print $5}' | sed 's/<\/span//' | awk '{print "Humidity  "$0"%"}'
注意预览图最左上角还有的，这个功能是电脑启动时长。
{% endcodeblock %}
{% codeblock lang:bash %}
uptime | awk '{print "" $3 " " $4 " " $5 }' | sed -e 's/.$//g';
{% endcodeblock %}

这样**左上角**的就已经完成了。

接下来是左下角的的系统使用信息，效果如图。  
{%img center /images/post/140728/system_info.png %}

首先需要准备一个字体：用于显示我们的这些圆圈。。  
{%img center /images/post/140728/arcfont.png %}
[圆圈字体下载]

硬盘使用信息的图形
{% codeblock lang:bash %}
df -H |awk '/disk0/{print "X"int($5/2)"X"}'|sed "s/X0X/a/;s/X1X/b/;s/X2X/c/;s/X3X/d/;s/X4X/e/;s/X5X/f/;s/X6X/g/;s/X7X/h/;s/X8X/i/;s/X9X/j/;s/X10X/k/;s/X11X/l/;s/X12X/m/;s/X13X/n/;s/X14X/o/;s/X15X/p/;s/X16X/q/;s/X17X/r/;s/X18X/s/;s/X19X/t/;s/X20X/u/;s/X21X/v/;s/X22X/w/;s/X23X/x/;s/X24X/y/;s/X25X/z/;s/X26X/A/;s/X27X/B/;s/X28X/C/;s/X29X/D/;s/X30X/E/;s/X31X/F/;s/X32X/G/;s/X33X/H/;s/X34X/I/;s/X35X/J/;s/X36X/K/;s/X37X/L/;s/X38X/M/;s/X39X/N/;s/X40X/O/;s/X41X/P/;s/X42X/Q/;s/X43X/R/;s/X44X/S/;s/X45X/T/;s/X46X/U/;s/X47X/V/;s/X48X/W/;s/X49X/X/;s/X50X/Y/;s/\d//"
{% endcodeblock %}
硬盘使用信息的百分比
{% codeblock lang:bash %}
df -H |awk '/disk0/{print "Disk\n"$5}'
{% endcodeblock %}
CPU使用的图形
{% codeblock lang:bash %}
top -l 1|awk '/CPU usage/ {print "X"int($3/2)"X"}' | sed 's/\%//'| sed "s/X0X/a/;s/X1X/b/;s/X2X/c/;s/X3X/d/;s/X4X/e/;s/X5X/f/;s/X6X/g/;s/X7X/h/;s/X8X/i/;s/X9X/j/;s/X10X/k/;s/X11X/l/;s/X12X/m/;s/X13X/n/;s/X14X/o/;s/X15X/p/;s/X16X/q/;s/X17X/r/;s/X18X/s/;s/X19X/t/;s/X20X/u/;s/X21X/v/;s/X22X/w/;s/X23X/x/;s/X24X/y/;s/X25X/z/;s/X26X/A/;s/X27X/B/;s/X28X/C/;s/X29X/D/;s/X30X/E/;s/X31X/F/;s/X32X/G/;s/X33X/H/;s/X34X/I/;s/X35X/J/;s/X36X/K/;s/X37X/L/;s/X38X/M/;s/X39X/N/;s/X40X/O/;s/X41X/P/;s/X42X/Q/;s/X43X/R/;s/X44X/S/;s/X45X/T/;s/X46X/U/;s/X47X/V/;s/X48X/W/;s/X49X/X/;s/X50X/Y/"
{% endcodeblock %}
CPU使用的百分比
{% codeblock lang:bash %}
top -l 1|awk '/CPU usage/ {print "CPU\n"$3}'
{% endcodeblock %}
内存使用的图形(Mac内存给出的信息我不是特别清楚，所以计算结果不一定正确，
我是用$$\frac{Used+Wired}{Used+Wired+Free}$$的出的)
{% codeblock lang:bash %}
top -l 1|grep "PhysMem"|awk '{print "X"int(($2+$4)/($2+$4+$6)*50)"X"}'|sed "s/X0X/a/;s/X1X/b/;s/X2X/c/;s/X3X/d/;s/X4X/e/;s/X5X/f/;s/X6X/g/;s/X7X/h/;s/X8X/i/;s/X9X/j/;s/X10X/k/;s/X11X/l/;s/X12X/m/;s/X13X/n/;s/X14X/o/;s/X15X/p/;s/X16X/q/;s/X17X/r/;s/X18X/s/;s/X19X/t/;s/X20X/u/;s/X21X/v/;s/X22X/w/;s/X23X/x/;s/X24X/y/;s/X25X/z/;s/X26X/A/;s/X27X/B/;s/X28X/C/;s/X29X/D/;s/X30X/E/;s/X31X/F/;s/X32X/G/;s/X33X/H/;s/X34X/I/;s/X35X/J/;s/X36X/K/;s/X37X/L/;s/X38X/M/;s/X39X/N/;s/X40X/O/;s/X41X/P/;s/X42X/Q/;s/X43X/R/;s/X44X/S/;s/X45X/T/;s/X46X/U/;s/X47X/V/;s/X48X/W/;s/X49X/X/;s/X50X/Y/;s/\d//"
{% endcodeblock %}
内存使用的百分比
{% codeblock lang:bash %}
top -l 1|grep "PhysMem"|awk '{print "RAM\n"int(($2+$4)/($2+$4+$6)*100)"%"}'
{% endcodeblock %}
最后分子结构式的边框就要自己画了。。。

到此就差最后中间的时钟了，效果如图：  
{%img center /images/post/140728/time.png %}

这个的代码也是非常简单的
{% codeblock lang:bash %}
date +"%l:%M"
{% endcodeblock %}
但是这样只是显示了一个时间而已，要做出层次的感觉，
这时候还是需要上Photoshop了，把背景图片中要用来遮罩的部分扣下来，
然后再将其作为一个图片的Geeklets添加，调整大小和位置来将其和背景对齐，
右键将其放置在最前。这样数字时钟就被遮挡住了。。

当然GeekTool能做的不仅限于这些，还有很多很多，
只要你会用shell就能添加各种各样你想要的效果，
只不过我个人还是喜欢一个干净简洁的Mac桌面，
shell上还是只菜鸟，
所以不想添加太多繁杂的内容。


[GeekTool]:http://projects.tynsoe.org/en/geektool/download.php
[App Store]:https://itunes.apple.com/us/app/geektool/id456877552?mt=12
[Weather]:www.weather.com
[圆圈字体下载]:https://raw.githubusercontent.com/fatestigma/beatifyMacDesktop/master/GeekTool/ARCfont.ttf
