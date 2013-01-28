---
layout: post
category: 随笔
title: 如何制作越狱版本iOS软件
published: true
---

###如何制作一个越狱版本的ios应用？
1. 编译生成Distribution版本的程序
2. 右击生成的程序，选择显示包内容，选择Info.plist文件添加字段：SigerIdentity，字段值：Apple OS Application Signing
3. 创建Payload文件夹，将程序拖进，压缩并更改压缩后缀为*.ipa
4. 拖进生成的ipa文件于iTurns或通过同步工具安装即可