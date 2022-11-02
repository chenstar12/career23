---
layout: post
title:  "计算机图形学 - Computer Graphic"
date:   2022-09-28 09:03:00
categories: 计算机基础 计算机视觉
tags: 图形学 opengl webgl three.js 游戏
excerpt: 计算机图形学及相关应用
mathjax: true
permalink: /graphic
---

* content
{:toc}


# 总结

- 图形 + 数学 + 体系结构 + 操作系统 + 软件工程 + 设计模式 + 编译原理 + (C++功力) ≈ 更好的3D 软件工程师

## WebGL，OpenGL和OpenGL ES

OpenGL作为一个规范/接口告诉驱动如何和 GPU 通信，发展至今嵌入式设备崛起，OpenGL ES 也应运而生，WebGL 是基于 OpenGL ES 可以让其在浏览器上通过 Javascript 调用的规范/接口，但 WebGL 门槛不低，要和GPU通信，就需要了解计算机图形学知识，那肯定也需要用到着色器，所以 Three.js 封装好成为三维引擎，也不用知道那么多底层知识，就可以创建 Web 3D。

OpenGL ES可以说是OpenGL为了满足嵌入式设备需求而开发一个特殊版本，是其一个子集；而WebGL，是为了网页渲染效果，将JavaScript和OpenGL ES 2.0结合在一起，通过增加OpenGL ES 2.0的一个JavaScript绑定得到。基本关系如[图](https://img-blog.csdnimg.cn/20200828172505903.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIzMDM0NTE1,size_16,color_FFFFFF,t_70)
- 实际应用过程中，前端的WebGL是通过JS语句，操作本地的OpenGL的部分接口，对于移动设备这是操作OpenGL ES的接口，来实现页面的图形的渲染，WebGL只是绑定外面接口的一层，内部的一些核心内容，如着色器，材质，灯光等都是需要借助GLSL ES语法来操作的。

[WebGL，OpenGL和OpenGL ES三者的关系](https://blog.csdn.net/qq_23034515/article/details/108283747)
- ![](https://p3-sign.toutiaoimg.com/tos-cn-i-tjoges91tu/TIjrlHx4QVO7yC~noop.image)

# 计算机图形学


## 如何让3D空间点坐标转为屏幕2D二维点坐标？

浅看 3D空间点 to 屏幕 2D二维点
- 当对每个物体🦖 建模的时候，物体本身是有自己的独立坐标系`局部坐标系` Local Space，但 🦖 🌸🌸🌱🌿 放到场景中就有不同放置位置，所有物体共享同一个坐标系，叫`世界坐标系` World Space
- 在`世界坐标系`场景下，我们是从正面某个位置去观察 🦖，如果视角变化至沿着Z轴负方向看呢？又是另外一个画面，叫做`视觉坐标系` View Space。
- ![](https://p3-sign.toutiaoimg.com/tos-cn-i-tjoges91tu/TIjrn35GD6Sakr~noop.image?_iz=58558&from=article.pc_detail&x-expires=1665021396&x-signature=ccdph1LNEKoVPdzXxsuUoZa1KDY%3D)

还有两个坐标系说明：
- `裁剪坐标系` Clip Space /DNC: 归一化处理，和 需要判断哪个片段需要展示在屏幕内。(这里不做展开跟机器学习归一化目标一致，统计学中数据收敛作用)
- `屏幕坐标系` Screen Space：根据裁剪坐标系计算，再转换为屏幕坐标。最后将数据传到光栅器。

通俗理解
- 我们每个人都是一个独立个体都以"自我为中心" (`局部`坐标系)
- 随着我们长大要进入社会 (`世界`坐标系)，学习、工作 ....
- 在这个过程中，有人仰视你，有人俯视你，你可以选择平视他们 或者 后脑勺对着他们，总之其他人会站在自己的角度看问题 或者 看你 (`视觉`坐标系) ....
- AnyWay，过程中你可能会生活磨平棱角 (`裁剪`坐标系)，变成了你小时候最讨厌的人，
- But，你就是你，是不一样的烟火，.... 让蔷薇开出一种结果 (`屏幕`坐标系)

## 图形学与线性代数

坐标系转换过程离不开线性代数基础知识
- （1）`局部`空间->`世界`空间，涉及矩阵的平移、缩放、旋转
  - 缩放：代表多少倍，缩放S1、S2、S3 倍数
    - ![](https://p3-sign.toutiaoimg.com/tos-cn-i-tjoges91tu/TIjrn3XA5q9ZqO~noop.image?_iz=58558&from=article.pc_detail&x-expires=1665021396&x-signature=qQsLXYyqsl2pyBWLN7U0oDzol34%3D)
  - 平移：
    - ![](https://p3-sign.toutiaoimg.com/tos-cn-i-tjoges91tu/TIjrn428e5mWnm~noop.image?_iz=58558&from=article.pc_detail&x-expires=1665021396&x-signature=skMIw87L75j58a2uY4iFlzPenNo%3D)
  - 旋转：由 P(x, y, z) 变为 P' (x', y', z')，旋转 θ 角度，略过 ... 变换 - LearnOpenGL CN
- （2）`世界`空间 -> `视觉`空间，构建线性变换矩阵
  - 任何方位观察到的物体都是不同的，从A 位置 变换至 B 位置，只要知道 变换前后的基向量，就能知道 运动至哪里，方法通过矩阵相乘(对几何空间的线性变化) ....
  - 矩阵向量乘积: 变换后的基向量* 未变化前的位置 (x, y) = 基向量变换后新(x', y')
- （3）`视觉`空间 -> `裁剪`空间 -> `屏幕`空间
  - 将 3D 点 表示到 2D 点， 投影->点积(实际上会更复杂些) ....
  - 再将能视觉展示的空间展示，不能展示的被剪裁掉
  - 剪裁后点位，会归一化处理，保证交付给发光二极管。

## 图形渲染管线

初中数学计算立方体面积，老师在 2D 黑板上画了XYZ 轴，并骗你这是个 3D立方体(视觉上)。
- ![](https://p3-sign.toutiaoimg.com/tos-cn-i-tjoges91tu/TIjrmCXAHtzkk1~noop.image)

任何用3D空间中表示的事物，在 🖥 中都是2D像素数组，而 WebGL/OpenGL 大部分工作也是把3D坐标转换为2D像素。这个过程叫做 图`形渲染管线` RenderPipeline。

结合上面那些个GPU 模型里的破管子，有一堆原始图形数据 经过 一个破管子后，最终输出至屏幕中的过程就是 `图形渲染管线`。

图形渲染管线 Render Pipeline 被划分为几个阶段，跟咱们的ByteCycle 流水线一样，每个阶段会把上个阶段的输出作为输入，也可以理解是函数式编程 pipeline 模式。也就是说，每个阶段都有专门的函数 / 小程序去处理，函数 / 小程序 ≈ 着色器 (Shader)。
- ![](https://p3-sign.toutiaoimg.com/tos-cn-i-tjoges91tu/TIjrmD8anxR5T~noop.image)

CPU 和 GPU 是通力合作的关系来渲染图像
- ![](https://p3-sign.toutiaoimg.com/tos-cn-i-tjoges91tu/TIjrmEUEPcy8Vh~noop.image?_iz=58558&from=article.pc_detail&x-expires=1665021396&x-signature=ZNfLxj6lWpJMg0cjTPnQGDJ6WZM%3D)

渲染管线抽象流程
- 3D坐标 转为 另一种3D坐标(后面会解释 从局部到世界坐标系)，并对顶点属性进行处理。
- 计算一个像素的最终颜色。例如一个立方体在灯光照射下会有阴影，这里也会将其处理。

顶点着色器
- 图元的装配：将 顶点着色器 输出的点作为输入，并绘制成图元形状。
- 几何着色器：将 图元形状 构造成新的图元 或 其他形状。
- 光栅化过程：把 图元 映射为 最终屏幕上相应的 像素。
- 片段着色器
- 测试与混合：例如 有3D遮挡场景 或 物体是透明，在这个过程中就需要判断是否在该帧被丢弃。
- ![](https://p3-sign.toutiaoimg.com/tos-cn-i-tjoges91tu/TIjrmF6JDTEgkK~noop.image?_iz=58558&from=article.pc_detail&x-expires=1665021396&x-signature=dzyTooVljPV7DrOLIcg4sl7o%2B7E%3D)

GLSL 语言，类似C语言，以下是顶点着色器的例子

```shell
 #version 300 es #声明了着色器版本号 300 代表是 3.0 之后版本
in vec4 aPos ;
# in = 输入变量 浮点型向量vec4 变量名称 aPos
# eg: aPos = {1.0, 1.0, 1.0, 1.0}
voidmain
{
  gl_Position = aPos ;
  # 顶点着色器的内置输出变量
}
```

# OpenGL


## OpenGl简介

OpenGL(Open Graphics Library)
>- OpenGL is a cross-language, cross-platform application programming interface (API) for rendering 2D and 3D vector graphics. The API is typically used to interact with a graphics processing unit (GPU), to achieve hardware-accelerated rendering.
>- OpenGL 是API 、是规范。GPU 硬件厂商需要满足统一OpenGL规范。而 OpenGL ES (Open Graphics Library for Embedded Systems) 是 OpenGL 子集，专门针对手机等嵌入式设备而设计的。

人话版：发动机(GPU) 驱动是方向盘、离合器等等，OpenGL 定义操纵 发动机(GPU) 发动规范，OpenGL 不关心方向盘是圆的还是方的，驱动 发动机(GPU) 干活啦。

用方向盘 (驱动器) -> 依照调用标准使用 (OpenGL) -> 指挥发动机 (GPU) 干活

# GLSL

GLSL(OpenGL Shading Language)

后面讲到图形渲染管线会说到，这里浅浅先通过例子感受下 立方体 demo。

代码阉割版：有类C语言的着色器语言、有我们熟悉JS语言、有矩阵相乘 ...


# WebGL

【2022-9-29】[Web 3D 从入门到跑路](https://www.toutiao.com/article/7148436169880142369)

## WebGL简介

WebGL(Web Graphics Library)
>- WebGL is a cross-platform, royalty-free open web standard for a low-level 3D graphics API based on OpenGL ES, exposed to ECMAScript via the HTML5 Canvas element.
>- WebGL 是在 OpenGL ES 基础上建立的在 浏览器跑起来的图形学标准，同理是浏览器厂商规范 ≈ 让JS 操作接口。光有规范是不够的，还要程序告诉 GPU 如何进行渲染。

WebGL，是一项用来在网页上绘制和渲染复杂三维图形（3D图形），并允许用户与之交互的技术。随着个人计算机和浏览器的性能越来越强，我们能够在Web上创建越来越精美、越来越复杂的3D图形。

从传统意义上来说，为了显示三维图形，开发者需要使用C或C++语言，辅以专门的计算机图形库，如OpenGL或Direct3D，来开发一个独立的应用程序。现在有了WebGL，我们只需要向已经熟悉的HTML和JS中添加一些额外的三维图形学的代码，就可以在网页上显示三维图形了。

WebGL是内嵌在浏览器中的，不必安装插件和库就可以直接使用它。

## 3D建模

3D 建模概念必备
- 如果你是个大导演，有一天你想请 安琪拉大宝贝儿 来北京 献歌一曲，
- 要有地点 Scene 场景，选择 人民大会堂作为 舞台吧；
- 要有灯光 Light 灯光, 才能让观众看到 安琪拉大宝贝儿 唱歌；
- 关于 安琪拉大宝贝 作为 模型，来之前要保养一下，皮肤看起来吹弹可破材质 Material；
- XXX 大品牌疯狂赞助，并要求她穿上新一季 服饰 和 配上妆发 贴图与纹理 Texture;
- 一切准备就绪后，N个机器 Camera 相机360 度无死角的拍摄，她唱 XXX歌曲。

脑海里构建出来的画面 ≈ 渲染器 Render
- ![](https://p3-sign.toutiaoimg.com/tos-cn-i-tjoges91tu/TIjro986FYHQIz~noop.image?_iz=58558&from=article.pc_detail&x-expires=1665021396&x-signature=DqOTlUFAjtE%2B02pUnmIXHCl7jGY%3D)

Three.js demo 自己去对照着想象这个👻，就是 安琪拉大宝贝儿 吧

以上几个关键词是：Scene 场景、Light 灯光、模型、材质 Material、Texture、Camera 相机、渲染器 Render。

理解了以上的几个概念后，对我们实践上手应用 Three.js 非常的关键。

### 3D建模工具: three.js 与 Unity

作者：[ThingJS](https://www.zhihu.com/question/273431132/answer/1531831742)

- Three.js是大多数开发者首次接触的WebGL 3D库
- Unity为游戏开发、美术、建筑、汽车设计、影视制作在内的创作者提供一整套软件解决方案。
- ThingJS是更为上层的抽象，不用关心渲染、mesh、光线等复杂概念

总结
- unity3d打包出来的web项目占用系统资源比较大(非常大)，但是开发复杂交互的项目比threejs快很多倍
- threejs只适合于做展示的项目，开发速度相对慢一些，因为这个库的封装层面相对unity3d更底层一些。

three.js优势
- Three.js是大多数开发者首次接触的WebGL 3D库，Threejs库的出现解决了底层的渲染细节和复杂的数据结构，可以支持如一个房间级别,或一个楼层级别的渲染,或符合特殊要求的大量同类模型的渲染。

three.js和Unity开发性能比较
- three.js对WebGL进行了封装,提供了更高层的渲染接口,提供摄影机、视口的控制,提供场景组织方式,能够加载多种文件格式,通过创建材质、贴图并编写shader来实现物体效果,创建立方体、球等基本元素,提供灯光、阴影、点云等等底层功能。
- 相比而言，Unity 3D需要下载和安装**Unity编辑器**以便创建3D项目，C#语言面向对象的特性完整，有利于程序设计。 
- Unity编辑器提供了丰富的游戏开发功能，包括场景的渲染、物体创建、各种效果在编辑中调整，应用商店提供了丰富的扩展能力，更适合影视动画开发生态。这一点和Unity的开发优势趋同，基于Unity强大的编辑器,提供了丰富的游戏开发功能。包括场景的渲染、物体创建、各种效果在编辑中调整、物理系统、地形系统、植物系统、粒子系统、过场动画编辑、人物角色动画编辑等等。并且通过应用商店提供了丰富的扩展能力

three.js不足之处
- 虽然Three.js底层引擎级别的三维图形库，有很多开源库对它进行扩展，但较为松散，适合做轻量级可视化应用，复杂应用则需要基于此库进行大量封装才行。尤其场景输出层面，需要3 3DSMax、Maya、CAD等专业美术人员,通过建模再做一定的导出工作才能得到需要的模型，团队协作成本高。

问题加餐：three.js，thingjs 3D框架对比
- ThingJS是新兴的3D框架，2018年诞生，由北京优锘科技公司研发，旨在简化3D应用开发。
- 轻量化的表现在：
  - 1、ThingJS封装了对模型交互事件的API、对模型的操作及层次关系，一个个具体的模型抽象把初学者从复杂的3D概念中解放出来，
  - 2、加载简单场景仅需1行代码，发布方式支持iframe, 微信和PC发布，在线化更方便
  - 3、ThingJS是一个完成的物联网可视化应用开发生态，提供CampusBuilder, CityBuilder等场景搭建SAAS、场景存储云空间（无需付费）和10万个场景资源……
- 如果是你是初学者，three.js用起来更花费时间，就一个加载模型、调光、选择模型弹框的功能，就能写出Threejs上百行代码，ThingJS是更为上层的抽象，不用关心渲染、mesh、光线等复杂概念，更适合项目团队提高开发效率。


## Three.js

### Three.js简介

WebGL学习有门槛，[Three.js](https://threejs.org/) 作为方向入门，就像 jQuery 一样的存在，可以快速的创建出三维场景。
- ![](https://p3-sign.toutiaoimg.com/tos-cn-i-tjoges91tu/TIjrlIT9pDUcGs~noop.image)

基于浏览器，通过Javascript 来实现编程技术，能在 2D屏幕 上看到 3D效果。
- 基础能力：数学、物理 ....
- 能力支持：基于 GPU 图形渲染管线架构设计，在 Web端 通过 WebGL (-> OpenGL ES -> OpenGL) 和 着色器 (着色器GLSL 语言实现)，实现驱动能力。(任何语言实现都是以硬件为基础)

### Three.js 效果

[官方示例](https://threejs.org/examples/#webgl_animation_keyframes)

<iframe src="https://threejs.org/examples/#webgl_animation_keyframes" width='100%' height='600' frameborder='0' scrolling='no' allowfullscreen="true"></iframe>

自定义开发Demo

<iframe src="{{ site.url }}/wqw/demo/three.js.html" frameborder='0' scrolling='no' allowfullscreen="true"></iframe>

- [鞋子](https://threejs.org/examples/webgl_loader_gltf_variants.html)
  - <iframe src="https://threejs.org/examples/webgl_loader_gltf_variants.html" width='100%' height='600' frameborder='0' scrolling='no' allowfullscreen="true"></iframe>
- 室内场景：[果盘](https://threejs.org/examples/webgl_loader_gltf_transmission.html)
  - <iframe src="https://threejs.org/examples/webgl_loader_gltf_transmission.html" width='100%' height='600' frameborder='0' scrolling='no' allowfullscreen="true"></iframe>
- [初音未来](https://threejs.org/examples/webgl_loader_mmd.html)
  - <iframe src="https://threejs.org/examples/webgl_loader_mmd.html" width='100%' height='600' frameborder='0' scrolling='no' allowfullscreen="true"></iframe>
- [人物模型](https://threejs.org/examples/webgl_loader_obj_mtl.html)
  - <iframe src="https://threejs.org/examples/webgl_loader_obj_mtl.html" width='100%' height='600' frameborder='0' scrolling='no' allowfullscreen="true"></iframe>
- [沙滩豪车](https://threejs.org/examples/webgl_materials_envmaps_groundprojected.html)
  - <iframe src="https://threejs.org/examples/webgl_materials_envmaps_groundprojected.html" width='100%' height='600' frameborder='0' scrolling='no' allowfullscreen="true"></iframe>

## 游戏

### Three.js

3D游戏：[First Three.js Game](https://discourse.threejs.org/t/smelly-cat-first-three-js-game/12894), [smelly cat](https://defnetuncer98.github.io/smellycat/#)

### Godot

【2022-10-13】[Godot 引擎](https://godotengine.org/)是一款免费、由开源社区驱动的 2D 和 3D 游戏引擎
- [中文文档](https://docs.godotengine.org/zh_CN/latest/getting_started/first_2d_game/index.html)

[程序员用12小时复刻《羊了个羊》，代码已开源](https://www.toutiao.com/article/7153542634227876383)
- [Godot版羊了个羊](https://gitcode.net/hello_tute/SheepASheep)


### 微信跳一跳

2018年风靡一时的小游戏：跳一跳，背后原理分析，[微信跳一跳解题报告](https://charlesliuyx.github.io/2018/01/19/%E5%BE%AE%E4%BF%A1%E8%B7%B3%E4%B8%80%E8%B7%B3%E8%A7%A3%E9%A2%98%E6%8A%A5%E5%91%8A/#%E6%B8%B8%E6%88%8F%E6%9C%BA%E5%88%B6)
1. 问题描述
2. 游戏机制
3. 解决方案



# 结束


