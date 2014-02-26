---
layout: post
title: 在keil中使用分散加载文件
categories: 工作
tags: 嵌入式 keil 分散加载
---
最近用keil开发一个项目用到了UCGUI，并且使用中文字库。结果这个中文字库就达到了250多k。这样就使我调试烧写的过程变的十分漫长，因为以前我在ADS上面解决过同样问题，所以知道这个问题可以用分散加载文件来解决。下面是我的分散加载文件（这里我把flash的后512k用来保存字库）。
``` [c]
LR_IROM2 0x08080000 0x0080000  {    
  font.bin 0x08080000 0x00080000  {                             
   hz_font12x12.o(+RO-DATA)             
   }
}
LR_IROM1 0x08000000 0x00080000  {  
  prog.bin 0x08000000 0x00080000  { 
   * (RESET, +First)
   *(InRoot$$Sections)
   .ANY (+RO)
  }
  RW_IRAM1 0x20000000 0x00020000  {  
   .ANY (+RW +ZI)
  }
}
```
另外，要去处“Use Memory Layout from Target Dialg”的挂钩，并把分散加载文件的路径加到“Scatter File”中。

![设置启用分散加载文件]({{ site.img_url }}/scatter file seting.PNG "设置启用分散加载文件")

还有一点需要注意如果这么写分散加载文件用fromelf是应该出现两个bin文件的，那么下面的那条语句中的binout就成为目录了。
```[shell]
fromelf --bin !L --output binout
```

![fromelf命令的设置]({{ site.img_url }}/fromelf out.PNG "fromelf命令的设置")

bin文件输出目录如下：

![bin文件输出目录]({{ site.img_url }}/out bin dir.PNG "bin文件输出目录")

