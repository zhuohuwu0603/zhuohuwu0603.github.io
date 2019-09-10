---

layout: post
title: 《实现领域驱动设计》笔记
category: 架构
tags: DDD
keywords: ddd cqrs

---

## 简介

* TOC
{:toc}

DDD绝非是什么标新立异之物，我更倾向于将其看成是软件发展的自然结果。就像20世纪六七十年代出现了软件危机之后，面向对象成为了人们的救赎；瀑布式开发过程遇到瓶颈时，敏捷被搬上了舞台；而DDD则是对传统的以数据为中心的建模方式的反思结果。

如果你的项目完全以数据为中心，所有的操作都通过对数据库的crud完成，那么你并不需要DDD。此时你的团队只需要一个漂亮的数据库表编辑器。如果你的系统只有25到30个业务操作， 这应该是相当简单的，你没有感受到由复杂性和业务变化所带来的痛苦。 当你的系统有三四十个use case的时候，软件的复杂性便暴露出来了，如果软件功能在未来几年不断变化，ddd将有助于你管理复杂性和应对变化。


守住三个基本原则

1. 必须通过领域建模来驱动设计
1. One principle behind DDD is to bridge the gap between domain experts and developers by using the same language to create the same understanding. 
2. Another principle is to reduce complexity by applying object oriented design and design patters to avoid reinventing the wheel.

### DDD入门

    public void saveConsumer(String id,name,age,address,...){
        Consumer consumer = new Consumer();
        if(id != null){
            ...
        }
        ...
        consumerDao.save(consumer);
    }

saveConsumer 至少存在三大问题

1. saveConsumer 的业务意图不明确，代码无法反应业务意图，使用同一个方法来处理多个用例流
2. 方法的实现本身增加了潜在的复杂性（比如复杂的参数校验）
3. Consumer 只是一个data holder

一种优化：每一个应用层方法对应一个单一的用例流

    interface Consumer{
        public change PersonalName(String firstName,String lastName);
        ...
    }

我们希望对对象行为的命名能够传达准确的业务含义，也即反映通用语言。要达到这样的目的，肯定不是先在类上定义属性，然后向客户端暴露getter和setter那么简单。那只是在创建纯数据模型。如果只提供setter 和getter 会怎么样？

1. 暴露了 对象的内部结构
1. setter 和getter 并没有业务价值，方法的名字没有业务含义。如果Consumer 只修改了地址 而没修改邮政编码会发生什么呢？显然是一种**领域逻辑的泄漏**。这要求开发对对象很熟悉（随着迭代并不总能办到）；**如果软件留下太多的地方让用户自己去理解，用户往往需要培训才能做出操作决定**。

## 限界上下文

领域上下文是一个显式的边界，领域模型便存在于这个边界之内。创建边界的原因在于：每个模型概念，包括它的属性和操作，在边界之内都具有特殊的含义。在很多情况下， 在不同模型中存在名字相同或相近的对象，但是它们的意思却不同。 当模型被一个显式的边界所包围时，其实每个概念的含义便是确定的了。 

考虑一个图书出版机构，它需要处理图书生命周期的不同阶段

1. 概念设计，计划出书。此时，连书名都没有
2. 联系作者，签订合同
3. 图书编辑、设计布局、插图。此时，图书是一些列稿件、注释、校正
3. 出版纸质书
5. 市场营销。此时，营销人员只关心书的简介
6. 将图书卖给销售商或读者。此时，重点是书的价格、重量、物流目的地。

如果整个系统只有一个Book对象，概念混淆、意见分歧和争论是不可避免的。如果我们将系统划分为3个上下文，每个上下文都有Book

1. 创作上下文，Book 是一个“作品”
2. 出版上下文，Book 可以视为一个印刷品（可能不准确）、出版物
3. 销售上下文，Book 可以视为一个 商品

如果你在不同的界限上下文中看到了完全相同的对象，通常意味着你的模型是错误的。有些相似的对象拥有不同的属性和行为（一个对象在不同上下文的“分身”），此时通常可以认为上下文边界的划分是合理的。 


## 其它

无论你选择做什么，总有人说你是错的，又总有这样那样的困难诱使你相信批评你的人是对的。要找到一条正确之路并坚持到最后，你需要的是勇气。 

一旦你没有被击倒，那么你所做的选择将双倍的补偿你



