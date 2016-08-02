---
title: 将xml代码或文件转为plist文件（xcode）
date: 2016-08-01 17:45:52
tags:    
---



### 前言

iOS开发过程中，plist文件的使用非常频繁，这里介绍一个小技巧给大家，主要包括：
	
> 1.通过Xcode编辑器编辑
> 2.直接修改plist源码

#### <font color="blue">Blue Elves</font> 提供[将xml代码或文件转为plist文件（xcode）](http://blog.csdn.net/tutuzhuz/article/details/52087479)技术支持

<!-- more -->

### ------需要转换的代码


``` xml

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
    <dict>
        <key>items</key>
        <array>
            <dict>
                <key>assets</key>
                <array>
                    <dict>
                        <key>kind</key>
                        <string>software-package</string>
                        <key>url</key>
                        <string>http://下载地址.ipa</string>
                    </dict>
                </array>
                <key>metadata</key>
                <dict>
                    <key>bundle-identifier</key>
                    <string>com.Business</string>
                    <key>bundle-version</key>
                    <string>1.0.0</string>
                    <key>kind</key>
                    <string>software</string>
                    <key>title</key>
                    <string>标题.ipa</string>
                </dict>
            </dict>
        </array>
    </dict>
</plist>

``` 

### 首先在xcode中新建一个空的plist文件，然后看图，打开方式---选择 源码 打开

{% img /images/article/zzz-xmlToPlist/image_1.png %}

{% img /images/article/zzz-xmlToPlist/image_2.png %}
### 然后将代码复制过去

{% img /images/article/zzz-xmlToPlist/image_3.png %}


### 然后再次改变打开方式

{% img /images/article/zzz-xmlToPlist/image_4.png %}

### 打开后就是这个样子了
{% img /images/article/zzz-xmlToPlist/image_5.png %}


### 是不是方便了很多呢，不需要下载其他软件啦

### 上面一段是源码，小伙伴们可以试一下
