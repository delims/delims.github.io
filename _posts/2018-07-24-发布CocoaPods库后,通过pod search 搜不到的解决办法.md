---
layout: post
author: delims
categories: iOS
---

>辛苦发布了CocoaPods框架后，通过pod search却搜不到，别着急，下面这个方法或许可以帮到你

search_index.json文件是pod search搜索时的缓存文件，删除 search_index.json

`rm ~/Library/Caches/CocoaPods/search_index.json`

打开终端重新使用 pod search 搜索你的框架,这时 search_index.json 缓存文件会被重新生成,你的框架就可以搜索出来了。


