---
layout: post
title:  "Web前端服务知识-Web-Serving"
date:   2020-08-07 19:17:00
categories: 技术工具 编程语言
tags: Web web Python Flask Django Fastapi Restful Swagger HTML JavaScript Session RPC 微服务 GraphQL UML Gunicorn supervisor genvent grequests node.js vue 前端 低代码 拖拽 api 异步 celery 分布式 apache qps 性能
author : 鹤啸九天
excerpt: Web开发相关技术知识点
mathjax: true
permalink: /web
---

* content
{:toc}

# 总结

- [前端和后端区别](https://zhuanlan.zhihu.com/p/83515211)
- [到底什么是前端、后端、后台啊？](https://www.zhihu.com/question/21923056)

## Web 3.0

【2022-8-9】目前web3.0还没到来。1.0是read，2.0是read&write这个大家基本赞同，但3.0增加了trust（或own）这个还有待验证。至少web3.0不是NFT和DApp能代表的。真正的Web3.0一定会伴随着设备和网络的升级一起来到，如果说1.0是PC+网线， 2.0是手机+4G，那3.0呢？目前的vr/ar/mr或者5g/6g都还早，需要进一步探索才能真的窥见web3.0全貌。

所以，目前的web3.0忽悠居多，大都是玩币那波人在炒作。web3.0是未来，但不是目前炒币人所定义的那个未来。
- 炒作web3.0概念的人还是17-18年炒币的那一波人，他们是玩金融的。利用区块链技术来回地玩金融，17-18年各种币层出不穷，被政府叫停后开始搞各种数字藏品和分布式APP。

## 前端、后端

前后端关系形象比喻: 前端是好看的皮囊，后端是复杂的肌肉骨骼，没有后者，前者啥也不是
- ![](https://pic3.zhimg.com/80/v2-a547451490a6aa70b0b4e66b30588a68_720w.jpg?source=1940ef5c)
- ![](https://pic1.zhimg.com/80/v2-bc6de7eff610c350e402c3fa22989bd0_720w.jpg?source=1940ef5c)

[前端与后端的关系](https://blog.csdn.net/m0_37105443/article/details/72961524)：近几年，前后端分离的思想主键深入，客户端+浏览器形成大前端，技术架构上逐渐的从传统的 后台MVC 向RESUFUI API+前端MV* 迁移，前端项目通过RESTful服务获取数据，RESTful API就是前后端的边界和桥梁。

前后端分离的好处是前端关注页面展现，后端关注业务逻辑，分工明确，职责清晰，前端工程师和后端工程师并行工作，提高开发效率。
- ![](https://pic3.zhimg.com/80/v2-5639c53c35af2954363b29664a52e278_720w.jpg?source=1940ef5c)

### 前后端演变历史

网站、应用等产品中，有三个很重要的东西 —— `浏览器`、`服务器`、`数据库`。

以php（也可以是python、java等）项目常见的流程，其过程一般是类似于下面这张图。
- ![](https://pic1.zhimg.com/80/v2-ea14325ee79f60d878d1b412d1e2a76d_720w.jpg?source=1940ef5c)
- 浏览器：“翻译”（即渲染）程序猿写的代码给用户看的，html文档主要会使用html、css、JavaScript三种语言
- 数据库：存放业务数据
- 服务器：自动操作（读写）数据库，如经常被提到的java、C++。

演变：
- 擅长html、css、JavaScript的程序猿，进化成了`前端工程狮`，天天倒腾浏览器，他们对用户体验负责。
- 擅长java的程序猿，进化成了`后端攻城狮`，天天倒腾数据库和服务器，他们对服务器性能及数据负责。
为了防止这两种不同的攻城狮工作内容混杂，双方约定一个发送请求的地址，和请求格式。
- 这种请求的地址和其相应的格式，又被称为`API`（接口）。
至此，做好API文档后，前端和后端终于可以老死不相往来，各自调试各自的代码。

这一不相往来的概念，也被称为`前后端分离`。于是诞生了一种新的"变态"——Node.js，这个玩意虽然是用前端最爱的JavaScript语言，但是可以操作服务器。不过Node.js主要是被前端用来做中间件（可以理解为为了分离的更彻底一点）的，因此很多时候也被纳入前端范畴。

随着互联网的迅猛发展，逐步进化出了更多生物：
- 擅长美工的人成了 `UI` （`美工`）
- 擅长沟通的人成了`产品经理` `PM` ，把客户和老板讲的东西理成结构化的文档，或是把用户的需求收集起来理成将来要做成软件的样子。
- 往网站上写文章，填内容实在是麻烦，而且要把网站流量做大，还得找个人出出主意，于是，`运营`也诞生了。
- 上线后服务器怎么老是不稳定，后端大佬们都去做新项目了，得找个hold的住服务器和机房的专家，然后`运维`出现了

### 前端

web前端分为网页设计师、网页美工、web前端开发工程师
- `网页设计师`是对网页的架构、色彩以及网站的整体页面代码负责
- `网页美工`只针对UI这块的东西，比如网站是否做的漂亮
- `web前端开发工程师`是负责交互设计的，需要和程序员进行交互设计的配合。

前端工程师主要的工作职责分为三大部分

| 类型 | 设备端 | 备注 |
|---|---|---|
| 传统的Web前端开发 | PC | - |
| 移动端开发 | APP：Android/iOS/小程序开发 | 移动端的开发任务量是比较大 |
| 大数据呈现端开发 | PC | 基于已有的平台完可视化展现，如大屏 | 

Web开发、APP开发以及小程序开发又统称为`大前端`

从应用范围来看，前端开发不仅被常人所知、且应用场景也要比后端广泛的太多太多。
- 一是`PC` (Personal Computer) 即个人电脑。目前电脑端仍是前端一个主要的领域，主要分为面向大众的各类网站，如新闻媒体、社交、电商、论坛等和面向管理员的各种 CMS (内容管理系统)和其它的后台管理系统。
- 二 Web `App` 是指使用 Web 开发技术，实现的有较好用户体验的 Web 应用程序。它是运行在手机和桌面端浏览中，随着移动端网络速度的提升，Web App 为我们提供了很大的便利。此外近两年 Google 提出了一种新的 Web App 形态，即 PWA(渐进增强 Web APP) 。
- 三 `WeChat` (微信) 这个平台，拥有大量的用户群体，因此它也是我们前端开发另一个重要的领域。微信的公众号与订阅号为市场营销和自媒体从业者，打造了一个新的天地。
- 四 `Hybrid` App (混合应用) 是指介于 Web App、原生 App (主要是 Android 或 iOS )之间的 App，它兼具原生 App 良好用户交互体验的优势和 Web App 跨平台开发的优势。
- 五 `Game`（游戏），HTML5 游戏从 2014 年 Egret 引擎开发的神经猫引爆朋友圈之后，就开始一发不可收拾。不过现在游戏开发变得越来越复杂，需要制作各种炫丽炫丽的效果，还要制作各炫丽于 2D 或者 3D 的场景。
- 六 `Desktop`桌面应用软件，就是我们日常生活中电脑中安装的各类软件。早期要开发桌面应用程序，就需要有专门的语言 UI (界面) 库支持，如 C++ 中的 Qt 库、MFC 库，Java 的 Swing、Python 的 PyQT 等，否则语言是没办法进行快速界面开发。
- 七 `Server` Node.js 一发布，立刻在前端工程师中引起了轩然大波，前端工程师们几乎立刻对这一项技术表露出了相当大的热情和期待。看到 Node.js 这个名字，初学者可能会误以为这是一个 Java 应用，事实上，Node.js 采用 C++ 语言编写而成，是一个 Java 的运行环境。

前端开发涉及到的内容包括Html、CSS、JavaScript、Android开发（采用Java或者kotlin）、iOS开发（采用OC或者Swift）、各种小程序开发技术（类Html），随着前端开发任务的不断拓展，前端开发后端化也是一个较为明显的趋势，比如Nodejs的应用。

### 后端

后端工程师分三大部分
- 平台设计：搭建后端的支撑服务容器
- 接口设计：针对于不同行业进行相应的功能接口设计，通常一个平台有多套接口，就像卫星导航平台设有民用和军用两套接口一样
- 功能实现：完成具体的业务逻辑实现。

后端开发通常需要根据业务场景进行不同语言的选择，另外后端开发的重点在于算法设计、数据结构、性能优化等方面，在具体的功能实现部分可以采用Java、Python或者PHP等编程语言来实现。

### 前端与后端技术栈

- 前端开发技术：html5、css3、javascript、ajax、jquery、Bootstrap、Node.js 、Webpack，AngularJs，ReactJs，VueJs等技术。
- 后端开发技术：  asp、php、jsp、.NET，以java为例，Struts spring springmvc Hibernate Http协议 Servlet Tomcat服务器等技术。

| 前端	| 后端|
|---|---|
| 编程语言 | HTML，CSS，JavaScript	| PHP，Python，SQL，Java，Ruby，.NET，Perl |
| 框架	| Angular.JS，React.JS，Backbone.JS，Vue.JS，Sass，Ember.JS，NPM	| Laravel，CakePHP，Express，CodeIgniter，Ruby on Rails，Pylon，ASP.NET |
| 数据库	| Local Storage, Core Data, SQLite, Cookies, Sessions	| MySQL，Casandra，Postgre SQL，MongoDB，Oracle，Sybase，SQL Server |
| 服务器	| -	| Ubuntu，Apache，Nginx，Linux，Windows |
| 其他	| AJAX，AMP，Atom，Babel，BEM，Blaze，Bourbon，Broccoli，Dojo，Flux，GraphQL，Gulp，Polymer，Socket.IO，Sublime Text	| - |


## 后端


# HTTP

HTTP常见的方法：
- `GET`：浏览器告知服务器：只 获取 页面上的信息并发给我。这是最常用的方法。
- `POST`：浏览器告诉服务器：想在 URL 上 发布 新信息。并且，服务器必须确保 数据已存储且仅存储一次。这是 HTML 表单通常发送数据到服务器的方法。
- `PUT`：类似 POST 但是服务器可能触发了存储过程多次，多次覆盖掉旧值。你可 能会问这有什么用，当然这是有原因的。考虑到传输中连接可能会丢失，在 这种 情况下浏览器和服务器之间的系统可能安全地第二次接收请求，而 不破坏其它东西。因为 POST 它只触发一次，所以用 POST 是不可能的。
- `DELETE`：删除给定位置的信息。


- 参考：[HTTP请求时POST参数到底应该怎么传?](https://blog.csdn.net/j550341130/article/details/82012961)，[HTTP POST/GET 在线请求测试工具](https://www.sojson.com/httpRequest/)

## HTTP请求头

- 请求三要素
  - ![](https://img-blog.csdn.net/2018082410162352)

- 根据应用场景的不同,HTTP请求的请求体有三种不同的形式, 通过header中的content-type指定, 这里只分析两个:
  1. **表单方式**：APPlication/x-www-form-urlencoded(默认类型)
      - 如果不指定其他类型的话, 默认是x-www-form-urlencoded, 此类型要求参数传递样式为<font color='blue'>key1=value1&key2=value2</font>
          - Flask代码：request.form得到字典
      - ![](https://www.seotest.cn/d/file/news/20190605/20180824110103426.png)
      - ![](https://img2018.cnblogs.com/blog/594801/201910/594801-20191029105138255-1197736174.png)
  2. **json方式**：application/json
      - 更适合传递大数据的形式, 参数样式就是json格式, 例如<font color='blue'>{"key1":"value1","key2":[1,2,3]}</font>等.
          - Flask代码：request.json得到字典
      - ![](https://www.seotest.cn/d/file/news/20190605/20180824110018525.png)
      - ![](https://img2018.cnblogs.com/blog/594801/201910/594801-20191029105052405-1022058048.png)

- GET方式获取地址栏参数
  - Flask代码：request.args得到字典
  - ![](https://img2018.cnblogs.com/blog/594801/201910/594801-20191029105256399-1220928345.png)


## HTTP响应头

- 响应三要素
  - ![](https://img-blog.csdn.net/20180824101548255)

## URL

【2022-3-15】[图解浏览器URL构成](https://www.toutiao.com/w/i1727282566350855)
- ![](https://p6.toutiaoimg.com/img/tos-cn-i-qvj2lq49k0/df328540238d4940ab1e779194f9135a~tplv-obj:1200:673.image?from=post)


## post/get参数获取


- [flask的post,get请求及获取不同格式的参数](https://www.cnblogs.com/leijiangtao/p/11757554.html)
- ![](https://img2018.cnblogs.com/blog/594801/201910/594801-20191029104937449-1769417565.png)

- PostMan界面
  - ![](https://img2018.cnblogs.com/blog/594801/201910/594801-20191029105052405-1022058048.png)


## 状态码

【2021-4-26】HTTP[状态码说明](http://restful.p2hp.com/resources/http-status-codes)，HTTP定义了四十个标准状态代码，可用于传达客户端请求的结果。状态代码分为以下五个类别

| 类别 | 要点       | 描述     | 示例    |
| ---- | ---------- | ---------------------- | ---------------------------- |
| 1xx  | 信息       | 通信传输协议级信息                           | 100（客户端发送请求）         |
| 2xx  | 成功       | 表示客户端的请求已成功接受                   | 200（OK），201（创建），202（已接受），204（无内容），                                                                                                        |
| 3xx  | 重定向     | 表示客户端必须执行一些其他操作才能完成其请求 | 301（永久移动），302（找到），303（见其他），304（未修改），307（临时重定向）                                                                                 |
| 4xx  | 客户端错误 | 此类错误状态代码指向客户端                   | 400（不良请求），401（未经授权），403（禁止），404（未找到），405（不允许的方法），406（不可接受），412（前提条件失败），415（不支持的媒体类型），499（超时） |
| 5xx  | 服务器错误 | 服务器负责这些错误状态代码                   | 500（内部服务器错误），501（未实施）       |




# API

【2022-1-19】API大全：[apishop](https://www.apishop.net/#/)，试用

- **API**(application programming interfaces)，即应用程序编程接口。API由服务器（Server）提供（服务器有各种各样的类型，一般我们浏览网页用到的是web server，即网络服务器），通过API，计算机可以读取、编辑网站数据，就像人类可以加载网页、提交信息等。通俗地，API可以理解为家用电器的插头，用户只提供插座，并执行将插头插入插座的行为，不需要考虑电器内部如何运作。从另外一个角度上讲API是一套协议，规定了与外界的沟通方式：如何发送请求和接受响应。

【2021-11-24】阿里面试题：如何实现幂等
- 实现方式很多，使用过的有例如先放个token在内存或redis中，然后post的时候带上，还得考虑token过期时间，定时删除等。或者根据数据库里的某字段唯一性比如订单告唯一。或者按状态机，使用过spring statemachine来做。我们生产上真实出现过重复创建payment，原因没查出😓当时还做了前端不允许刷新，后退，禁用对应按键。
- 幂等号是指token吗，用的是redis里的key自增。
- 分布式事务常见有xa，tcc或者利用mq来通过中间状态实现最终一致性。xa和bytetcc用在强一致性的场景。原理其实二者差不多都是二阶段提交。xa来说分tm，xa，APP。tm是协调者，程序员通过tm.begin 会开启一个事务放到线程上下文里，当其它的参与者一般是数据库在开事务时候会检查线程上下文是否有事务，如果有就加入。这样一个分布式事务里就含有好多子事务。当调用tm.commit 就会分成二阶段提交。xa使用场景限制在数据库，mq都可以掌控的情况下。而tcc放在分布式情况下，只不过tm从线程上下文变成一个涉及远程通讯的协调者，可以看bytetcc源码。tcc还有局限就是太麻烦。

## 理解RESTful API

- RESTful API即满足RESTful风格设计的API，RESTful表示的是一种互联网软件架构(以网络为基础的应用软件的架构设计)，如果一个架构符合REST原则，就称它为RESTful架构。RESTful架构的特点：
- 每一个URI代表一种资源；
- 客户端和服务器之间，传递这种资源的某种表现层；把"资源"具体呈现出来的形式，叫做它的"表现层"（Representation）。比如，文本可以用txt格式表现，也可以用HTML格式、XML格式、JSON格式表现，甚至可以采用二进制格式；图片可以用JPG格式表现，也可以用PNG格式表现。
- 客户端通过四个HTTP动词，四个表示操作方式的动词：GET、POST、PUT、DELETE。它们分别对应四种基本操作：GET用来获取资源，POST用来新建资源（也可以用于更新资源），PUT用来更新资源，DELETE用来删除资源。

## API设计规范

在移动互联网，分布式、微服务盛行的今天，现在项目绝大部分都采用的微服务框架，**前后端分离**方式,一般系统的大致整体架构图
- ![](https://filescdn.proginn.com/195712466d8fa71a7e933534a00b8251/1e22e56dc3ece99738d7eae1aa78dd1e.webp)

接口交互
- 前端和后端进行交互，前端按照约定请求URL路径，并传入相关参数，后端服务器接收请求，进行业务处理，返回数据给前端。

API与客户端用户的通信协议，总是使用HTTPS协议，以确保交互数据的传输安全。

其它要求：
- **限流**设计、**熔断**设计、**降级**设计，大部分都用不到，当用上了基本上也都在网关中加这些功能。

资料：
- [API接口设计规范](https://cloud.tencent.com/developer/article/1590150)
- [API接口规范](https://www.jianshu.com/p/3f8953f73a79)
- [RESTful API定义及使用规范](https://zhuanlan.zhihu.com/p/31298060)

### path路径规范

域名设计：
- 应该尽量将API部署在专用域名之下：https://api.example.com
- 如果确定API很简单,不会有进一步扩展,可以考虑放在主域名下：https://www.example.com/api

路径又称"终点"（end point），表示API的具体网址。
- 1、在RESTful架构中，每个网址代表一种**资源**(resource),所以网址中不能有动词，只能有**名词**。【所用的名词往往与数据库的**表格名**对应】
- 2、数据库中的表一般都是同种记录的"**集合**"(collection),所以API中的名词也应该使用复数。

例子: 
- https://api.example.com/v1/products
- https://api.example.com/v1/users
- https://api.example.com/v1/employees

| 动作 | 前缀 | 备注|
|---|---|---|
|获取|get|get{XXX}|
|获取|get|get{XXX}List|
|新增|add|add{XXX}|
|修改|update|update{XXX}|
|保存|save| save{XXX}|
|删除|delete| delete{XXX}|
|上传| upload| upload{XXX}|
|发送|send|send{XXX}|

### 版本控制

https://api.example.com/v{n}

- 1、应该将API的版本号放入URL。
- 2、采用多版本并存，增量发布的方式。
- 3、n代表版本号，分为整型和浮点型
  - 整型： 大功能版本，  如v1、v2、v3 ...
  - 浮点型： 补充功能版本， 如v1.1、v1.2、v2.1、v2.2 ...
- 4、对于一个 API 或服务，应在生产中最多保留 3 个最详细的版本

### 请求方式


- GET（SELECT）：    从服务器取出资源（一项或多项）。
- POST（CREATE）：   在服务器新建一个资源。
- PUT（UPDATE）：    在服务器更新资源（客户端提供改变后的完整资源）。
- DELETE（DELETE）： 从服务器删除资源。

例子： 
- GET    /v1/products      获取所有商品
- GET    /v1/products/ID   获取某个指定商品的信息
- POST   /v1/products      新建一个商品
- PUT    /v1/products/ID   更新某个指定商品的信息
- DELETE /v1/products/ID   删除某个商品,更合理的设计详见【9、非RESTful API的需求】
- GET    /v1/products/ID/purchases      列出某个指定商品的所有投资者
- GET    /v1/products/ID/purchases/ID   获取某个指定商品的指定投资者信息

### 请求参数

传入参数分为4种类型：
- 1、cookie：         一般用于OAuth认证
- 2、request header： 一般用于OAuth认证
- 3、请求body数据：
- 4、地址栏参数： 
  - 1）restful 地址栏参数 /v1/products/ID ID为产品编号，获取产品编号为ID的信息
  - 2）get方式的查询字段
详情：
- Query
  - url?后面的参数，存放请求接口的参数数据。
- Header
  - 请求头，存放公共参数、requestId、token、加密字段等。
- Body
  - Body 体，存放请求接口的参数数据。
- 公共参数
  - APP端请求：network（网络：wifi/4G）、operator（运营商：中国联通/移动）、platform（平台：iOS/Android）、system（系统：ios 13.3/android 9）、device（设备型号：iPhone XY/小米9）、udid（设备唯一标识）、apiVersion（API版本号：v1.1）
  - Web端请求：appKey（授权key），调用方需向服务方申请 appKey（请求时使用） 和 secretKey（加密时使用）。
- 安全规范
  - 敏感参数加密处理：登录密码、支付密码，需加密后传输，建议使用 非对称加密。
- 参数命名规范：建议使用**驼峰**命名，首字母小写。
- requestId 建议携带唯一标示追踪问题。

若记录数量很多，服务器不可能返回全部记录给用户。API应该提供分页参数及其它筛选参数，过滤返回结果。
- /v1/products?page=1&pageSize=20     指定第几页，以及每页的记录数。
- /v1/products?sortBy=name&order=asc  指定返回结果按照哪个属性排序，以及排序顺序。


### 返回格式

【2021-11-16】
- [RestFul API 统一响应格式与自动包装](https://zhuanlan.zhihu.com/p/126603159)
- [如何设计 API 接口，实现统一格式返回？](https://jishuin.proginn.com/p/763bfbd6c335)

#### 整体格式

响应结构有两种：
- 方式一：**包装**响应格式，会在真正的响应数据外面包装一层，比如code、message等数据，如果接口报错，响应Status依然为200，更改响应数据里的code。
- 方式二：**不包装**响应数据，需要什么返回什么，如果接口报错，更改响应Status，同时换另一种响应格式，告知前端错误信息。

两种方式各有千秋，这里不谈孰优孰劣

第一种：
- code只是示例，实际项目中应为项目提前定义好的业务异常code，如10025，如果希望得到建议，可以参考另一篇文章异常及错误码定义。
- 这种方式接口响应的Status均为200，使用响应体中的code来区分状态。
无论如何，接口返回响应的数据结构都是一致的。实现提来也比较简单，每个接口都返回统一的一个类型即可，也可以各自返回各自的，再统一进行包装。

```json
// 接口正常
{
    code: 200, 
    msg: null,
    data: {
        "name": "张三",
        "age": 108
    }
}
// 接口异常
{
    code: 500,
    msg: "系统开小差了，稍后再试吧。"
}
```

第二种：不包装响应数据
- 这种方式，成功的时候只返回请求的数据，而失败时才会返回失败信息，两种情况数据结构是不同的，需要通过响应的status来区分。
- 这里的code指的是业务上的**错误码**，并不是简单的http status

```json
// 正常
{
    "name": "张三",
    "age": 108
}
// 异常
{
    code: 500,
    msg: ""
}
```

#### 典型数据字段

后端返回给前端数据, 一般用JSON体方式，定义如下：

```shell
{
    #返回状态码
    code:integer,       
    #返回信息描述
    message:string,
    #返回值
    data:object
}
```

##### （1）code
- code表示返回状态码，一般开发时需要什么，就添加什么。如接口要返回用户权限异常，加一个状态码为101吧，下一次又要加一个数据参数异常，就加一个102的状态码。这样虽然能够照常满足业务，但状态码太凌乱了，可以参考HTTP请求返回的状态码
- ![](https://pic1.zhimg.com/80/v2-675d06d91948b580be09a845eaef869c_720w.jpg)
常见的HTTP状态码：
- 200 - 请求成功
- 301 - 资源（网页等）被永久转移到其它URL
- 404 - 请求的资源（网页等）不存在
- 500 - 内部服务器错误
这样做的好处是就把错误类型归类到某个**区间**内，如果区间不够，可以设计成4位数。
- 1000～1999 区间表示参数错误
- 2000～2999 区间表示用户错误
- 3000～3999 区间表示接口异常
前端开发人员在得到返回值后，根据状态码就可以知道，大概什么错误，再根据message相关的信息描述，可以快速定位。

总结：
- 接口正常访问情况下，服务器返回2××的HTTP状态码；如200一切正常、201资源被创建、204资源被删除
- 当用户访问接口出错时，服务器会返回给一个合适的4××或者5××的HTTP状态码；以及一个application/json格式的消息体，消息体中包含错误码code和错误说明message。
  - 5××错误(500 =< status code)为服务器或程序出错，客户端只需要提示“服务异常，请稍后重试”即可，该类错误不在每个接口中列出。
  - 4××错误(400 =< status code<500)为客户端的请求错误，需要根据具体的code做相应的提示和逻辑处理，message仅供开发时参考，不建议作为用户提示。

##### （2）Message
- 这个字段相对理解比较简单，就是发生错误时，如何友好的进行提示。一般的设计是和code状态码一起设计

##### （3）data
- 需要返回给前端的数据。这个data内的数据一定要是JSON格式，方便前端的解析。

数据常见的有2个大类：
- 业务操作结果
  - 业务操作的过程，能否封装、优化要看实际情况，但是业务操作的最终结果，即最终得到的要返回给前端的数据，可以使用AOP统一封装的前面提到的统一格式中，而不用每次手动封装。
- 参数校验结果
  - 参数的校验如果不使用第三方库，会在代码中多出很多的冗余代码，所以，最好使用oval、hibernate validate或者Spring等参数校验方式，可以大幅度美观代码。

#### 字段

- 数据脱敏
  - 用户手机号、用户邮箱、身份证号、支付账号、邮寄地址等要进行脱敏，部分数据加 * 号处理。
- 属性名命名时，建议使用**驼峰**命名，首字母小写。
- 属性值为空时，严格按类型返回**默认值**。
- 金额类型/时间日期类型的属性值，如果仅用来显示，建议后端返回可以显示的字符串。
- 业务逻辑的状态码和对应的文案，建议后端两者都返回。
- 调用方不需要的属性，不要返回。

| 参数 | 类型| 说明|备注|
|---|---|---|---|
|code|Number|结果码|成功=1失败=-1未登录=401无权限=403|
|showMsg|String|显示信息|系统繁忙，稍后重试|
|errorMsg|String|错误信息|便于研发定位问题|
|data|Object|数据|JSON 格式|

```json
{
    "code": 1,
    "showMsg": "success",
    "errorMsg": "",
    "data": {
        "list": [],
        "pagination": {
            "total": 100,
            "currentPage": 1,
            "prePageCount": 10
        }
    }
}
```

API接口：

```shell
# response：
#----------------------------------------
{
   status: 200,               // 详见【status】

   data: {
      code: 1,                // 详见【code】
      data: {} || [],         // 数据
      message: '成功',        // 存放响应信息提示,显示给客户端用户【须语义化中文提示】
      sysMsg: 'success'       // 存放响应信息提示,调试使用,中英文都行
      ...                     // 其它参数，如 total【总记录数】等
   },

   message: '成功',            // 存放响应信息提示,显示给客户端用户【须语义化中文提示】
   sysMsg: 'success'          // 存放响应信息提示,调试使用,中英文都行
}
#----------------------------------------
【status】:
           200: OK       400: Bad Request        500：Internal Server Error       
                         401：Unauthorized
                         403：Forbidden
                         404：Not Found

【code】:
         1: 获取数据成功 | 操作成功             0：获取数据失败 | 操作失败
```

### 签名设计（权限）

权限分为
- none：无需任何授权；
- token：需要用户登录授权，可通过header Authorization和Cookie CoSID传递；
- admintoken：需要管理员登录授权，可通过header Authorization和Cookie CoCPSID传递；
- token或admintoken：用户登录授权或管理员登录授权都可以；
- sign：需要签名，一般用于服务端内部相互调用，详见 孩宝API HMAC-SHA1签名。

签名验证没有确定的规范，自己制定就行，可以选择使用 对称加密、 非对称加密、 单向散列加密 等，分享下原来写的签名验证，供参考。
- [Go 签名验证](https://mp.weixin.qq.com/s?__biz=MjM5NDM4MDIwNw==&mid=2448835322&idx=1&sn=80d2d77c9c81d63b482b2651fab9a19e&scene=21#wechat_redirect)
- [PHP 签名验证](https://mp.weixin.qq.com/s?__biz=MjM5NDM4MDIwNw==&mid=2448834957&idx=1&sn=fe3c63ad05a2412856892ad790c26fae&scene=21#wechat_redirect)


### 日志平台设计

日志平台有利于故障定位和日志统计分析。

日志平台的搭建可以使用的是 ELK 组件，使用 Logstash 进行收集日志文件，使用 Elasticsearch 引擎进行搜索分析，最终在 Kibana 平台展示出来。

### 幂等性设计

我们无法保证接口的每一次调用都是有返回结果的，要考虑到出现网络异常的情况。
- 举个例子，订单创建时，我们需要去减库存，这时接口发生了超时，调用方进行了重试，这时是否会多扣一次库存？

解决这类问题有 2 种方案：
- 一、服务方提供相应的查询接口，调用方在请求超时后进行查询，如果查到了，表示请求处理成功了，没查到就走失败流程。
- 二、调用方只管重试，服务方保证一次和多次的请求结果是一样的。

对于第二种方案，就需要服务方的接口支持**幂等性**。

大致设计思路是这样的：
- 调用接口前，先获取一个全局唯一的令牌（Token）
- 调用接口时，将 Token 放到 Header 头中
- 解析 Header 头，验证是否为有效 Token，无效直接返回失败
- 完成业务逻辑后，将业务结果与 Token 进行关联存储，设置失效时间
- 重试时不要重新获取 Token，用要上次的 Token

### 非Restful API

- 1、实际业务开展过程中，可能会出现各种的api不是简单的restful 规范能实现的。
- 2、需要有一些api突破restful规范原则。
- 3、特别是移动互联网的api设计，更需要有一些特定的api来优化数据请求的交互。

- 1)、删除单个，批量删除  ： DELETE  /v1/product      body参数{ids：[]}
- 2)、页面级API :  把当前页面中需要用到的所有数据通过一个接口一次性返回全部数据

### 接口文档

- 1、尽量采用**自动化**接口文档，可以做到在线测试，同步更新。
  - 生成的工具为apidoc，详细阅读官方文档：http://apidocjs.com
- 2、应包含：接口BASE地址、接口版本、接口模块分类等。
- 3、每个接口应包含：
  - 接口地址：不包含接口BASE地址。
  - 请求方式: get、post、put、delete等。
  - 请求参数：数据格式【默认JSON、可选form data】、数据类型、是否必填、中文描述。
  - 响应参数：类型、中文描述。

### 测试工具

推荐Chrome浏览器插件`Postman`作为接口测试工具， [Postman下载地址](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop)
- ![](https://pic2.zhimg.com/80/v2-2c3dcfc9251b9d1c2be62d24ba2d5f51_720w.jpg)

### 示例

```python
# !/usr/bin/env python
# -*- coding:utf8 -*- 

# **************************************************************************
# * Copyright (c) 2021 *.com, Inc. All Rights Reserved
# **************************************************************************
# * @file main.py FAQ推荐服务
# * @author ****
# * @date 2021/11/06 10:44
# **************************************************************************

from flask import Flask, request, render_template, make_response, g
from flasgger import Swagger
from functools import wraps # 装饰器工具，用于时间统计
import logging # 日志模块
import os
import json
import time

# flask服务
app = Flask(__name__)
main_dir = '..'
log_path =  '{}/logs/recommend/'.format(main_dir)
# -------- log -------------
if not os.path.exists(log_path):
    # 目录不存在，进行创建操作
    os.makedirs(log_path)
# logging.basicConfig函数对日志的输出格式及方式做相关配置
# 第一步，创建一个logger
logger = logging.getLogger()
logger.setLevel(logging.INFO)  # Log等级总开关
# 第二步，创建一个handler，用于写入日志文件
rq = time.strftime('%Y%m%d%H%M', time.localtime(time.time()))
log_name = log_path + rq + '.log'
logfile = log_name
fh = logging.FileHandler(logfile, mode='w')
fh.setLevel(logging.DEBUG)  # 输出到file的log等级的开关
# 第三步，定义handler的输出格式
formatter = logging.Formatter("%(asctime)s - %(filename)s[line:%(lineno)d] - %(levelname)s: %(message)s")
fh.setFormatter(formatter)
# 第四步，将logger添加到handler里面
logger.addHandler(fh)
# --------- 启动服务 ----------
project_name = 'aionsite_newhouse' # aionsite_newhouse,contract_center,isc_ssc,homespeech_smarthome

# 配置文件主目录
conf_dir = 'infrastructure/engines/legacy'

# swagger api
Swagger(app)

@app.route('/', methods=['GET', 'POST'])
def home():
    # 服务主页
    res = {"msg":"-", "status":0, "req":{} , "resp":{}}
    req_data = {}
    if request.method == 'GET':
        req_data = request.values # 表单形式
    elif request.method == 'POST':
        req_data = request.json # json形式
    else:
        res['msg'] = '异常请求(非GET/POST)'
        res['status'] = -1
    res['req'] = req_data
    res['msg'] = "服务主页"
    return render_template("index.html")

@app.route('/recommend', methods=['GET', 'POST'])
def recommend():
    """
    新房驻场客服问题推荐请求接口
    2021-11-5
    ---
    tags:
      - NLU服务接口
    parameters:
      - name: product
        in: path
        type: string
        required: true
        description: newhouse (新房电子驻场客服）
      - name: project_id
        in: path
        type: string
        required: true
        description: 楼盘id
      - name: city_name
        in: path
        type: string
        required: True
        description: 城市名
      - name: position
        in: path
        type: string
        required: false
        description: 展示位置, detail（盘源详情页）、pop（弹窗页）
      - name: raw_text
        in: path
        type: string
        required: false
        description: 检索词(暂时不用)
    responses:
      500:
        description: 自定义服务端错误
      200:
        description: 自定义状态描述
    """
    # 各业务线接口, product字段区分，status >= 0合法，1截取topn，2补足topn
    res = {"message":"-", "errno":0, "data":{}}
    req_data = {}
    if request.method == 'GET':
        req_data = request.values # 表单形式
    elif request.method == 'POST':
        req_data = request.json # json形式
    else:
        res['message'] = '异常请求(非GET/POST)'
        res['errno'] = -1
    req_dict = {}
    # 参数处理
    for k in ['product', 'project_id', 'city_name', 'position']:
        req_dict[k] = req_data.get(k, "-")
    # 保存请求信息
    #res['req'].update(req_data)
    if req_dict['product'] != 'newhouse':
        res.update({"errno": -2, "message":"product取值无效"})
        return res
    if req_dict['position'] not in ('detail', 'pop'):
        res.update({"errno": -3, "message":"position缺失/取值无效"})
        return res
    # 处理请求信息
    res['data'] = {
            "project_id": 671289,
            "project_name": "天朗·富春原颂", 
            "pv": 230, # 楼盘累计请求数（包含点击、文字、语音所有触发请求的行为）
            "percent": 0.02, # 累计累计请求数占比
            "question_num": 9, # 累计点击问题数
            "click_num": 24, # 累计点击次数，含：更多问题
            "question_list":
            [ # 字段说明：问题id，点击次数，点击占比（分母限该楼盘），楼盘名
                {"pos": 1, "question_id": 8, "click_num": 5, "click_percent": 26.316, "question_name": "项目基本信息"}, 
                {"pos": 2, "question_id": 1, "click_num": 5, "click_percent": 26.316, "question_name": "售楼处地址及接待信息"}, 
                {"pos": 3, "question_id": 2, "click_num": 3, "click_percent": 15.789, "question_name": "在售户型信息？"}, 
                {"pos": 4, "question_id": 6, "click_num": 2, "click_percent": 10.526, "question_name": "此项目激励政策是什么？"}, 
                {"pos": 5, "question_id": 13, "click_num": 1, "click_percent": 5.263, "question_name": "带看规则及注意事项是什么？"}, 
                {"pos": 6, "question_id": 10, "click_num": 1, "click_percent": 5.263, "question_name": "物业信息"}, 
                {"pos": 7, "question_id": 4, "click_num": 1, "click_percent": 5.263, "question_name": "当前有哪些开发商优惠？"}, 
                {"pos": 8, "question_id": 3, "click_num": 1, "click_percent": 5.263, "question_name": "在售楼栋信息？"}
            ],
    }
    res['errno'] = 0
    res['message'] = '请求正常'
    # 后处理：①空值 ②不足5-6个 ③超纲
    # 默认配置信息: detail页(详情页: 8,2,1,28,3,6)，pop页(弹窗页: 8,2,1,3,6)
    default_info = {}
    # 弹窗页默认答案
    default_info['pop'] = [
        {"pos": 1, "question_id": 8, "click_num": 0, "click_percent": 0.0, "question_name": "项目基本信息"}, 
        {"pos": 2, "question_id": 2, "click_num": 0, "click_percent": 0.0, "question_name": "在售户型信息？"},
        {"pos": 3, "question_id": 1, "click_num": 0, "click_percent": 0.0, "question_name": "售楼处地址及接待信息"},
        {"pos": 4, "question_id": 3, "click_num": 0, "click_percent": 0.0, "question_name": "在售楼栋信息？"},
        {"pos": 5, "question_id": 6, "click_num": 0, "click_percent": 0.0, "question_name": "此项目激励政策是什么？"}
    ]
    # 详情页默认答案
    default_info['detail'] = [
        {"pos": 1, "question_id": 8, "click_num": 0, "click_percent": 0.0, "question_name": "项目基本信息"}, 
        {"pos": 2, "question_id": 2, "click_num": 0, "click_percent": 0.0, "question_name": "在售户型信息？"},
        {"pos": 3, "question_id": 1, "click_num": 0, "click_percent": 0.0, "question_name": "售楼处地址及接待信息"},
        {"pos": 4, "question_id": 28, "click_num": 0, "click_percent": 0.0, "question_name": "项目笔记"},
        {"pos": 5, "question_id": 3, "click_num": 0, "click_percent": 0.0, "question_name": "在售楼栋信息？"},
        {"pos": 6, "question_id": 6, "click_num": 0, "click_percent": 0.0, "question_name": "此项目激励政策是什么？"}
    ]
    top_num = len(default_info[req_dict['position']])
    diff_num = res['data']['question_num'] - top_num
    diff_num = -2
    if diff_num > 0:
        res['data']['question_list'] = res['data']['question_list'][:top_num]
        res['errno'] = 1
        res['message'] = '截取TopN'
    elif diff_num == 0:
        pass
    else: # 补默认值
        id_list = [i["question_id"] for i in res['data']['question_list']]
        cnt = 0
        for i in default_info[req_dict['position']]:
            if cnt + diff_num >= 0:
                break
            if i["question_id"] in id_list:
                continue
            i["pos"] = res['data']['question_list'][-1]['pos'] + 1
            res['data']['question_list'].append(i)
            cnt += 1
        res['errno'] = 2
        res['message'] = '补足topN'
    logger.info(json.dumps(res, ensure_ascii=False))
    return res

if __name__ == '__main__':
    #app.run(host='0.0.0.0', port=8089)
    app.run(host='10.200.24.101', port=8092)
    #app.run(host='10.200.24.101', port=8092)
```

## 自动生成APIs文档

- 【2022-1-26】国产[apidoc](https://apidocjs.com/), [github](https://github.com/apidoc/apidoc), [创业公司，在沙河碧水庄园别墅办公，月薪高达10w](https://www.ixigua.com/7056418647228547615)
- 【2020-8-22】[自动为Flask写的API生成帮助文档](https://segmentfault.com/a/1190000013420209)
  - ![](https://segmentfault.com/img/remote/1460000013420214?w=1760&h=1424)
- [使用swagger 生成 Flask RESTful API](https://segmentfault.com/a/1190000010144742)
- [Flask 系列之 构建 Swagger UI 风格的 WebAPI](https://www.cnblogs.com/hippieZhou/p/10848023.html), 基于 Flask 而创建 Swagger UI 风格的 WebAPI 包有很多，如
    - [flasgger](https://github.com/rochacbruno/flasgger)
    - [flask-swagger-ui](https://github.com/sveint/flask-swagger-ui)
    - [swagger-ui-py](https://github.com/PWZER/swagger-ui-py)
    - [flask_restplus](https://www.cnblogs.com/leejack/p/9162367.html)
    - ![](https://img2018.cnblogs.com/blog/749711/201905/749711-20190511131630516-1117259038.png)

### swagger

[Swagger UI](https://swagger.io/tools/swagger-ui/)是一款Restful接口的文档在线自动生成+功能测试功能软件；通过swagger能够清晰、便捷地调试符合Restful规范的API；
- [体验地址](https://petstore.swagger.io/?_ga=2.77229205.1805143947.1643255578-735781494.1643255578#/pet/addPet)
- ![](https://static1.smartbear.co/swagger/media/images/tools/opensource/swagger_ui.png?ext=.png)

#### flasgger

flasgger 配置文件解析：
- 在flasgger的配置文件中，以**yaml格式**描述了flasgger页面的内容；
- **tags标签**中可以放置对这个api的描述和说明；
- **parameters标签**中可以放置这个api所需的参数
  - 如果是GET方法，可以放置url中附带的请求参数
  - 如果是POST方法，可以将参数放置在body参数 **schema子标签**下面；
- responses标签中可以放置返回的信息，以状态码的形式分别列出，每个状态码下可以用schema标签放置返回实体的格式；

- 【2020-8-26】页面测试功能（try it out）对GET/POST无效，传参失败
  - 已提交issue：[Failed to get parameters by POST method in “try it out” feature](https://github.com/flasgger/flasgger/issues/428)
- 【2021-7-19】参考帖子[Parameter in body does not work in pydoc #461](https://github.com/flasgger/flasgger/issues/461)，正确的使用方法：parameter针对url path里的参数，如果启用post需要新增body参数，里面注明post参数信息

- 安装：
    - flask_restplus实践失败，个别依赖不满足，放弃
    - pip install [flasgger](https://github.com/flasgger/flasgger)
- 测试：如下 


```python
# coding:utf8

# **************************************************************************
# * 
# * Copyright (c) 2020, Inc. All Rights Reserved
# * 
# **************************************************************************
# * @file main.py
# * @author wangqiwen
# * @date 2020/08/22 08:32
# **************************************************************************

from flask import Flask, request, render_template, jsonify, request
#from flask_restplus import Api
from flasgger import Swagger, swag_from

app = Flask(__name__)
# swagger api封装，每个接口的注释文档中按照yaml格式排版
Swagger(app)

@app.route('/api/<arg>')
#@app.route("/index",methods=["GET","POST"])
#@app.route("/index/<int,>")
def hello_world():

    """
    API说明
    副标题（点击才能显示）
    ---
    tags:
      - 自动生成示例
    parameters:
      - name: arg # path参数区
        in: path # （1）字段形式，输入框
        type: string
        enum: ['all', 'rgb', 'cmyk'] # 枚举类型
        required: false
        description: 变量含义（如取值 all/rgb/cmyk）
        schema: # 设置默认值，如detail （可省略）
          type: string
          example: rgb
      - name: body  # post 参数区（与get冲突, GET方法下去掉body类型！）
        in: body # （2）body形式，文本框格式
        required: true
        schema:
          id: 用户注册
          required:
            - username
            - password
          properties:
            username:
              type: string
              description: 用户名.
            password:
              type: string
              description: 密码.
            inn_name:
              type: string
              description: 客栈名称.
    responses:
      500:
        description: 自定义服务端错误
      200:
        description: 自定义状态描述
        schema:
          id: awesome
          properties:
            language:
              type: string
              description: The language name
              default: Lua
    """ 
    res = {"code":0, "msg":'-', "data":{}}
    if request.method == 'GET':
        req_data = request.values # 表单形式
    elif request.method == 'POST':
        req_data = request.json # json形式
    else:
        res['msg'] = '异常请求(非GET/POST)'
        res['status'] = -1
    # return jsonify(result) # 方法①
    # return json.dumps(res, ensure_ascii=False) # 方法②
    return render_template('index.html')

@app.route("/tmp",methods=["GET","POST"])
def tmp():
    """
        临时接口
    """
    #return null # 【2022-1-27】swagger上回报错！
    return {} # 返回非空才行，如json或字典格式

if __name__ == '__main__':
    #app.run()
    #app.run(debug=True)
    app.run(debug=True, host='10.26.15.30', port='8044')

# */* vim: set expandtab ts=4 sw=4 sts=4 tw=400: */
```
只要将yaml格式的flasgger描述性程序放置在两组双引号之间的位置，即可实现flasgger的基本功能

flasgger配置文件解析：
- 在flasgger的配置文件中，以yaml的格式描述了flasgger页面的内容；
- tags 标签中可以放置对这个api的描述和说明；
- parameters 标签中可以放置这个api所需的参数，如果是GET方法，可以放置url中附带的请求参数，如果是POST方法，可以将参数放置在 schema 子标签下面；
- responses 标签中可以放置返回的信息，以状态码的形式分别列出，每个状态码下可以用schema标签放置返回实体的格式；

注意
- 以上yaml描述文本可以单独放在文件中，api用@符号引入，示例：

```python
import random
from flask import Flask, jsonify, request
from flasgger import Swagger
from flasgger.utils import swag_from

app = Flask(__name__)
Swagger(app)

@app.route('/api/<string:language>/', methods=['GET'])
@swag_from('index.yml') # 引用yaml描述文档
def index(language):
    language = language.lower().strip()
    features = [
        "awesome", "great", "dynamic", 
        "simple", "powerful", "amazing", 
        "perfect", "beauty", "lovely"
    ]
    size = int(request.args.get('size', 1))
    if language in ['php', 'vb', 'visualbasic', 'actionscript']:
        return "An error occurred, invalid language for awesomeness", 500
    return jsonify(
        language=language,
        features=random.sample(features, size)
    )

app.run(debug=True)
```

![](https://changsiyuan.github.io/images/flasgger/flasgger.png)

flasgger的不足
- flasgger的配置文件中，对于POST方法，在描述POST body的 schema 标签中，不支持以yaml格式描述的数组或嵌套的object，这使得页面上面无法显示这类POST body的example；
- 解决方案：将这类POST body的example放置在 description 部分（三横杠”—“上面的部分），由于 description 部分是用html格式解析的，所以可以以html的语法编写；

[python--Flasgger使用心得](https://changsiyuan.github.io/2017/05/20/2017-5-20-flasgger/)

## API 性能指标

【2022-7-25】[一文详解吞吐量、QPS、TPS、并发数等高并发大流量指标](https://www.toutiao.com/article/7123847014781141518)

阿里双11高并发场景经常提到QPS、TPS、RT、吞吐量等指标，这些高并发高性能指标都是什么含义？如何来计算？

### 系统吞吐量

系统吞吐量指的是系统在单位时间内可处理的事务的数量，是用于衡量系统性能的重要指标。

例如在网络领域，某网络的系统吞吐量指的是单位时间内通过该网络成功传递的消息包数量。

举一个生活中的例子，一说就懂，比如：成都双流国际机场年旅客吞吐量达4011.7万人次，这里的系统单位时间就是年，完成的数量这里就是飞行人数。

上面谈到的是机场的吞吐量，而系统吞吐量指的是系统(比如服务器)在单位时间内可处理的事务的数量，是一个评估系统承受力的重要指标。

系统吞吐量有几个重要指标参数：
- QPS
- TPS
- 响应时间
- 并发数

### QPS 每秒查询量

QPS(Queries Per Second)：大家最熟知的就是QPS，这里我就不多说了，简要意思就是“每秒查询率”，是一台服务器每秒能够相应的查询次数，是对一个特定的查询服务器在规定时间内所处理流量多少的衡量标准。

### TPS 每秒处理量

TPS(Transactions Per Second)：意思是每秒钟系统能够处理的**交易**或**事务**的数量，它是衡量系统处理能力的重要指标。

具体事务的定义都是人为的，可以一个接口、多个接口、一个业务流程等等。

举一个例子，比如在web性能测试中，一个事务是指事务内第一个请求发送到接收到最后一个请求的响应的过程，以此来计算使用的时间和完成的事务个数。

以单接口定义为事务为例，每个事务包括了如下3个过程：
- a. 向服务器发请求
- b. 服务器自己的内部处理（包含应用服务器、数据库服务器等）
- c. 服务器返回结果给客户端。

总结，在web性能测试中一个事务表示“从用户发送请求->web server接受到请求，进行处理-> web server向DB获取数据->生成用户的object(页面)，返回给用户”的过程。

怎么计算TPS的呢？
- 举一个最简单的例子，如果每秒能够完成100次上面这三个过程，那TPS就是100。
- 一般的，评价系统性能均以每秒钟完成的技术交易的数量来衡量。

比如大家熟知的阿里双11，‍一秒峰值完成58.3万笔订单，这样就量化了系统处理高并发的重要指标。

QPS与TPS的区别
- 上面分别谈完了QPS与TPS，我们再来看看两者有什么区别呢？
- 假如对于一个页面的一次访问算一个TPS，但一次页面请求，可能产生N次对服务器的请求，服务器对这些请求，就可计入QPS之中，即QPS=N*TPS。

又假如对一个查询接口（单场景）压测，且这个接口内部不会再去请求其它接口，那么TPS=QPS。

### RT响应时间

RT（Response-time）响应时间：执行一个请求从开始到最后收到响应数据所花费的总体时间，即从客户端发起请求到收到服务器响应结果的时间。

该请求可以是任何东西，从内存获取，磁盘IO，复杂的数据库查询或加载完整的网页。

暂时忽略传输时间，响应时间是处理时间和等待时间的总和,处理时间是完成请求要求的工作所需的时间，等待时间是请求在被处理之前必须在队列中等待的时间。

响应时间是一个系统最重要的指标之一，它的数值大小直接反应了系统的快慢。

并发数Concurrency
一文详解吞吐量、QPS、TPS、并发数等高并发大流量指标
并发数是指系统同时能处理的请求数量，这个也反应了系统的负载能力。

并发，指的是多个事情，在同一段时间段内发生了，大家都在争夺统一资源。

比如：当有多个线程在操作时，如果系统只有一个 CPU，则它根本不可能真正同时进行一个以上的线程，它只能把 CPU 运行时间划分成若干个时间段，再将时间段分配给各个线程执行，在一个时间段的线程代码运行时,其它线程处于挂起状态，这种方式我们称之为并发（Concurrent）。
- ![](https://p3.toutiaoimg.com/origin/tos-cn-i-qvj2lq49k0/f50ef8a818c4459a984cd02dee46277f?from=pc)

【2022-8-5】[性能之巅-优化你的程序](https://www.toutiao.com/article/6725742697404957191/)
- outline：关注&指标&度量，基础理论知识，工具&方法，最佳实践，参考资料

性能优化关注：CPU、内存、磁盘IO、网络IO等四个方面
- CPU：
  - 处理器、核心
  - 超线程、多线程
  - 协程
  - 调度、上下文切换
- 内存
  - 虚拟内存、地址空间
  - 分页、换页
  - 总线
  - 分配器、对象池
- 磁盘IO
  - 缓存
  - 控制器
  - 交换分区
  - 磁盘文件映射
- 网络IO
  - 套接字、连接
  - 网卡和中断、
  - 协议、tcp/udp
  - select/epoll
- ![](https://p3-sign.toutiaoimg.com/pgc-image/76ee0b5d28f44d079bf476600ca1beb4~noop.image?_iz=58558&from=article.pc_detail&x-expires=1660284418&x-signature=PiFiq5KLvPGO%2FxRq0TV7FJPVYdQ%3D)

性能指标：经常关注的指标
- 吞吐率
  - 吞吐量（throughtput）：评价工作执行的速率，每秒操作量
- 响应时间（RT）、延迟（latency）
  - 一次操作完成的时间，包括等待+服务时间，也包括返回结果，req-resp延时，延迟跟吞吐量相关联，吞吐量增大，延迟会升高
- QPS/IOPS
  - 并发数：同时能处理的连接数
  - QPS：每秒查询数
  - IOPS：每秒i/o操作量
- TP99
  - 响应时间的分布，通常关心的是top 90
- 资源使用率
  - 资源使用比例，繁忙程度；CPU、存储和网络贷款
​
时间度量：从cpu cycle到网络IO，自上到下，时间量级越大。
- 网络i/o：内网（几ms），公网（几时ms）
- 磁盘i/o：1-10ms
- 固态硬盘i/o（山村）：50-150us
- CPU访问主存DRAM：100ns
- L1-L3 Cache：1-20ns
- CPU cycle：0.2-0.3 ns
- ![](https://p3-sign.toutiaoimg.com/pgc-image/18b287bf905f4d64a2f1fd91f56e5b40~noop.image?_iz=58558&from=article.pc_detail&x-expires=1660284418&x-signature=Va6MphOEt9OvSU6UdPSjiffqJ9w%3D)

注：
- 1s = 10^3 ms（毫秒） = 10^6 us（微秒） = 10^9 ns（纳秒） = 10^10 ps（皮秒）

性能分析工具
- ![](https://p3-sign.toutiaoimg.com/pgc-image/7c6b1a9cda24431993cea2182fec0180~noop.image?_iz=58558&from=article.pc_detail&x-expires=1660284418&x-signature=9QEepr%2FTP2P3UGltReSOJGGUh7I%3D)

# SDK

SDK是什么
- SDK全称software development kit，软件开发工具包。第三方服务商提供的实现产品软件某项功能的工具包
- 一般都是一些软件工程师为特定的软件包、软件框架、硬件平台、操作系统等建立应用软件时的开发工具的集合。


- API是一个函数，有其特定的功能；而SDK是一个很多功能函数的集合体，一个工具包。
- API是数据接口，SDK相当于开发集成工具环境，要在SDK的环境下来调用API。
- API接口对接过程中需要的环境需要自己提供，SDK不仅提供开发环境，还提供很多API。
- 简单功能调用，API调用方便快捷；复杂功能调用，SDK功能齐全。

# RPC

资料
- 【2020-12-25】[为啥需要RPC，而不是简单的HTTP？](https://www.toutiao.com/i6898582988620202500/)
- 【2021-11-24】[1万行代码，单机50万QPS，今年最值得学习的开源RPC框架！](http://it.taocms.org/11/94072.htm)

企业开发的模式一直定性为**HTTP接口**开发，即常说的 RESTful 风格的服务接口。对于接口不多、系统与系统交互较少的情况下，解决信息孤岛初期常使用的一种通信手段；
- 优点：简单、直接、开发方便。利用现成的http协议进行传输。要写一大份接口文档，严格地标明输入输出是什么，说清楚每一个接口的请求方法，以及请求参数需要注意的事项等。
- 但是对于大型企业来说，**内部子系统较多、接口非常多**的情况下，RPC框架的好处就显示出来了
  - 首先，**长链接**，不必每次通信都要像http一样去3次握手什么的，减少了网络开销；
  - 其次，RPC框架一般都有**注册中心**，有丰富的监控管理；
  - 发布、下线接口、动态扩展等，对调用方来说是**无感知**、统一化的操作。

## 什么是RPC

- [什么是RPC](https://www.jianshu.com/p/7d6853140e13)

|调用类型|过程| 代码 |示意图| 备注 |
|---|---|---|--——| --- |
| 本地函数调用 | 传参→本地函数代码→执行→返回结果| int result = Add(1, 2); | ![](http://cdn1.taocms.org/imgpxy.php?url=gnp%3Dtmf_xw%3F046%2FA4JAWAtPcfAaM6mUOrOJtvOA29yCSf7ciISf1Fccln8svRpUwftH6VbDxxRbifkHGL95EQ6UrM431yOYhkcxzerY%2Fgnp_zibmm_zs%2Fnc.cipq.zibmm%2F%2F%3Asptth) | 所有动作发生同一个进程空间 |
| 远程过程调用 | 传参→远程调用→远程执行→返回结果 | int result = Add(1, 2);（socket通信） |![](http://cdn1.taocms.org/imgpxy.php?url=gnp%3Dtmf_xw%3F046%2FQMTbiMcucicJlpTXIbigNciUc0rVf7I0psdaYGsMbi2mjCdr7M6nsVAG4h1DxxRbifkHGL95EQ6UrM431yOYhkcxzerY%2Fgnp_zibmm_zs%2Fnc.cipq.zibmm%2F%2F%3Asptth) | 跨进程、跨服务器 | 


RPC（Remote Procedure Call）**远程过程调用**，简单的理解是一个节点请求另一个节点提供的服务
- **本地过程调用**：如果需要将本地student对象的age+1，可以实现一个addAge()方法，将student对象传入，对年龄进行更新之后返回即可，本地方法调用的函数体通过函数指针来指定。
- **远程过程调用**：上述操作的过程中，如果addAge()这个方法在服务端，执行函数的函数体在远程机器上，如何告诉机器需要调用这个方法呢？
  - 1.首先客户端需要告诉服务器，需要调用的函数，这里函数和进程ID存在一个映射，客户端远程调用时，需要查一下函数，找到对应的ID，然后执行函数的代码。
  - 2.客户端需要把本地参数传给远程函数，本地调用的过程中，直接压栈即可，但是在远程调用过程中不再同一个内存里，无法直接传递函数的参数，因此需要客户端把参数转换成字节流，传给服务端，然后服务端将字节流转换成自身能读取的格式，是一个序列化和反序列化的过程。
  - 3.数据准备好了之后，如何进行传输？网络传输层需要把调用的ID和序列化后的参数传给服务端，然后把计算好的结果序列化传给客户端，因此TCP层即可完成上述过程，gRPC中采用的是HTTP2协议。

总结
- Client端 ：Student student = Call(ServerAddr, addAge, student)
  - 1. 将这个调用映射为Call ID。
  - 2. 将Call ID，student（params）序列化，以二进制形式打包
  - 3. 把2中得到的数据包发送给ServerAddr，这需要使用网络传输层
  - 4. 等待服务器返回结果
  - 5. 如果服务器调用成功，那么就将结果反序列化，并赋给student，年龄更新
- Server端
  - 1. 在本地维护一个Call ID到函数指针的映射call_id_map，可以用Map<String, Method> callIdMap
  - 2. 等待服务端请求
  - 3. 得到一个请求后，将其数据包反序列化，得到Call ID
  - 4. 通过在callIdMap中查找，得到相应的函数指针
  - 5. 将student（params）反序列化后，在本地调用addAge()函数，得到结果
  - 6. 将student结果序列化后通过网络返回给Client

- 图示
    - ![](https://upload-images.jianshu.io/upload_images/7632302-ca0ba3118f4ef4fb.png)
- 微服务的设计中，一个服务A如果访问另一个Module下的服务B，可以采用HTTP REST传输数据，并在两个服务之间进行序列化和反序列化操作，服务B把执行结果返回过来。
- 由于HTTP在应用层中完成，整个通信的代价较高，远程过程调用中直接基于TCP进行远程调用，数据传输在传输层TCP层完成，更适合对效率要求比较高的场景，RPC主要依赖于客户端和服务端之间建立Socket链接进行，底层实现比REST更复杂。
- ![](https://upload-images.jianshu.io/upload_images/7632302-19ad38cdd9a4b3ec.png)

## RPC框架

### 为什么需要RPC框架呢？

如果没有统一的RPC框架，各个团队的服务提供方就需要各自实现一套序列化、反序列化、网络框架、连接池、收发线程、超时处理、状态机等“业务之外”的重复技术劳动，造成整体的低效。

RPC框架的职责，就是要屏蔽各种复杂性：
- （1）调用方client感觉就像调用本地函数一样，来调用服务；
- （2）提供方server感觉就像实现一个本地函数一样，来实现服务；

### 什么时候需要RPC

- RPC通信方式，已经不仅仅是远程，这个远程就是指不在一个进程内，只能通过其他协议来完成，通常都是TCP或者是Http。
- 希望是和在同一个进程里，一致的体验
- http做不到，Http（TCP）本身的三次握手协议，就会带来大概1MS的延迟。每发送一次请求，都会有一次建立连接的过程，加上Http报文本身的庞大，以及Json的庞大，都需要作一些优化。
- 一般的场景下，没什么问题，但是对于Google这种级别的公司，他们接受不了。几MS的延迟可能就导致多出来几万台服务器，所以他们想尽办法去优化，优化从哪方面入手？
  - 1.减少传输量。
  - 2.简化协议。
  - 3.用长连接，不再每一个请求都重新走三次握手流程
- Http的协议就注定了，在高性能要求的下，不适合用做线上分布式服务之间互相使用的通信协议。
- RPC服务主要是针对大型企业的，而HTTP服务主要是针对小企业的，因为RPC效率更高，而HTTP服务开发迭代会更快。

### RPC基本组件

一个完整的RPC架构里面包含了四个核心的组件，分别是Client ,Server,Client Stub以及Server Stub，这个Stub大家可以理解为存根。分别说说这几个组件：
- 客户端（Client），服务的调用方。
- 服务端（Server），真正的服务提供者。
- 客户端存根，存放服务端的地址消息，再将客户端的请求参数打包成网络消息，然后通过网络远程发送给服务方。
- 服务端存根，接收客户端发送过来的消息，将消息解包，并调用本地的方法。
- ![](https://p3-tt.byteimg.com/origin/pgc-image/28f3cdf8370647f9a2966b4bf352e52b?from=pc)

### 常见RPC框架

有哪些常见的，出圈的RPC框架呢？
- （1）gRPC，Google出品，支持多语言；
- （2）Thrift，Facebook出品，支持多语言；
- （3）Dubbo，阿里开源的，支持Java；
- （4）bRPC，百度开源的，支持C++，Java；
- （5）tRPC，腾讯RPC框架，支持多语言；
- （6）srpc，作者是搜狗的媛架构师liyingxin，基于WF，代码量1W左右：
  - ① 非常适合用来学习RPC的架构设计；
  - ② 又是一个工业级的产品，QPS可以到50W，应该是行业能目前性能最好的RPC框架了吧，有不少超高并发的线上应用都使用它。
- （7）。。。

### gRPC与REST

- REST通常以业务为导向，将业务对象上执行的操作映射到HTTP动词，格式非常简单，可以使用浏览器进行扩展和传输，通过JSON数据完成客户端和服务端之间的消息通信，直接支持请求/响应方式的通信。不需要中间的代理，简化了系统的架构，不同系统之间只需要对JSON进行解析和序列化即可完成数据的传递。
- 但是REST也存在一些弊端，比如只支持请求/响应这种单一的通信方式，对象和字符串之间的序列化操作也会影响消息传递速度，客户端需要通过服务发现的方式，知道服务实例的位置，在单个请求获取多个资源时存在着挑战，而且有时候很难将所有的动作都映射到HTTP动词。
- 正是因为REST面临一些问题，因此可以采用gRPC作为一种替代方案
- gRPC 是一种基于**二进制流**的消息协议，可以采用基于**Protocol Buffer**的IDL定义grpc API,这是Google公司用于序列化结构化数据提供的一套语言中立的序列化机制，客户端和服务端使用HTTP/2以Protocol Buffer格式交换二进制消息。
- gRPC的优势是，设计复杂更新操作的API非常简单，具有高效紧凑的进程通信机制，在交换大量消息时效率高，远程过程调用和消息传递时可以采用双向的流式消息方式，同时客户端和服务端支持多种语言编写，互操作性强；
- 不过gRPC的缺点是不方便与JavaScript集成，某些防火墙不支持该协议。
- 注册中心：当项目中有很多服务时，可以把所有的服务在启动的时候注册到一个注册中心里面，用于维护服务和服务器之间的列表，当注册中心接收到客户端请求时，去找到该服务是否远程可以调用，如果可以调用需要提供服务地址返回给客户端，客户端根据返回的地址和端口，去调用远程服务端的方法，执行完成之后将结果返回给客户端。这样在服务端加新功能的时候，客户端不需要直接感知服务端的方法，服务端将更新之后的结果在注册中心注册即可，而且当修改了服务端某些方法的时候，或者服务降级服务多机部署想实现负载均衡的时候，我们只需要更新注册中心的服务群即可。
- ![](https://upload-images.jianshu.io/upload_images/7632302-0b09dd85b8baa318.png)

### thrift

- [thrift c++ rpc](https://www.cnblogs.com/Forever-Kenlen-Ja/p/9649724.html)
- 【2020-12-26】thrift是Facebook开源的一套rpc框架，目前被许多公司使用
    - 使用IDL语言生成多语言的实现代码，程序员只需要实现自己的业务逻辑
    - 支持序列化和反序列化操作，底层封装协议，传输模块
    - 以同步rpc调用为主，使用libevent evhttp支持http形式的异步调用
    - rpc服务端线程安全，客户端大多数非线程安全
    - 相比protocol buffer效率差些，protocol buffer不支持rpc，需要自己实现rpc扩展，目前有grpc可以使用
    - 由于thrift支持序列化和反序列化，并且支持rpc调用，其代码风格较好并且使用方便，对效率要求不算太高的业务，以及需要rpc的场景，可以选择thrift作为基础库
![](https://img2018.cnblogs.com/blog/524932/201809/524932-20180915020117562-1191051189.png)


### sRPC

【2021-11-24】[1万行代码，单机50万QPS，今年最值得学习的开源RPC框架！](http://it.taocms.org/11/94072.htm)
- [github地址](https://github.com/sogou/srpc)，作者[知乎](https://www.zhihu.com/people/liyingxin1412/posts)

#### 什么是srpc？

- 基于WF的轻量级，超高性能，工业级RPC框架，兼容多协议，例如百度bRPC，腾讯tRPC，Google的gRPC，以及FB的thrift协议。

#### srpc特点

srpc有些什么特点？
- （1）支持多种IDL格式，包括Protobuf，Thrift等，对于这类项目，可以一键迁移；
- （2）支持多种序列化方式，包括Protobuf，Thrift，json等；
- （3）支持多压缩方法，对应用透明，包括gzip，zlib，lz4，snappy等；
- （4）支持多协议，对应用透明，包括http，https，ssl，tcp等；
- （5）高性能；不同客户端线程压力下的性能表现非常稳定，QPS在50W左右，优于同等压测配置的bRPC与thrift。
  - ![](http://cdn1.taocms.org/imgpxy.php?url=gnp%3Dtmf_xw%3F046%2FgXKBtRFZZne43bDPrPX1MRzprUHQqfyBH2rOtM10b9hL3t4JdGxTVlDxxRbifkHGL95EQ6UrM431yOYhkcxzerY%2Fgnp_zibmm_zs%2Fnc.cipq.zibmm%2F%2F%3Asptth)
- （6）轻量级，低门槛，1W行左右代码，只需引入一个静态库；

#### 设计思路

srpc的架构设计思路是怎样的？

作为一个RPC框架，srpc的架构是异常清晰的，用户需要关注这3个层次：
- （1）IDL接口描述文件层；
- （2）RPC序列化协议层；
- （3）网络通讯层；

同时，每一层次又提供了多种选择，用户可以任意的组合
- ![](http://cdn1.taocms.org/imgpxy.php?url=gnp%3Dtmf_xw%3F046%2FweBRbi95Ko72pjseO1IXggym7TYHnQPtz04CuPci3QTgHeykEpciyIKsNDxxRbifkHGL95EQ6UrM431yOYhkcxzerY%2Fgnp_zibmm_zs%2Fnc.cipq.zibmm%2F%2F%3Asptth)
- （1）IDL层，用户可以选择Protobuf或者Thrift；
- （2）协议层，可以选择Thrift，bRPC，tRPC等；
画外音：因此，才能和其他RPC框架无缝互通。
- （3）通信层，可以选择tcp或者http；

RPC的客户端要做什么工作，RPC的服务端要做什么工作，srpc框架又做了什么工作呢？

首先必须在IDL中要定义好：
- （1）逻辑请求包request；
- （2）逻辑响应包response；
- （3）服务接口函数method；
- ![](http://cdn1.taocms.org/imgpxy.php?url=gnp%3Dtmf_xw%3F046%2FwarKvQllah52wJ8UciZaiPGcgVUR0vyssGCJutjtLVPeJ8jWqy1Qlq5YDxxRbifkHGL95EQ6UrM431yOYhkcxzerY%2Fgnp_zibmm_zs%2Fnc.cipq.zibmm%2F%2F%3Asptth)

RPC-client的工作就异常简单了：
- （1）调用method；
- （2）绑定回调函数，处理回调；
对应上图中顶部方框的**绿色**部分。

RPC-server的工作也非常简单，像实现一个本地函数一样，提供远程的服务：
- （1）实现method；
- （2）接受request，逻辑处理，返回response；
对应上图中底部方框的**黄色**部分。

srpc框架完成了绝大部分的工作：
- （1）对request序列化，压缩，处理生成二进制报文；
- （2）连接池，超时，任务队列，异步等处理；
- （3）对request二进制报文处理，解压缩，反序列化；
对应上图中中间的方框的**红色**部分，以及大部分流程。

在这个过程中，srpc采用插件化设计，各种复杂性细节，对接口调用方与服务提供方，都是透明的，并且具备良好的扩展性。
- ![](http://cdn1.taocms.org/imgpxy.php?url=gnp%3Dtmf_xw%3F046%2FAHBbiQaiZSATQVPSEHrghOWhdBNLaima3b67OzKRqIkGGkVzBpDujwl51DxxRbifkHGL95EQ6UrM431yOYhkcxzerY%2Fgnp_zibmm_zs%2Fnc.cipq.zibmm%2F%2F%3Asptth)

另外，定义好IDL之后，服务端的代码可以利用框架提供的工具自动生成代码，业务服务提供方，只需要专注于业务接口的实现即可，你说帅气不帅气？
画外音：具体的生成工具，与生成方法，请参看git上的文档。

最后，我觉得这个srpc最帅气的地方之一，就是：开源版本即线上工程版本，更新快，issue响应快，并且文档真的很全！
画外音：不少大公司，公司内部的版本和开源的版本是两套代码，开源版本没有文档，KPI完成之后，开源就没人维护了，结果坑了一大票人。

#### 如何快速上手

三个步骤
- 第一步：定义IDL描述文件。
- 第二步：生成代码，并实现ServiceIMPL，server端就搞定了。
- 第三步：自己定义一个请求客户端，向服务端发送echo请求。

代码：

```c++
// (1) 第一步：定义IDL描述文件
syntax = "proto3";// proto2 or proto3
message EchoRequest {
   string message = 1;
   string name = 2;

};

message EchoResponse {
   string message = 1;

};

service Example {
   rpc Echo(EchoRequest) returns (EchoResponse);

};

// (2) 第二步：生成代码，并实现ServiceIMPL，server端就搞定了
class ExampleServiceImpl : public Example::Service
{
public
   void Echo(EchoRequest *request,
        EchoResponse *response,
        RPCContext *ctx) override
    {
       response->set_message("Hi, " + request->name());
    }
};

// (3) 第三步：自己定义一个请求客户端，向服务端发送echo请求。
int main()
{
   Example::SRPCClient client("127.0.0.1", 1412);
   EchoRequest req;
   req.set_message("Hello, srpc!");
   req.set_name("zhangsan");
   client.Echo(&req, 
        [](EchoResponse *response, RPCContext *ctx){});
   return 0;
}
```

# GraphQL

## GraphQL简介
  - GraphQL是一种新的API标准，它提供了一种比REST更有效、更强大和更灵活的替代方案。
- Facebook开发并开源的，现在由来自世界各地的公司和个人组成的大型社区维护。
- GraphQL本质上是一种**基于api的查询语言**，现在大多数应用程序都需要从服务器中获取数据，这些数据存储可能存储在数据库中，API的职责是提供与应用程序需求相匹配的存储数据的接口。
- 数据库无关的，而且可以在使用API的任何环境中有效使用，GraphQL是基于API之上的一层封装，目的是为了更好，更灵活的适用于业务的需求变化。
- 总结
  - 强大的API查询语言
  - 客户端服务器间通信中介
  - 比Restful API更高效、灵活

## GraphQL 对比 REST API 

- 【2021-2-6】总结

| 维度     | Restful API                    | GraphQL                                                                                                     |
| -------- | ------------------------------ | ----------------------------------------------------------------------------------------------------------- |
| 接口     | 接口灵活性差、操作流程繁琐     | 声明式数据获取，接口数据精确返回，查询流程简洁，照顾了客户端的灵活性                                        |
| 扩展性   | 不断编写新接口（依赖于服务端） | 一个服务仅暴露一个 GraphQL 层，消除了服务器对数据格式的硬性规定，客户端按需请求数据，可进行单独维护和改进。 |
| 传输协议 | HTTP协议，不能灵活选择网络协议 | 传输层无关、数据库技术无关，技术栈选择更灵活                                                                |

## 介绍
- 举例说明：前端向后端请求一个book对象的数据及其作者信息。
- REST API动图演示：
  - ![](https://pic4.zhimg.com/v2-c9260410f4c294c8e0e4ce94d4ac6767_b.webp)
- GraphQL动图演示：
  - ![](https://pic1.zhimg.com/v2-6a2d8af7087b156cf3dde52ccf5d7d08_b.webp)
- 与REST多个endpoint不同，每一个的 GraphQL 服务其实对外只提供了一个用于调用内部接口的端点，所有的请求都访问这个暴露出来的唯一端点。
  - ![](https://pic2.zhimg.com/80/v2-6c849555869fbd25ceb69e2530273949_720w.jpg)
- GraphQL 实际上将多个 HTTP 请求聚合成了一个请求，将多个 restful 请求的资源变成了一个从根资源 POST 访问其他资源的 Comment 和 Author 的图，多个请求变成了一个请求的不同字段，从原有的**分散式**请求变成了**集中式**的请求，因此GraphQL又可以被看成是**图数据库**的形式。
  - ![](https://pic4.zhimg.com/80/v2-4efc7e2a78697e086b5bceec3f82b3c7_720w.jpg)

- GraphQL的核心概念：**图表模式**（Schema）
- GraphQL设计了一套Schema模式（可以理解为语法），其中最重要的就是数据类型的定义和支持。类型（Type）就是模式（Schema）最核心的东西
- 什么是类型？
  - 对于数据模型的抽象是通过类型（Type）来描述的，每一个类型有若干字段（Field）组成，每个字段又分别指向某个类型（Type）。这很像Java、C#中的类（Class）。
  - GraphQL的Type简单可以分为两种，一种叫做Scalar Type(标量类型)，另一种叫做Object Type(对象类型)。

## GraphQL特点总结

- **声明式数据获取**（可以对API进行查询）: 声明式的数据查询带来了接口的精确返回，服务器会按数据查询的格式返回同样结构的 JSON 数据、真正照顾了客户端的灵活性。
- 一个微服务仅暴露**一个 GraphQL 层**：一个微服务只需暴露一个GraphQL endpoint，客户端请求相应数据只通过该端点按需获取，不需要再额外定义其他接口。
- **传输层无关、数据库技术无关**：带来了更灵活的技术栈选择，比如我们可以选择对移动设备友好的协议，将网络传输数据量最小化，实现在网络协议层面优化应用。

## GraphQL接口设计

- GraphQL获取数据三步骤
  - 首先要设计数据模型，用来描述数据对象，它的作用可以看做是VO，用于告知GraphQL如何来描述定义的数据，为下一步查询返回做准备；
  - 前端使用模式查询语言（Schema）来描述需要请求的数据对象类型和具体需要的字段（称之为声明式数据获取）；
  - 后端GraphQL通过前端传过来的请求，根据需要，自动组装数据字段，返回给前端。
- ![](https://pic4.zhimg.com/v2-4bafe3f3e71c4b06d58b9d5b556df6df_b.webp)
- 设计思想：GraphQL 以图的形式将整个 Web 服务中的资源展示出来，客户端可以按照其需求自行调用，类似添加字段的需求其实就不再需要后端多次修改了。
- 创建GraphQL服务器的最终目标是：允许查询通过图和节点的形式去获取数据。
  - ![](https://pic2.zhimg.com/80/v2-942b7a4fbc8c8e5dcd016bc072895a9d_720w.jpg)

## GraphQL支持的数据操作
- GraphQL对数据支持的操作有：
  - 查询（Query）：获取数据的基本查询。
  - 变更（Mutation）：支持对数据的增删改等操作。
  - 订阅（Subscription）：用于监听数据变动、并靠websocket等协议推送变动的消息给对方。

## GraphQL执行逻辑

- 有人会问：
  - 使用了GraphQL就要完全抛弃REST了吗？
  - GraphQL需要直接对接数据库吗？
  - 使用GraphQL需要对现有的后端服务进行大刀阔斧的修改吗？
- 答案是：NO！不需要！
- 它完全可以以一种不侵入的方式来部署，将它作为前后端的中间服务，也就是，现在开始逐渐流行的 前端 —— 中端 —— 后端 的三层结构模式来部署！
- 那就来看一下这样的部署模式图：
  - ![](https://pic3.zhimg.com/80/v2-bd8655c1d1d472088ae593674491df12_720w.jpg)
- 完全可以搭建一个GraphQL服务器，专门来处理前端请求，并处理后端服务获取的数据，重新进行组装、筛选、过滤，将完美符合前端需要的数据返回。
- 新的开发需求可以直接就使用GraphQL服务来获取数据了，以前已经上线的功能无需改动，还是使用原有请求调用REST接口的方式，最低程度的降低更换GraphQL带来的技术成本问题！
- 如果没有那么多成本来支撑改造，那么就不需要改造！
- 只有当原有需求发生变化，需要对原功能进行修改时，就可以换成GraphQL了。

## GraphQL服务框架：

- 框架
  - Apollo Engine:一个用于监视 GraphQL 后端的性能和使用的服务。
  - Graphcool(github): 一个 BaaS（后端即服务），它为你的应用程序提供了一个 GraphQL 后端，且具有用于管理数据库和存储数据的强大的 web ui。
  - Tipe (github): 一个 SaaS（软件即服务）内容管理系统，允许你使用强大的编辑工具创建你 的内容，并通过 GraphQL 或 REST API 从任何地方访问它。
  - AWS AppSync：完全托管的 GraphQL 服务，包含实时订阅、离线编程和同步、企业级安全特性以及细粒度的授权控制。
  - Hasura：一个 BaaS（后端即服务），允许你在 Postgres 上创建数据表、定义权限并使用 GraphQL 接口查询和操作。
- 工具
  - graphiql (npm): 一个交互式的运行于浏览器中的 GraphQL IDE。
  - Graphql Language Service: 一个用于构建 IDE 的 GraphQL 语言服务（诊断、自动完成等） 的接口。
  - quicktype (github): 在 TypeScript、Swift、golang、C#、C++ 等语言中为 GraphQL 查 询生成类型。
- 想要获取更多关于Graphql的一些框架、工具，可以去awesome-graphql：一个神奇的社区，维护一系列库、资源等。更多Graphql的知识，可以去http://GraphQL.cn

# LAMP

LAMP 环境搭建指的是在 **Linux** 操作系统中分别安装 **Apache** 网页服务器、**MySQL** 数据库服务器和 **PHP** 开发服务器，以及一些对应的扩展软件。
- ![](https://lamp.sh/usr/uploads/lamp.png)
LAMP 环境是当前极为流行的搭建动态网站的开源软件系统，拥有良好的稳定性及兼容性。而且随着开源软件的蓬勃发展，越来越多的企业和个人选择在 LAMP 开发平台上搭建自己的网站。
- LAMP占全球网站总数的 52.19％（2013 年 7 月数据），其余的网站平台（如 Microsoft IIS 开发平台、Linux Nginx 开发平台、Google 开发平台等）占用了剩余的份额。
- LNMP环境中，使用 **Nginx** 网页服务器取代了 Apache 网页服务器。Nginx 是一款高性能的 HTTP 网页服务器和反向代理服务器，它的执行效率极高，配置相比 Apache 也较为简单，所以在短时间内被国内外很多大型公司所采用，大有取代 Apache 的势头（目前还是以 Apache 为主流的）。
- WAMP环境：Windows
- ![](https://img2020.cnblogs.com/blog/465934/202112/465934-20211214162329371-13136215.png)
- 参考：[LAMP环境搭建和LNMP环境搭建](http://c.biancheng.net/linux_tutorial/16/)

## LAMP一键安装

[LAMP一键安装包](https://lamp.sh/), [github](https://github.com/teddysun/lamp) 是一个用 Linux Shell 编写的可以为 Amazon Linux/CentOS/Debian/Ubuntu 系统的 VPS 或服务器安装 LAMP(Linux + Apache + MySQL/MariaDB + PHP) 生产环境的 Shell 脚本。包含一些可选安装组件如：
- Zend OPcache, ionCube Loader, PDFlib, XCache, APCu, imagick, gmagick, libsodium, memcached, redis, mongodb, swoole, yaf, yar, msgpack, psr, phalcon, grpc, xdebug
- 其他诸如：OpenSSL, ImageMagick, GraphicsMagick, Memcached, phpMyAdmin, Adminer, Redis, re2c, KodExplorer
- 同时还有一些辅助脚本如：虚拟主机管理、Apache、MySQL/MariaDB、PHP 及 PhpMyAdmin、Adminer 的升级等。

程序目录
- MySQL 安装目录: /usr/local/mysql
- MySQL 数据库目录：/usr/local/mysql/data（默认路径，安装时可更改）
- MariaDB 安装目录: /usr/local/mariadb
- MariaDB 数据库目录：/usr/local/mariadb/data（默认路径，安装时可更改）
- PHP 安装目录: /usr/local/php
- Apache 安装目录： /usr/local/apache

默认的网站根目录： /data/www/default

```shell
# wget/git
yum -y install wget git      # for Amazon Linux/CentOS
apt-get -y install wget git  # for Debian/Ubuntu
# lamp包下载
git clone https://github.com/teddysun/lamp.git
cd lamp
chmod 755 *.sh
# 开始安装
./lamp.sh
# 使用方法
lamp add     # 创建虚拟主机
lamp del     # 删除虚拟主机
lamp list    # 列出虚拟主机
lamp version # 显示当前版本
# 升级
cd ~/lamp
git reset --hard         # Resets the index and working tree
git pull                 # Get latest version first
chmod 755 *.sh
./upgrade.sh             # Select one to upgrade
./upgrade.sh apache      # Upgrade Apache
./upgrade.sh db          # Upgrade MySQL or MariaDB
./upgrade.sh php         # Upgrade PHP
./upgrade.sh phpmyadmin  # Upgrade phpMyAdmin
./upgrade.sh adminer     # Upgrade Adminer
# 卸载
./uninstall.sh

# MySQL 或 MariaDB 命令
/etc/init.d/mysqld (start|stop|restart|status)
# Apache 命令
/etc/init.d/httpd (start|stop|restart|status)
# Memcached 命令（可选安装）
/etc/init.d/memcached (start|stop|restart|status)
# Redis 命令（可选安装）
/etc/init.d/redis-server (start|stop|restart|status)

```


## Apache

Apache HTTP 服务器是世界上最广泛使用的 web 服务器。它是一个免费，开源，并且跨平台的 HTTP 服务器，包含强大的特性，并且可以使用很多模块进行扩展。

[如何在 CentOS 8 上安装 Apache](https://cloud.tencent.com/developer/article/1626789)
- Apache 在默认的 CentOS 源仓库中可用，并且安装非常直接。在基于 RHEL 的发行版中，Apache 软件包和服务被称为 httpd


```shell
# 安装Apache, 用 root 或者其他有 sudo 权限的用户身份
yum install httpd httpd-devel
# Apache 随系统启动：
chkconfig --levels 235 httpd on
# 启动apache服务
/bin/systemctl start httpd.service
```
Apache专用：
- 服务目录  /etc/httpd
- 主配置文件 /etc/httpd/conf/httpd.conf
- 网站数据目录    /var/www/html
- 访问日志  /var/log/httpd/access_log
- 错误日志  /var/log/httpd/error_log


## PHP

### php安装

- mac下安装php
  - [官方安装方法](https://www.php.net/manual/zh/install.macosx.php)
- [centos下安装php环境](https://www.php.cn/centos/460292.html)的方法：
  - 首先安装并启动apache
  - 然后安装mysql；
  - “yum install php php-devel”命令安装php；
  - 最后重启apache，访问服务器所在ip即可
    - apache默认就是使用80端口

```shell
# 安装Apache
yum install httpd httpd-devel
# 启动apache服务
/bin/systemctl start httpd.service
# 如果访问失败，需要关闭防火墙
systemctl stop firewalld.service #停止firewall
systemctl disable firewalld.service #禁止firewall开机启动
firewall-cmd --state #查看默认防火墙状态（关闭后显示notrunning，开启后显示running）
# 安装mysql
yum install mysql mysql-server
# 启动mysql
systemctl start mysql.service
# 安装php
yum install php php-devel
# /var/www/html/下建立一个PHP文件index.php,加入代码：/var/www/html/下建立一个PHP文件index.php,加入代码：
# 注意：在centos7通过yum安装PHP7，首先在终端运行
rpm -ivh http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm # 安装epel-release
rpm -Uvh htt[ps](http://www.111cn.net/fw/photo.html)://mirror.webtatic.com/yum/el7/webtatic-release.rpm
# 启动php
/bin/systemctl start httpd.service

php -v # 显示当前PHP版本
# 安装php扩展
yum install php-mysql php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc
# 再次重启Apache
/bin/systemctl start httpd.service

```

### php语法

[官方文档](https://www.php.net/manual/zh/index.php)





# Python Web框架

参考：
- [Python Web服务器并发性能测试](https://blog.csdn.net/bandaoyu/article/details/88546515)
- [从0到1，Python Web开发的进击之路](https://zhuanlan.zhihu.com/p/25038203)
  - ![](https://pic1.zhimg.com/80/v2-a165341ad880a5bcb1e2bd8d6623d800_720w.png)
  - ![](https://pic4.zhimg.com/80/v2-8a8fd9da3b83b31a331105ce3b81f75b_720w.png)

Python 常见部署方法有 ：
- `fcgi` ：用 spawn-fcgi 或者框架自带的工具对各个 project 分别生成监听进程，然后和 http 服务互动
- `wsgi` ：利用 http 服务的 mod_wsgi 模块来跑各个 project(Web 应用程序或框架简单而通用的 Web 服务器 之间的接口)。
- `uWSGI` 是一款像 php-cgi 一样监听同一端口，进行统一管理和负载平衡的工具，uWSGI，既不用 wsgi 协议也不用 fcgi 协议，而是自创了一个 uwsgi 的协议，据说该协议大约是 fcgi 协议的 10 倍那么快。

其实 WSGI 是分成 **server** 和 **framework** (即 application) 两部分 (当然还有 **middleware** 中间件)。

严格说 WSGI 只是一个**协议**, 规范 server 和 framework 之间连接的接口。

- 所有的 Python Web框架都要遵循 WSGI 协议

WSGI 中有一个非常重要的概念：每个Python Web应用都是一个**可调用**（callable）的对象。
- 在 flask 中，这个对象就是 app = Flask(name) 创建出来的 app，图中的绿色Application部分。
- 要运行web应用，必须有 web server，如熟悉的apache、nginx，或者python中的gunicorn，werkzeug提供的WSGIServer，是图的黄色Server部分
- Server和Application之间怎么通信，就是WSGI的功能，规定了 app(environ, start_response) 的接口，server会调用 application，并传给它两个参数：environ 包含了请求的所有信息，start_response 是 application 处理完之后需要调用的函数，参数是状态码、响应头部还有错误信息。
- ![](https://img-blog.csdn.net/20170530093502586)
- WSGI application 非常重要的特点是可以嵌套。可以写个application，调用另外一个 application，然后再返回（类似一个 proxy）。一般来说，嵌套的最后一层是业务应用，中间就是 middleware。好处是可以解耦业务逻辑和其他功能，比如限流、认证、序列化等都实现成不同的中间层，不同的中间层和业务逻辑是不相关的，可以独立维护；而且用户也可以动态地组合不同的中间层来满足不同的需求。
- Flask基于Werkzeug WSGI工具箱和Jinja2 模板引擎。Flask也被称为“microframework”，因为它使用简单的核心，用extension增加其他功能。Flask没有默认使用的数据库、窗体验证工具。然而，Flask保留了扩增的弹性，可以用Flask-extension加入这些功能：ORM、窗体验证工具、文件上传、各种开放式身份验证技术。Flask是一个核心，而其他功能则是一些插件
- ![](https://img-blog.csdn.net/20170530093535180)

Flask是怎么将代码转换为可见的Web网页?
- 从Web程序的一般流程来看，当客户端想要获取动态资源时，（比如ASP和PHP这类语言写的网站），会发起一个HTTP请求（比如用浏览器访问一个URL），Web应用程序就会在服务器后台进行相应的业务处理（比如对数据库进行操作或是进行一些计算操作等），取出用户需要的数据，生成相应的HTTP响应（当然，如果访问的是 静态资源 ，服务器则会直接返回用户所需的资源，不会进行业务处理）
- ![](https://img-blog.csdn.net/20170530093546915)
- 实际应用中，不同的请求可能会调用相同的处理逻辑，即Web开发中所谓的路由分发
- ![](https://img-blog.csdn.net/20170530093643676)
- Flask中，使用werkzeug来做路由分发，werkzeug是Flask使用的底层WSGI库（WSGI，全称 Web Server Gateway interface，或者 Python Web Server Gateway Interface，是为 Python 语言定义的Web服务器和Web应用程序之间的一种简单而通用的接口）。
- WSGI将Web服务分成两个部分：服务器和应用程序。
  - WGSI服务器只负责与网络相关的两件事：接收浏览器的HTTP请求、向浏览器发送HTTP应答；
  - 而对HTTP请求的具体处理逻辑，则通过调用WSGI应用程序进行。
- ![](https://img-blog.csdn.net/20170530093621801)

参考：
- [Flask运行原理解析](https://blog.csdn.net/sunhuaqiang1/article/details/72808619)
- [Flask应用运行过程剖析](https://blog.csdn.net/weixin_34250434/article/details/89072137)

WSGI server 把服务器功能以 WSGI 接口暴露出来。比如 mod_wsgi 是一种 server, 把 apache 的功能以 WSGI 接口的形式提供出来。
- WSGI framework 就是我们经常提到的 Django 这种框架。不过需要注意的是, 很少有单纯的 WSGI framework , 基于 WSGI 的框架往往都自带 WSGI server。比如 Django、CherryPy 都自带 WSGI server 主要是测试用途, 发布时则使用生产环境的 WSGI server。而有些 WSGI 下的框架比如 pylons、bfg 等, 自己不实现 WSGI server。使用 paste 作为 WSGI server。
- Paste 是流行的 WSGI server, 带有很多中间件。还有 flup 也是一个提供中间件的库。
搞清楚 WSGI server 和 application, 中间件自然就清楚了。除了 session、cache 之类的应用, 前段时间看到一个 bfg 下的中间件专门用于给网站换肤的 (skin) 。中间件可以想到的用法还很多。
- 这里再补充一下, 像 django 这样的框架如何以 fastcgi 的方式跑在 apache 上的。这要用到 flup.fcgi 或者 fastcgi.py (eurasia 中也设计了一个 fastcgi.py 的实现) 这些工具, 它们就是把 fastcgi 协议转换成 WSGI 接口 (把 fastcgi 变成一个 WSGI server) 供框架接入。
  - 整个架构是这样的: django -> fcgi2wsgiserver -> mod_fcgi -> apache 。
- 虽然我不是 WSGI 的粉丝, 但是不可否认 WSGI 对 python web 的意义重大。有意自己设计 web 框架, 又不想做 socket 层和 http 报文解析的同学, 可以从 WSGI 开始设计自己的框架。在 python 圈子里有个共识, 自己随手搞个 web 框架跟喝口水一样自然, 非常方便。或许每个 python 玩家都会经历一个倒腾框架的

uWSGI 的主要特点如下：
- 超快的性能。
- 低内存占用（实测为 apache2 的 mod_wsgi 的一半左右）。
- 多app管理。
- 详尽的日志功能（可以用来分析 app 性能和瓶颈）。
- 高度可定制（内存大小限制，服务一定次数后重启等）。

Django就没有用异步，通过线程来实现并发，这也是WSGI普遍的做法，跟tornado不是一个概念

作者：[HylaruCoder](https://www.zhihu.com/question/297267614/answer/505683007)

简单说下几种部署方式 
- Flask 内置 WebServer + Flask App = **弱鸡版**本的 Server, **单进程**（单 worker) / 失败挂掉 / 不易 Scale
- Gunicorn + Flask App = **多进程**（多 worker) / 多线程 / 失败自动帮你重启 Worker / 可简单Scale
- 多 Nginx + 多 Gunicorn + Flask App = **小型**多实例 Web 应用，一般也会给 gunicorn 挂 supervisor

在生产环境中, 一般都是请求的走向都是 Nginx -> gunicorn -> flask/django app 

第一个问题，Flask 作为一个 Web 框架，内置了一个 webserver, 但这自带的 Server 到底能不能用？ 
- 官网的介绍： While lightweight and easy to use, Flask’s built-in server is not suitable for production as it doesn’t scale well. Some of the options available for properly running Flask in production are documented here. 
- 很显然，内置的 webserver 是能用的。但不适合放在生产环境。这个 server 本来就是给开发者用的。框架本身并不提供生产环境的 web 服务器，SpringBoot 这种内置 Tomcat 生产级服务器 是例外。 
- 查看 flask 代码的时候也可以看到这个 WebServer 的名称也叫做 run_simple , too simple 的东西往往不太适合生产。 

```python
from werkzeug.serving import run_simple
run_simple('localhost', 5000, application, use_reloader=True)
```

来看看为什么？ 假设我们使用的是 Nginx + Flask Run 来当作生产环境，全部部署在一台机器上。 

劣势如下： 
- 『单 Worker』只有一个进程在跑所有的请求，而由于实现的简陋性，内置 webserver 很容易卡死。并且只有一个 Worker 在跑请求。在多核 CPU 下，仅仅占用一核。当然，其实也可以多起几个进程。
- 『缺乏 Worker 的管理』接上，加入负载量上来了，Gunicorn 可以调节 Worker 的数量。而这个东西，内置的 Webserver 是不适合做这种事情的。

一言以蔽之，太弱，几个请求就打满了。 

第二个问题，Gunicorn 作为 Server 相对而言可以有什么提升。 

gunicorn 的优点如下
- 帮忙 scale worker, 进程挂了自动重启
- 用 python 的框架 flask/django/webpy 配置起来都差不多。
- 还有**信号**机制。可以支持多种配置。

在管理 worker 上，使用了 pre-fork 模型，即一个 master 进程管理多个 worker 进程，所有请求和响应均由 Worker 处理。Master 进程是一个简单的 loop, 监听 worker 不同进程信号并且作出响应。比如接受到 TTIN 提升 worker 数量，TTOU 降低运行 Worker 数量。如果 worker 挂了，发出 CHLD, 则重启失败的 worker, 同步的 Worker 一次处理一个请求。 

PS: 如果没有静态资源并且无需反向代理的话，抛弃 Nginx 直接使用 Gunicorn 和 Flask app 也能搞定。

## 并发与并行

并发与并行：
- `并发`(concurrency)：指在同一时刻只能有**一条**指令执行，但多个进程指令被快速的轮换执行，使得在宏观上具有多个进程同时执行的效果，但在微观上并不是同时执行的，只是把时间分成若干段，使多个进程快速交替的执行。
- `并行`(parallel)：指在同一时刻，有**多条**指令在多个处理器上同时执行。所以无论从微观还是从宏观来看，二者都是一起执行的。

并发在同一时刻只有一条指令执行，只不过进程(线程)在CPU中快速切换，速度极快，给人看起来就是“同时运行”的印象，实际上同一时刻只有一条指令进行。但实际上如果我们在一个应用程序中使用了多线程，线程之间的轮换以及上下文切换是需要花费很多时间的。

当并发执行累加操作不超过百万次时，速度会比串行执行累加操作要慢。
- 原因：线程有创建和上下文切换的开销，导致并发执行的速度会比串行慢的情况出现。

多个线程可以执行在单核或多核CPU上，单核CPU也支持多线程执行代码，CPU通过给每个线程分配CPU时间片(机会)来实现这个机制。CPU为了执行多个线程，就需要不停的切换执行的线程，这样才能保证所有的线程在一段时间内都有被执行的机会。

此时，CPU分配给每个线程的执行时间段，称作它的时间片。CPU时间片一般为几十毫秒。CPU通过时间片分配算法来循环执行任务，当前任务执行一个时间片后切换到下一个任务。

但是，在切换前会保存上一个任务的状态，以便下次切换回这个任务时，可以再加载这个任务的状态。所以任务从保存到再加载的过程就是一次上下文切换。

根据多线程的运行状态来说明：多线程环境中，当一个线程的状态由Runnable转换为非Runnable(Blocked、Waiting、Timed_Waiting)时，相应线程的上下文信息(包括CPU的寄存器和程序计数器在某一时间点的内容等)需要被保存，以便相应线程稍后再次进入Runnable状态时能够在之前的执行进度的基础上继续前进。而一个线程从非Runnable状态进入Runnable状态可能涉及恢复之前保存的上下文信息。这个对线程的上下文进行保存和恢复的过程就被称为上下文切换。

摘自：[redis为什么这么快](https://mp.weixin.qq.com/s/fmpaPxD3domczqmVQlIT7g)

## Gunicorn 多进程实现并发

### 问题

【2022-3-8】gunicorn是多进程，多个进程之间session不共享！详见[地址](https://blog.csdn.net/lpwmm/article/details/103951609)
- Gunicorn中的worker实际上对应的是**多进程**,默认配置每个worker之间是**独立**存在的进程, worker会实例化一个新的Flask对象跑起来

解决方法：
- 使用固定的SECRET_KEY，app.config\['SECRET_KEY'] = 'XXXXX' —— 【注】仅限单机有效，无法解决多机部署同步
- 将session数据存放到数据库中, 如使用redis —— 【注】多机有效，更靠谱
这样Gunicorn中使用worker实现的多进程之间就不会出现数据不同步的情况了.

### 介绍

Gunicorn（“绿色独角兽”）是一个被广泛使用的**高性能**的python WSGI UNIX HTTP服务器，移植自Ruby的独角兽（Unicorn）项目，使用pre-fork worker模式具有使用非常简单，轻量级的资源消耗，以及高性能等特点。
- pre-fork worker模式: 一个中央master进程来管理一系列的工作进程，master并不知道各个独立客户端。所有的请求和响应完全由工作进程去完成。master通过一个循环不断监听各个进程的信号并作出相应反应，这些信号包括TTIN、TTOU和CHLD。TTIN和TTOU告诉master增加或者减少正在运行的进程数，CHLD表明一个子进程被终止了，在这种情况下master进程会自动重启这个失败的进程。

Gunicorn是主流的WSGI容器之一，它易于配置，兼容性好，CPU消耗很少，它支持多种worker模式：
- **同步**worker：默认模式，也就是一次只处理一个请求。最简单的同步工作模式
- **异步**worker：通过Eventlet、Gevent实现的异步模式,gevent和eventlet都是基于greenlet库，利用python协程实现的
- 异步IOWorker：目前支持gthread和gaiohttp; gaiohttp利用aiohttp库实现异步IO，支持web socket; gthread采用的事线程工作模式，利用线程池管理连接
- Tronado worker：tornado框架,利用python Tornado框架实现

工作模式是通过work_class参数配置的值：缺省值：sync
- sync
- Gevent
- Eventlet
- tornado
- gaiohttp
- gthread

参考：
- [Gunicorn使用详解](https://www.cnblogs.com/shijingjing07/p/9110619.html)
- [Gunicorn的使用](https://www.jianshu.com/p/8ea438251e44/)

- ![](https://img.jbzj.com/file_images/article/201907/2019722104302288.jpg?2019622104331)

Gunicorn(Green Unicorn)是一个WSGI HTTP服务器,python自带的有个web服务器，叫做 wsgiref，Gunicorn的优势在于，它使用了pre-fork worker模式，gunicorn在启动时，会在主进程中预先fork出指定数量的worker进程来处理请求，gunicorn依靠操作系统来提供负载均衡，推荐的worker数量是(2*$num_cores)+1
python是单线程的语言，当进程阻塞时，后续请求将排队处理。所用pre-fork worker模式，极大提升了服务器请求负载。

安装

```shell
pip install gunicorn
```

使用, 编写wsgi接口, test.py代码

```python
from flask import Flask
app = Flask(__name__)

def application(environ,start_response):
    start_response('200 OK',[('Content-Type','text/html')])
    return b'<h1>Hello,web!</h1>'
```

使用gunicorn监听请求，运行以下命令
- -w:指定fork的worker进程数
- -b:指定绑定的端口
- test:模块名,python文件名
- application:变量名,python文件中可调用的wsgi接口名称

```shell
# 启动并发进程, 注意：不是. 而是 :
gunicorn -w 2 -b 0.0.0.0:8000 test:application
# 查询配置信息
gunicorn -h
```

gunicorn相关参数
- 1) -c CONFIG,--config=CONFIG, 指定一个配置文件（py文件）
- 2) -b BIND,--bind=BIND 与指定socket进行板顶
- 3) -D,--daemon 后台进程方式运行gunicorn进程
- 4) -w WORKERS,--workers=WORKERS, 工作进程的数量
  - 获取CPU个数: python -c 'import multiprocessing;print(multiprocessing.cpu_count())'
- 5) -k WORKERCLASS,--worker-class=WORKERCLASS, 工作进程类型，包括sync（默认）,eventlet,gevent,tornado,gthread,gaiohttp
- 6) --backlog INT 最大挂起的连接数
- 7)--log-level LEVEL 日志输出等级
- 8) --access-logfile FILE 访问日志输出文件
- 9)--error-logfile FILE 错误日志输出文件

gunicorn可以写在配置文件中，下面举列说明配置文件的写法,gunicorn.conf.py

```python
bind = "0.0.0.0:8000"
workers = 2
```

常用项目配置

```python
# config.py
import os
import gevent.monkey
gevent.monkey.patch_all()

import multiprocessing

# debug = True # 用于开发环境，非线上环境
# 开启debug项后，在启动gunicorn的时候可以看到所有可配置项的配置

loglevel = 'debug' # debug、info、warning、error、critical
bind = "0.0.0.0:7001"
# 日志文件。 注意：如果log不存在，启动会报错
pidfile = "log/gunicorn.pid"
accesslog = "log/access.log"
errorlog = "log/debug.log"
daemon = True # 开启后台运行

# 启动的进程数
workers = multiprocessing.cpu_count()
worker_class = 'gevent'
x_forwarded_for_header = 'X-FORWARDED-FOR'
```

运行以下命令:

```shell
# 使用配置文件启动服务
gunicorn -c gunicorn.conf.py test:application
```

【2021-11-30】项目中使用的gunicorn配置文件：gunicorn_config.py

```python
# !/usr/bin/env python
# -*- coding:utf8 -*- 

# **************************************************************************
# * Copyright (c) 2021 ke.com, Inc. All Rights Reserved
# **************************************************************************
# * @file gunicorn并发配置
# * @author wangqiwen004@ke.com
# * @date 2021/11/30 12:17
# **************************************************************************


__author__ = "wangqiwen004@ke.com"
__copyright__ = "Copyright of beike (2021)."

import os
from numbers import Number
import psutil

core_number = psutil.cpu_count(logical=True)
if isinstance(core_number, Number):
    workers_num = core_number
else:
    workers_num = 5

port = os.getenv('PORT', '7399')
bind = '0.0.0.0:{}'.format(port)  # 绑定地址
backlog = 128
workers = workers_num
print('Child process number: {} with port: {}'.format(workers, port))

graceful_timeout = 3

proc_name = 'bwbd_server'
preload_app = True

# */* vim: set expandtab ts=4 sw=4 sts=4 tw=400: */
```

启动脚本：服务入口脚本 start_all.py, 里面的app是flask初始化后的对象
- gunicorn --pid=run.pid --config gunicorn_config.py --daemon start_all:app

```shell
#! /bin/bash
export LANG='UTC-8'
export LC_ALL='en_US.UTF-8'

echo "MATRIX_CODE_DIR:" "$MATRIX_CODE_DIR"

if [ 0"$MATRIX_CODE_DIR" = "0" ];then
    export MATRIX_CODE_DIR="`pwd`"
    export MATRIX_APPLOGS_DIR="."
    export MATRIX_PRIVDATA_DIR="."
fi

LOG_FILE=$MATRIX_APPLOGS_DIR/log
if [ ! -e $LOG_FILE ];then
    mkdir $LOG_FILE
fi
export ENVTYPE=`cat "$MATRIX_CODE_DIR"/env_flag.in`
echo "condition is: "$ENVTYPE

case $1 in
    start)
        echo "run start ... "$MATRIX_PRIVDATA_DIR
        source $MATRIX_PRIVDATA_DIR/venv/bin/activate
        cd $MATRIX_CODE_DIR
        #source install.sh
        # gunicorn --pid=run.pid --config gunicorn_config.py --daemon start_all:app
        $MATRIX_PRIVDATA_DIR/venv/bin/gunicorn \
                --pid=$LOG_FILE/run.pid \
                --config bin/gunicorn_config.py \
                --daemon start_all:app \
        deactivate
        echo "start with nohup"
    ;;
    run)
        source $MATRIX_PRIVDATA_DIR/venv/bin/activate
        cd $MATRIX_CODE_DIR
        #source install.sh
        $MATRIX_PRIVDATA_DIR/venv/bin/gunicorn \
                --pid=$LOG_FILE/run.pid \
                --config bin/gunicorn_config.py \
                start_all:app
    ;;
    *)
        echo "Using option{start|run|stop}"
    ;;
esac

```


## uvicorn

类似gunicorn

FastAPI 推荐使用 uvicorn 来运行服务，Uvicorn 是基于 uvloop 和 httptools 构建的闪电般快速的 ASGI 服务器
-  uvloop 用于替换标准库 asyncio 中的事件循环，使用 Cython 实现，它非常快，可以使 asyncio 的速度提高 2-4 倍。asyncio 不用我介绍吧，写异步代码离不开它。
- httptools 是 nodejs HTTP 解析器的 Python 实现。
- ASGI 服务器是**异步网关协议接口**，一个介于网络协议服务和 Python 应用之间的标准接口，能够处理多种通用的协议类型，包括 HTTP，HTTP2 和 WebSocket。ASGI 帮助 Python 在 Web 框架上和 Node.JS 及 Golang 相竟争，目标是获得高性能的 IO 密集型任务，ASGI 支持 HTTP2 和 WebSockets，WSGI 是不支持的。
  - Python 仍缺乏异步的网关协议接口，ASGI 的出现填补了这一空白.


安装：
- pip install uvicorn

服务启动
- 命令行方式

```shell
# main.py里的app应用
uvicorn main:app
# --reload: 热启动，方便代码的开发
uvicorn main:app --reload
# 指定host和port
uvicorn main:app --reload --host 192.XXX.XXX --port 8001
uvicorn main:app --host 10.200.24.101  --port 8094
```

- Python代码方式, demo.py

```python
from fastapi import FastAPI
 
app = FastAPI()

@app.get("/")
def root():
    return {"name":"wangqien", "value":23}

# 异步接口
@app.get("/index")
async def index():
    return {"name":"wangqien", "value":23}

@app.get("/items/{item_id}")
async def read_item(item_id: str, q: str = None, short: bool = False):
    item = {"item_id": item_id}
    if q:
        item.update({"q": q})
    if not short:
        item.update(
            {"description": "This is an amazing item that has a long description"}
        )
    return item
 
if __name__ == '__main__':
    import uvicorn

    uvicorn.run(app=app)
    # 自定义host、port
    uvicorn.run(app, host="192.XXX.XXX", port=8001)
    # 设置日志级别
    uvicorn.run("demo:app", host="127.0.0.1", port=5000, log_level="info")
    # 设置热更新
    uvicorn.run(app="demo:app", host="127.0.0.1", port=8000, reload=True, debug=True)
```


## supervisor

[使用Gunicorn与Supervisor部署Flask](https://blog.csdn.net/henghenghalala/article/details/103685602)

supervisor用作流程管理器，应该：
- 用其文件描述符将套接字移交给uvicorn，supervisor始终将其用作0，并且必须在本fcgi-program节中进行设置。或为每个uvicorn进程使用UNIX域套接字。

一个简单的主管配置可能看起来像这样： administratord.conf：

```
[supervisord]

[fcgi-program:uvicorn]
socket=tcp://localhost:8000
command=venv/bin/uvicorn --fd 0 example:App
numprocs=4
process_name=uvicorn-%(process_num)d
stdout_logfile=/dev/stdout
stdout_logfile_maxbytes=0
```

运行: supervisord -n

supervisor实现程序的后台守护运行, 也可以实现开机自动重启

```shell
# 下载安装supervisor， 请注意此时所在目录还是项目目录 /root/xubobo/project
pip install supervisor
# 初始化配置文件
echo_supervisord_conf > supervisor.conf
# 此时目录下多了一个配置文件， 修改此配置文件
vim supervisor.conf
# 在配置文件最底部加入如下配置
[program: githook]
command=/root/xubobo/githook/venv/bin/gunicorn -w 3 -b 0.0.0.0:5000 app:app   ; 启动命令
directory=/root/xubobo/githook                                   ; 项目的文件夹路径
startsecs=0                                                      ; 启动时间
stopwaitsecs=0                                                   ; 终止等待时间
autostart=true                                                   ; 是否自动启动
autorestart=true                                                 ; 是否自动重启
stdout_logfile=/data/python/SMT/log/gunicorn.log                 ; log 日志
stderr_logfile=/data/python/SMT/log/gunicorn.err                 ; 错误日志

# 指定配置文件来启动supervisord
supervisord -c supervisor.conf
# 检查状态
supervisorctl -c supervisor.conf status
# 可以看到 githook 应用已经是RUNNING状态了
```

supervisor的常用命令

```shell
# 通过配置文件启动supervisor
supervisord -c supervisor.conf       
# 察看supervisor的状态
supervisorctl -c supervisor.conf status
# 重新载入 配置文件
supervisorctl -c supervisor.conf reload
# 启动指定/所有 supervisor管理的程序进程
supervisorctl -c supervisor.conf start [all]|[appname]
# 关闭指定/所有 supervisor管理的程序进程
supervisorctl -c supervisor.conf stop [all]|[appname]
```

## gevent/grequests

[python中grequests模块简单应用](https://blog.csdn.net/cong_da_da/article/details/84325849)

用requests向某个url批量发起POST请求，交互的内容很简单，若开启多线程去访问占用太多资源不合理。
想到了使用协程gevent，grequests 模块相当于是封装了gevent的requests模块

安装
- 若是联网安装，直接 pip install grequests 完事
- 若是离线安装，需要先安装 greenlet模块 和 gevent模块 ，再安装 grequests模块 

```python
# coding:utf-8
adata = json.dumps({"key": "value"})
header = {"Content-type": "appliaction/json", "Accept":"application/json"}
url = "http://www.baidu.com"

task = []
req = grequests.request("POST", url=url, data=adata, headers=header)
task.append(req)
# 此处map的requests参数是装有实例化请求对象的列表,其返回值也是列表， size参数可以控制并发的数量
resp = grequests.map(task)
print resp
# 查看返回值的属性值，我们关注的一般就是text json links  url headers 等了
print dir(resp[0])
#==============
urls = ["http://www.baidu.com", "http://www.baidu.com", "http://www.baidu.com"]
req = (grequests.get(u) for u in urls)
resp = grequests.map(req)
```

grequests和requests的对比
- grequests比单个的requests请求要快一点，当网络延迟较高时，grequests优势比较明显，修改 map的size参数进行尝试,合理的设置size值，不要太大，超过阀值再大也没有用

```python
# coding:utf-8
import grequests
import time
import json
import requests
 
 
adata = json.dumps({"key": "value"})
header = {"Content-type": "appliaction/json", "Accept":"application/json"}

def use_grequests(num):
    task = []
    urls = ["http://hao.jobbole.com/python-docx/" for i in range(num)]
    while urls:
        url = urls.pop(0)
        rs = grequests.request("POST", url, data=adata, headers=header)
        task.append(rs)
    resp = grequests.map(task, size=5)
    return resp
 
def use_requests(num):
    urls = ["http://hao.jobbole.com/python-docx/" for i in range(num)]
    index = 0
    while urls:
        url = urls.pop(0)
        resp = requests.post(url=url, headers=header, data=adata)
        index += 1
        if index % 10 == 0:
            print u'目前是第{}个请求'.format(index)

def main(num):
    time1 = time.time()
    finall_res = use_requests(num)
    print finall_res
    time2 = time.time()
    T = time2 - time1
    print u'use_requests发起{}个请求花费了{}秒'.format(num, T)
 
    print u'正在使用grequests模块发起请求...'
    time3 = time.time()
    finall_res2 = use_grequests(num)
    print finall_res2
    time4 = time.time()
    T2 = time4 - time3
    print u'use_grequests发起{}个请求花费了{}秒'.format(num, T2)
 
if __name__ == '__main__':
    main(100)
```

手动使用gevent配合requests模块

```python
# coding:utf-8
import gevent
import time
from gevent import monkey
import requests
monkey.patch_all()
 
datali = [x for x in range(100)]
task = []
def func(i):
    print u'第{}个请求'.format(i)
    url = "http://hao.jobbole.com/python-docx/"
    resp = requests.get(url=url)
    return resp
 
time1 = time.time()
for i in datali:
    task.append(gevent.spawn(func, i))
res = gevent.joinall(task)
print len(res)
time2 = time.time()
T = time2 - time1
print u'消耗了{}秒'.format(T)
```


## web框架

### 为什么要有web框架？（MVC）

不用框架设计 Python 网页应用程序
- 最原始、直接的办法是使用CGI标准（1998年流行）。 
- 从应用角度解释它是如何工作： 
  - 首先开发一个Python脚本，输出HTML代码
  - 然后保存成.cgi扩展名的文件，latestbooks.cgi，上传至服务器
  - 通过浏览器访问此文件。

代码示例：

```python
#!/usr/bin/env python
import MySQLdb
# 输出html内容
print "Content-Type: text/html\n"
print "<html><head><title>Books</title></head>"
print "<body>"
print "<h1>Books</h1>"
print "<ul>"
# 连接数据库
connection = MySQLdb.connect(user='me', passwd='letmein', db='my_db')
cursor = connection.cursor()
cursor.execute("SELECT name FROM books ORDER BY pub_date DESC LIMIT 10")
# 取数，组装html元素
for row in cursor.fetchall():
    print "<li>%s</li>" % row[0]

print "</ul>"
print "</body></html>"
# 关闭连接
connection.close()
```

简单、直接，易懂，但是带来的问题：
- 代码冗余：如果多处连接数据库，那每个独立的CGI脚本，不应该重复写数据库连接的代码。 比较实用的办法是写一个共享函数，可被多个代码调用。
- 开发效率低，容易出错：开发者不用关注如何输出Content-Type以及完成所有操作后去关闭数据库，初始化和释放 相关的工作应该交给一些通用的框架来完成。
- 不安全：每个页面都分别对应独立的数据库和密码
- 初学者容易出错：没有Python开发经验的web设计师，页面显示的逻辑与从数据库中读取书本记录分隔开，这样 Web设计师的重新设计不会影响到之前的业务逻辑。

Web框架为应用程序提供了一套程序框架， 这样可以专注于编写清晰、易维护的代码，而无需从头做起。如用django框架实现以上功能：
- 分成4个Python的文件，(models.py , views.py , urls.py ) 和html模板文件 (latest_books.html )

```python
# ① models.py (the database tables)

from django.db import models

class Book(models.Model):
    name = models.CharField(max_length=50)
    pub_date = models.DateField()

# ② views.py (the business logic)
from django.shortcuts import render_to_response
from models import Book

def latest_books(request):
    book_list = Book.objects.order_by('-pub_date')[:10]
    return render_to_response('latest_books.html', {'book_list': book_list})

# ③ urls.py (the URL configuration)
from django.conf.urls.defaults import *
import views

urlpatterns = patterns('',
    (r'^latest/$', views.latest_books),
)

# ④ latest_books.html (the template)
<html><head><title>Books</title></head>
<body>
<h1>Books</h1>
<ul>
{ % for book in book_list % }
<li>{ { book.name } }</li>
{ % endfor % }
</ul>
</body></html>
```

不用关心语法细节；只要用心感觉整体的设计。 这里只关注分割后的几个文件：
- models.py 文件主要用一个 Python 类来描述数据表。 称为 `模型`(model) 。 运用这个类，可以通过简单的 Python 的代码来创建、检索、更新、删除 数据库中的记录而无需写一条又一条的SQL语句。
- views.py文件包含了页面的**业务逻辑**。 latest_books()函数叫做`视图`。
- urls.py 指出了什么样的 URL 调用什么的视图。 在这个例子中 /latest/ URL 将会调用 latest_books() 这个函数。 如果你的域名是example.com，任何人浏览网址 http://example.com/latest/ 将会调用latest_books()这个函数。
- latest_books.html 是 html `模板`，描述了这个页面的设计是如何的。 使用带基本逻辑声明的模板语言，如{ % for book in book_list % }

这些部分松散遵循的模式称为**模型-视图-控制器**(MVC)。 简单的说， MVC 是一种软件开发的方法，它把代码的定义和数据访问的方法（`模型`）与请求逻辑 （`控制器`）还有用户接口（`视图`）分开来。


### 常见web框架

![](https://p3.toutiaoimg.com/large/tos-cn-i-qvj2lq49k0/4106964c366b40e390b15fbaed9e7bfe)

如图：
1. 浏览器发起http请求，与服务器交互
1. wsgi服务器接收到流量后，转由各个Python web框架处理
1. 常规web框架包含三个部分：
  - 框架控制逻辑：依次为 路由系统、业务逻辑、耦合适配（数据库+模板）
  - 网页静态资源：html、js、css等，一般放static目录下
  - 数据库：通过ORM交互
1. 路由系统
1. 业务处理逻辑
1. 适配：模板、数据库

基于python的web框架，如tornado、flask、webpy都是在这个范围内进行增删裁剪的。例如tornado用的是自己的异步非阻塞“wsgi”，flask则只提供了最精简和基本的框架。Django则是直接使用了WSGI，并实现了大部分功能。

### 对比

- Python web框架的性能响应排行榜
    - 从并发性上看Fastapi完全碾压了 Flask (实际上也领先了同为异步框架的tornado 不少)
    - ![](https://pic4.zhimg.com/80/v2-7e31e8992685cc11594c5c31a65bc357_720w.jpg)
- 【2020-11-26】[Python Web 框架：Django、Flask 与 Tornado 的性能对比](https://www.jianshu.com/p/9960a9667a5c)，结论
   - Django：Python 界最全能的 web 开发框架，battery-include 各种功能完备，可维护性和开发速度一级棒。常有人说 Django 慢，其实主要慢在 Django ORM 与数据库的交互上，所以是否选用 Django，取决于项目对数据库交互的要求以及各种优化。而对于 Django 的同步特性导致吞吐量小的问题，其实可以通过 Celery 等解决，倒不是一个根本问题。Django 的项目代表：Instagram，Guardian。
   - Tornado：天生异步，性能强悍是 Tornado 的名片，然而 Tornado 相比 Django 是较为原始的框架，诸多内容需要自己去处理。当然，随着项目越来越大，框架能够提供的功能占比越来越小，更多的内容需要团队自己去实现，而大项目往往需要性能的保证，这时候 Tornado 就是比较好的选择。Tornado项目代表：知乎。
   - Flask：微框架的典范，号称 Python 代码写得最好的项目之一。Flask 的灵活性，也是双刃剑：能用好 Flask 的，可以做成 Pinterest，用不好就是灾难（显然对任何框架都是这样）。Flask 虽然是微框架，但是也可以做成规模化的 Flask。加上 Flask 可以自由选择自己的数据库交互组件（通常是 Flask-SQLAlchemy），而且加上 celery +redis 等异步特性以后，Flask 的性能相对 Tornado 也不逞多让，也许Flask 的灵活性可能是某些团队更需要的。

作者：Tim_Lee
链接：https://www.jianshu.com/p/9960a9667a5c


## Flask

Flask作为Web框架，它的作用主要是为了开发Web应用程序。

flask内核内置了两个最重要的组件，所有其它的组件都是通过易扩展的插件系统集成进来的。这两个内置的组件分别是werkzeug和jinja2。
- werkzeug是一个用于编写Python WSGI程序的工具包，它的结构设计和代码质量在开源社区广受褒扬，其源码被尊为Python技术领域最值得阅读的开源库之一。
- jinja2是一个功能极为强大的模板系统，它完美支持unicode中文，每个模板都运行在安全的沙箱环境中，使用jinja2编写的模板代码非常优美。

- 【2021-3-18】flask 1.0和1.1版本的差异，api返回值类型不同，前者不能直接返回python的dict类型，需要用json.dumps转换为string，后者可以直接返回dict结构，自动转换
- ![](https://pic3.zhimg.com/v2-ddbbe5dcf4fa4b35f11bca5f0546ecc3_1440w.jpg?source=172ae18b)
- [用Python 的Flask实现 RESTful API(学习篇)](https://zhuanlan.zhihu.com/p/32202156)

### Web原理

Web应用程序 (World Wide Web)诞生最初的目的，是为了利用互联网交流工作文档
- ![](https://pic1.zhimg.com/80/v2-9ea20556dcf97872c66cf8f3b4b51f20_1440w.jpg)

一切从**客户端**发起**请求**开始
- 所有Flask程序都必须创建一个程序**实例**。
- 当客户端想要获取资源时，一般会通过**浏览器**发起**HTTP请求**。
- 此时，Web服务器使用一种名为**WEB服务器网关接口**的`WSGI`（Web Server Gateway Interface）协议，把来自客户端的请求都交给Flask程序实例。
- Flask使用`Werkzeug`来做**路由分发**（URL请求和视图函数之间的对应关系）。根据每个URL请求，找到具体的**视图**函数。
- 在Flask程序中，**路由**一般是通过程序实例的装饰器实现。通过调用视图函数，获取到数据后，把数据传入HTML**模板**文件中，模板引擎负责渲染HTTP响应数据，然后由Flask返回响应数据给浏览器，最后浏览器显示返回的结果。


- 安装：pip install flask

werkzeug用法

```python
from werkzeug.wrappers import Request, Response

@Request.application
def application(request):
    return Response('Hello World!')

if __name__ == '__main__':
    from werkzeug.serving import run_simple
    run_simple('localhost', 4000, application)
```



### 快速入门


【2022-3-2】[Python Web 开发框架Flask快速入门](https://zhuanlan.zhihu.com/p/146874556)

网站项目开发的基本模式
- ![](https://pic4.zhimg.com/80/v2-1bd35ee5ed0fbe27f1b00b4d86f8e17b_1440w.jpg)

一个Web应用程序包含了三个部分，前端，服务端，数据库。
- 数据库负责存储数据，作为数据存储和查询的引擎；
- 前端网站作为用户直接查看的页面，负责展示数据。
- Flask 负责对数据库进行操作，将数据库中的数据渲染至前端。

大致工作：
- ![](https://pic4.zhimg.com/80/v2-b40de7859d3c2dd906749c3841996767_1440w.jpg)

#### 原始版本

新建一个Python文件，比如叫做 helloFlask.py

```python
from flask import Flask
app = Flask(__name__)

# 日志系统配置
handler = logging.FileHandler('app.log', encoding='UTF-8')
#设置日志文件，和字符编码
logging_format = logging.Formatter(
            '%(asctime)s - %(levelname)s - %(filename)s - %(funcName)s - %(lineno)s - %(message)s')
handler.setFormatter(logging_format)
app.logger.addHandler(handler)
#设置日志存储格式，也可以自定义日志格式满足不同的业务需求

@app.route('/')
def hello_world():
    app.logger.info('hi...')
    # app.logger.info('message info is %s', message, exc_info=1)。
    #app.logger.exception('%s', e) # 异常信息
    return 'Hello World!'

if __name__ == '__main__':
    #app.run() # 本地访问：只能从你自己的计算机上访问
    app.run(host='0.0.0.0') # 外网可访问

```

- 运行：python helloFlask.py
- 浏览器上输入 http://127.0.0.1:5000/，便会看到 Hello World！ 字样
- ![](https://picb.zhimg.com/80/v2-ea6c68e52462fb5025992cbb6b9728ed_720w.jpg)


#### 改进：路由

不访问索引页，而是直接访问想要的那个页面？
- route() 装饰器把一个函数绑定到对应的 URL 上

```python
@app.route('/')
def index():
    return 'Index Page'
# 路由到字页面
#   注：也可以在一个函数上附着多个规则
@app.route('/hello')
def hello():
    return 'Hello World'
```

#### 改进：模板

怎么给服务器的用户呈现一个漂亮的页面呢？
- 肯定需要用到 html、css ，如果想要更炫的效果还要加入 js
- 问题：这么一大堆字段串全都写到视图中通过 return 返回，这样定义就太麻烦了吧，因为定义字符串是不会出任何效果和错误的，如果有一个专门定义前端页面的地方就好了。
- Flask 配备了 [Jinja2 模板引擎](https://docs.jinkan.org/docs/jinja2/)。

jinja2是一个功能极为强大的模板系统，它完美支持unicode中文，每个模板都运行在安全的沙箱环境中，使用jinja2编写的模板代码非常优美。

jinjia2示例：使用时，去掉%与大括号中间的空格（规避jekyll语法）

```html
{ % extends "layout.html" % }
{ % block body % }
  <ul>
  { % for user in users % }
    <li><a href="{ { user.url } }">{ { user.username } }</a></li>
  { % endfor % }
  </ul>
{ % endblock % }
```

Flask 会在 templates 文件夹里寻找模板。如在templates下面创建模板index.html

```python
from flask import render_template
# render_template() 方法来渲染模板
@app.route('/index/')
def hello(name=None):
    name = "张三"
    return render_template('index.html', name=name)
```

html文件调用变量

```html
<!doctype html>
<title>Hello from Flask</title>
<!-- 使用模板判断语句：if else endif -->
{ % if name % }
  <h1>Hello { { name } }!</h1>
{ % else % }
  <h1>Hello World!</h1>
{ % endif % }
<!-- 使用循环语句 -->
{ % for i in range(1,10) % }
    { % for j in range(1,i+1) % }
        { { j } } x { { i } } = { { i*j } }
    { % endfor % }
    <br>
{ % endfor % }
```

输入 127.0.0.1:5000/index，即可看到：
- ![](https://pic1.zhimg.com/80/v2-b03185816de1197a546da550d12aa4d4_1440w.jpg)

模板中使用变量：
- 在 html 中定义 \{\{ 变量名 \} }
- 在 flask 中设定变量的key 和 value：render_template('index.html', name="张三")

#### 改进：缓存

【2022-3-2】[Github上最受欢迎的Python轻量级框架Flask入门](https://zhuanlan.zhihu.com/p/37750930)

为了避免重复计算，将已经计算的 pi(n)值缓存起来，下次直接查询。同时不再只返回一个单纯的字符串，我们返回一个json串，里面有一个字段cached用来标识当前的结果是否从缓存中直接获取的。
- 为什么缓存类PiCache需要使用RLock呢？因为考虑到**多线程**环境下Python的字典读写不是完全线程**安全**的，需要使用**锁**来保护一下数据结构。

```python
import math
import threading

from flask import Flask, request
from flask.json import jsonify

app = Flask(__name__)

class PiCache(object):
    """ 计算圆周率 """
    def __init__(self):
        self.pis = {}
        self.lock = threading.RLock() # 使用进程锁

    def set(self, n, pi):
        with self.lock: # 开锁
            self.pis[n] = pi

    def get(self, n):
        with self.lock: # 开锁
            return self.pis.get(n)
# 全局变量用于缓存
cache = PiCache()


@app.route("/pi")
def pi():
    n = int(request.args.get('n', '100'))
    result = cache.get(n)
    if result:
        return jsonify({"cached": True, "result": result})
    s = 0.0
    for i in range(1, n):
        s += 1.0/i/i
    result = math.sqrt(6*s)
    cache.set(n, result)
    return jsonify({"cached": False, "result": result})

if __name__ == '__main__':
    app.run()
```

运行python flask_pi.py，打开浏览器访问http://localhost:5000/pi?n=1000000，可以看到页面输出

```shell
{
  "cached": false,
  "result": 3.141591698659554
}
```

#### 改进：分布式缓存——redis

上面的缓存仅仅是内存缓存，有问题：
- 进程重启后，缓存结果消失，下次计算又得重新开始。
- 如果开启第二个端口5001来提供服务，那这第二个进程也无法享受第一个进程的内存缓存，而必须重新计算。
所以这里要引入**分布式缓存**Redis来共享计算缓存，避免跨进程重复计算，避免重启重新计算。

```python
import math
import redis

from flask import Flask, request
from flask.json import jsonify

app = Flask(__name__)


class PiCache(object):
    # 使用redis实现分布式缓存
    def __init__(self, client):
        self.client = client # redis连接

    def set(self, n, result):
        # 设置值
        self.client.hset("pis", str(n), str(result))

    def get(self, n):
        # 获取值
        result = self.client.hget("pis", str(n))
        if not result:
            return
        return float(result)

client = redis.StrictRedis() # redis客户端
cache = PiCache(client) # 缓存值


@app.route("/pi")
def pi():
    n = int(request.args.get('n', '100'))
    result = cache.get(n)
    if result:
        return jsonify({"cached": True, "result": result})
    s = 0.0
    for i in range(1, n):
        s += 1.0/i/i
    result = math.sqrt(6*s)
    cache.set(n, result)
    return jsonify({"cached": False, "result": result})

if __name__ == '__main__':
    app.run('127.0.0.1', 5000)
```

#### 改进：数据库

sql语句就可以操作数据库

ORM框架
- O是object，也就**类对象**的意思，R是relation，翻译成中文是**关系**，也就是关系数据库中**数据表**的意思，M是mapping，是**映射**的意思。
- ORM框架帮我们把类和数据表进行了一个映射，可以通过**类**和**类对象**就能操作所对应的表格中的数据。
- ORM框架还有一个功能，可以根据我们设计的类**自动生成数据库中的表格**，省去了自己建表的过程。
- ![](https://pic4.zhimg.com/80/v2-65fcb5f05e5d322adb53136ba320a65b_1440w.jpg)

用Flask进行数据库开发的步骤如下：
- 配置连接数据库的选项
- 定义模型类
- 通过类和对象完成数据库增删改查操作

详见：[flask的orm框架(SQLAlchemy)](https://www.cnblogs.com/chichung/p/9782919.html)

| 名字    | 备注| 
| SQLALCHEMY_DATABASE_URI   | 用于连接的数据库 URI 。例如: sqlite:////tmp/test.dbmysql://username:password@server/db |
| SQLALCHEMY_BINDS |    一个映射 binds 到连接 URI 的字典。更多 binds 的信息见用 Binds 操作多个数据库。|
| SQLALCHEMY_ECHO   | 如果设置为Ture， SQLAlchemy 会记录所有 发给 stderr 的语句，这对调试有用。(打印sql语句) |
| SQLALCHEMY_RECORD_QUERIES | 可以用于显式地禁用或启用查询记录。查询记录 在调试或测试模式自动启用。更多信息见get_debug_queries()。|
| SQLALCHEMY_NATIVE_UNICODE | 可以用于显式禁用原生 unicode 支持。当使用 不合适的指定无编码的数据库默认值时，这对于 一些数据库适配器是必须的（比如 Ubuntu 上 某些版本的 PostgreSQL ）。|
| SQLALCHEMY_POOL_SIZE  | 数据库连接池的大小。默认是引擎默认值（通常 是 5 ）|
| SQLALCHEMY_POOL_TIMEOUT   | 设定连接池的连接超时时间。默认是 10 。|
| SQLALCHEMY_POOL_RECYCLE   | 多少秒后自动回收连接。这对 MySQL 是必要的， 它默认移除闲置多于 8 小时的连接。注意如果 使用了 MySQL ， Flask-SQLALchemy 自动设定 这个值为 2 小时。|

字段类型

| 类型名 | python中类型 | 说明 |
|---|---|---|
| Integer | int | 普通整数，一般是32位 |
| SmallInteger | int | 取值范围小的整数，一般是16位 |
| BigInteger | int或long | 不限制精度的整数 |
| Float | float | 浮点数 |
| Numeric | decimal.Decimal | 普通整数，一般是32位 |
| String | str | 变长字符串 |
| Text | str | 变长字符串，对较长或不限长度的字符串做了优化 |
| Unicode | unicode | 变长Unicode字符串 |
| UnicodeText | unicode | 变长Unicode字符串，对较长或不限长度的字符串做了优化 |
| Boolean | bool | 布尔值 |
| Date | datetime.date | 时间 |
| Time | datetime.datetime | 日期和时间 |
| LargeBinary | str | 二进制文件 |

字段属性：

| 选项名 | 说明 | 
|---|---|
| primary_key | 如果为True，代表表的主键 | 
| unique | 如果为True，代表这列不允许出现重复的值 | 
| index | 如果为True，为这列创建索引，提高查询效率 | 
| nullable | 如果为True，允许有空值，如果为False，不允许有空值 | 
| default | 为这列定义默认值 | 


```python
from flask import Flask

# Flask中使用mysql数据库，需要安装一个flask-sqlalchemy的扩展
# pip install flask-sqlalchemy
# 要连接mysql数据库，还需要安装 flask-mysqldb
# pip install flask-mysqldb

from flask_sqlalchemy import SQLAlchemy


app = Flask(__name__)

#设置连接数据库的URL
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:mysql@127.0.0.1:3306/Flask_test'
#设置每次请求结束后会自动提交数据库中的改动
app.config['SQLALCHEMY_COMMIT_ON_TEARDOWN'] = True
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = True
#查询时会显示原始SQL语句
app.config['SQLALCHEMY_ECHO'] = True
db = SQLAlchemy(app)

class Role(db.Model):
    # 定义表名
    __tablename__ = 'roles'
    # 定义列对象，字段属性
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(64), unique=True)
    us = db.relationship('User', backref='role')


class User(db.Model):
    __tablename__ = 'users'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(64), unique=True, index=True)
    email = db.Column(db.String(64),unique=True)
    pswd = db.Column(db.String(64))
    role_id = db.Column(db.Integer, db.ForeignKey('roles.id'))

if __name__ == '__main__':
    db.drop_all() # 删表
    db.create_all() # 建表
    ro1 = Role(name='admin')
    ro2 = Role(name='user')
    #db.session.add(ro1) # 插入数据——单挑
    db.session.add_all([ro1,ro2]) # 插入数据——多条
    db.session.commit() # 修改已有表，需要额外commit才生效
    us1 = User(name='wang',email='wang@163.com',pswd='123456',role_id=ro1.id)
    us2 = User(name='zhang',email='zhang@189.com',pswd='201512',role_id=ro2.id)
    us3 = User(name='chen',email='chen@126.com',pswd='987654',role_id=ro2.id)
    us4 = User(name='zhou',email='zhou@163.com',pswd='456789',role_id=ro1.id)
    db.session.add_all([us1,us2,us3,us4])
    db.session.commit()
    User.query.get(name='wang') # 查询主键
    #查询roles表id为1的角色
    ro1 = Role.query.get(1)
    #查询该角色的所有用户
    ro1.us
    User.query.all() # 返回所有查询结果
    User.query.first() # 返回查询结果第一个
    User.query.filter_by(name='wang').all() # 查询：wang
    User.query.filter(User.name!='wang').all() # 逻辑非，返回名字不等于wang的所有数据
    User.query.filter(User.name.endswith('g')).all() # 模糊查询
    # 逻辑与
    from sqlalchemy import and_
    User.query.filter(and_(User.name!='wang',User.email.endswith('163.com'))).all()
    # 逻辑或
    from sqlalchemy import or_
    User.query.filter(or_(User.name!='wang',User.email.endswith('163.com'))).all()
    user = User.query.first()
    user.name = 'dong' # 更新数据
    User.query.filter_by(name='zhang').update({'name':'li'}) # 更新数据：update
    db.session.commit()
    
    db.session.delete(user) # 删除数据
    db.session.commit()
    User.query.all()
    
    app.run(debug=True)
```

表结构

| 列名 | 说明 | 类型 | 
| --- | --- | --- | 
| id | 表id（自动递增，主键） | int | 
| provincename | 省名 | varchar |
 | cityname | 省会名 | varchar |
| usernumber | 用户数量 | int |

完整代码：

```python
from flask import Flask,render_template,request,redirect
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)

#设置数据库连接
host='127.0.0.1'
user='root'
passwd="wangqiwen"
port=3306
db='newhouse_database'
#设置连接数据库的URL
#app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:asd8283676@127.0.0.1:3306/web'
app.config['SQLALCHEMY_DATABASE_URI'] = f'mysql://{user}:{passwd}@{host}:{port}/{db}'
app.logger.info(app.config['SQLALCHEMY_DATABASE_URI'])
#设置每次请求结束后会自动提交数据库中的改动
app.config['SQLALCHEMY_COMMIT_ON_TEARDOWN'] = True
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = True
#查询时会显示原始SQL语句
app.config['SQLALCHEMY_ECHO'] = True
db = SQLAlchemy(app) # [2022-3-2]注意：先配置app再包sql！否则报错！https://blog.csdn.net/weixin_45455015/article/details/100059108

#设置数据库连接
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:asd8283676@127.0.0.1:3306/web'

#定义模型
class City(db.Model):
    # 表模型
    id = db.Column(db.Integer,primary_key=True,autoincrement=True)
    provincename = db.Column(db.String(255))
    cityname = db.Column(db.String(255))
    usernumber = db.Column(db.Integer)

#查询所有数据
@app.route("/select")
def selectAll():
    cityList = City.query.order_by(City.id.desc()).all()
    return render_template("index.html",city_list = cityList)

@app.route('/')
def index():
    return selectAll()

#添加数据
@app.route('/insert',methods=['GET','POST'])
def insert():
    #进行添加操作
    province = request.form['province']
    cityname = request.form['city']
    number = request.form['number']
    city = City(provincename=province,cityname=cityname,usernumber=number)
    db.session.add(city)
    db.session.commit()
    #添加完成重定向至主页
    return redirect('/')

@app.route("/insert_page")
def insert_page():
    #跳转至添加信息页面
    return render_template("insert.html")


#删除数据
@app.route("/delete",methods=['GET'])
def delete():
    #操作数据库得到目标数据，before_number表示删除之前的数量，after_name表示删除之后的数量
    id = request.args.get("id")
    city = City.query.filter_by(id=id).first()
    db.session.delete(city)
    db.session.commit()
    return redirect('/')

#修改操作
@app.route("/alter",methods=['GET','POST'])
def alter():
    # 可以通过请求方式来改变处理该请求的具体操作
    # 比如用户访问/alter页面  如果通过GET请求则返回修改页面 如果通过POST请求则使用修改操作
    if request.method == 'GET':
        id = request.args.get("id")
        province = request.args.get("provincename")
        cityname = request.args.get("cityname")
        usernumber = request.args.get("usernumber")
        city = City(id = id,provincename=province,cityname=cityname,usernumber = usernumber)
        return render_template("alter.html",city = city)
    else:
        #接收参数，修改数据
        id = request.form["id"]
        province = request.form['province']
        cityname = request.form['city']
        number = request.form['number']
        city = City.query.filter_by(id = id).first()
        city.provincename = province
        city.cityname = cityname
        city.usernumber = number
        db.session.commit()
        return redirect('/')

if __name__ == "__main__":
    db.create_all() # 必须有，自动建表
    app.run(debug = True,host='0.0.0.0',port=8080)
```

定义模板：
- index.html
- alter.html
- insert.html

网页美化：
- 将[Layui](https://layuiweb.com/index.htm)相关文件放入项目的static目录下（static目录用于存放静态文件如css，js文件可以让网站看起来更加美观）
  - ![](https://pic2.zhimg.com/80/v2-d14699e06ba1c46ca539de4596b98e91_1440w.jpg)
- 添加到各个html页面：引入LayUI需要在HTML文件中导入LayUI的CSS文件和JS模块

效果：
- ![](https://pic4.zhimg.com/80/v2-7f4c4182f3534b952c6b9b3ee58a02a3_1440w.jpg)
- ![](https://pic3.zhimg.com/80/v2-0ea376382cd4496789b12a3965c55bd6_1440w.jpg)


#### 改进：域名

购买域名
- 在公网的机器上部署好的服务器，现在还只能通过公网访问，要想通过域名能访问网站，还需要设置域名解析。
- 要给 web服务配置上域名，还需要到服务商网站购买域名，例如：阿里云、腾讯云等等。
域名备案
- 购买了域名之后无法直接使用，需要进行域名备案，同样在服务商那里备案即可。

#### 改进：反向代理

反向代理
- 域名解析完成之后，还需要做的工作是反向代理。
- 使用 nginx 服务器可以作为域名代理服务。

通过这几个步骤就可以让你的web服务成为面向互联网的web服务啦。

可以参考的最简单 nginx 配置文件配置如下：
- 这个代表将 http://flask.codejiaonang.com 域名，映射到本地web服务器的8848端口

```json
server {
listen 80;
autoindex on;
server_name flask.codejiaonang.com;
access_log /usr/local/nginx/logs/access.log combined;
location / {
        proxy_pass http://127.0.0.1:8848/;

        }
}
```

#### 扩展：类形式api,MethodView

【2022-3-2】[Github上最受欢迎的Python轻量级框架Flask入门](https://zhuanlan.zhihu.com/p/37750930)

类似Django，Flask也支持**类形式**的API编写方式。下面使用Flask原生支持的MethodView来改写一下上面的服务

```python
import math
import redis

from flask import Flask, request
from flask.json import jsonify
from flask.views import MethodView

app = Flask(__name__)


class PiCache(object):

    def __init__(self, client):
        self.client = client

    def set(self, n, result):
        self.client.hset("pis", str(n), str(result))

    def get(self, n):
        result = self.client.hget("pis", str(n))
        if not result:
            return
        return float(result)

client = redis.StrictRedis()
cache = PiCache(client)

class PiAPI(MethodView):

    def __init__(self, cache):
        self.cache = cache

    def get(self, n):
        result = self.cache.get(n)
        if result:
            return jsonify({"cached": True, "result": result})
        s = 0.0
        for i in range(1, n):
            s += 1.0/i/i
        result = math.sqrt(6*s)
        self.cache.set(n, result)
        return jsonify({"cached": False, "result": result})

# as_view提供了参数可以直接注入到MethodView的构造器中
# 我们不再使用request.args，而是将参数直接放进URL里面，这就是RESTFUL风格的URL
app.add_url_rule('/pi/<int:n>', view_func=PiAPI.as_view('pi', cache))


if __name__ == '__main__':
    app.run('127.0.0.1', 5000)
```

flask默认的MethodView挺好用，但是也不够好用，它无法在一个类里提供多个不同URL名称的API服务。所以接下来引入flask的扩展flask-classy来解决这个问题。

```python
import math
import redis

from flask import Flask
from flask.json import jsonify
from flask_classy import FlaskView, route  # 扩展

app = Flask(__name__)

# pi的cache和fib的cache要分开
class PiCache(object):

    def __init__(self, client):
        self.client = client

    def set_fib(self, n, result):
        self.client.hset("fibs", str(n), str(result))

    def get_fib(self, n):
        result = self.client.hget("fibs", str(n))
        if not result:
            return
        return int(result)

    def set_pi(self, n, result):
        self.client.hset("pis", str(n), str(result))

    def get_pi(self, n):
        result = self.client.hget("pis", str(n))
        if not result:
            return
        return float(result)


client = redis.StrictRedis()
cache = PiCache(client)


class MathAPI(FlaskView):

    @route("/pi/<int:n>")
    def pi(self, n):
        result = cache.get_pi(n)
        if result:
            return jsonify({"cached": True, "result": result})
        s = 0.0
        for i in range(1, n):
            s += 1.0/i/i
        result = math.sqrt(6*s)
        cache.set_pi(n, result)
        return jsonify({"cached": False, "result": result})

    @route("/fib/<int:n>")
    def fib(self, n):
        result, cached = self.get_fib(n)
        return jsonify({"cached": cached, "result": result})

    def get_fib(self, n): # 递归，n不能过大，否则会堆栈过深溢出stackoverflow
        if n == 0:
            return 0, True
        if n == 1:
            return 1, True
        result = cache.get_fib(n)
        if result:
            return result, True
        result = self.get_fib(n-1)[0] + self.get_fib(n-2)[0]
        cache.set_fib(n, result)
        return result, False

MathAPI.register(app, route_base='/')  # 注册到app


if __name__ == '__main__':
    app.run('127.0.0.1', 5000)
```


### app配置


```python
app = Flask(__name__)    # 这是实例化一个Flask对象，最基本的写法
app = Flask(__name__, template_folder='templates', static_url_path='/xxxxxx')

# Flask初始化
def __init__(self, import_name, static_path=None, static_url_path=None,
                 static_folder='static', template_folder='templates',
                 instance_path=None, instance_relative_config=False,
                 root_path=None):

app.run('127.0.0.1', 5000) # 设置ip、端口
app.run(debug=True)  # debug = True 是指进入调试模式
```

其它参数：
- template_folder：**模板**所在文件夹的名字
- root_path：可以不用填，会自动找到，当前执行文件，所在目录地址，在return render_template时会将上面两个进行拼接，找到对应的模板地址
- static_folder：**静态文件**所在文件的名字，默认是static，可以不用填
- static_url_path：静态文件的**地址前缀**，写成什么，访问静态文件时，就要在前面加上这个
  - 在根目录下创建目录，templates和static，则return render_template时，可以找到里面的模板页面；
  - 如在static文件夹里存放11.png，在引用该图片时，静态文件地址为：/xxxxxx/11.png
- instance_relative_config：默认为False，当设置为True时，from_pyfile会从instance_path指定的地址下查找文件。
- instsnce_path：指定from_pyfile查询文件的路径，不设置时，默认寻找和app.run()的执行文件同级目录下的instance文件夹；如果配置了instance_path（注意需要是绝对路径），就会从指定的地址下里面的文件
  - instance_path和instance_relative_config是配合来用的、这两个参数是用来**找配置文件**的，当用 app.config.from_pyfile('settings.py')这种方式导入配置文件的时候会用到


### 路由

添加路由关系的本质：
- 将url和视图函数封装成一个**Rule对象**，添加到Flask的url_map字段中

#### 绑定方式

路由绑定的两种方式

```python
#方式一
@app.route('/index.html',methods=['GET','POST'],endpoint='index')
def index():
    return 'Index'
        
#方式二
def index():
    return "Index"

self.add_url_rule(rule='/index.html', endpoint="index", view_func=index, methods=["GET","POST"])    #endpoint是别名
# 或
app.add_url_rule(rule='/index.html', endpoint="index", view_func=index, methods=["GET","POST"])
app.view_functions['index'] = index
```

#### 路由参数

可传入参数：

```python
@app.route('/user/<username>')   # 常用   不加参数的时候默认是字符串形式的
@app.route('/post/<int:post_id>')  # 常用   #指定int，说明是整型的
@app.route('/post/<float:post_id>')
@app.route('/post/<path:path>')
@app.route('/login', methods=['GET', 'POST'])

# 类型说明
DEFAULT_CONVERTERS = {
    'default':          UnicodeConverter,
    'string':           UnicodeConverter,
    'any':              AnyConverter,
    'path':             PathConverter,
    'int':              IntegerConverter,
    'float':            FloatConverter,
    'uuid':             UUIDConverter,
}
```

#### 反向生成URL： url_for

endpoint("name")   # 别名，相当于django中的name

 
```python
from flask import Flask, url_for

@app.route('/index',endpoint="xxx")  #endpoint是别名
def index():
    v = url_for("xxx")
    print(v)
    return "index"

@app.route('/zzz/<int:nid>',endpoint="aaa")  #endpoint是别名
def zzz(nid):
    v = url_for("aaa",nid=nid)
    print(v)
    return "index2"

# =============== 子域名访问============
@app.route("/static_index", subdomain="admin")
def static_index():
    return "admin.bjg.com"

# ===========动态生成子域名===========
@app.route("/index",subdomain='<xxxxx>')
def index(xxxxx):
    return "%s.bjg.com" %(xxxxx,)

```

### 传参

传递请求参数的方式有两种
- 一是打包成 JSON 之后再传递
  - 一般用 `POST` 请求来传递参数，然后用 FLASK 中 request 模块的 get_json() 方法获取参数。
- 二是直接放进 URL 进行传递 。
  - 示例URL:　 http://127.0.0.1:5000/login?name=%27%E8%83%A1%E5%86%B2%27&nid=2
  - 一般用 `GET` 请求传递参数，然后从 request.args 中用 get() 方法获取参数
  - 不过需要说明的是用 POST 请求也可以通过 URL 的方式传递参数，而且获取参数的方式与 GET 请求相同。

获取请求数据，及相应
- request
  - request.form   # POST请求的数据
  - request.args   # GET请求的数据，不是完全意义上的字典，通过.to_dict可以转换成字典
  - request.querystring  #GET请求，bytes形式的
- response
  - return render_tempalte()    
  - return redirect()
  - return ""
  - v = make_response(返回值)  #可以把返回的值包在了这个函数里面，然后再通过.set_cookie绑定cookie等
- session
  - 存在浏览器上，并且是加密的
  - 依赖于：secret_key


```python
from flask import request, jsonify

@app.route('/', methods = ["GET", "POST"])
def post_data():
    # 假设有如下 JSON 数据
    #{"obj": [{"name":"John","age":"20"}] }
    
    #可以通过 request 的 args 属性来获取GET参数
    name = request.args.get("name")
    age = request.args.get("age")

    # ----- POST -----
    # 方法一
    data = request.get_json()                # 获取 JSON 数据
    data = pd.DataFrame(data["obj"])   # 获取参数并转变为 DataFrame 结构
    
    # 方法二
    # data = request.json        # 获取 JOSN 数据
    # data = data.get('obj')     #  以字典形式获取参数
    
    # ======= 统一 ======
    if request.method == 'POST':
        data = request.json
        data = request.form.to_dict()
        data = request.values
    elif request.method == 'GET':
        data = request.args

    # 经过处理之后得到要传回的数据
    res = some_function(data)
    
    # 将 DataFrame  数据再次打包为 JSON 并传回
    # 方法一
    res = '\{\{"obj": {} \} }'.format(res.to_json(orient = "records", force_ascii = False))
    # 方法二
    # res = jsonify({"obj":res.to_json(orient = "records", force_ascii = False)})
    
    return res
```

### app模块化 blueprint

一个py文件中写入了很多路由, 维护代码非常麻烦，时长出现重名，错乱，然而并不能想普通py代码一样直接导入别的路由 → 路由**模块化**

Flask提供一个特有的模块化处理方式blueprint，Blueprint 是一个存储操作方法的容器，这些操作在这个Blueprint 被注册到一个应用之后就可以被调用，Flask 可以通过Blueprint来组织URL以及处理请求。

Flask使用Blueprint让应用实现模块化，在Flask中，Blueprint具有如下属性：
- 一个应用app可以具有多个Blueprint
- 可以将一个Blueprint**注册**到任何一个未使用的URL下比如 “/”、“/sample”或者子域名
- 在一个应用中，一个模块可以注册**多次**
- Blueprint可以单独具有自己的模板、静态文件或者其它的通用操作方法，它并不是必须要实现应用的视图和函数的
- 在一个应用初始化时，就应该要注册需要使用的Blueprint
但是一个Blueprint并不是一个完整的应用，它不能独立于应用运行，而必须要注册到某一个应用中。

蓝图/Blueprint对象用起来和一个应用/Flask对象差不多，最大的区别在于**蓝图对象没有办法独立运行**，必须将它注册到一个应用对象上才能生效

使用蓝图可以分为三个步骤, 应用启动后,通过/admin/可以访问到蓝图中定义的视图函数

```python
# 1,创建一个蓝图对象
admin=Blueprint('admin',__name__)　
# 2,在这个蓝图对象上进行操作,注册路由,指定静态文件夹,注册模版过滤器
@admin.route('/')
def admin_home():
    return 'admin_home'
# 3,在应用对象上注册这个蓝图对象
app.register_blueprint(admin,url_prefix='/admin')

```

运行机制
- 蓝图是保存了一组将来可以在应用对象上执行的操作，注册路由就是一种操作. 当在应用对象上调用 route 装饰器注册路由时,这个操作将修改对象的url_map路由表. 然而，蓝图对象根本没有路由表，当我们在蓝图对象上调用route装饰器注册路由时,它只是在内部的一个延迟操作记录列表defered_functions中添加了一个项
- 当执行应用对象的 register_blueprint() 方法时，应用对象将从蓝图对象的 defered_functions 列表中取出每一项，并以自身作为参数执行该匿名函数，即调用应用对象的 add_url_rule() 方法，这将真正的修改应用对象的路由表

蓝图的url前缀
- 当我们在应用对象上注册一个蓝图时，可以指定一个url_prefix关键字参数（这个参数默认是/）
- 在应用最终的路由表 url_map中，在蓝图上注册的路由URL自动被加上了这个前缀，这个可以保证在多个蓝图中使用相同的URL规则而不会最终引起冲突，只要在注册蓝图时将不同的蓝图挂接到不同的自路径即可

url_for
url_for('admin.index') # /admin/


### 全局变量

- 参考: [Flask 上下文全局变量](https://www.jianshu.com/p/dfe1ee1dc1ec)
- Flask 在分发请求之前激活(或推送)程序和请求上下文，请求处理完成后再将其删除。程 序上下文被推送后，就可以在线程中使用 current_app 和 g 变量。类似地，请求上下文被 推送后，就可以使用 request 和 session 变量。如果使用这些变量时我们没有激活程序上 下文或请求上下文，就会导致错误。

| 变量名      | 上下文     | 说明                                                   |
| ----------- | ---------- | ------------------------------------------------------ |
| current_app | 程序上下文 | 当前激活程序的程序实例                                 |
| g           | 程序上下文 | 处理请求时用作临时存储的对象，每次请求都会重设这个变量 |
| request     | 请求上下文 | 请求对象，封装了客户端发出的HTTP请求中的内容           |
| session     | 请求上下文 | 用户会话，用于存储请求之间需要记住的值的词典           |

- 代码示例：[flask中4种全局变量](https://www.jianshu.com/p/f24e2c9b548e)

#### session

**Session设置**

- 代码

```python
from flask import Flask,session
import os
from datetime import timedelta

app = Flask(__name__)
app.secret_key = "sdsfdsgdfgdfgfh"   # 设置session时，必须要加盐，否则报错

app.config['SECRET_KEY'] = os.urandom(24)
app.config['PERMANENT_SESSION_LIFETIME'] = timedelta(days=7)
#SESSION_TYPE = "redis"
# 添加数据到session中
# 操作session的时候 跟操作字典是一样的。
# SECRET_KEY

@app.route('/')
def hello_world():
    session['username'] = 'zhangsan'
    # 如果没有指定session的过期时间，那么默认是浏览器关闭就自动结束
    # 如果设置了session的permanent属性为True，那么过期时间是31天。
    session.permanent = True
    return 'Hello World!'

@app.route('/get/')
def get():
    # session['username']   如果username不存在则会抛出异常
    # session.get('username')   如果username不存在会得到 none 不会报错 推荐使用
    return session.get('username')

@app.route('/delete/')
def delete():
    print(session.get('username'))
    session.pop('username')
    print(session.get('username'))
    return 'success'

@app.route('/clear/')
def clear():
    print(session.get('username'))
    # 删除session中的所有数据
    session.clear()
    print(session.get('username'))
    return 'success'

if __name__ == '__main__':
    app.run(debug=True)
```

### 分布式session

- 【2020-9-11】以上代码仅适用单机版，如果部署在分布式环境，流量负载均衡，会出现session找不到的现象
- 分布式session一致性：
  - 客户端发送一个请求，经过负载均衡后该请求会被分配到服务器中的其中一个，由于不同服务器含有不同的web服务器(例如Tomcat)，不同的web服务器中并不能发现之前web服务器保存的session信息，就会再次生成一个JSESSIONID，之前的状态就会丢失

- 【2020-9-18】Flask Session共享的一种实现方式：使用出问题（待核实原因），改用redis直接存储session变量
  - [flask-session 在redis中存储session](https://www.cnblogs.com/jackadam/p/9822680.html)

```python
import os
from flask import Flask, session, request
from flask_session import Session
from redis import Redis

app = Flask(__name__)
app.config['SESSION_TYPE'] = 'redis'   #session存储格式为redis
app.config['SESSION_REDIS'] = Redis(    #redis的服务器参数
    host='192.168.1.3',                 #服务器地址
    port=6379)                           #服务器端口

app.config['SESSION_USE_SIGNER'] = True   #是否强制加盐，混淆session
app.config['SECRET_KEY'] = os.urandom(24)  #如果加盐，那么必须设置的安全码，盐
app.config['SESSION_PERMANENT'] = False  #sessons是否长期有效，false，则关闭浏览器，session失效
app.config['PERMANENT_SESSION_LIFETIME'] = 3600   #session长期有效，则设定session生命周期，整数秒，默认大概不到3小时。
Session(app)


@app.route('/')
def default():
    return session.get('key', 'not set')

@app.route('/test/')
def test():
    session['key'] = 'test'
    return 'ok'

@app.route('/set/')
def set():
    arg = request.args.get('key')
    print(arg)
    session['key'] = arg
    return 'ok'


@app.route('/get/')
def get():
    return session.get('key', 'not set')


@app.route('/pop/')
def pop():
    session.pop('key')
    return session.get('key', 'not set')


@app.route('/clear/')
def clear():
    session.clear()
    return session.get('key', 'not set')

if __name__ == "__main__":
    app.run(debug=True)
```


- 解决方法：
    - 参考：
        - [如何配置 flask将session 保存在redis中](https://www.cnblogs.com/wangkun122/articles/9118009.html)
        - [4种分布式session解决方案](https://blog.csdn.net/qq_35620501/article/details/95047642)
        - [分布式session的几种实现方式](https://www.cnblogs.com/daofaziran/p/10933221.html)

- 方案一：**客户端存储**
    - 直接将信息存储在cookie中
    - cookie是存储在客户端上的一小段数据，客户端通过http协议和服务器进行cookie交互，通常用来存储一些不敏感信息
    - 缺点：
        - 数据存储在客户端，存在安全隐患
        - cookie存储大小、类型存在限制
        - 数据存储在cookie中，如果一次请求cookie过大，会给网络增加更大的开销
- 方案二：**session复制**
    - session复制是小型企业应用使用较多的一种服务器集群session管理机制，在真正的开发使用的并不是很多，通过对web服务器(例如Tomcat)进行搭建集群。
    - 存在的问题：
        - session同步的原理是在同一个局域网里面通过发送广播来异步同步session的，一旦服务器多了，并发上来了，session需要同步的数据量就大了，需要将其他服务器上的session全部同步到本服务器上，会带来一定的网路开销，在用户量特别大的时候，会出现内存不足的情况
    - 优点：
        - 服务器之间的session信息都是同步的，任何一台服务器宕机的时候不会影响另外服务器中session的状态，配置相对简单
        - Tomcat内部已经支持分布式架构开发管理机制，可以对tomcat修改配置来支持session复制，在集群中的几台服务器之间同步session对象，使每台服务器上都保存了所有用户的session信息，这样任何一台本机宕机都不会导致session数据的丢失，而服务器使用session时，也只需要在本机获取即可
- 方案三：**session绑定**
    - Nginx介绍：Nginx是一款自由的、开源的、高性能的http服务器和反向代理服务器
    - Nginx能做什么：反向代理、负载均衡、http服务器（动静代理）、正向代理
    - 如何使用nginx进行session绑定
        - 利用nginx的反向代理和负载均衡，之前是客户端会被分配到其中一台服务器进行处理，具体分配到哪台服务器进行处理还得看服务器的负载均衡算法(轮询、随机、ip-hash、权重等)，但是我们可以基于nginx的ip-hash策略，可以对客户端和服务器进行绑定，同一个客户端就只能访问该服务器，无论客户端发送多少次请求都被同一个服务器处理
    - 缺点：
        - 容易造成单点故障，如果有一台服务器宕机，那么该台服务器上的session信息将会丢失
        - 前端不能有负载均衡，如果有，session绑定将会出问题
    - 优点：
        - 配置简单
- 方案四：**session持久化到数据库**
    - 如：基于redis存储session方案
    - 原理：就不用多说了吧，拿出一个数据库，专门用来存储session信息。保证session的持久化。
    - 优点：服务器出现问题，session不会丢失
    - 缺点：如果网站的访问量很大，把session存储到数据库中，会对数据库造成很大压力，还需要增加额外的开销维护数据库。
    - 优点：
        - 企业中使用的最多的一种方式
        - spring为我们封装好了spring-session，直接引入依赖即可
        - 数据保存在redis中，无缝接入，不存在任何安全隐患
        - redis自身可做集群，搭建主从，同时方便管理
    - 缺点：
        - 多了一次网络调用，web容器需要向redis访问
    - 基于redis存储session方案流程示意图
![](https://img-blog.csdnimg.cn/2019070810495327.png)

- 方案五：**session复制**
    - terracotta实现session复制
    - Terracotta的基本原理是对于集群间共享的数据，当在一个节点发生变化的时候，Terracotta只把变化的部分发送给Terracotta服务器，然后由服务器把它转发给真正需要这个数据的节点。对服务器session复制的优化。

```python
SESSION_TYPE = "redis"

#在settings.py中写上这句话就能够让flask把session写在  redis中去
SESSION_REDIS = Redis(host='192.168.0.94', port='6379')

```

- 【2020-9-24】[深夜，我偷听到程序员要对Session下手……](https://www.toutiao.com/i6875568455475528203/)，演变历史：
    - **单机服务器**(静态) → 单机服务器(动态) → **分布式服务器**（Nginx） → Redis**独立存储** → **Token时代**
    - （1）**单台Web服务器-静态**：一个web服务器，每天处理的不过是一些静态资源文件，像HTML、CSS、JS、图片等等，按照HTTP协议的规范处理请求即可。
        - ![](https://p6-tt.byteimg.com/origin/pgc-image/da73c2849fb04ad3b5e47ec55dc47d0a)
    - （2）**单台Web服务器-动态**：
        - 动态交互的网络应用开始如雨后春笋般涌现，像各种各样的论坛啊，购物网站啊之类
        - Session诞生：记住每一个请求背后的用户是谁
        - 浏览器登陆以后，服务器分配一个session id，表示一个会话，然后返回给浏览器保存着。后续再来请求的时候，带上，就能知道是谁
        - ![](https://p6-tt.byteimg.com/origin/pgc-image/1c616a10971e41929a9408e990eb3a12)
    - （3）**分布式Web服务器**：
        - 没几年，互联网的发展实在是太快，用户量蹭蹭上涨，session id数量也与日俱增，服务器不堪重负
        - 增加nginx来进行负载均衡，单台服务器变成了3台web服务器组成的小集群
        - ![](https://p6-tt.byteimg.com/origin/pgc-image/3fd170a8dec5461996e19b3d9c6ee107)
        - 压力虽然减少，但session id的管理问题却变得复杂起来
            - 请求如果发到某台机器，登记了session id，但下次请求说不定就发到第二胎，一会儿又发到第三台，这样各个服务器上的信息不一致，就会出现一些异常情况，用户估计要破口大骂：这什么辣鸡网站？
            - （3.1）nginx：同一个用户来的请求都发给同一台机器
        - 好景不长，各服务器相继出现宕机情况，这时候nginx还得把请求交给还在工作的机器，原来的问题就又出现了
            - （3.2）session同步：有新增、失效的情况都给其他机器招呼一下，大家都管理一份，这样就不会出现不一致的问题
            - ![](https://p3-tt.byteimg.com/origin/pgc-image/a94ead3997324b24ac73ad59cccdc576)
        - 搞了半天，又回到从前，一个人管理所有session id的情况了，不仅如此，还要抽出时间和几位兄弟同步，把session id搬来搬去，工作量不减反增了。
    - （4）**独立缓存**——Redis
        - session id都统一存在redis里面
        - ![](https://p6-tt.byteimg.com/origin/pgc-image/ea7f5139129c416ab80ca4efb60c2764)
    - （5）**Token时代**
        - Redis也不是万能的，也有崩溃的风险，一崩溃就全完了
        - JWT（JSON Web Token） 技术，硬说让redis来管理保存session id负担太重了，以后不保存了
        - 没有session id，但是换了一个token，用它来识别用户
        - ![](https://p3-tt.byteimg.com/origin/pgc-image/863480eff55a489b879373ff4fb7dcf1)
        - 第一部分是JWT的基本信息，然后把用户的身份信息放在第二部分，接着和第一部分合在一起做一个计算，计算的时候加入了一个只有我们才知道的密钥secretkey，计算结果作为第三部分。最后三部分拼在一起作为最终的token发送给客户端保存着···再收到这个token的时候，就可以通过同样的算法验证前面两部分的结果和第三部分是不是相同，就知道这个token是不是伪造的啦！因为密钥只有我们知道，别人没办法伪造出一个token的！最后确认有效之后，再取第二部分的用户身份信息，就知道这是谁了
        - ![](https://p3-tt.byteimg.com/origin/pgc-image/32c0bd9dfa704f0d808a452106bfa930)
    - JWT：目前有两种实现方式
        - ![](https://img2018.cnblogs.com/blog/1552472/201911/1552472-20191115165339758-1975183863.png)
        - JWS(JSON Web Signature)
            - ![](https://img2018.cnblogs.com/blog/1552472/201911/1552472-20191115161445577-896569505.png)
            - 分成三个部分：
                - 头部（Header）：用于描述关于该JWT的最基本的信息，例如:其类型、以及签名所用的算法等。JSON内容要经Base64 编码生成字符串成为Header。
                - 载荷（PayLoad）：payload的五个字段都是由JWT的标准所定义的。
                    - iss: 该JWT的签发者
                    - sub: 该JWT所面向的用户
                    - aud: 接收该JWT的一方
                    - exp(expires): 什么时候过期，这里是一个Unix时间戳
                    - iat(issued at): 在什么时候签发的
                    - 后面的信息可以按需补充。 JSON内容要经Base64 编码生成字符串成为PayLoad。
                - 签名（signature）：这个部分header与payload通过header中声明的加密方式，使用密钥secret进行加密，生成签名。JWS的主要目的是保证了数据在传输过程中不被修改，验证数据的完整性。但由于仅采用Base64对消息内容编码，因此不保证数据的不可泄露性。所以不适合用于传输敏感数据。
        - JWE(JSON Web Encryption)
            - 相对于JWS，JWE则同时保证了安全性与数据完整性。 JWE由五部分组成：
            - ![](https://img2018.cnblogs.com/blog/1552472/201911/1552472-20191115161640088-1851802272.png)
            - JWE的计算过程相对繁琐，不够轻量级，因此适合与数据传输而非token认证，但该协议也足够安全可靠，用简短字符串描述了传输内容，兼顾数据的安全性与完整性
            - 具体生成步骤为：
                - JOSE含义与JWS头部相同。
                - 生成一个随机的Content Encryption Key （CEK）。
                - 使用RSAES-OAEP 加密算法，用公钥加密CEK，生成JWE Encrypted Key。
                - 生成JWE初始化向量。
                - 使用AES GCM加密算法对明文部分进行加密生成密文Ciphertext,算法会随之生成一个128位的认证标记Authentication Tag。 6.对五个部分分别进行base64编码。
    -  Python实现：PyJWT
    - 详情：[flask项目--认证方案Json Web Token(JWT)](https://www.cnblogs.com/oklizz/p/11414429.html)

```python
import jwt
from jwt import PyJWTError
from datetime import datetime, timedelta

payload = {  # jwt设置过期时间的本质 就是在payload中 设置exp字段, 值要求为格林尼治时间
    "user_id": 1,
    'exp': datetime.utcnow() + timedelta(seconds=30)
}

screct_key = "test"
# 生成token
token = jwt.encode(payload, key=screct_key, algorithm='HS256')
print(token)
# 验签token  返回payload    pyjwt会自动校验过期时间
try:
    data = jwt.decode(token, key=screct_key, algorithms='HS256')
    print(data)
except PyJWTError as e:
    print("jwt验证失败: %s" % e)
```

### 分布式唯一id

Twitter使用的分布式唯一id

Snowflake算法代码:

```python
# Twitter's Snowflake algorithm implementation which is used to generate distributed IDs.
# https://github.com/twitter-archive/snowflake/blob/snowflake-2010/src/main/scala/com/twitter/service/snowflake/IdWorker.scala

import time
import logging

from src.utils.exceptions import InvalidSystemClock


# 64位ID的划分
WORKER_ID_BITS = 5
DATACENTER_ID_BITS = 5
SEQUENCE_BITS = 12

# 最大取值计算
MAX_WORKER_ID = -1 ^ (-1 << WORKER_ID_BITS)  # 2**5-1 0b11111
MAX_DATACENTER_ID = -1 ^ (-1 << DATACENTER_ID_BITS)

# 移位偏移计算
WOKER_ID_SHIFT = SEQUENCE_BITS
DATACENTER_ID_SHIFT = SEQUENCE_BITS + WORKER_ID_BITS
TIMESTAMP_LEFT_SHIFT = SEQUENCE_BITS + WORKER_ID_BITS + DATACENTER_ID_BITS

# 序号循环掩码
SEQUENCE_MASK = -1 ^ (-1 << SEQUENCE_BITS)

# Twitter元年时间戳
TWEPOCH = 1288834974657


logger = logging.getLogger('flask.app')


class IdWorker(object):
    """
    用于生成IDs
    """

    def __init__(self, datacenter_id, worker_id, sequence=0):
        """
        初始化
        :param datacenter_id: 数据中心（机器区域）ID
        :param worker_id: 机器ID
        :param sequence: 其实序号
        """
        # sanity check
        if worker_id > MAX_WORKER_ID or worker_id < 0:
            raise ValueError('worker_id值越界')

        if datacenter_id > MAX_DATACENTER_ID or datacenter_id < 0:
            raise ValueError('datacenter_id值越界')

        self.worker_id = worker_id
        self.datacenter_id = datacenter_id
        self.sequence = sequence

        self.last_timestamp = -1  # 上次计算的时间戳

    def _gen_timestamp(self):
        """
        生成整数时间戳
        :return:int timestamp
        """
        return int(time.time() * 1000)

    def get_id(self):
        """
        获取新ID
        :return:
        """
        timestamp = self._gen_timestamp()

        # 时钟回拨
        if timestamp < self.last_timestamp:
            logging.error('clock is moving backwards. Rejecting requests until {}'.format(self.last_timestamp))
            raise InvalidSystemClock

        if timestamp == self.last_timestamp:
            self.sequence = (self.sequence + 1) & SEQUENCE_MASK
            if self.sequence == 0:
                timestamp = self._til_next_millis(self.last_timestamp)
        else:
            self.sequence = 0

        self.last_timestamp = timestamp

        new_id = ((timestamp - TWEPOCH) << TIMESTAMP_LEFT_SHIFT) | (self.datacenter_id << DATACENTER_ID_SHIFT) | \
                 (self.worker_id << WOKER_ID_SHIFT) | self.sequence
        return new_id

    def _til_next_millis(self, last_timestamp):
        """
        等到下一毫秒
        """
        timestamp = self._gen_timestamp()
        while timestamp <= last_timestamp:
            timestamp = self._gen_timestamp()
        return timestamp

if __name__ == '__main__':
    worker = IdWorker(1, 2, 0)
    print(worker.get_id())
```


### 进程、协程和线程

定义
- `进程`：操作系统中资源分配/拥有的最小单位
- `线程`：操作系统中独立调度的最小单位
  - 线程是操作系统的内核对象，多线程编程时，如果线程数过多，就会导致频繁的上下文切换，这些 cpu时间是一个额外的耗费。
- `协程`：非操作系统调度，而是程序猿**代码**控制
  - 协程在应用层模拟的线程，避免了上下文切换的额外耗费，兼顾了多线程的优点。简化了高并发程序的复杂度。
  - **goroutine** 就是**协程**。 不同的是，Golang 在 runtime、系统调用等多方面对 goroutine 调度进行了封装和处理，当遇到长时间执行或者进行系统调用时，会主动把当前 goroutine 的CPU (P) 转让出去，让其他 goroutine 能被调度并执行，也就是 Golang 从语言层面支持了协程。

区别
- 线程之间可以共享内存
- ![](https://p1-tt.byteimg.com/origin/pgc-image/a8fe60b01ff7415a9ec130d8fcfcae2e?from=pc)


### flask高并发问题

- 【2021-6-18】[Flask+Gunicorn(协程)高并发的解决方法探究](https://www.toutiao.com/i6735284444271215117), 直接使用flask的python code.py的方式运行，简单但不能解决高并发问题，不稳定，有一定概率遇到连接超时无返回的情况，不能用于生产环境。
  - （1）通过设置app.run()的参数，来达到多进程的效果。但threaded与processes不能同时打开，如果同时设置的话，将会出错
    - app.run(threaded=1, processes=2)
  - （2）使用gevent做协程解决高并发问题
  - （3）通过Gunicorn(with gevent)的形式对app进行包装，从而来启动服务【推荐】
    - 指定进程和端口号： -w: 表示进程（worker） --bind：表示绑定ip地址和端口号（bind） —threads 多线程 -k 异步方案

[image](https://p1-tt.byteimg.com/origin/pgc-image/a8fe60b01ff7415a9ec130d8fcfcae2e?from=pc)

![](https://p1-tt.byteimg.com/origin/pgc-image/9102d29dad204e608f407ef1f84b3922?from=pc)


### flask异步调度

异步本质上并不比同步快。 在执行**并发IO** 绑定任务时， 异步是有益的。但是 对于 **CPU密集型** 任务，则未必有用，因此传统的 Flask 视图仍然适用于大多数用例。 Flask 异步支持的引入，带来本地化编写和使用异步代码的可能性。

由于 Flask本身的实现方式， Flask的异步支持性能不如**异步优先框架**。如果代码主要是基于异步的，那么可以考虑 Quart 。 Quart 是一个 Flask 的重新实现，但是基于 ASGI 标准而不是WSGI。这允许它处理大量并发请求、长时间运行 的请求和 websocket ，而不需要多个工作进程或线程。

同时，也可以用 **Gevent** 或 Eventlet 运行 Flask ，以获得**异步**请求处理的诸多好处。 这些库修补低级 Python 函数来实现这一点，而 async/ await 和 ASGI 则使用标准的现代 Python 功能。决定应该使用 Flask 还是 Quart 或其他东西 最终还是取决于项目的具体需求。

几种方案：
- flask async/await
- celery
- gevent

#### async/await

安装 Flask 时使用了额外的 async （ 即用 pip install flask\[async] 命令安装），那么路由、出错处理器、请求前、请 求后和拆卸函数都可以是协程函数。这样，视图可以使用 async def 定义，并 使用 await 。 pip install flask\[async] 命令会用到 contextvars.ContextVar ， 因此需要 Python 3.7 以上版本。

```python
@app.route("/get-data")
async def get_data():
    data = await async_db_query(...)
    return jsonify(data)
```


####  celery

【2021-12-1】[celery + flask 异步调用任务](https://blog.csdn.net/yukai08008/article/details/109725238)

有一些非常耗时的任务，无法实现实时的RPC调用。因此计划使用celery + flask提供异步任务调度服务

一个请求的服务过程是这样：
- 1 服务器接到一个请求（一个几k到几百k的文本）
- 2 服务器计算摘要作为键值，将其加入异步任务。
- 3 服务器将摘要返回，状态为calculating。
- 4 异步任务执行耗时计算，结果有两个副本。一个存在本地(pkl),一个发往目标服务器。这样如果目标服务器没收到，服务就把本地的副本读取再发送，避免重新计算。

```python
from flask import Flask
from celery import Celery # 消息队列中间件
from celery.result import AsyncResult
import time

app = Flask(__name__)
# 用以储存消息队列
app.config['CELERY_BROKER_URL'] = 'redis://127.0.0.1:6379/0'
# 用以储存处理结果
app.config['CELERY_RESULT_BACKEND'] = 'redis://127.0.0.1:6379/0'

celery_ = Celery(app.name, broker=app.config['CELERY_BROKER_URL'])
celery_.conf.update(app.config)


@celery_.task
def my_background_task(arg1, arg2):
     # 两数相加
     time.sleep(10)
     return arg1+arg2


@app.route("/sum/<arg1>/<arg2>")
def sum_(arg1, arg2):
    # 发送任务到celery,并返回任务ID,后续可以根据此任务ID获取任务结果
    result = my_background_task.delay(int(arg1), int(arg2))
    return result.id


@app.route("/get_result/<result_id>")
def get_result(result_id):
    # 根据任务ID获取任务结果
    result = AsyncResult(id=result_id)
    return str(result.get())
```

启动方法
- 作用分别是: 启动flask服务(基本web服务)，启动celery服务(异步任务)，启动flower服务（监控任务）

```shell
# 运行flask服务
gunicorn entry_ner_cpu:app -b 0.0.0.0:5001
# 使用celery执行异步任务
celery -A entry_ner_cpu.celery_ worker
# 使用flower监督异步任务
flower --basic_auth=admin:admin --broker=redis://127.0.0.1:6379/0 --address=0.0.0.0 --port=5556
```

flower监控面板
- ![](https://img-blog.csdnimg.cn/2020111619075451.png)


### 模板使用

[Flask模板引擎：Jinja2常用语法整理](https://www.jianshu.com/p/0a5f87332dea)

在jinja2中，存在三种语法：
- 1、控制结构 { % % }
- 2、变量取值 \{\{ \} }
- 3、注释 \{\# \#\}

jinja2支持python中所有的Python数据类型比如列表、字段、对象等

if/for控制语句

前端的Jinja2语法中，if可以进行判断：存在的参数是否满足条件。
- 跟python很像，只是需要添加：大括号+百分号
- ![](https://pic2.zhimg.com/80/v2-7c8b008840b966bee55a1ae71caf5e11_720w.jpg)

Jinja2的for循环变量不需要\{\{ \} }传入，不支持continue和break，但是和Python一样可以对Python的可迭代对象进行循环遍历。
- 每一次循环的对象是一个字段，使用.key直接拿到value值，如：\{\{ good.name \} }，或 \{\{ good[ "name" ] \} }
- 问题：如果不知道字典中key呢？当做list遍历key即可

在一个循环代码块内部调用loop的属性可以获得循环中的状态数据
- loop.index: 当前迭代的索引（从1开始）
- loop.index0: 当前迭代的索引（从0开始）
- loop.first: 是否是第一次迭代，返回True，False
- loop.last: 是否是最后一次迭代，返回True，False
- loop.length: 返回序列长度

过滤器
- 过滤器的本质是一个转换函数，有时候不仅需要输出程序参数还要对参数进行修改转换才能输出，此时需要用到过滤器，过滤器写在变量后面，中间用 | 隔开，|左右没有空格

自定义过滤器
- 可以自己用Python语言实现一个自定义过滤器使用add_template_filter进行注册调用，或者使用修饰器template_filter注册

```python
@app.template_filter('t_func')
def t_func(t):
    t2 = time.time()
    diff = t2 - t
    if diff < 60:
        return "刚刚"
    elif 60 <= diff < 60 * 60:
        return "%d分钟之前" % int(diff / 60)
    elif 3600 <= diff < 3600 * 24:
        return "%d小时之前" % int(diff / 3600)
    else:
        return "很久之前"
# 另一种注册方式
# app.add_template_filter(t_func, 't_func')
```

完整示例：

```python
from flask import Flask  #导入模块
from flask import render_template

app  = Flask(__name__)

@app.route('/table')  #定义第一页视图
def choice():
    goods = [{'name':'包包', 'price':'500元'}, \
             {'name':'口红', 'price':'300元'}, \
             {'name':'冰淇淋', 'price':'20元'}]
    # locals指定所有变量
    return render_template('goods.html', **locals()) # 直接传入局部变量

@app.route('/user')
def user():
    user = 'dongGe'
    #user = ['dongGe'] # 列表
    #user = {'name':'dongGe'} # 字典
    return render_template('user.html',user=user)

@app.route('/loop')
 def loop():
    fruit = ['apple','orange','pear','grape']
    return render_template('loop.html',fruit=fruit)
    #return render_template('first.html', **locals()) # 直接把当前所有变量传下去

if __name__ == '__main__':
    app.run(debug=True)
```

web页面代码 
- 注意：为了避开jeklly语法冲突，<font color='blue'>{号、%号和{或}中间间用空格隔开，实际使用时去掉！</font>

```html
 <html>
 <head> 
   <!-- 过滤器 -->
   <p>{ { name|default('小明', true)} }</p>
   <p>{ { name or '小明'} }</p>
   <p>{ { name|default('小明')|replace('小', '大')} }</p> <!-- 多个过滤器可以连续写 -->
   <p>{ { '   去除收尾空格   '|trim } }</p>
   <p>{ { '我和'~name~'出去玩了'} }</p> <!-- 字符串连接，用~ -->
   <!-- 可迭代对象过滤器 -->
   { % set items=[4,1,7,2] % }
   <p>第一个元素{ { items|first } }</p> <!-- 第一个元素 -->
   <p>最后一个元素{ { items|last } }</p> <!-- 最后一个元素 -->
   <!-- 函数 -->
   <p>列表长度{ { items|count } }</p>
   <p>{ { 2.22|string + '你好'} }</p> <!-- 转string -->
   <p>{ { 2.222|int } }</p>
   <p>{ { -2.222|abs } }</p>
   <p>{ { 2.222|round(2) } }</p>  <!-- 保留小数，比如保留2为小数 -->
   <p>{ { [3,6]|max } }</p> 
   <p>{ { [3,6]|min } }</p>
   <p>{ { [1, 2, 3, 4, 5]|reverse|join(',')} }</p> <!-- 反转、连接 -->
<!-- with语句定义的变量只能够在 with语句块中使用, 一旦超过了代码块就不能使用; -->
<!-- set 语句作用域比with大 -->
  { % set items=[4,1,7,2] % }
   <p>列表转字符串{ { [4,1,7,2]|join(',') } }</p> <!-- 元素连接 -->
   <p>列表升序{ { [4,1,7,2]|sort } }</p>
   <p>列表降序{ { [4,1,7,2]|sort(true) } }</p> <!-- 降序排列 -->

   { % set items = [{"name": "苹果", "price": 23}, {"name": "西瓜", "price": 33}, {"name": "西红柿", "price": 25}] % }
   <!-- items = [{"name": "苹果", "price": 23}, {"name": "西瓜", "price": 33}, {"name": "西红柿", "price": 25}] -->
    <p>根据某个属性排序 { { items|sort(attribute='price', reverse=true) } }</p>
    { % for item in items|sort(attribute='price', reverse=true) % }
        <p> { { item.name } } </p>
    { % endfor % }

    <p>文章发表于 { { t| t_func } }</p> <!-- 自定义过滤器 -->
   <!-- 判断语句 -->
     { % if user % }
        <title> hello { {user} } </title>
        <!-- <title> hello {{user[0]}} </title> -->
        <!-- <title> hello {{user.name}} </title> -->
    { % else % }
         <title> welcome to flask </title>        
    { % endif % }

{ % with age=3 % }
    <!-- 判断语句：算术运算 -->
    { % if age == 1 % }
        <p>age为1</p>
    { % elif age == 2 % }
        <p>age为2</p>
    { % else % }
        <p>age不为1和2</p>
    { % endif % }
{ % endwith % }
 </head>

 <body>
     <h1>hello world</h1>
    <ul> <!-- 循环遍历语句：列表 -->
        { % for index in fruit % }
            <li>{ { index } }</li>
        { % endfor % }
    </ul>
  <!-- good.html -->
    <table>
      <thead>
          <th>商品名称</th>
          <th>商品价格</th>
      </thead>
      <tbody> <!-- 循环遍历语句：字典 -->
      { % for good in goods % }
          <tr>
              <td> { {good.name} }    </td>
              <td> { {good.price} }    </td>
          </tr>
        <!-- 使用loop关键词获取循环信息 -->
        <p>循环长度{ { loop.length } }</p>
        <p>当前索引{ { loop.index0 } }</p>
        <p>是否结束{ { loop.last } }</p>
      { % endfor % }
      <!-- 字典遍历，不用指定key -->
      { % for k in goods % }
        { { k } } -> { { goods[k] } }
      { % endfor % }
      </tbody>
    </table>
  <!-- 变量赋值 -->
  { % set links = [
      ('home',url_for('.home')),
      ('service',url_for('.service')),
      ('about',url_for('.about')),
    ] % }
  <p>set的值 { { a } }</p> <!-- 变量赋值：另一种 -->
  { % with b='321' % }
    <p>with的值 { { b } } </p>
  { % endwith % }
  <nav>
      { % for label,link in links % }
          <!-- loop获取循环信息，loop.index表示下标, 从1开始 -->
          { % if not loop.first % }|{ % endif % }
          <a href="{ % if link is current_link % }#
          { % else % }
          { { link } }
          { % endif % }
          ">{ { label } }</a>
      { % endfor % }
  </nav>
<!-- 静态文件加载：url_for -->
  <script src="{ { url_for('static', filename='js/tmp.js') } }"></script>
  <script type="text/javascript" src="static/js/tmp.js"></script> <!-- 或更直接的方式 -->
   <!-- 空白控制 -->
<div>
    { % if True % }
        yay
    { % endif % }
</div>

 </body>
 </html>
 ```


### 文件上传下载

参考：
- [Python Flask:一个极简的web服务+文件上传](https://xu3352.github.io/python/2018/04/29/python-flask-web-server)
- [Python实现文件上传下载](https://blog.csdn.net/songling515010475/article/details/106409521)，使用socket

步骤
- 限制指定的后缀文件才可以上传
- 上传成功后, 跳转到成功页面
- 成功页面可以再返回上传页面
- 文件上传到指定的目录, 目录需要提前创建好



```python
import os
from flask import Flask, request, redirect, url_for
from werkzeug import secure_filename

UPLOAD_FOLDER = '/tmp/uploads'
ALLOWED_EXTENSIONS = set(['txt', 'pdf', 'png', 'jpg', 'jpeg', 'gif'])

app = Flask(__name__)
app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER

def allowed_file(filename):
    return '.' in filename and \
           filename.rsplit('.', 1)[1] in ALLOWED_EXTENSIONS

@app.route('/', methods=['GET', 'POST'])
@app.route('/upload/', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        file = request.files['file']
        if file and allowed_file(file.filename):
            filename = secure_filename(file.filename)
            file.save(os.path.join(app.config['UPLOAD_FOLDER'], filename))
            return redirect(url_for('upload_success', filename=filename))
    return '''
    <!doctype html>
    <title>Upload new File</title>
    <h1>Upload new File</h1>
    <form action="" method=post enctype=multipart/form-data>
      <p><input type=file name=file>
         <input type=submit value=Upload>
    </form>
    '''

@app.route('/upload_success')
def upload_success():
    return '''
    <!doctype html>
    <title>上传成功</title>
    <h1>上传成功</h1>
    <a href="/upload/">继续上传</a>
    '''

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080, debug=True)
```

另一个上传/下载完整示例

```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import os, sys
from flask import Flask, render_template, request, send_file, send_from_directory

app = Flask(__name__)
BASE_PATH = os.path.dirname(os.path.abspath(__file__))

@app.route("/")
def index():
    # 文件上传页面
    html="""<html>
        <head>
          <title>文件上传测试</title>
        </head>
        <body>
            <form action="/upload" method="POST" enctype="multipart/form-data">
                <input type="file" name="file" multiple="multiple" />
                <input type="submit" value="提交" />
            </form>
        </body>
        </html>"""
    return html

@app.route("/upload", methods=["POST"])
def upload_file():
    try:
        # f = request.files["file"]
        for f in request.files.getlist('file'):
            filename = os.path.join(BASE_PATH, "upload", f.filename)
            print(filename)
            f.save(filename)
        return "file upload successfully!"
    except Exception as e:
        return "failed!"


@app.route("/download/<filename>", methods=["GET"])
def download_file(filename):
    # 下载方法：http://10.200.24.101:8093/download/log.txt
    dir = os.path.join(BASE_PATH, 'download')
    return send_from_directory(dir, filename, as_attachment=True)


def mkdir(dirname):
    dir = os.path.join(BASE_PATH, dirname)
    if not os.path.exists(dir):
        os.makedirs(dir)


if __name__ == "__main__":
    mkdir('download')
    mkdir('upload')
    app.run(host="10.200.24.101", port=8093, debug=False)
```


## Django

【2021-7-16】python为后端，vue为前端的web开发框架整合demo，拿来即用，[django-vue-demo](https://github.com/realjf/django-vue-demo.git)，安装详情：[Django后端 + Vue前端 构建Web开发框架](https://realjf.io/python/django-vue-web/)，覆盖node、mysql、vue等工具包
- Django在线教程：[Django Book](http://djangobook.py3k.cn/)，[中文](http://djangobook.py3k.cn/2.0/)


### Django是什么？

Django是一个开放源代码的Web应用框架，由Python写成。采用了MVC的软件设计模式，即模型M，视图V和控制器C（注：实际上是MTV！）。它最初是被开发来用于管理劳伦斯出版集团旗下的一些以新闻内容为主的网站的。并于2005年7月在BSD许可证下发布。这套框架是以比利时的吉普赛爵士吉他手Django Reinhardt来命名的。
- Django的主要目标是使得开发复杂的、数据库驱动的网站变得简单。
- Django注重组件的重用性和“可插拔性”，**敏捷开发**和**DRY法则**（Don't Repeat Yourself）。
在Django中Python被普遍使用，甚至包括配置文件和数据模型。

### MVC模式

MVC 由（试图View/控制器Controller/模型Model）组成，实际应用中
- **模型**（Model）用来处理应用程序数据逻辑
- **视图**（View）用来处理我们从M拿过来的数据（页面渲染，template）
- **控制器**（Controller）定义程序行为选择相应的视图

大部分开发语言中都有MVC框架，MVC框架的核心思想是解耦，降低各功能模块之间的耦合性，方便变更，更容易重构代码，最大程度上实现代码的重用

### MTV开发模式

Django是一个基于MVC构造的框架。但是在Django中，控制器接受用户输入的部分由框架自行处理，所以 Django 里更关注的是**模型**（Model）、**模板**(Template)和**视图**（Views），称为**MTV模式**。它们各自的职责如下：
- (1) **模型**（Model），即数据存取层——处理与数据相关的所有事务：如何存取、如何验证有效性、包含哪些行为以及数据之间的关系等。
- (2) **视图**（View），即表现层——处理与表现相关的决定：如何在页面或其他类型文档中进行显示。
- (3) **模板**(Template)，即业务逻辑层——存取模型及调取恰当模板的相关逻辑。模型与模板的桥梁。

MTV基于MVC，并在MVC的基础上做了更细的划分，区别主要在于C和T,C之前是控制器，现在变成了Template,把C融入到了View里。

MTV模式包含：**视图**（View）负责**业务逻辑**，并在适当时候调用Model和Template 模板（Template）负责如何把页面展示给用户 模型（Model）处理应用程序数据逻辑
Django的MTV模式


### Django项目结构

每个django**项目**中可以包含多个APP（**应用**），相当于一个大型项目中的分系统、**子模块**、功能部件等等，相互之间比较独立，但也有联系。所有的APP共享项目资源。

Django文件结构：以新建应用test为例
- `manage.py` ： django管理主程序，一个项目一个
  - 创建数据库：python manage.py makemigrations，# 执行后，生成migrations目录，000*开头的文件里面包含sql语句
  - python manage.py migrate
  - 启动服务：python manage.py runserver 127.0.0.1:8000
  - django默认有跨站请求保护机制，settings文件中将它关闭
- `wsgi.py` ： 网络通信主接口
  - 设置项目使用哪个settings.py
  - os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'test.settings')
  - application = get_wsgi_application()
- `urls.py` ：url路由文件，网址入口，关联到对应的views.py中的一个函数（或者generic类），访问网址就对应一个函数。
  - 新建应用路由导入：更改urls.py文件，新增 from test import urls
  - url_patterns 中新增test应用的路由规则，如：path(r'api/', include(test.urls))
- `settings.py` ：Django 的设置，配置文件，比如 DEBUG 的开关，静态文件的位置等
  - 变量 INSTALLED_APPS 引入新建的app
  - 如果网页模板位置不在 templates，就需要修改：TEMPLATES 变量
  - 如果静态资源位置不在 statics ，就需要修改：STATIC_URL 变量
  - 配置数据库相关参数，如果用自带的sqlite，不需要修改。否则修改 DATABASE 变量，在mysql数据库创建mysite库
- `views.py` 处理用户发出的请求，调用 model.py
  - 从urls.py中对应过来, 通过渲染templates中的网页可以将显示内容，比如登陆后的用户名，用户请求的数据，输出到网页。
  - test/views.py 文件中记录业务处理逻辑，函数必须继承自 request，且返回不能是字符串！
  - def index(request)
  - return render(request, 'index.html', {模板变量})
- `templates` ：文件夹 ，views.py 中的函数渲染templates中的Html模板，得到动态内容的网页，可用缓存来提高速度。
  - 引用 static 目录下静态资源
  - 包含 jinja2 语法规则
- `models.py` 与数据库操作相关，定义数据库表格式，一张表一个类，存取数据时用到，不用数据库时可省略。
  - 必须继承自 model.Model，如：class UserInfo(model.Model)
- forms.py 表单，用户在浏览器上输入数据提交，对数据的验证工作以及输入框的生成等工作，可选。
- admin.py 后台，Django自带强大的后台功能和ORM框架，可以用很少量的代码就拥有一个强大的后台。
  - manage.py统计目录下运行下列两个命令使数据库生效。

```shell
python manage.py syncdb # Django 1.6.x 及以下
# Django 1.7 及以上的版本需要用以下命令
python manage.py makemigrations [appname] #appname 即为此处的Blog
python manage.py migrate
# 创建超级用户
python manage.py createsuperuser # 密码以密文形式存储
# ------ 修改密码 -------
# 方法①
python manage.py changepassword username # 修改用户密码——这种方式会校验密码强度
# 方法②
python manage.py shell # 进入shell环境，执行以下代码：
# 此方法不会校验密码强度，可设置简单的密码
from django.contrib.auth.models import User
user = User.objects.get(username='用户名')
user.set_password('新的密码')
user.save()

```

可以通过 http://localhost:8000/admin/ 来访问后台

### 创建django项目

[django基本命令教程](https://code.ziqiangxuetang.com/django/django-basic.html)

- （1）创建项目
  - django-admin startproject mysite
  - mysite下的文件： #整个项目的容器文件夹，这个名称可以随意起 
    - \__init__.py # 这个文件告诉python这个目录是一个包 
    - settings.py # 该Django项目的配置/设置 
    - urls.py  # Django项目的总路由管理文件
    - wsgi.py  #一个兼容的web服务器入口，在项目运行发布的时候用到 
    - manage.py #一个实用的命令行工具，可以让我们以各种方式和Django项目进行交互
- （2）启动服务：
  - cd mysite
  - python manage.py runserver
  - python manage.py runserver 8000 # 指定端口
  - python manage.py runserver 0.0.0.0:8000 # 监听机器所有ip（机器可能有多个ip）http://10.200.24.101:8080/
  - python manage.py runserver 10.200.24.101:8080 # 监听指定ip（需要提前添加到settings里的ALLOW_HOST中）
  - 访问：http://127.0.0.1:8000/
- （3）创建blog应用
  - python manage.py startapp blog
  - 目录结构
    - blog/ \__init__.py     admin.py     migrations/ \__init__.py     models.py     tests.py     views.py app.py
- （4）编辑settings.py文件
  - 在文件尾部找到INSTALLED_APPS元组。把新建的app以模块的形式添加到元组里
- （5）Controller：
  - 路由urls.py（有3种方式：函数、类和外部
- （6）建立数据模型
- （7）修改view
- （8）创建超级用户admin
- （9）启动服务器


### view 视图

#### view

views.py文件：

```python
from django.http import HttpResponse
# 视图函数 hello
def hello(request):
  # 每个视图函数至少要有一个参数，通常叫request。
  # 这是一个触发这个视图、包含当前Web请求信息的对象，是类django.http.HttpRequest的一个实例
  return HttpResponse("Hello world") # 啥也不做，返回一个HttpResponse对象

# ----- 接收参数 ------
from django.http import Http404, HttpResponse
import datetime
# 从url正则里提取的参数offset
def hours_ahead(request, offset):
    try:
        offset = int(offset) # 字符串值转换为整数
    except ValueError:
        raise Http404() # 解析错误时，返回404页面
    dt = datetime.datetime.now() + datetime.timedelta(hours=offset)
    html = "<html><body>In %s hour(s), it will be %s.</body></html>" % (offset, dt)
    return HttpResponse(html)
```

运行：python manage.py runserver，将看到Django的欢迎页面，而看不到Hello world显示页面。
- 因为mysite项目还对hello视图一无所知。需要通过一个详细描述的URL来显式的告诉并且激活这个视图。

#### viewset

针对同个资源的查询, 只是数量不同, 却需要定义两个不同的类视图, 太过冗余，于是, 视图集出现了, 它的作用就是将对同一资源的不同请求方式整合到一个视图当中.


### 路由

用 URLconf 绑定视图函数和URL
- RLconf 就像是 Django 所支撑网站**目录**。 
- 本质是 URL 模式以及要为该 URL 模式调用的视图函数之间的**映射表**。 
- 以这种方式告诉 Django，对于这个 URL 调用这段代码，对于那个 URL 调用那段代码。

URLconf（即 urls.py 文件）

```python
from django.conf.urls.defaults import * # Django URLconf的基本构造。 包含了一个patterns函数。

# Uncomment the next two lines to enable the admin:
# from django.contrib import admin
# admin.autodiscover()

urlpatterns = patterns('',
    # Example:
    # (r'^mysite/', include('mysite.foo.urls')),

    # Uncomment the admin/doc line below and add 'django.contrib.admindocs'
    # to INSTALLED_APPS to enable admin documentation:
    # (r'^admin/doc/', include('django.contrib.admindocs.urls')),

    # Uncomment the next line to enable the admin:
    # (r'^admin/', include(admin.site.urls)),
)
# Django 期望能从 ROOT_URLCONF 模块中找到它。 urlpatterns 变量定义了 URL 以及用于处理这些 URL 的代码之间的映射关系。

# -----------
from django.conf.urls.defaults import * 
from mysite.views import hello # 引入了 hello 视图

# 所有指向 URL /hello/ 的请求都应由 hello 这个视图函数来处理
urlpatterns = patterns('',
    ('^hello/$', hello), # 把hello视图函数作为一个对象传递，而不是调用它
    (r'^time/plus/(\d+)/$', hours_ahead), # 通配符，提取到offset变量中，如：def hours_ahead(request, offset)
)
# ------------ 流线型化(Streamlining)函数导入 -----------
from django.conf.urls.defaults import *
# 传入一个包含模块名和函数名的字符串，而不是函数对象本身
# 使用这个技术，就不必导入视图函数了；Django 会在第一次需要它时根据字符串所描述的视图函数的名字和路径，导入合适的视图函数。
urlpatterns = patterns('',
    (r'^hello/$', **'mysite.views.hello'** ),
    (r'^time/$', **'mysite.views.current_datetime'** ),
    (r'^time/plus/(d{1,2})/$', **'mysite.views.hours_ahead'** ),
)
# 进一步简化：公共前缀提前
urlpatterns = patterns(**'mysite.views'** ,
    (r'^hello/$', **'hello'** ),
    (r'^time/$', **'current_datetime'** ),
    (r'^time/plus/(d{1,2})/$', **'hours_ahead'** ),
)
# 多视图混合
urlpatterns = patterns('',
    (r'^hello/$', 'mysite.views.hello'),
    (r'^time/$', 'mysite.views.current_datetime'),
    (r'^time/plus/(\d{1,2})/$', 'mysite.views.hours_ahead'),
    (r'^tag/(\w+)/$', 'weblog.views.tag'),
)
# 改进：，分隔开
urlpatterns = patterns('mysite.views',
    (r'^hello/$', 'hello'),
    (r'^time/$', 'current_datetime'),
    (r'^time/plus/(\d{1,2})/$', 'hours_ahead'),
)
urlpatterns += patterns('weblog.views',
    (r'^tag/(\w+)/$', 'tag'),
)
# 改进：使用include
urlpatterns = patterns('',
    (r'^(?P<username>\w+)/blog/', include('foo.urls.blog')),
)

```

### templates 模板

将页面的设计和Python的代码分离开，会更干净简洁更容易维护。 
- 使用 Django的 **模板系统** (Template System)来实现这种模式

模板是一个文本，用于分离文档的表现形式和内容。 模板定义了占位符以及各种用于规范文档该如何显示的各部分基本逻辑（模板标签）。 模板通常用于产生HTML，但是Django的模板也能产生任何基于文本格式的文档。

```html
<html>
<head><title>Ordering notice</title></head>

<body>

<h1>Ordering notice</h1>

<p>Dear { { person_name } },</p>
<p>Thanks for placing an order from { { company } }. It's scheduled to
ship on { { ship_date|date:"F j, Y" } }.</p>
<p>Here are the items you've ordered:</p>

<ul>
{ % for item in item_list % }
    <li>{ { item } }</li>
{ % endfor % }
</ul>

{ % if ordered_warranty % }
    <p>Your warranty information will be included in the packaging.</p>
{ % else % }
    <p>You didn't order a warranty, so you're on your own when
    the products inevitably stop working.</p>
{ % endif % }

<p>Sincerely,<br />{ { company } }</p>

</body>
</html>
```

Django 模板含有很多内置的tags和filters
- 用两个大括号括起来的文字（例如 { { person_name } } ）称为 `变量`(variable)
- 被大括号和百分号包围的文本(例如 { % if ordered_warranty % } )是 `模板标签`(template tag)
- filter过滤器是一种最便捷的转换变量输出格式的方式, { {ship_date\|date:”F j, Y” } }

使用Django模板的最基本方式如下：
- 用原始的模板代码字符串创建一个 Template 对象， Django同样支持用指定模板文件路径的方式来创建 Template 对象;
- 调用模板对象的render方法，并且传入一套变量context。它将返回一个基于模板的展现字符串，模板中的变量和标签会被context值替换。

```python
from django import template

t = template.Template('My name is { { name } }.')
c = template.Context({'name': 'Adrian'}) # 环境变量
print t.render(c) # My name is Adrian. # 填充模板
c = template.Context({'name': 'Fred'})
print t.render(c) # My name is Fred.
```

句点查找可以多级深度嵌套

句点查找规则可概括为： 当模板系统在变量名中遇到点时，按照以下顺序尝试进行查找：
- 字典类型查找 （比如 foo["bar"] )
- 属性查找 (比如 foo.bar )
- 方法调用 （比如 foo.bar() )
- 列表类型索引查找 (比如 foo[bar] )
  - 不能使用负数列表索引！，如 item.-1


### model 模型

#### 1. 创建数据库

创建数据库 （注意设置 数据的字符编码）
- 由于Django自带的orm是data_first类型的ORM，使用前必须先创建数据库

```sql
create database day70 default character set utf8 collate utf8_general_ci;
```

#### 2. 数据库设置

修改project中的settings.py文件中设置；
- 连接 MySQL数据库（Django默认使用的是sqllite数据库）

```python
DATABASES = {
    'default': {
    'ENGINE': 'django.db.backends.mysql',
    'NAME':'day70',
    'USER': 'eric',
    'PASSWORD': '123123',
    'HOST': '192.168.182.128',
    'PORT': '3306',
    }
}
# 增加以下语句，可以查看orm操作执行的原生SQL语句
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'console':{
            'level':'DEBUG',
            'class':'logging.StreamHandler',
        },
    },
    'loggers': {
        'django.db.backends': {
            'handlers': ['console'],
            'propagate': True,
            'level':'DEBUG',
        },
    }
}
```

#### 3.  修改mysql默认方式

修改project 中的__init__py 文件设置 Django默认连接MySQL的方式

```python
import pymysql
pymysql.install_as_MySQLdb()
```

#### 4. 注册

setings文件注册APP

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app01.apps.App01Config',
]
```

#### 5. models.py创建表

```python
# 新增字段可以为空（免得迁移时报错）
startdate = models.CharField(max_length=255, verbose_name="任务开始时间",null=True, blank=True)
# （1）字符串类
name=models.CharField(max_length=32)
# models.CharField  对应的是MySQL的varchar数据类型
# char 和 varchar的区别 :
# char和varchar的共同点是存储数据的长度，不能 超过max_length限制，
# 不同点是varchar根据数据实际长度存储，char按指定max_length（）存储数据；所有前者更节省硬盘空间；
# 详情：
EmailField(CharField)：
IPAddressField(Field)
URLField(CharField)
SlugField(CharField)
UUIDField(Field)
FilePathField(Field)
FileField(Field)
ImageField(FileField)
CommaSeparatedIntegerField(CharField)
# （2）时间字段
models.DateTimeField(null=True)
date=models.DateField()
# （3）数字字段
(max_digits=30,decimal_places=10)总长度30小数位 10位）
# 数字：
num = models.IntegerField()
num = models.FloatField() # 浮点
price=models.DecimalField(max_digits=8,decimal_places=3) # 精确浮点
 # （4）枚举字段
 choice=(
        (1,'男人'),
        (2,'女人'),
        (3,'其他')
    )
lover=models.IntegerField(choices=choice) #枚举类型
# （5）其它字段
db_index = True # 表示设置索引
unique(唯一的意思) = True # 设置唯一索引
# 联合唯一索引
class Meta:
unique_together = (
 ('email','ctime'),
)
# 联合索引（不做限制）
index_together = (
('email','ctime'),
)
ManyToManyField(RelatedField)  #多对多操作
```

更多字段介绍见[原文](https://www.cnblogs.com/sss4/articles/7070942.html)



#### 6. 数据迁移

在winds cmd或者Linux shell的项目的manage.py目录下执行

```shell
python manage.py makemigrations  # 把你写在models中的代码翻译成增、删、改的 SQL 语句
python manage.py migrate         # 将上述翻译的SQL语句去数据库执行
python manage.py migrate --fake    # 假设 migrate 把所有SQL语句执行成功
python manage.py inspectdb > models.py # 检查DB中已经创建完毕的表结构，生成model.py
```

表创建之时，新增字段既没有设置默认值，也没有设置新增字段可为空，去对应原有数据导致


### ORM 对象关系映射

ORM：Object Relational Mapping(关系对象映射)
- 类名 --> 数据库中的表名
- 类属性对应 --> 数据库里的字段
- 类实例对应 --> 数据库表里的一行数据

![](https://images2018.cnblogs.com/blog/1308093/201805/1308093-20180510154540784-44644315.png)

obj.id  obj.name.....类实例对象的属性

Django orm的优势：
- Django的orm操作本质上会根据对接的数据库引擎，翻译成对应的sql语句；用Django开发的项目无需关心程序底层使用的是MySQL、Oracle、sqlite....，如果数据库迁移，只需要更换Django的数据库引擎即可；

```shell
python manage.py makemigrations  #根据app下的migrations目录中的记录，检测当前model层代码是否发生变化？
python manage.py migrate         #把orm代码转换成sql语句去数据库执行
python manage.py migrate --fake    #只记录变化，不提交数据库操作
python manage.py inspectdb > models.py #检查DB中已经创建完毕的表结构，生成model.py
```

 https://www.cnblogs.com/sss4/articles/7070942.html


#### 示例

Web 应用中，主观逻辑经常牵涉到与数据库的交互。 数据库驱动网站 在后台连接数据库服务器，从中取出一些数据，然后在 Web 页面用漂亮的格式展示这些数据。

```python
from django.shortcuts import render_to_response
from mysite.books.models import Book

def book_list(request):
    books = Book.objects.order_by('name')
    return render_to_response('book_list.html', {'books': books})
```

把数据存取逻辑、业务逻辑和表现逻辑组合在一起的概念有时被称为软件架构的 Model-View-Controller (MVC)模式。 在这个模式中， Model 代表数据存取层，View 代表的是系统中选择显示什么和怎么显示的部分，Controller 指的是系统中根据用户输入并视需要访问模型，以决定使用哪个视图的那部分。

```python
# 创建应用books：python manage.py startapp books
# 
from django.db import models

class Publisher(models.Model):
    name = models.CharField(max_length=30)
    address = models.CharField(max_length=50)
    city = models.CharField(max_length=60)
    state_province = models.CharField(max_length=30)
    country = models.CharField(max_length=50)
    website = models.URLField()

class Author(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=40)
    email = models.EmailField()

class Book(models.Model):
    title = models.CharField(max_length=100)
    authors = models.ManyToManyField(Author) # 多对多字段 叫做 authors 
    publisher = models.ForeignKey(Publisher)
    publication_date = models.DateField()
```

Django 可以根据 model.py 自动生成这些 CREATE TABLE 语句
- 除非单独指明，否则Django会自动为每个模型生成一个自增长的整数主键字段每个Django模型都要求有单独的主键id

模型激活：在数据库中创建这些表
- 编辑 settings.py 文件， 找到 INSTALLED_APPS 设置
- 添加‘mysite.books’ 到 INSTALLED_APPS 的末尾
- 创建数据库表：
  - python manage.py validate # 检查语法
  - python manage.py sqlall books # 执行
  - python manage.py syncdb # 提交到数据库

#### ORM连接

把一对多，多对多，分为**正向**和**反向**查找两种方式。

正向查找：ForeignKey在 UserInfo表中，如果从UserInfo表开始向其他的表进行查询，这个就是正向操作，反之如果从UserType表去查询其他的表这个就是反向操作。
- 一对多：models.ForeignKey(其他表)
- 多对多：models.ManyToManyField(其他表)
- 一对一：models.OneToOneField(其他表)

正向连表操作总结：
- 所谓正、反向连表操作的认定无非是Foreign_Key字段在哪张表决定的，
- Foreign_Key字段在哪张表就可以哪张表使用Foreign_Key字段连表，反之没有Foreign_Key字段就使用与其关联的 小写表名；
- 1对多：对象.外键.关联表字段，values(外键字段__关联表字段)
- 多对多：外键字段.all()

反向连表操作总结：
- 通过value、value_list、fifter 方式反向跨表：小写表名__关联表字段
- 通过对象的形式反向跨表：小写表面_set().all()

应用场景：
- **一对多**：当一张表中创建一行数据时，有一个单选的下拉框（可以被重复选择）
  - 例如：创建用户信息时候，需要选择一个用户类型【普通用户】【金牌用户】【铂金用户】等。
- **多对多**：在某表中创建一行数据是，有一个可以多选的下拉框
  - 例如：创建用户信息，需要为用户指定多个爱好
- **一对一**：在某表中创建一行数据时，有一个单选的下拉框（下拉框中的内容被用过一次就消失了
  - 例如：原有含10列数据的一张表保存相关信息，经过一段时间之后，10列无法满足需求，需要为原来的表再添加5列数据


### form 表单

HttpRequest对象包含当前请求URL的一些信息：

| 属性/方法 | 说明    | 举例 |
| request.path  | 除域名以外的请求路径，以正斜杠开头 | "/hello/" |
| request.get_host()    | 主机名（比如，通常所说的域名）   | "127.0.0.1:8000" or "www.example.com" |
| request.get_full_path()   | 请求路径，可能包含查询字符串    | "/hello/?print=true" |
| request.is_secure()   | 如果通过HTTPS访问，则此方法返回True， 否则返回False | True 或者 False |

还有：
- request.META 是一个Python字典，包含了所有本次HTTP请求的Header信息，比如用户IP地址和用户Agent（通常是浏览器的名称和版本号）。 注意，Header信息的完整列表取决于用户所发送的Header信息和服务器端设置的Header信息。
- HttpRequest对象还有两个属性包含了用户所提交的信息： request.GET 和 request.POST。二者都是类字典对象，可以通过它们来访问GET和POST数据。
  - request.GET和request.POST都有get()、keys()和values()方法，你可以用用 for key in request.GET 获取所有的键。
  - POST数据是来自HTML中的〈form〉标签提交的，而GET数据可能来自〈form〉提交也可能是URL中的查询字符串(the query string)。

view函数里，要始终用这个属性或方法来得到URL，而不要手动输入

```python
# BAD!
def current_url_view_bad(request):
    return HttpResponse("Welcome to the page at /current/")
# GOOD
def current_url_view_good(request):
    return HttpResponse("Welcome to the page at %s" % request.path)
```

[示例](http://djangobook.py3k.cn/2.0/chapter07/)：

```html
<html>
<head>
    <title>Search</title>
</head>
<body>
    <form action="/search/" method="get">
        <input type="text" name="q">
        <input type="submit" value="Search">
    </form>
</body>
</html>
```


```python
# urls.py
urlpatterns = patterns('',
    # ...
    (r'^search-form/$', views.search_form),
    (r'^search/$', views.search),  # 指向search方法
    # ...
)

# views.py
def search(request):
    if 'q' in request.GET:
        message = 'You searched for: %r' % request.GET['q'] # q 对应html里的input内容
    else:
        message = 'You submitted an empty form.'
    return HttpResponse(message)
```

### Django REST Framework

前后端分离的架构设计越来越流行，业界甚至出现了API优先的趋势。显然API开发已经成为后端程序员的必备技能了，那作为Python程序员特别是把Django作为自己主要的开发框架的程序员，推荐使用Django REST framework（DRF）这个API框架。

Django REST framework（DRF）框架文档齐全，社区较稳定，而且由于它是基于Django这个十分全面的框架而设计开发的，能够让开发者根据自己的业务需要，使用极少的代码量快速的开发一套符合RESTful风格的API，并且还支持自动生成API文档。

[快速入门文档](https://www.django.cn/course/show-20.html)

```shell
# 创建项目目录mkdir tutorial
cd tutorial# 创建一个virtualenv来隔离我们本地的包依赖关系virtualenv env
source env/bin/activate  # 在Windows下使用 `env\Scripts\activate`# 在创建的虚拟环境中安装 Django 和 Django REST frameworkpip install django
pip install djangorestframework# 创建一个新项目和一个单个应用django-admin.py startproject tutorial .  # 注意结尾的'.'符号cd tutorial
django-admin.py startapp quickstart
cd ..
python manage.py migrate
python manage.py createsuperuser
```

#### （1）定义序列化程序

创建一个名为 tutorial/quickstart/serializers.py的文件，用于数据表示。

```python
from django.contrib.auth.models import User, Group
from rest_framework import serializers


class UserSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = User
        fields = ('url', 'username', 'email', 'groups')


class GroupSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Group
        fields = ('url', 'name')
```

#### （2）编写视图

打开 tutorial/quickstart/views.py 文件开始写视图代码

```python
from django.contrib.auth.models import User, Group
from rest_framework import viewsets
from tutorial.quickstart.serializers import UserSerializer, GroupSerializer

# 将所有常见行为分组写到叫 ViewSets 的类中
class UserViewSet(viewsets.ModelViewSet):
    """
    允许用户查看或编辑的API路径。
    """
    queryset = User.objects.all().order_by('-date_joined')
    serializer_class = UserSerializer

class GroupViewSet(viewsets.ModelViewSet):
    """
    允许组查看或编辑的API路径。
    """
    queryset = Group.objects.all()
    serializer_class = GroupSerializer
```

#### 编写路由

在tutorial/urls.py中开始写连接API的URLs。

```python
from django.conf.urls import url, include
from rest_framework import routers
from tutorial.quickstart import views

router = routers.DefaultRouter()
router.register(r'users', views.UserViewSet)
router.register(r'groups', views.GroupViewSet)

# 使用自动URL路由连接我们的API。
# 另外，我们还包括支持浏览器浏览API的登录URL。
urlpatterns = [
    url(r'^', include(router.urls)),
    url(r'^api-auth/', include('rest_framework.urls', namespace='rest_framework'))
]
```

#### 全局设置

想打开分页，希望API只能由管理员使用。设置模块都在 tutorial/settings.py 中。

```python
INSTALLED_APPS = (
    ...
    'rest_framework',
)

REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAdminUser',
    ],
    'PAGE_SIZE': 10
}
```

#### 测试api

命令行启动服务器。
- python manage.py runserver

现在可以从命令行访问我们的API，使用诸如 curl ...

```shell
curl -H 'Accept: application/json; indent=4' -u admin:password123 http://127.0.0.1:8000/users/
# 或使用httpie
http -a admin:password123 http://127.0.0.1:8000/users/
# 返回结果
{
    "count": 2,
    "next": null,
    "previous": null,
    "results": [
        {
            "email": "admin@example.com",
            "groups": [],
            "url": "http://127.0.0.1:8000/users/1/",
            "username": "admin"
        },
        {
            "email": "tom@example.com",
            "groups": [                ],
            "url": "http://127.0.0.1:8000/users/2/",
            "username": "tom"
        }
    ]
}
```

或直接通过浏览器，转到URL http://127.0.0.1:8000/users/
- ![](https://www.django.cn/media/upimg/quickstart_20180907202647_560.png)

### Django+vue

【2022-2-22】[Django+Vue前后端分离实战](https://www.cnblogs.com/zhangxue521/p/12957816.html)
- ![](https://img2020.cnblogs.com/blog/1004904/202005/1004904-20200525162859257-1990729941.png)


## Fastapi

[Fastapi](https://github.com/tiangolo/fastapi)

![](https://pic1.zhimg.com/v2-76eee9e74c12fdf22c682fe5475f2ab2_1440w.jpg?source=172ae18b)

- [FastAPI使用小结](https://zhuanlan.zhihu.com/p/136621431)
- [全面拥抱 FastApi — 多应用程序管理蓝图APIRouter](https://zhuanlan.zhihu.com/p/120885265)
- [（入门篇）Python框架之FastAPI——一个比Flask和Tornado更高性能的API 框架](https://zhuanlan.zhihu.com/p/131618992)
- [（进阶篇）Python web框架FastAPI——一个比Flask和Tornada更高性能的API 框架](https://mp.weixin.qq.com/s?__biz=MzU3MzQxMjE2NA==&mid=2247486752&idx=1&sn=0036ae16fe1a80895e2b31d02d1dac84&chksm=fcc34b0bcbb4c21d104541dc28fa1786eafd77072da068b3e8b537a13dbb8f96cb9c643fd6e3&scene=21#wechat_redirect)
- [（完结篇）Python框架之FastAPI——一个比Flask和Tornado更高性能的API 框架](https://zhuanlan.zhihu.com/p/131625459)

### 简介

- FastAPI是一个现代、快速（高性能）的 Web 框架，基于标准 Python 类型提示，使用 Python 3.6+ 构建 API。
- 几点感受：
    - 性能并发更强了，支持异步 async
    - 基于 Pydantic 的类型声明，自动校验参数
    - 自动生成交互式的 API 接口文档
        - Django REST Framework 的主要功能是自动 API 文档。 API 文档有个标准叫 [Swagger](https://swagger.io/) ，用 JSON 或 YAML 来描述。
    - 上手简单，能快速编码
- 主要特征是：
    - 高速：与NodeJS和Go相当，拥有高性能。 现有最快的Python框架之一。
        - 并发性能可以和 NodeJS 以及 Go 相媲美。它是基于Starlette框架, 类似于Starlette 的一个子类。
    - 快速编码：将功能开发速度提高约200％至300％。
    - 更少的Bug：减少约40％的人为（开发人员）导致的错误。
    - 直观：更好的编辑支持。补全任何地方。更少的调试时间。
    - 简单：方便使用和学习。减少阅读文档的时间。
    - 简介：最小化代码重复。每个参数声明的多个要素。更少的错误。
    - 健壮：获取便于生产的代码。带自动交互式文档。
    - 基于标准：基于（并完全兼容）API 的开放标准：OpenAPI（以前称为Swagger）和 JSON Schema。
- [文档](https://fastapi.tiangolo.com)

- 【2021-5-8】问题：严格的参数验证、没有类似flask的全局变量

### 部署

```shell
# 安装
pip install fastapi
pip install uvicorn
pip install gunicorn # 或者
#使用uvicorn启动
uvicorn sql_app.main:app --reload
# 指定host和port
uvicorn main:app --host=0.0.0.0 --port=8800
```

访问
- http://127.0.0.1:8000
- 打开自动生成的[文档](http://127.0.0.1:8000/docs)：http://127.0.0.1:8000/docs，可以动态传入数据
    - ![](https://picb.zhimg.com/80/v2-27e0a1f1fa58c3fbde1839b010e482ff_720w.jpg)


### 使用

#### 代码示例

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def index():
    return "Hello world"
# 异步请求
@app.get("/index")
async def index():
    return "Hello world"

@app.get("/items/{item_id}")
async def read_item(item_id: str, q: str = None, short: bool = False):
    item = {"item_id": item_id}
    if q:
        item.update({"q": q})
    if not short:
        item.update(
            {"description": "This is an amazing item that has a long description"}
        )
    return item

from typing import Optional
from fastapi import FastAPI, Path, Query

# 指定参数格式、合法范围
@app.get("/bar/{foo}")
# @app.post("/bar") # post请求
async def read_item(
        foo: int = Path(1, title='描述'),
        age: int = Query(..., le=120, title="年龄"),
        name: Optional[str] = Query(None, min_length=3, max_length=50, regex="^xiao\d+$")
):
    return {"foo": foo, "age": age, "name": name}

# Path方法获取请求路径里面的参数如 http://127.0.0.1:8000/bar/123
# Query方法获取请求路径后面的查询参数如 http://127.0.0.1:8000/bar?name=xiaoming&age=18
# Body方法获取请求体里面的参数，前提是请求头得用accept: application/json

```

## 跨域 —— CORS

【2022-2-25】


### flask解法

安装CORS包：

```shell
pip install -U flask-cors
```

添加2行代码：

```python
from flask import Flask
from flask_cors import CORS

app = Flask(__name__)
CORS(app)

@app.route("/")
def helloWorld():
  return "Hello, cross-origin-world!"
```


# 前端

- 【2020-8-22】[微信聊天窗口界面](https://github.com/kuangwk/wechat-chat-interface)
    - ![](https://github.com/kuangwk/wechat-chat-interface/raw/css/screenshot.png)
- 【2020-8-28】[EasyMock](https://www.easy-mock.com/login)[文档](https://easy-mock.com/docs)，[Github地址](https://github.com/easy-mock/easy-mock)，Easy Mock 是一个可视化，并且能快速生成 模拟数据 的持久化服务
    - ![](https://camo.githubusercontent.com/e3e9c378dd2f6d8349922f9e3cb0f7ee095533a9/687474703a2f2f696d672e736f756368652e636f6d2f6632652f33313362333661616137643061336166303837313863333861323836393533342e706e67)
- 【2021-5-13】[机器学习建模与部署--以垃圾消息识别为例](https://kuhungio.me/2019/flask_vue_ml/?utm_source=zhihu&utm_campaign=ml_sys_design#%E5%89%8D%E7%AB%AF-vue), 项目地址 [kuhung/flask_vue_ML](https://github.com/kuhung/flask_vue_ML)
  - ![](https://kuhungio.me/images/flask_vue_ML/flask_vue_ml.jpg)

## 简介


前端三要素
- HTML(结构层) : 超文本标记语言(Hyper Text Markup Language) ，决定网页的结构和内容
- CSS(表现层) : 层叠样式表(Cascading Style sheets) ，设定网页的表现样式。CSS预处理器：
  - ①SASS：基于Ruby，通过服务端处理，功能强大。解析效率稿。需要学习 Ruby 语言，上手难度高于LESS。
  - ②LESS：基于 NodeJS，通过客户端处理，使用简单。功能比 SASS 简单，解析效率也低于 SASS，但在实际开发中足够了，所以后台人员如果需要的话，建议使用 LESS。
- JavaScript(行为层) : 是一种弱类型脚本语言，其源代码不需经过编译，而是由浏览器解释运行,用于控制网页的行为
  - ①原生JS开发，也就是让我们按照【ECMAScript】标准的开发方式，简称是ES,特点是所有浏览器都支持。
  - ②TypeScript是一种由微软开发的自由和开源的编程语言。它是JavaScript的一个超集，而且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程。由安德斯海尔斯伯格（C#、Delphi、TypeScript 之父； .NET 创立者）主导。该语言的特点就是除了具备 ES 的特性之外还纳入了许多不在标准范围内的新特性，所以会导致很多浏览器不能直接支持 TypeScript 语法，需要编译后（编译成 JS ）才能被浏览器正确执行。

【2021-7-21】[Vue快速入门学习笔记(更新ing)](https://www.cnblogs.com/melodyjerry/p/13768594.html)

## js

- 总结
  - [JavaScript基础知识总结笔记](https://blog.csdn.net/weixin_41651627/article/details/79106164)
  - [JavaScript笔记总结](https://blog.csdn.net/weixin_43862596/article/details/109783431)
  - [JS知识点思维导图](https://github.com/daniellidg/javascript-knowhow)

- JavaScript一种直译式脚本语言，一种基于对象和事件驱动并具有安全性的客户端脚本语言；也是一种广泛应用客户端web开发的脚本语言。简单地说，JavaScript是一种运行在浏览器中的解释型的编程语言。

更多编程语言介绍：
- [编程语言发展历史]({{ site.baseurl}}{% post_url 2010-07-26-computer-history %}#编程语言变迁)

### 运行机制

- 【2020-8-26】[JavaScript运行机制](https://www.toutiao.com/i6748661672522547719/)
- JavaScript语言的一大特点就是**单线程**，也就是说，同一个时间只能做一件事。那么，为什么JavaScript不能有多个线程呢？这样能提高效率啊。
- JavaScript的单线程，与它的用途有关。作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。比如，假定JavaScript同时有两个线程，一个线程在某个DOM节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？
- 所以，为了避免复杂性，从一诞生，JavaScript就是单线程，这已经成了这门语言的核心特征，将来也不会改变。
- 所有任务分成两种
  - 一种是**同步任务**（synchronous）
  - 另一种是**异步任务**（asynchronous）。同步任务指的是，在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；
    - 异步任务指的是，不进入主线程、而进入”任务队列”（task queue）的任务，只有”任务队列”通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。
        - 异步执行的运行机制如下：(这种运行机制又称为Event Loop（事件循环）)
            - 所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。
            - 主线程之外，还存在一个”任务队列”（task queue）。只要异步任务有了运行结果，就在”任务队列”之中放置一个事件。
            - 一旦”执行栈”中的所有同步任务执行完毕，系统就会读取”任务队列”，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
            - 主线程不断重复上面的第三步。
        - ![](https://p1-tt.byteimg.com/origin/pgc-image/e508259a15a04b3bbda091e0989390fb?from=pc)

```javascript
console.log(1);
setTimeout(function(){
console.log(3);
},0);
console.log(2);
//请问数字打印顺序是什么？
```

### html引入js

js在html中的应用一般有两种方式：内联和外链；
- 内嵌：内联就是在html文件中通过在**script标签**里面直接写js语法
- 外链：外链是指在html文件中通过**script标签**的**src属性**引入js文件

推荐使用外链的方式，把js和html分离，方便开发和日后维护

#### 一、在页面中嵌入js

这是在页面使用js最简单的方式了。用 \<\script\>\<\/script\> 把js代码标识清楚。

最好是把script元素写在</body>前面，script元素的内容就是js代码。

像这样：

```html
<script>
// 在这里写js
function test(){
alert('hello，world');
}
test();
</script>
```

#### 二、引用外部js文件

引用外部js文件，可以使js文件和HTML文件相分离、一个js文件可被多个HTML文件使用、维护起来也更方便等等。
- 1、先写好js代码，存为后缀为.js的文件，如jquery-1.7.1min.js
- 2、在HTML文件中的 < /body > 前添加代码，src是js文件的路径
- 3、如果js文件里，有函数，在HTML文件里，用 a href="#" onclick="js_method();return false;" 这种方法进行触发调用

```html
<script src="../js/jquery-1.7.1min.js"></script>
...
</body>
```

#### 总结

HTML中嵌入JavaScript的三种方式
- οnclick="js代码“
- script 脚本块
- script src引入外部js文件

```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HTML中嵌入JavaScript的第三种方式</title>
</head>
<body>
  <!-- 第一种方式: onclick -->
     <!--点击按钮弹出消息框-->
    <input type="button" value="Hello" onclick="window.alert('Hello JavaScript!')"/>
    <input type="button" value="Hello" onclick="window.alert ('Hello MengYangChen');
                                                alert('Hello MengYang')
                                                alert('Hello Meng')"/>
  <!-- 第二种方式: 脚本块 -->
    <input type="button" value="我是一个。。"/>
    <script type="text/javascript">
        window.alert("hello world!");
    </script>
    <input type="button" value="我是一个按钮对象"/>
  <!-- 第三种方式: 外部js引用 -->
  <script type="text/javascript" src="file/JS1.js"></script>
</body>
</html>
```

### 加载顺序

js加载顺序
- 计算机读代码的顺序是从上往下读的, html文件中的顺序: <span style='color:blue'> <\head> → <\body> → body后方</span>

javascript代码位置
- (1) <\head> 里面：
  - 网页主体（body）还未加载，所以这里适合放一些**立即执行且不需要引入什么参数**来进行操作的的**自定义函数**，立即执行的语句则很可能会出错（视浏览器而定）
- (2) <\body> 里面：
  - 这里可以放函数也可以放立即执行的语句，但是如果需要和网页元素互动的（比如获取某个标签的值或者给某个标签赋值），Javascript代码务必在标签的后面
- (3) <\body> 下面：
  - 整个网页已经**加载完毕**，所以最适合放需要立即执行的命令，而自定义函数之类的则不适合。

推荐位置
- 规范起见，推荐放到body结束标签的末尾，包含到body标签内

```html
<body>
    <div>
        <span>这里是你页面的内容</span>
    </div>
    <!-- js内容放在</body>上面,页面内容下面 -->
    <script src="js/jquery-1.12.1.min.js" type="text/javascript"></script>
    <script>
        function hello(){
            console.log("你好");
        }
    </script>
</body>
```

这样处理的好处是
- 1、无需担心因页面未完成加载，造成DOM节点获取不到，使脚本报错的问题
- 2、而且能避免因脚本运行缓慢造成页面卡死的问题。

另外，Yahoo的前端优化指南里就有这一条。[参考](https://zhuanlan.zhihu.com/p/372362414)

### 基础知识

- JS中的变量的**数据类型**
  - 数据类型有7种： number、boolean、symbol、string、object、undefined、function。null 有属于自己的数据类型 Null
  - String：字符串类型。用""和''包裹的内容，称为字符串。
  - Number：数值类型。可以是小数，也可以是正数。Number可以表示十进制，八进制，十六进制整数，浮点数，科学记数法，最大整数是2^53，BigInt可以表述任意大的整数
  - boolean：真假，可选值true/false。
  - Object：（复杂数据类型）
  - Null：表示为空的引用。var a = null; null表示一个对象不存在，其数据类型为Object
  - Undefined：未定义，用var声明的变量，没有进行初始化赋值。var a; 
    - 声明了但未赋值的变量，其值是 undefined ，typeof 也返回 undefined
    - 任何变量均可通过设置值为 undefined 进行清空。其类型也将是 undefined
    - 空值与 undefined 不是一回事，空的字符串变量既有值也有类型。
- 语句
  - 语句分号（ ；）结尾，大括号包裹语句块（基本与Java语法类似）；严格区分大小写；没有添加分号时浏览器自动添加，但是消耗资源并且可能添加出错
- 类型判断（[js数据类型判断](https://www.cnblogs.com/yadiblogs/p/10750775.html)）
  - typeof(a)
  - toString最完美
  - instanceof
    - instanceof 是用来判断 A 是否为 B 的实例，表达式为：A instanceof B，如果 A 是 B 的实例，则返回 true,否则返回 false。 在这里需要特别注意的是：instanceof 检测的是原型
  - constructor
    - constructor是原型prototype的一个属性，当函数被定义时候，js引擎会为函数添加原型prototype
    - ![](https://img2018.cnblogs.com/blog/1334093/201904/1334093-20190422154822998-1507326377.png)
    - 注意：①null 和 undefined 无constructor，这种方法判断不了。②如果自定义对象，开发者重写prototype之后，原有的constructor会丢失
- 元素遍历, [js数组遍历](https://www.cnblogs.com/woshidouzia/p/9304603.html)
  - 1.for循环： 4个元素的arr， 普通for循环最优雅
    - for(j = 0; j < arr.length; j++) # 循环4次
    - for(j = 0,len=arr.length; j < len; j++) # 循环4次，优化，算一次长度
    - for(j = 0; arr[j]!=null; j++) # 性能弱于上面
  - forin
    - for(a in arr) # 循环5次，末尾是undefine
  - 2.foreach循环
  - 3.map循环
  - 4.forof遍历
    - for(let value of arr) # 需要ES6支持，forin＜性能＜for
  - 5.filter遍历
  - 6.every遍历
  - 7.some遍历
  - 8.reduce
  - 9.reduceRight
  - 10.find
  - 11.findIndex
  - 12.keys，values，entries
- 总结: [JS几种数组遍历方式总结](https://blog.csdn.net/function__/article/details/79555301)
  - ![](https://dailc.github.io/jsfoundation-perfanalysis/staticresource/performanceAnalysis/demo_js_performanceAnalysis_jsarrayGoThrough_1.png)
- [JavaScript基础知识整理](https://zhuanlan.zhihu.com/p/68963487)
- 变量
  - 变量是用于存储信息的"容器"，是命名的内存空间，可以使用变量名称找到该内存空间；
  - JavaScript 的变量是松散类型（弱类型）的，就是用来保存任何类型的数据。在定义变量的时候不需要指定变量的数据类型。
  - JavaScript 定义变量有四种方法：const、let、var，还有一种是直接赋值，比如a = " a"（不规范，不推荐使用）
    - var 定义的变量可以修改，如果不初始化会输出undefined，不会报错。
    - let let是块级作用域，定义的变量只在let 命令所在的代码块内有效，变量需要先声明再使用。
    - const 定义的变量不可以修改，而且必须初始化，const定义的是一个恒定的常量，声明一个只读的常量或多个，一旦声明，常量值就不能改变。
  - 作用域
    - 在函数外声明的变量作用域是**全局**的，全局变量在 JavaScript 程序的任何地方都可以访问；
    - 在函数内声明的变量作用域是**局部**的（函数内），函数内使用 var 声明的变量只能在函数内容访问。
- 对象
  - JavaScript 对象是拥有属性和方法的数据，是变量的容器。对象：是封装一个事物的属性和功能的程序结构，是内存中保存多个属性和方法的一块存储空间。JavaScript中所有事物都是对象：数字、字符串、日期、数组等。JavaScript对象可以是字面量创建、分配给变量，数组和其他对象的属性、
作为参数传递给函数、有属性和作为返回值。
- JS**对象**分为三类：
  - **内置**对象（静态对象）：js本身已经写好的对象，可以直接使用不需要定义它。
    - 常见的内置对象有 Global、Math（它们也是本地对象，根据定义每个内置对象都是本地对象）。
  - **本地**对象（非静态对象）：必须实例化才能使用其方法和属性的就是本地对象。
    - 常见的本地对象有 Object、Function、Data、Array、String、Boolean、Number、RegExp、Error等
  - **宿主**对象：js运行和存活的地方，它的生活环境就是DOM（文档对象模式）和BOM（浏览器对象模式）。
- JavaScript函数
  - 使用函数前要先定义才能调用，函数的定义分为三部分：函数名，参数列表
  - 四种**调用**模式：
    - 函数调用模式（通过函数调用）
    - 方法调用模式（通过对象属性调用）
    - 构造函数模式（如果是作为构造函数来调用，那么this指向new创建的新对象）
    - 函数上下文（借用方法模式：它的this指向可以改变，而前三种模式是固定的）；
    - 函数上下文就是函数作用域；基本语法：apply 和 call 后面都是跟两个参数。）
  - 在javascript函数中，函数的**参数**一共有两种形式：（实际参数与形式参数）
    - **形参**：在函数定义时所指定的参数就称之为“函数的形参”。
    - **实参**：在函数调用时所指定的参数就称之为“函数的实参”。
- this
  - 方法中的this指向调用它所在方法的对象。单独使用this，指向全局对象。函数中，函数所属者默认绑定到this上。
- 闭包
  - 闭包是指有权访问另一个函数作用域中的变量的函数。创建闭包就是创建了一个不销毁的作用域。闭包需要了解的几个概念： 作用域链、执行上下文、变量对象。
- Window
  - 所有浏览器都支持 window 对象。它表示浏览器窗口。所有 JavaScript 全局对象、函数以及变量均自动成为 window 对象的成员。
  - 全局变量是 window 对象的属性。全局函数是 window 对象的方法。
  - 如：Document对象包含当前文档的信息，例如：标题、背景、颜色、表格等，screen，location，history等
- JSON
  - JSON 是一种轻量级的数据交换格式；JSON是独立的语言 ；JSON 易于理解。
- 输出方式
  - document.write()    //向body中写入字符串，输出到页面，会以HTML的语法解析里面的内容
  - cosole.log()    //向控制台输出
  - alert()     //弹出框，会以文本的原格式输出
  - prompt('提示文字'，'默认值') // 输入框---不常用

```javascript
myObj =  { "name":"Nya", "age":21, "car":null };
// 访问对象JSON值,嵌套的JSON对象，使用点号和括号访问嵌套的JSON对象
x = myObj.name;
x = myObj["name"];

console.log(typeof null);   //返回object

function demo(){  
    console.log('demo');  
}  
console.log(typeof demo);   // 返回function 
// 分支
var a = 1;
switch(a){
    case 1:
        console.log("1");
        break;
    case 2:
        console.log("2");
        break;
    default:
        console.log("其他");
        break;
}
// for
function p(i){
    document.write(i);
    document.write("<br>");
 }
for(var i = 0; i < 10; i++){
     p(i);
 }
// for in
for (x in myObj){
    document.write(myObj[x] + "<br />")
}

//new创建对象
var person = new Person();
person.name = "Nya";
person.age = 21;
person.sex = "男";
//创建了对象的一个新实例，并向其添加了四个属性
//函数创建对象
function person(name, age, sex){
    this.name = name;
    this.age = age;
    this.sex = sex  //在JS中，this通常指向的是我们正在执行的函数本身，或者是指向该函数所属的对象（运行时）
}
//创建对象实例
var myFather = new person("Ton", 51, "男");
var myMother = new person("Sally", 49, "女");
// 返回一个包含所有的cookie的字符串，每条cookie以分号和空格(; )分隔(即key*=*value键值对)：
allCookies = document.cookie;
// 设置高度、宽度
document.write("可用宽度: " + screen.availWidth + '高度：' +screen.availHeight); 
//改变当前网页地址（加载新的网页）：
location.href = 'http://www.baidu.com';
//返回（当前页面的)整个URL：
document.write(location.href);
// 自调用函数，匿名函数
(function () {
    var x = "Hello!!";      // 我将调用自己
})();
//以上函数实际上是一个匿名自我调用的函数(没有函数名)
```

- html示例：
  - 上一页/下一页

```html
<script type="text/javascript"> 
    自己编写的js代码
</script>
<!-- ① 将上面的代码放在<head></head>或者<body></body>之间 -->
<!-- ② 直接保存为js文件，然后外部调用<script type="text/javascript" src="js文件"></script> -->

<input type="button" value="Back" onclick="goBack()">
<script>
    function goBack(){
        window.history.back()
    }
</script>

<input type="button" value="Forward" onclick="goForward()">
<script>
    function goBack(){
        window.history.forwardk()
    }
</script>

<!-- 交互事件 -->
<h1 onclick="this.innerHTML='Ooops!'">点击文本!</h1>

<!-- 操作DOM元素，例：向button元素分配onclick事件 -->
document.getElementById("myBtn").onclick=function(){displayDate()};
<!-- 操作style样式 -->
document.getElementsByClassName('box')[0].style.background = 'red';

```

### JavaScript 框架（库）

前端三大框架：Angular、React、Vue
- jQuery: 大家熟知的JavaScript框架，优点是简化了DOM操作，缺点是DOM操作太频繁,影响前端性能;在前端眼里使用它仅仅是为了兼容IE6、7、8。
  - jQuery 函数是 $() 函数（jQuery 函数）。jQuery 库包含以下功能：
  - HTML 元素选取、元素操作、CSS 操作、HTML 事件函数、JavaScript 特效和动画、
  - HTML DOM 遍历和修改、AJAX、Utilities
  - 面向对象编程包括 创建对象、原型继承、class继承。
  - 类是对象的类型模板；实例是根据类创建的对象。
- （1）`Angular`: Google收购的前端框架，由一群Java程序员开发，其特点是将后台的MVC模式搬到了前端并增加了模块化开发的理念，与微软合作，采用TypeScript语法开发;对后台程序员友好，对前端程序员不太友好;最大的缺点是版本迭代不合理(如: 1代-> 2代，除了名字，基本就是两个东西;截止发表博客时已推出了Angular6)。
  - 其最为核心的特性为：MVC、模块化、自动化双向数据绑定、语义化标签及依赖注入等。
- （2）`React`: Facebook出品，一款高性能的JS前端框架;特点是提出了新概念[虚拟DOM]用于减少真实DOM操作，在内存中模拟DOM操作，有效的提升了前端渲染效率;缺点是使用复杂，因为需要额外学习一门[JSX] 语言。
  - React被称为构建用户接口而提供的Javascript库；主要用来构建UI，其专注于MVC的V部分。
- （3）`Vue`:一款渐进式JavaScript框架，所谓渐进式就是逐步实现新特性的意思，如实现模块化开发、路由、状态管理等新特性。其特点是综合了Angular (模块化)和React (虚拟DOM)的优点;。
  - vue.js 是用来构建web应用接口的一个库，技术上，Vue.js 重点集中在MVVM模式的ViewModel层，它连接视图和数据绑定模型通过两种方式。
  - 【2021-10-28】[Vue可视化拖拽编辑工具附源码](https://www.toutiao.com/i7023912492678005262)，拖拽生成vue代码
  - ![](https://p6.toutiaoimg.com/origin/pgc-image/6ca0532facf74b0aac668b0ca44bf132?from=pc)
- Axios :前端通信框架；因为Vue 的边界很明确，就是为了处理DOM，所以并不具备通信能力，此时就需要额外使用一个通信框架与服务器交互；当然也可以直接选择使用jQuery提供的AJAX通信功能。
- D3.js
  - 数据可视化和图表是Web应用中不可或缺的一部分。d3.js就是最流行的可视化库之一，它允许绑定任意数据到DOM，然后将数据驱动转换应用到Document中。

UI框架
- Ant-Design：阿里巴巴出品，基于React的UI框架
- ElementUI、 iview、 ice: 基于Vue的UI框架
- Bootstrap：Twitter推出的一个用于前端开发
- AmazeUI：又叫"妹子UI"，一款HTML5跨屏前端框架

JavaScript构建工具
- Babel: JS编译工具，主要用于浏览器不支持的ES新特性，比如用于编译TypeScript
- WebPack: 模块打包器，主要作用是打包、压缩、合并及按序加载

### JavaScript本地储存

- 【2021-3-23】[JavaScript本地储存有localStorage、sessionStorage、cookie多种方法](https://www.jb51.net/article/197357.htm)
1. **sessionStorage**：仅在当前会话下有效，关闭页面或浏览器后被清除；
  - setItem(key,value) 设置数据
  - getItem(key) 获取数据
  - removeItem(key) 移除数据
  - clear() 清除所有值
2. **localStorage** ：HTML5 标准中新加入的技术，用于长久保存整个网站的数据，保存的数据没有过期时间，直到手动去删除；
  - localStorage和sessionStorage最大一般为5MB，仅在客户端（即浏览器）中保存，不参与和服务器的通信；
  - 主要方法同sessionStorage
3. **Cookie**
  - Cookie 是一些数据, 存储于你电脑上的文本文件中，用于存储 web 页面的用户信息. Cookie 数据是以键值对的形式存在的，每个键值对都有过期时间。如果不设置时间，浏览器关闭，cookie就会消失，当然用户也可以手动清除cookie. Cookie每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题, Cookie内存大小受限，一般每个域名下是4K左右，每个域名大概能存储50个键值对
  - 通过访问document.cookie可以对cookie进行创建，修改与获取。默认情况下，cookie 在浏览器关闭时删除，你还可以为 cookie的某个键值对 添加一个过期时间. 如果设置新的cookie时，某个key已经存在，则会更新这个key对应的值，否则他们会同时存在cookie中
- **总结**
  - 相同点：都保存在浏览器端
  - 不同点
    - ① **传递方式**不同
      - cookie数据始终在**同源http请求**中携带（即使不需要），即cookie在浏览器和服务器间来回传递。
      - sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。
    - ② **数据大小**不同
      - cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下。
      - 存储大小限制也不同，cookie数据不能超过4k，同时因为每次http请求都会携带cookie，所以cookie只适合保存很小的数据，如会话标识。
      - sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。
    - ③ 数据**有效期**不同
      - sessionStorage：仅在**当前浏览器窗口**关闭前有效，自然也就不可能持久保持；
      - localStorage：**始终有效**，窗口或浏览器关闭也一直保存，因此用作持久数据；
      - cookie只在设置的cookie**过期时间之前**一直有效，即使窗口或浏览器关闭。
    - ④ **作用域**不同
      - sessionStorage不在**不同浏览器窗口**中共享，即使是同一个页面；
      - localStorage 在所有同源窗口中都是共享的；
      - cookie也是在**所有同源窗口**中都是共享的。
      - Web Storage 支持事件通知机制，可以将数据更新的通知发送给监听者。Web Storage 的 api 接口使用更方便。

- 代码

```js
// ============sessionStorage============
// 添加数据
window.sessionStorage.setItem("name","李四")
window.sessionStorage.setItem("age",18)
// 获取数据
console.log(window.sessionStorage.getItem("name")) // 李四
// 清除某个数据
window.sessionStorage.removeItem("gender")
// 清空所有数据
window.sessionStorage.clear()
// ============localStorage============
// 添加数据
window.localStorage.setItem("name","张三")
window.localStorage.setItem("age",20)
window.localStorage.setItem("gender","男")
// 获取数据
console.log(window.localStorage.getItem("name")) // 张三
// 清除某个数据
window.localStorage.removeItem("gender")
// 清空所有数据
window.localStorage.clear()
// ===========cookie==========
// 设置cookie
document.cookie = "username=orochiz"
document.cookie = "age=20"
// 读取cookie
var msg = document.cookie
console.log(msg) // username=orochiz; age=20
// 添加过期时间（单位：天）
var d = new Date() // 当前时间 2019-9-25
var days = 3    // 3天
d.setDate(d.getDate() + days)
document.cookie = "username=orochiz;"+"expires="+d
// 删除cookie （给某个键值对设置过期的时间）
d.setDate(d.getDate() - 1)
console.log(document.cookie)
```

### 异步请求Ajax

- 请求：
  - 同步请求:只有当一次请求完全结束以后才能够发起另一次请求
  - 异步请求:不需要其他请求结束就可以向服务器发起请求
- 向服务器发起请求的时候，服务器不会像浏览器响应整个页面，而是只有局部刷新。它是一个异步请求，浏览器页面只需要进行局部刷新，效率非常的高

## HTML

### 文字效果

【2022-9-25】[html文字效果](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Introduction_to_HTML/Advanced_text_formatting)

六个标题元素标签 —— < h1 >、< h2 >、< h3 >、< h4 >、< h5 >、< h6 >。每个元素代表文档中不同级别的内容;
-  < h1 > 表示主标题（the main heading）
- < h2 > 表示二级子标题（subheadings）
- < h3 > 表示三级子标题（sub-subheadings），等等
- < span > 任一元素看起来像一个顶级标题, span元素没有语义。当想要对它用 CSS（或者 JS）时，可以用它包裹内容，且不需要附加任何额外的意义

斜体、加粗、下划线
- < i > 被用来传达传统上用**斜体**表达的意义：外国文字，分类名称，技术术语，一种思想……
- < b > 被用来传达传统上用**粗体**表达的意义：关键字，产品名称，引导句……
- < u > 被用来传达传统上用**下划线**表达的意义：专有名词，拼写错误……

大量的 HTML 元素可以来标记计算机代码：
- < code>: 用于标记计算机通用代码。
- < pre>: 用于保留空白字符（通常用于代码块）——如果您在文本中使用缩进或多余的空白，浏览器将忽略它，您将不会在呈现的页面上看到它。但是，如果您将文本包含在< pre> < /pre>标签中，那么空白将会以与你在文本编辑器中看到的相同的方式渲染出来。
- < var>: 用于标记具体变量名。
- < kbd>: 用于标记输入电脑的键盘（或其他类型）输入。
- < samp>: 用于标记计算机程序的输出。

HTML 还支持将时间和日期标记为可供机器识别的格式的 < time> 元素

```html
<p>我是一个段落，千真万确。</p>
<h1>我是文章的标题</h1>

<h1>静夜思</h1>
<p>床前明月光 疑是地上霜</p>
<p>举头望明月 低头思故乡</p>

<span style="font-size: 32px; margin: 21px 0;">这是顶级标题吗？</span>

无序列表
<ul>
  <li>豆浆</li>
  <li>油条</li>
  <li>豆汁</li>
  <li>焦圈</li>
</ul>

有序列表，可以嵌套

<ol>
  <li>沿着条路走到头</li>
  <li>右转</li>
  <li>直行穿过第一个十字路口</li>
  <li>在第三个十字路口处左转</li>
  <li>继续走 300 米，学校就在你的右手边</li>
  <ul>
    <li>豆浆</li>
    <li>焦圈</li>
  </ul>
</ol>

<p>This liquid is <strong>highly toxic（加粗）</strong> —
if you drink it, <strong>you may <em>die（斜体）</em></strong>.</p>

<p>
  红喉北蜂鸟（学名：<i>Archilocus colubris（斜体）</i>）
  菜单上有好多舶来词汇，比如 <i lang="uk-latn">vatrushka</i>（东欧乳酪面包）,
  <i lang="id">nasi goreng</i>（印尼炒饭）以及<i lang="fr">soupe à l'oignon</i>（法式洋葱汤）。
  <b>加粗</b>，<u>下划线</u>
  总有一天我会改掉写<u style="text-decoration-line: underline; text-decoration-style: wavy;">措字（下划线，波浪线）</u>的毛病。
</p>
缩略语：abbr用法
<p>第 33 届 <abbr title="夏季奥林匹克运动会">奥运会</abbr> 将于 2024 年 8 月在法国巴黎举行。</p>

上标、下标
<p> 上标：X<sup>2, 下标<sub>3 </p>

<pre><code>const para = document.querySelector('p');

para.onclick = function() {
  alert('噢，噢，噢，别点我了。');
}</code></pre>

<p>请不要使用 <code>&lt;font&gt;</code> 、 <code>&lt;center&gt;</code> 等表象元素。</p>

<p>在上述的 JavaScript 示例中，<var>para</var> 表示一个段落元素。</p>


<p>按 <kbd>Ctrl</kbd>/<kbd>Cmd</kbd> + <kbd>A</kbd> 选择全部内容。</p>

<pre>$ <kbd>ping mozilla.org</kbd>
<samp>PING mozilla.org (63.245.215.20): 56 data bytes
64 bytes from 63.245.215.20: icmp_seq=0 ttl=40 time=158.233 ms</samp></pre>

<time datetime="2016-01-20">2016 年 1 月 20 日</time>
<!-- 标准简单日期 -->
<time datetime="2016-01-20">20 January 2016</time>
<!-- 只包含年份和月份-->
<time datetime="2016-01">January 2016</time>
<!-- 只包含月份和日期 -->
<time datetime="01-20">20 January</time>
<!-- 只包含时间，小时和分钟数 -->
<time datetime="19:30">19:30</time>
<!-- 还可包含秒和毫秒 -->
<time datetime="19:30:01.856">19:30:01.856</time>
<!-- 日期和时间 -->
<time datetime="2016-01-20T19:30">7.30pm, 20 January 2016</time>
<!-- 含有时区偏移值的日期时间 -->
<time datetime="2016-01-20T19:30+01:00">7.30pm, 20 January 2016 is 8.30pm in France</time>
<!-- 调用特定的周 -->
<time datetime="2016-W04">The fourth week of 2016</time>

```


### 分栏

【2020-8-24】Web页面分栏效果实现
- HTML中Frame实现左右分栏或上下分栏
- FrameSet中的Cols属性就控制了页面中的左右分篮，相应的rows则控制上下分栏
- 分栏的比例就有用逗号分开的10，*来确定



```html
<frameset border=10 framespacing=10 cols=”10,*” frameborder="1"   TOPMARGIN="0"  LEFTMARGIN="0" MARGINHEIGHT="0" MARGINWIDTH="0">
  <frame>
 <frame>
</framset>
```




### 下拉框

- 【2021-6-8】下拉框提供选项，触发onchange时间，[示例](https://bbs.csdn.net/topics/270085808)：

```html
<html>
<head>
    <script language="javascript" type="text/javascript">
    function modify(osel){ // 下拉框动作变更通知
        value = osel.options[osel.selectedIndex].text; //text
        alert('你已选择：'+value);
        //sessionStorage.setItem("product", value); // 本地session
        //var product = window.sessionStorage.getItem('product');
        //content.innerHTML += product;
    }
    function SetIndex(v){
      var s=document.getElementById('selectSS');
      s.selectedIndex=v;
      if(s.onchange)s.onchange();
      //sessionStorage.setItem("product", v);
    }
    </script>
</head>

<body>
  <select id="selectSS" onChange="modify(this)">
        <option value="1">第一项</option>
        <option selected value="2">第二项(默认选中)</option>
        <option value="3">第三项</option>
        <option value="4">第四项</option>
  </select>
  <a href="#" onclick="SetIndex(0)">重置</a>
   
   <div class=content>
   获取的选项内容：
   </div>
</body>
</html>
```

### 输入框提示

HTML5的datalist可以实现[历史消息提示](https://www.cnblogs.com/jacko/p/6034196.html)

```html
<input id="country_name" name="country_name" type="text" list="city" />  
<datalist id="city">  
    <option value="中国 北京">  
    <option value="中国 上海">  
    <option value="中国 广州">  
    <option value="中国 深圳">  
    <option value="中国 东莞">  
</datalist> 

```

## ajax

- [ajax post 请求发送 json 字符串](https://www.cnblogs.com/virgosnail/p/10108997.html)

代码：

```javascript
    $.ajax({
        // 请求方式
        type:"post",
        // contentType 
        contentType:"application/json",
        // dataType
        dataType:"json",
        // url
        url:url,
        // 把JS的对象或数组序列化一个json 字符串
        data:JSON.stringify(data),
        // result 为请求的返回结果对象
        success:function (result) {
            if (200 == result.code){
                alert("启动成功");
            }else{
                alert("启动失败");
            }
        }
    });
```


## node.js

node.js、npm、vue、webpack之间的关系
- **node.js**是javascript运行的环境，以前只能浏览器解析js，现在直接用chrome的v8引擎封装成nodejs，实现js独立于浏览器也可以解析运行
- **npm**，前端依赖包管理器（包含在nodejs中），类似maven，帮助下载和管理前端的包，这个下载源是外国服务器，如果想提高下载速度的话，建议更换成淘宝镜像，类似maven之于阿里云镜像。
- **vue.js** 前端框架，其他大火的前端框架：anjularjs
- **WebPack** webpack能够把.vue后缀名的文件打包成浏览器能够识别的js，而这个.vue文件装换需要打包器vue-loader→npm下载→node包管理工具
  - 可以看做是模块打包机，它做的事情：分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其转换和打包为合适的格式供浏览器使用。

## bootstrap

[bootstrap模板集合](http://www.cssmoban.com/cssthemes/houtaimoban/)


## vue

【2021-7-21】
- [vue学习笔记（超详细）](https://blog.csdn.net/fmk1023/article/details/111381876)
- [（Web前端）优秀的后台管理框架收集](https://blog.csdn.net/Mr_Quinn/article/details/88565830)
- [vue视频教程](https://scrimba.com/g/gvuedocs)


### vue介绍

Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。


### npm与yarn

Facebook、Google、Exponent 和 Tilde 联合推出了一个新的 JS 包管理工具 — Yarn

Yarn 是为了弥补 npm 的一些缺陷而出现的：
- npm 安装包（packages）的速度不够快，拉取的 packages 可能版本不同
- npm 允许在安装 packages 时执行代码，这就埋下了安全隐患

Yarn 没想要完全替代 npm，它只是一个新的 CLI 工具，拉取的 packages 依然来自 npm 仓库。仓库本身不会变，所以获取或者发布模块的时候和原来一样。

### vue安装

1. 安装nodejs，命令行输入 node –v检测安装是否成功
2. 安装vue-cli脚手架：npm i -g @vue/cli-init,命令行输入 vue –v检测安装是否成功
3. 新建项目vue init webpack myVue
4. 打开项目文件夹 myVue，npm install 下载依赖
5. 运行项目 npm start

项目结构：
- ![](https://p26.toutiaoimg.com/img/tos-cn-i-qvj2lq49k0/a82831d256f94bea8bcaaa301a4470b6~tplv-obj:1067:1858.image?from=post)

### vue hello world

#### vue代码运行方式

运行vue的三种方式：
- ① 在第三方网页里调试，如：[CodeSandBox](https://codesandbox.io/s/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-hello-world?file=/index.html)，可以再浏览器里直接调试
- ② 直接用html文件，如以下示例代码
- ③ 使用vue-cli工具 —— 新手不建议，除非已使用node.js工具

#### vue示例

简易示例：
- html、js、css分离
- html中变量引用自js代码
- 注：注意去掉大括号中间的空格（jekyll语法规避）

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Vue 测试实例 - 菜鸟教程(runoob.com)</title>
  <script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script>
  <!-- 开发环境版本，包含了有帮助的命令行警告 -->
  <!-- <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script> -->
  <!-- 生产环境版本，优化了尺寸和速度 -->
  <!-- <script src="https://cdn.jsdelivr.net/npm/vue@2"></script> -->
</head>

<body>
  <div id="app">
    <p>{ { message } }</p>
    <br>{ {v} }
  </div>

  <script>
    new Vue({
      el: '#app',
      data: {
        message: 'Hello Vue.js!',
        v : "你好"
      }
    })
  </script>

</body>
</html>
```

Vue.js 的核心是一个允许采用简洁的**模板语法**来声明式地将数据渲染进 DOM 的系统
- 数据和 DOM 已经被建立了**关联**，所有东西都是**响应式**的。浏览器控制台修改message变量，可以看到页面取值随时变化
- 开发者不再和 HTML 直接交互了。一个 Vue 应用会将其挂载到一个 DOM 元素上（即示例中的app）

### vue目录结构

Vue项目结构：
- ![](https://p26.toutiaoimg.com/img/tos-cn-i-qvj2lq49k0/a82831d256f94bea8bcaaa301a4470b6~tplv-obj:1067:1858.image?from=post)

顶级目录：

| 目录/文件 |   说明|
|---|-----|
| build | 项目构建(webpack)相关代码 |
| config |  配置目录，包括端口号等。我们初学可以使用默认的。|
| node_modules |    npm 加载的项目依赖模块 |
| src | 这里是我们要开发的目录，基本上要做的事情都在这个目录里。里面包含了几个目录及文件：<br> - assets: 放置一些图片，如logo等。<br> - components: 目录里面放了一个组件文件，可以不用。<br> - App.vue: 项目入口文件，我们也可以直接将组件写这里，而不使用 components 目录。<br> - main.js: 项目的核心文件。|
| static |  静态资源目录，如图片、字体等。|
| test |    初始测试目录，可删除 |
| .xxxx文件   | 这些是一些配置文件，包括语法配置，git配置等。|
| index.html |  首页入口文件，你可以添加一些 meta 信息或统计代码啥的。|
| package.json  | 项目配置文件。|
| README.md | 项目的说明文档，markdown 格式|

APP.vue 文件 是项目核心

src下文件：

```shell
-src
    -assets # 静态资源
        -css 
        -js 
    -components # 公共组件
        index.js #  整合公共组件
    -filters # 过滤器
        index.js # 整合过滤器
    -pages # 路由组件
        -home # 某个路由组件
            home.vue  # 路由组件
            -components # 路由组件的子组件
                banner.vue
                list.vue
    -router # 路由
        index.js
    -store # 仓库
        index.js # 创建仓库并导出
        mutations.js # 根级别下 的state mutations getters
        acions.js # 根级别下的actions
        -modules # 模块
    -utils # 工具类
        alert.js # 弹框
        request.js # 数据交互
    App.vue # 根组件
    main.js # 入口文件
```

### MVVM模式

[Vue.js 60分钟快速入门](https://www.cnblogs.com/keepfool/p/5619070.html)

MVVM模式（Model-View-ViewModel）在Vue.js中ViewModel是如何和View以及Model进行交互的
- ![](http://cn.vuejs.org/images/mvvm.png)

ViewModel是Vue.js的核心，它是一个Vue实例。Vue实例是作用于某一个HTML元素上的，这个元素可以是HTML的body元素，也可以是指定了id的某个元素。
当创建了ViewModel后，双向绑定是如何达成的呢？
- 首先，我们将上图中的DOM Listeners和Data Bindings看作两个工具，它们是实现双向绑定的关键。
- 从View侧看，ViewModel中的DOM Listeners工具会帮我们监测页面上DOM元素的变化，如果有变化，则更改Model中的数据；
- 从Model侧看，当我们更新Model中的数据时，Data Bindings工具会帮我们更新页面中的DOM元素。

【2022-3-10】[图解vue的MVVM架构](https://www.toutiao.com/w/i1726646484331596)

Vue是MVVM架构，响应式，轻量级框架。

主要特点：
- 1、轻量级
- 2、双向数据绑定
- 3、指令
- 4、组件化
- 5、客户端路由
- 6、状态管理

MVVM架构是指：
- 数据层（Model）：应用数据以及逻辑。
- 视图层（View）：页面UI组件。
- 视图数据模型（ViewModel）：数据与视图关联起来，数据和 DOM 已经建立了关联，是响应式的，使编程人员脱离复杂的界面操作
  - ViewModel主要功能是实现数据双向绑定：
  - 1.数据变化后更新视图
  - 2.视图变化后更新数据

与jQuery比较会更清晰：
- jQuery想要修改界面中某个标签，需要以下几步：
  1. 搜索web页面DOM树 
  2. 根据选择器选择到DOM  
  3. 将数据更新到节点

![](https://p9.toutiaoimg.com/img/tos-cn-i-qvj2lq49k0/b963a949aab044f8acd8a2bc50f8d3f0~tplv-obj:2002:2009.image?from=post)

### vue实例

每个 Vue 应用都是通过用 Vue 函数创建一个新的 Vue 实例开始；虽然没有完全遵循 MVVM 模型，但是 Vue 的设计也受到了它的启发。因此经常用 vm (ViewModel 的缩写) 这个变量名表示 Vue 实例
- 一个**Vue 应用**由 一个通过 new Vue 创建的**根 Vue 实例**，以及可选的嵌套的、可复用的**组件树**组成
- 所有的 Vue 组件都是 Vue 实例，并且接受相同的选项对象 (一些根实例特有的选项除外)。

```
根实例
└─ TodoList
   ├─ TodoItem
   │  ├─ TodoButtonDelete
   │  └─ TodoButtonEdit
   └─ TodoListFooter
      ├─ TodosButtonClear
      └─ TodoListStatistics
```

Vue 实例被创建时，它将 data 对象中的所有的 property 加入到 Vue 的响应式系统中。当这些 property 的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。
- 数据改变时，视图会进行重渲染。
- 注意：
  - 只有当实例被创建时就已经存在于 data 中的 property 才是响应式的。
  - 关闭响应功能：Object.freeze(obj)

```js
// 我们的数据对象
var data = { a: 1 }
// Object.freeze(obj) // 关闭响应
// 该对象被加入到一个 Vue 实例中
var vm = new Vue({
  data: data
})
// 获得这个实例上的 property, 返回源数据中对应的字段
vm.a == data.a // => true
// 设置 property 也会影响到原始数据
vm.a = 2
data.a // => 2
// ……反之亦然
data.a = 3
vm.a // => 3
vm.b = 'hi' // 不管用，非创建时定义
// ------ vue自带函数，带$符号 ------
vm.$data === data // => true
vm.$el === document.getElementById('example') // => true
// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```

#### 实例生命周期

![](https://cn.vuejs.org/images/lifecycle.png)


#### 实例生命周期钩子

每个 Vue 实例在被创建时都要经过一系列的初始化过程
- 需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。
- 同时会运行一些**生命周期钩子**的函数，这给了用户在不同阶段添加自己的代码的机会。

created 钩子用来在实例被创建之后执行代码：

```js
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```

其他钩子：如 mounted、updated 和 destroyed。生命周期钩子的 this 上下文指向调用它的 Vue 实例。

### vue语法

【2022-3-15】[vue.js教程](https://cn.vuejs.org/v2/guide/computed.html)

#### vue经典代码结构

【2022-3-1】[vue教程](https://www.runoob.com/vue2/vue-start.html)

打开页面 http://localhost:8080/，一般修改后会自动刷新，显示效果如下所示
- ![](https://www.runoob.com/wp-content/uploads/2017/01/AEDE7289-0479-4F14-A9C9-898470E5620E.jpg)

```js
<!-- 展示模板 -->
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <hello></hello>
  </div>
</template>
 
<script>
// 导入组件
import Hello from './components/Hello'
 
export default {
  name: 'app',
  components: {
    Hello
  }
}
</script>

<!-- 样式代码 -->
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

Vue.js的指令是以v-开头的，它们作用于**HTML元素**，指令提供了一些特殊的特性，将指令绑定在元素上时，指令会为绑定的目标元素添加一些特殊的行为，我们可以将指令看作特殊的HTML特性（attribute）。

Vue.js提供了一些常用的内置指令，接下来我们将介绍以下几个内置指令：
- v-if 指令：**条件渲染**指令，它根据表达式的真假来删除和插入元素
- v-show 指令：也是**条件渲染**指令，和v-if指令不同的是，使用v-show指令的元素始终会被渲染到HTML，它只是简单地为元素设置CSS的style属性。
- v-else 指令：为v-if或v-show添加一个“else块”。v-else元素必须立即跟在v-if或v-show元素的后面——否则它不能被识别。
- v-for 指令：基于一个数组渲染一个列表，它和JavaScript的遍历语法相似
- v-bind 指令：v-bind指令可以在其名称后面带一个参数，中间放一个冒号隔开，这个参数通常是HTML元素的特性（attribute），如 v-bind:class
- v-on 指令：用于给监听DOM事件，它的用语法和v-bind是类似的，例如监听< a >元素的点击事件，如 < a v-on:click="doSomething">
  - 两种形式调用方法：绑定一个方法（让事件指向方法的引用），或者使用内联语句。

#### vue模板语法

几种语法：
- 文本：数据绑定最常见的形式，用“Mustache”语法 (双大括号) 的文本插值
  - v-once 指令只执行一次插值
- 原始html：用 v-html 指令输出真正的 HTML
  - 注意：动态渲染的任意 HTML 可能会非常危险，因为它很容易导致 XSS 攻击。请只对可信内容使用 HTML 插值，绝不要对用户提供的内容使用插值。
- Attribute：v-bind命令渲染属性
- JavaScript 表达式：Vue.js提供了完全的 JavaScript 表达式支持
  - 表达式会在所属 Vue 实例的数据作用域下作为 JavaScript 被解析。
  - 限制：每个绑定都只能包含单个表达式，所以下面的例子都不会生效。
  - 模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 Math 和 Date 。你不应该在模板表达式中试图访问用户定义的全局变量。

```html
<!-- 文本 -->
<span>Message: { { msg } }</span>
<span v-once>这个将不会改变: { { msg } }</span>
<!-- 原始html -->
<p>Using mustaches: { { rawHtml } }</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
<!-- 属性 -->
<div v-bind:id="dynamicId"></div>
<!-- bool型属性控制组件是否展示 -->
<button v-bind:disabled="isButtonDisabled">Button</button> 
<!-- js表达式 -->
{ { number + 1 } }
{ { ok ? 'YES' : 'NO' } }
{ { message.split('').reverse().join('') } }
<div v-bind:id="'list-' + id"></div>
<!-- 这是语句，不是表达式 -->
{ { var a = 1 } }
<!-- 流控制也不会生效，请使用三元表达式 -->
{ { if (ok) { return message } } }
```



#### 绑定元素：DOM文本

Vue.js 的核心是一个允许采用简洁的**模板语法**来声明式地将数据渲染进 DOM 的系统
- 数据和 DOM 已经被建立了**关联**，所有东西都是**响应式**的。浏览器控制台修改message变量，可以看到页面取值随时变化
- 开发者不再和 HTML 直接交互了。一个 Vue 应用会将其挂载到一个 DOM 元素上（即示例中的app）

```html
  <div id="app">
    <p>{ { message } }</p>
    <br>{ {v} }
  </div>

  <script>
    new Vue({
      el: '#app',
      data: {
        message: 'Hello Vue.js!',
        v : "你好"
      }
    })
  </script>
```

### vue指令

指令 (Directives) 是带有 v- 前缀的特殊 attribute。
- 当然，不一定都是v-开头，可以缩写

指令 attribute 的值预期是单个 JavaScript 表达式 (v-for 是例外)。
- 指令的职责：当表达式的值改变时，将其产生的**连带**影响，响应式地作用于 DOM。
- 一些指令能够接收一个“参数”，在指令名称之后以冒号表示。
- 动态参数：从 2.6.0 开始，用方括号括起来的 JavaScript 表达式作为一个指令的参数；约束：
  - 动态参数预期会求出一个字符串，异常情况下值为 null。这个特殊的 null 值可以被显性地用于移除绑定。任何其它非字符串类型的值都将会触发一个警告。
  - 动态参数表达式有一些语法约束，因为某些字符，如空格和引号，放在 HTML attribute 名里是无效的
- 修饰符 (modifier) 是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。

```html
<!-- 完整语法 -->
<a v-bind:href="url">...</a>
<!-- 缩写 -->
<a :href="url">...</a>
<!-- 动态参数的缩写 (2.6.0+) -->
<a :[key]="url"> ... </a>

<!-- 根据表达式 seen 的值的真假来插入/移除 <p> 元素 -->
<p v-if="seen">现在你看到我了</p>
<!-- 指令参数 -->
<!-- href 是参数，告知 v-bind 指令将该元素的 href attribute 与表达式 url 的值绑定 -->
<a v-bind:href="url">...</a>
<!-- click是参数 -->
<a v-on:click="doSomething">...</a>
<!-- 动态参数
attributeName 会被作为一个 JavaScript 表达式进行动态求值，作为最终的参数来使用。
-->
<a v-bind:[attributeName]="url"> ... </a>
<!-- 修饰符
.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault() -->
<form v-on:submit.prevent="onSubmit">...</form>
```

#### v-bind：更改属性

对应替换以上代码的html、js部分即可

```html
<div id="app-2">
  <span v-bind:title="message">
    鼠标悬停几秒钟查看此处动态绑定的提示信息！
  </span>
</div>
```

v-bind attribute 被称为指令
- 将元素节点（app-2）的 title attribute 和 Vue 实例的 message property 保持一致

```js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: '页面加载于 ' + new Date().toLocaleString()
  }
})
```

#### v-if 条件与循环

控制切换一个元素是否显示也相当简单：
- 控制台输入 app3.seen = false，文字消失

```html
<div id="app-3">
  <p v-if="seen">现在你看到我了</p>
  <!-- 加else -->
  <h1 v-if="awesome">Vue is awesome!</h1>
  <h1 v-else>Oh no 😢</h1>
</div>
<!-- if-else-if -->
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else>
  Not A/B
</div>

<!-- 应用到多个元素上 -->
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

注：
- v-else 元素必须紧跟在带 v-if 或者 v-else-if 的元素的后面，否则它将不会被识别
- v-else-if 也必须紧跟在带 v-if 或者 v-else-if 的元素之后。
- v-if只能添加到一个元素上，如何应用到多个元素上？template


```js
  var app3 = new Vue({
    el: '#app-3',
    data: {
      seen: true
    }
  })
```

不仅可以把数据绑定到 DOM 文本或 attribute，还可以绑定到 DOM 结构。此外，Vue 也提供一个强大的过渡效果系统，可以在 Vue 插入/更新/移除元素时自动应用过渡效果。

#### v-show（条件展示）

另一个用于根据条件展示元素的选项是 v-show 指令。用法大致一样，不同的是带有 v-show 的元素始终会被渲染并保留在 DOM 中。v-show 只是简单地切换元素的 CSS property display。

注意
- v-show 不支持 < template > 元素，也不支持 v-else。

```html
<h1 v-show="ok">Hello!</h1>
```

v-if 与 v-show：
- v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
- v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
- 相比之下，v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。
一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。

#### v-for 

v-for 指令可以绑定数组的数据来渲染一个项目列表
- 在控制台里，输入 app4.todos.push({ text: '新项目' })，会发现列表最后添加了一个新项目。
- 数据方法
  - push()
  - pop()
  - shift()
  - unshift()
  - splice()
  - sort()
  - reverse()

```html
<div id="app-4">
  <ol>
    <!-- 可以用of代替in -->
    <li v-for="todo in todos">
      { { todo.text } }
    </li>
  </ol>
<!-- 可以访问所有父作用域的 property（如parentMessage），以及索引index -->
  <ul id="app-4">
  <li v-for="(item, index) in items">
    { { parentMessage } } - { { index } } - { { item.message } }
  </li>
</ul>
<!-- 第二个参数 -->
<div v-for="(value, name) in object">
  { { name } }: { { value } }
</div>
<!-- 第三个参数 -->
<div v-for="(value, name, index) in object">
  { { index } }. { { name } }: { { value } }
</div>
<!-- 遍历对象属性 -->
<ul id="app-4" class="demo">
  <li v-for="value in object">
    { { value } }
  </li>
</ul>
<!-- 指定遍历时采用的key -->
<div v-for="item in items" v-bind:key="item.id">
  <!-- 内容 -->
</div>
<!-- template分组 -->
<ul>
  <template v-for="item in items">
    <li>{ { item.msg } }</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>

</div>

```

js部分

```js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: '学习 JavaScript' },
      { text: '学习 Vue' },
      { text: '整个牛项目' }
    ],
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ],
    object: {
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
      publishedAt: '2016-04-10'
    }
  }
})
```

注：
- 不推荐同时使用 v-if 和 v-for。请查阅风格指南以获取更多信息。
- 当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的优先级。请查阅列表渲染指南以获取详细信息。

数组过滤

```html
<ul v-for="set in sets">
  <li v-for="n in even(set)">{ { n } }</li>
</ul>
```

```js
data: {
  sets: [[ 1, 2, 3, 4, 5 ], [6, 7, 8, 9, 10]]
},
methods: {
  even: function (numbers) { // 过滤部分数据
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```


#### v-on事件监听（处理用户输入）

用 v-on 指令添加一个事件监听器，调用在 Vue 实例中定义的方法
- reverseMessage 方法更新了应用的状态，但没有触碰 DOM —— 所有的 DOM 操作都由 Vue 来处理，代码只需要关注逻辑层面即可。

```html
<div id="app-5">
  <p>{ { message } }</p>
  <button v-on:click="reverseMessage">反转消息</button>
</div>
```

```js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```

#### v-model：双向绑定

Vue 还提供了 v-model 指令，它能轻松实现表单输入和应用状态之间的双向绑定。

```html
<div id="app-6">
  <p>{ { message } }</p>
  <input v-model="message">
</div>
```

```js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```

### vue组件系统

组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。仔细想想，几乎任意类型的应用界面都可以抽象为一个组件树
- ![](https://cn.vuejs.org/images/components.png)
- 大型应用中，有必要将整个应用程序划分为组件，以使开发更易管理。

```html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

Vue 组件类似于自定义元素——它是 Web 组件规范的一部分，这是因为 Vue 的组件语法部分参考了该规范；二者关键差别：
- Web Components 规范已经完成并通过，但未被所有浏览器原生实现。目前 Safari 10.1+、Chrome 54+ 和 Firefox 63+ 原生支持 Web Components。相比之下，Vue 组件不需要任何 polyfill，并且在所有支持的浏览器 (IE9 及更高版本) 之下表现一致。必要时，Vue 组件也可以包装于原生自定义元素之内。
- Vue 组件提供了纯自定义元素所不具备的一些重要功能，最突出的是跨组件数据流、自定义事件通信以及构建工具集成。

虽然 Vue 内部没有使用自定义元素，不过在应用使用自定义元素、或以自定义元素形式发布时，依然有很好的互操作性。Vue CLI 也支持将 Vue 组件构建成为原生的自定义元素。


一个组件本质上是一个拥有预定义选项的一个 Vue 实例。在 Vue 中注册组件很简单

```js
// 定义名为 todo-item 的新组件
Vue.component('todo-item', {
  template: '<li>这是个待办项</li>'
})

var app = new Vue(...)
```

调用刚定义的组件 todo-item

```html
<ol>
  <!-- 创建一个 todo-item 组件的实例 -->
  <todo-item></todo-item>
</ol>
```

这会为每个待办项渲染同样的文本，不符合预期。应该能从**父作用域**将数据传到**子组件**才对。修改一下组件的定义，使之能够接受一个 prop

```js
Vue.component('todo-item', {
  // todo-item 组件现在接受一个"prop"，类似于一个自定义 attribute。
  props: ['todo'], // 这个 prop 名为 todo。
  template: '<li>{ { todo.text } }</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: '蔬菜' },
      { id: 1, text: '奶酪' },
      { id: 2, text: '随便其它什么人吃的东西' }
    ]
  }
})
```

用 v-bind 指令将待办项传到循环输出的每个组件中

```html
<div id="app-7">
  <ol>
    <!--
      现在我们为每个 todo-item 提供 todo 对象; todo 对象是变量，即其内容可以是动态的。需要为每个组件提供一个“key”
    -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
  </ol>
</div>
```

### 计算属性和侦听器

待补充


### vue路由

直接路由，不用路由库
- computed成员 --> ViewComponent()方法 --> 路由到路径字典routes

```js
const NotFound = { template: '<p>Page not found</p>' }
const Home = { template: '<p>home page</p>' }
const About = { template: '<p>about page</p>' }

const routes = {
  '/': Home,
  '/about': About
}

new Vue({
  el: '#app',
  data: {
    currentRoute: window.location.pathname
  },
  computed: {
    ViewComponent () {
      return routes[this.currentRoute] || NotFound
    }
  },
  render (h) { return h(this.ViewComponent) }
})
```

第三方路由，如 Page.js 或者 Director，整合起来也一样简单


### vue框架

#### 拖拽生成页面

【2022-3-16】低代码平台 [Variant Form](https://www.vform666.com/)，只用拖拖拽拽就生成vue页面代码，体验地址：[vform表单设计器](http://120.92.142.115/)


#### vue-element-admin（个人）

- [vue-element-admin](https://panjiachen.github.io/vue-element-admin-site/zh/)，[github](https://github.com/PanJiaChen/vue-admin-template/blob/master/README-zh.md)：极简的 vue admin 管理后台。它只包含了 Element UI & axios & iconfont & permission control & lint, [demo体验地址](https://panjiachen.github.io/vue-element-admin/#/dashboard)
- ![](https://img-blog.csdnimg.cn/20190315091844174.png)

#### 机器人平台demo

【2022-2-22】滴滴机器人平台采用这个模板，django+vue框架，前后端分离
- [前端地址](https://github.com/rhyspang/bot_fe)
- [后端地址](https://github.com/rhyspang/bot_service), 庞胜

```shell
# ------ 前端部分 -------
# brew install yarn
# install dependency
yarn install # 安装依赖包
# npm install
# develop
yarn run dev # 运行前端环境
# npm run dev
```

后端：
- Django执行manage.py 提示 NameError: name '_mysql' is not defined 问题
- 原因是：Mysqldb 不兼容 python3.5 以后的版本
- 解决：用pymysql代替MySQLdb，[参考](https://www.jianshu.com/p/1f0c8e3c438b)
  - 打开项目在setting.py的init.py，或直接在当前py文件最开头添加
  - import pymysql 
  - pymysql.install_as_MySQLdb()

```shell
# ------ 后端部分 -------
pip install -r requirements.txt
# 准备mysql环境
mysql -h 127.0.0.1 -P 3306 -u root -pwangqiwen
vim bot_service/settings.py # 修改mysql连接信息，DATABASES选项
pip install pymysql # 安装Python的mysql接口；默认连接Python2版本的MySQL，需要改成pymysql
vim bot_service/__init__.py # 添加以下内容
# import pymysql
# pymysql.install_as_MySQLdb()

# 注释ready函数，自动创建数据库、表
vim bot_service/apps/knowledge/apps.py
# 否则报错：启动本地数据库，创建as_bot
#    报错：as_bot.knowledge...表不存在，哪里有建表语句？
python manage.py makemigrations knowledge # 建库
python manage.py migrate # 
python manager.py createsuperuser # 创建管理员账户，wqw，robot@123
# 启动后端服务
python manage.py runserver 127.0.0.1:8000
```


【2022-2-22】[Django+Vue前后端分离实战](https://www.cnblogs.com/zhangxue521/p/12957816.html)

```shell
# 克隆项目
git clone https://github.com/PanJiaChen/vue-admin-template.git
# 也可以多下载个完整的模版，用到组件时，copy过来
# git clone https://github.com/PanJiaChen/vue-element-admin.git

# 进入项目目录
cd vue-admin-template

# 安装依赖， 建议不要用 cnpm 安装 会有各种诡异的bug 可以通过如下操作解决 npm 下载速度慢的问题
npm install --registry=https://registry.npm.taobao.org

# 本地开发 启动项目
npm run dev
# ps：启动的时候报错了，提示提示vue-cli-service: command not found
# 进入到项目目录下，执行：rm –rf node_modules and npm install
```


#### Layui

[Layui](https://layuiweb.com/index.htm)，或[站点](https://layui.org.cn/demo/table.html)，[官方文档](https://layuiweb.com/doc/index.htm)

layui（谐音：类 UI) 是一套开源的 Web UI 解决方案，采用自身经典的模块化规范，并遵循原生 HTML/CSS/JS 的开发方式，极易上手，拿来即用。其风格简约轻盈，而组件优雅丰盈，从源代码到使用方法的每一处细节都经过精心雕琢，非常适合网页界面的快速开发。layui 区别于那些基于 MVVM 底层的前端框架，却并非逆道而行，而是信奉返璞归真之道。准确地说，它更多是面向后端开发者，你无需涉足前端各种工具，只需面对浏览器本身，让一切你所需要的元素与交互，从这里信手拈来。


#### Flask + Vue

【2021-11-25】[浅谈如何用使用flask和vue，快速开发常用应用](https://baiyue.one/archives/1694.html), flask是python的一种通用的web框架，诞生至今已有10年了，虽然官网界面比较复古极简，但使用者还是不在少数。纯后端的api开发，还可以看向fastapi，都是当今最主流的两个选择。
- ![](https://cdn.jsdelivr.net/gh/Baiyuetribe/yyycode@dev/img/20/yyycode_com20200924111604.png)

flask和vue结合的优势
- vue有着简洁、易懂的前端界面开发逻辑
- flask有着python独特的语义化代码，非常适合处理各种数据
- 两种语言都对小白非常友好，python更是当下最广泛的编程语言，用户量庞大
- 当前vue在vite的帮助下，可以快速定制开发环境，不论是html模板的pug语法，还是style的stylus语法，都是简化输入，突出逻辑框架，开发者的精力可以更集中在逻辑设计上。
- 容易上手，带来的自然高效率、易维护、易复制

数据库代码：

```sql
-- SQLite
CREATE TABLE books (
    id int,
    title varcha(255),
    author varcha(255),
    price int
)；
-- 插入数据
INSERT INTO books VALUES ('1','商业周刊','期刊','15');
INSERT INTO books VALUES ('2','新闻周刊','新闻','25');
INSERT INTO books VALUES ('3','有机化学','书籍','46');
INSERT INTO books VALUES ('4','读者文摘','读者','48');
```

flask入口文件app.py

```python
import sqlite3
from flask import Flask
from flask import jsonify,render_template
from flask_cors import CORS

app = Flask(__name__)
# CORS(app, supports_credentials=True)    #解决跨域问题
cors = CORS(app, resources={r"/api/*": {"origins": "*"}})   #两种模式都行
@app.route('/')
def home():
    return render_template('index.html',title='flask & vue')
@app.route('/api/books')
def books():
    conn = sqlite3.connect('books.db')
    conn.row_factory = sqlite3.Row
    cur = conn.cursor()
    sql = 'select * from books'
    rows = cur.execute(sql).fetchall()
    rows = [dict(row) for row in rows]
    return jsonify(rows)
if __name__ == "__main__":
    app.run(debug=True,port=3000)
```

前端文件：index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <!-- 引入vue -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <!-- 引入axios -->
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
</head>
<body>
    <div id="app">
    <h1>书籍信息展示</h1>
    <table border="1" >
        <tr>
            <td>#</td>
            <td>标题</td>
            <td>作者</td>
            <td>价格</td>
        </tr>
        <!-- 定义行 -->
        <tr v-for="book in books">
            <td>[[book.id]]</td>
            <td>[[book.title]]</td>
            <td>[[book.author]]</td>
            <td>[[book.price]]</td>
        </tr>
    </table>
    </div>
    <script>
        var app = new Vue({
            el: '#app',
            data: {
                books:[],
                msg: '说你好'
            },
            delimiters: ["[[","]]"],    //用于避免与flask冲突
            mounted: function(){
                this.fetchData()
            },
            methods: {
                fetchData(){    // axios异步请求数据
                    axios.get('/api/books').then(res =>{
                        this.books = res.data
                        console.log(this.books)
                    }).catch(err=>{
                        console.log(err)
                    })
                }
            }
        })
    </script>
</body>
</html>
```

最终效果
- 由于flask采用的是jinja模板，也就是采用\{\{\} }模式与web页面交互，因此会与vue的默认分隔符号相冲突，因此在script里需要修改delimiters: [ "[["," ]]"]。当然还有其他修改方式，这里推荐的也是相对简单的方法。
- ![](https://cdn.jsdelivr.net/gh/Baiyuetribe/yyycode@dev/img/20/yyycode_com20200924113511.png)

## 低代码平台

- 【2021-11-15】[基于 magic-api 搭建自己的低代码平台](https://www.toutiao.com/i7000242091813126670/)，2021 开年“低代码”成了热门话题，各大云厂商都在加码。

- 阿里推出了易搭，通过简单的拖拽、配置，即可完成业务应用的搭建
- 腾讯则是推出了微搭，通过行业化模板、拖放式组件和可视化配置快速构建多端应用（小程序、H5 应用、Web 应用等），打通了小程序、云函数。

### 低代码项目

低代码开源项目：百度 amis、h5-Dooring 和 magic-api。
- 百度 amis（前端）：百度 amis 是一套前端低代码框架，通过 JSON 配置就能生成各种后台页面，极大减少开发成本，甚至可以不需要了解前端。
  - ![](https://p9.toutiaoimg.com/origin/pgc-image/f75e701db958418d8a5ecb0a5af497ed?from=pc)
- [h5-Dooring](http://h5.dooring.cn)（前端）：h5-Dooring，让 H5 制作像搭积木一样简单, 轻松搭建 H5 页面, H5 网站, PC 端网站, 可视化设计。
  - ![](https://p9.toutiaoimg.com/origin/pgc-image/61b16a01a9bc46ae88391a5165c8b3e2?from=pc)
- magic-api（后端）：magic-api 是一个基于 Java 的接口快速开发框架，编写接口将通过 magic-api 提供的 UI 界面完成，自动映射为 HTTP 接口，无需定义 Controller、Service、Dao、Mapper、XML、VO 等 Java 对象即可完成常见的 HTTP API 接口开发。

【2022-3-9】GO语言实现的客户关系管理系统 YAO，开源低代码应用引擎：Yao，无需编写一行代码，即可快速创建 Web 服务和管理后台，大幅解放生产力。该工具内置了一套数据管理系统，通过编写 JSON，帮助开发者完成数据库模型、API 接口编写、管理后台界面搭建等工作，实现 90% 常见界面交互功能。内置管理系统与 Yao 并不耦合，开发者亦可采用 VUE、React 等任意前端技术实现管理界面。
- 项目示例 [IoT管理平哪台](https://github.com/YaoApp/yao)：An example of cloud + edge IoT application, an unattended intelligent warehouse management system that supports face recognition and RFID.
- ![](https://p3.toutiaoimg.com/img/tos-cn-i-qvj2lq49k0/cac9a04840b5412fb20f132603cef688~tplv-obj:1747:960.image?from=post)

【2022-3-16】低代码平台 [Variant Form](https://www.vform666.com/)，只用拖拖拽拽就生成vue页面代码，体验地址：[vform表单设计器](http://120.92.142.115/)


### python生成前端代码

【2021-12-8】[详解一个Python库，用于构建精美数据可视化web app](https://www.toutiao.com/i7039182714125353479/). Python 库 Streamlit，它可以为机器学习和数据分析构建 web app。它的优势是入门容易、纯 Python 编码、开发效率高、UI精美。
- ![](https://p6.toutiaoimg.com/origin/tos-cn-i-qvj2lq49k0/c26adac17c1d461cb4301318440c8045?from=pc)
- 对于交互式的数据可视化需求，完全可以考虑用 Streamlit 实现。特别是在学习、工作汇报的时候，用它的效果远好于 PPT。因为 Streamlit 提供了很多前端交互的组件，所以也可以用它来做一些简单的web 应用。

安装：

```shell
pip install streamlit # 安装
streamlit hello # 检查是否安装成功
```

主要功能：
- 文本组件：文本组件是用来在网页上展示各种类型的文本内容
- 数据组件：
  - dataframe 和 table 组件可以展示表格。前者可动态展示。数据类型包括 pandas.DataFrame、pandas.Styler、pyarrow.Table、numpy.ndarray、Iterable、dict。
  - json组件：显示json格式，展示地更美观，并且提供交互，可以展开、收起 json 的子节点。
  - metric 组件用来展示指标的变化，数据分析中经常会用到。
    - st.metric(label="Temperature", value="70 °F", delta="1.2 °F")
- 图标组件：包含两部分，一部分是原生组件，另一部分是渲染第三方库。
  - 原生组件只包含 4 个图表，line_chart、area_chart 、bar_chart 和 map，分别展示折线图、面积图、柱状图和地图。
  - 第三方库：matplotlib.pyplot、Altair、vega-lite、Plotly、Bokeh、PyDeck、Graphviz。
- 输入组件
  - Streamlit 提供的输入组件都是基本的，都是我们在网站、移动APP上经常看到的。包括：
    - button：按钮
    - download_button：文件下载
    - file_uploader：文件上传
    - checkbox：复选框
    - radio：单选框
    - selectbox：下拉单选框
    - multiselect：下拉多选框
    - slider：滑动条
    - select_slider：选择条
    - text_input：文本输入框
    - text_area：文本展示框
    - number_input：数字输入框，支持加减按钮
    - date_input：日期选择框
    - time_input：时间选择框
    - color_picker：颜色选择器
  - 它们包含一些公共的参数：
    - label：组件上展示的内容（如：按钮名称）
    - key：当前页面唯一标识一个组件
    - help：鼠标放在组件上展示说明信息
    - on_click / on_change：组件发生交互（如：输入、点击）后的回调函数
    - args：回调函数的参数
    - kwargs：回调函数的参数
  - ![](https://p6.toutiaoimg.com/origin/tos-cn-i-qvj2lq49k0/50d4f1b2b7e745cf9cf44d3d02e1938f.png?from=pc)
- 多媒体组件
  - Streamlit 定义了 image、audio 和 video 用于展示图片、音频和视频。可以展示本地多媒体，也通过 url 展示网络多媒体。
  - ![](https://p6.toutiaoimg.com/origin/tos-cn-i-qvj2lq49k0/d16ae8c021a54c3b952a7fe8b16b8a74.png?from=pc)
- 状态组件
  - 状态组件用来向用户展示当前程序的运行状态，包括：
    - progress：进度条，如游戏加载进度
    - spinner：等待提示
    - balloons：页面底部飘气球，表示祝贺
    - error：显示错误信息
    - warning：显示报警信息
    - info：显示常规信息
    - success：显示成功信息
    - exception：显示异常信息（代码错误栈）
  - ![](https://p6.toutiaoimg.com/origin/tos-cn-i-qvj2lq49k0/9fcc5e3ec3b7418ca48f339bc1001822.png?from=pc)

Streamlit 可以展示纯文本、Markdown、标题、代码和LaTeX公式。

my_code.py

```python
import streamlit as st

# markdown
st.markdown('Streamlit is **_really_ cool**.')
# 设置网页标题
st.title('This is a title')
# 展示一级标题
st.header('This is a header')
# 展示二级标题
st.subheader('This is a subheader')
# metric
st.metric(label="Temperature", value="70 °F", delta="1.2 °F")
# json组件
st.json({
'foo': 'bar',
'stuff': [
'stuff 1',
'stuff 2',
],
})

# 展示代码，有高亮效果
code = '''def hello():
print("Hello, Streamlit!")'''
st.code(code, language='python')
# 纯文本
st.text('This is some text.')
# LaTeX 公式
st.latex(r'''
a + ar + a r^2 + a r^3 + \cdots + a r^{n-1} =
\sum_{k=0}^{n-1} ar^k =
a \left(\frac{1-r^{n}}{1-r}\right)
''')

# 图表
chart_data = pd.DataFrame(np.random.randn(20, 3), columns=['a', 'b', 'c'])
st.line_chart(chart_data)
# 第三方图表
import matplotlib.pyplot as plt
arr = np.random.normal(1, 1, size=100)
fig, ax = plt.subplots()
ax.hist(arr, bins=20)
st.pyplot(fig)

# 交互输入框：下拉框
option = st.selectbox('下拉框', ('选项一', '选项二', '选项三'))
# 输出组件，可以输出字符串、DataFrame、普通对象等各种类型数据。
st.write('选择了：', option)
```

执行：streamlit run my_code.py ，streamlit 会启动 web 服务，加载指定的源文件。浏览器访问 http://localhost:8501/ 即可。

当源代码被修改，无需重启服务，在页面上点击刷新按钮就可加载最新的代码，运行和调试都非常方便。


- **页面布局**。之前我们写的 Streamlit 都是按照代码执行顺序从上至下展示组件，Streamlit 提供了 5 种布局：
  - sidebar：侧边栏，如：文章开头那张图，页面左侧模型参数选择
  - columns：列容器，处在同一个 columns 内组件，按照从左至右顺序展示
  - expander：隐藏信息，点击后可展开展示详细内容，如：展示更多
  - container：包含多组件的容器
  - empty：包含单组件的容器
- **控制流**。控制 Streamlit 应用的执行，包括
  - stop：可以让 Streamlit 应用停止而不向下执行，如：验证码通过后，再向下运行展示后续内容。
  - form：表单，Streamlit 在某个组件有交互后就会重新执行页面程序，而有时候需要等一组组件都完成交互后再刷新（如：登录填用户名和密码），这时候就需要将这些组件添加到 form 中
  - form_submit_button：在 form 中使用，提交表单。
- **缓存**。这个比较关键，尤其是做机器学习的同学。刚刚说了， Streamlit 组件交互后页面代码会重新执行，如果程序中包含一些复杂的数据处理逻辑（如：读取外部数据、训练模型），就会导致每次交互都要重复执行相同数据处理逻辑，进而导致页面加载时间过长，影响体验。
  - 加入缓存便可以将第一次处理的结果存到内存，当程序重新执行会从内存读，而不需要重新处理。
  - 使用方法也简单，在需要缓存的函数加上 @st.cache 装饰器即可。前两天我们讲过 Python 装饰器。

```python
DATE_COLUMN = 'date/time'
DATA_URL = ('https://s3-us-west-2.amazonaws.com/'
'streamlit-demo-data/uber-raw-data-sep14.csv.gz')
@st.cache
def load_data(nrows):
data = pd.read_csv(DATA_URL, nrows=nrows)
lowercase = lambda x: str(x).lower()
data.rename(lowercase, axis='columns', inplace=True)
data[DATE_COLUMN] = pd.to_datetime(data[DATE_COLUMN])
return data
```


# 结束
















