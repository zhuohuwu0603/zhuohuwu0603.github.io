---

layout: post
title: 为什么netty比较难懂？
category: 技术
tags: Netty
keywords: JAVA netty

---

## 前言

到目前为止，笔者关于netty写了十几篇博客，内容非常零碎，笔者一直想着有一个总纲的东西来作为主干，来将这些零碎place it in context。本文是一个初步的尝试，会随着理解的深入保持更新。

从上往下看，netty有哪些东西

![](/public/upload/netty/learn_netty.png)


为什么很多人会觉得学习netty代码比较难（这也是笔者最初的感受）？**因为对于很多人来说，是先接触了netty，才第一次接触nio、同步操作异步化 等技术/套路，也就是上图“核心” 部分的东西才第一次接触，除了要理解netty代码本身的抽象之外，还需理解很多新概念。**并且，极易带来的错误观念是，因为netty是这样用的，便容易认为这些特性只能这样用。比如一想起nio便认为只能像netty那样用，但hadoop中传输文件块代码 便与netty的有所不同。 