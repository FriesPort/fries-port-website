---
sidebar_position: 1
---

# Docusurus介绍

建站方面当之无愧第一人气的是使用PHP开发的WordPress。WordPress作为一个历史悠久的CMS（内容管理系统）拥有海量的插件以及几乎所有疑难杂症的解决方式。但是WordPress的页面是动态页面，对于我的需求而言太过复杂。因此需要寻找一个更简单直接的解决方案来制作我的网站。

我的需求如下：
- 可以简单的通过编写markdown格式的文本制作网页。
- 可以扩展其他功能。
- 如果可能的话使用静态网页，以减少服务器方面的开销。

对于这类需求，比较符合条件的是文档制作器。其中扩展性主要分为两个阵营：react和vue。react阵营较为出名的是由Meta（Facebook）开发的Docusurus，vue阵营较为出名的是vue自己的主力静态网站生成器vitepress。

以前学过一点点Vue3，但是没整明白，又因为我学习的网站设计课程[MIT Web Lab](https://weblab.mit.edu/)使用的框架是react，在这个过程中个人觉得react比vue好理解，最终选择了Docusurus。

互联网上关于Docusurus的中文资料比较少，特此记录一下自己部署笔记。

## 参考文献
本次教程使用的资料主要有Docusurus和github pages的官方文档。