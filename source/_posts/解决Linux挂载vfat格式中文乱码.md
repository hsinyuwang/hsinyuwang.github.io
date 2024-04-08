---
title: 解决Linux挂载vfat格式中文乱码
date: 2024-04-07 16:06:24
tags: [嵌入式]
---

配置内核 File systems  --->  DOS/FAT/NT Filesystems  --->
修改为：

```
(936) Default codepage for FAT     
(utf8) Default iocharset for FAT 
```

![](1.png)

含义：
codepage 是 charset encoding 的别称
936 是 GBK 编码
详见：[http://en.wikipedia.org/wiki/Codepage](http://en.wikipedia.org/wiki/Codepage)

为了使用 cp936 必须打开对它的支持
在File systems  ---> -*- Native language support  --->Simplified Chinese charset (CP936, GB2312)

![](2.png)

![](3.png)


