---
layout: post
title:  "在线教育系列-Online Training"
date:   2022-07-06 21:45:00
categories: 深度学习
tags: 对话系统 教育 培训 机器人 用户画像
excerpt: 智能培训在在线教育中的探索、应用
author: 鹤啸九天
mathjax: true
---

* content
{:toc}


# 智能培训


# 房产实践

## 用户画像

培训初版画像尽量满足这几个条件：
- （1）便于获取、计算：有现成的数据源，计算方法相对确定
- （2）容易理解：无需过多解释
- （3）原子性：标签、属性之间粒度适中且相对独立
- （4）有潜在业务场景：对培训、任务分发、微聊等有一定价值
- （5）扩展性：兼容其他标签，方便扩展

培训领域画像示例：
- ① 学习态度：区分经纪人在培训产品上重视程度，基于过往培训频次、时长，计算数值，映射到几个区间，如：应付（参与培训1次失败就放弃了）、一般（参与几次培训，分数不理想）、认真（参与几次培训，分数逐渐提升至及格）、重视（参与较多次培训且分数较高）
- ② 学习能力：衡量经纪人培训分数增速，分为：慢、一般、快
- ③ 个性化特征：表达连贯性、普通话标准程度、逻辑性等，跟培训内容关系不大的特征
- ④ 知识掌握程度：对应一系列知识点得分，往图谱方向规划


## AI 培训介绍

【2020年12月15日】
- [北京链家AI讲盘大赛收官，AI和真人双评委评审](https://tech.ifeng.com/c/82E2U6J2iJW)
- [北京链家AI讲盘大赛收官 科技赋能行业服务能力再升级](https://finance.sina.com.cn/tech/2020-12-16/doc-iiznezxs7256961.shtml)

- 12月15日消息，今天，北京链家举办了首届社区百科AI讲盘大赛的决赛，从23000多名经纪人中最终产生的9支经纪人队伍，在AI人工智能和真人双评委的评审下，完成了一场战队之间，以及和AI机器人战队的对决。
- 北京链家总经理李峰岩表示，“在产业互联网时代，如何利用科技创新赋能经纪人成长，进而提升我们对客户的服务品质，一直是链家探索的方向。希望借助此次大赛的推广实践，进一步探究房产经纪行业的未来，让经纪人的成长更加专业化、系统化，从而推动整个行业的升级迭代。”
- ![](https://d.ifengimg.com/w445_h250_q90_webp/x0.ifengimg.com/res/2020/911FE760764ED7F2DDE6F6E6FE5A1739A63CDF50_size237_w445_h250.png)

## AI 讲盘

社区百科AI讲盘通过AI场景化智能技术，帮助房产经纪人实景演练讲盘带看能力。同时，由北京链家学院与贝壳“小贝助手”联合打造的这款AI讲盘产品，已在2020年7月正式上线应用。

### 业务需求

带看时的讲盘是经纪人非常重要的专业能力之一，却长期存在着<span style='color:red'>带看质量不高、时间把控不好、讲解信息不全面、线下练习占用作业时间</span>等问题，制约了经纪人的专业性成长，以及行业服务品质的进一步升级。


### 数据资源

社区百科AI讲盘产品依托于链家和贝壳打造的，国内数据量最大、覆盖面最广、颗粒度最细的房屋信息数据库——楼盘字典，从而拥有了海量、真实的底层大数据。

社区百科AI讲盘产品有一个特殊的数据库—由北京链家几千名一线员工搭建的“**社区档案**”。
- 每一个楼盘都拥有属于自己的一套完备的、个性化的档案资料，已实现了**人机对话**技术与**行业知识图谱**的深度结合，从而让楼盘及社区信息可以全面、真实呈现。

目前“社区档案”中的每个楼盘都有近**100个**题干标签，共超过**4**大题库**238**个问题，包含通用题库，商品房、公房、教育资源拓展题库等，从**小区周边**交通规划、商业人文配套、评估价格、落户政策等基础问题，再到**教育资源**等延展问题，形成了对北京6900多个社区信息的全面覆盖。
- 此外，“社区档案”还会根据客户的反馈，以及实际情况的变化进行内容更新。

### 产品功能

社区百科AI讲盘产品为经纪人营造了多元化的培训场景，机器人小贝可以针对不同经纪人的特点“因材施教”。
- 经纪人通过学习社区资料实战演练讲盘，而小贝能够智能识别经纪人讲盘时的信息全面性、准确度、发音咬字、表达逻辑、讲盘流畅度、连贯性等核心要素，并在闯关结束后给出评分和反馈建议。
- 这一产品能够让经纪人精准知晓问题所在，从而更有针对性地进行练习提升，在高度还原**真实场景**的学习环境中，帮助经纪人收获快速成长。

### 产品效果

北京链家一位经纪人表示：
- 作为刚入行的新人，以前进行讲盘时心里总会发怵，和客户交流时担心自己的专业度不够，现在则可以利用一切碎片时间，随时随地练习讲盘，大大提升了自己的专业能力和自信心。

李峰岩表示
> 自内部推行以来，有超过**96%**的经纪人在该产品中参与了练习，产品所具备的全面丰富详尽的楼盘社区资料库，让经纪人对自己所服务的商圈及目标商圈有更好、更深、更细致的了解，进一步提升了经纪人的专业能力和服务品质。


## VR 带看模拟训练

VR带看培训侧重过程模拟


# 结束