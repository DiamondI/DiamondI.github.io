---
title: Misc-9-big from hackme.inndy.tw
categories: ctf
---

# big

这是一道来自[hackme.inndy.tw](https://hackme.inndy.tw)的题目，Misc第九道。题目提示：`It's a big file, read the flag.`

<!-- more -->

首先将文件下载下来，发现是一个只有1KB的大小的xz压缩文件。解压之后得到一个大小为2.5MB的，不知道是什么类型的文件。用Hex Fiend打开后发现这可能是在文件头上做了手脚，如下图所示：

<img src="https://ws1.sinaimg.cn/large/006tNbRwly1fyl4uih3bpj30le0okn62.jpg" width="400px"/>

可以看到可能是在xz的压缩文件的文件头上加了点什么东西。去搜索了一番之后发现xz的文件头应该是：

```
FD 37 7A 58 5A 00 00
```

对比上面的图片，发现就是在文件的开头加了`20`。用Hex Fiend这个十六进制的编辑器将其删除之后再保存，这下就得到了一个可以继续解压的xz压缩文件了。再解压之后就得到了一个大小为**17GB**（是的，你没看错，就是**17GB**这么大！）的txt文件，当时我的表情是这样的：Σ( ° △ °|||)︴这么大的文件，我手上没有哪个可视化的编辑器是可以正常打开它的。幸好Mac电脑的预览可以只看前面一点点（用head命令也可以起到类似的作用，不过没有预览直观），发现前面每一行的内容都是`THISisNOTFLAG{}`。然后想到要不从后面往前找试试看，就尝试用tail命令看了最后几行，结果flag就放在最后一行。拿到flag之后赶紧把这占空间的文本文件删了。
