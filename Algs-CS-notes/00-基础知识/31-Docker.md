
## 解决的问题

不同的机器有不同的操作系统、不同的库和组件，将应用部署到多台机器上要进行大量的环境配置操作；

Docker 主要解决环境配置问题，是一种虚拟化技术，对进程隔离，使进程独立于宿主OS和其它进程；无需修改应用程序代码；

## 与虚拟机比较

虚拟机也是虚拟化技术，与 Docker 的区别：模拟硬件，在硬件上安装OS；

<div align="center"> <img src="https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/be608a77-7b7f-4f8e-87cc-f2237270bf69.png" width="500"/> </div><br>

### 启动速度

- 虚拟机：先启动虚拟机的OS，再启动应用；

- Docker：相当于启动宿主OS的一个进程；

### 占用资源

- 虚拟机：是一个完整OS，占用大量的磁盘、内存和 CPU 资源；

- Docker：只是一个进程，只需将应用以及组件打包，运行时占用资源少；

### Docker 的其他优势

- 易迁移：提供一致性的运行环境；（已打包的应用）

- 易扩展：用基础镜像扩展得到新的镜像，且开源社区提供了大量镜像，容易扩展得到想要的镜像；

## 使用场景

- 持续集成：频繁地将代码集成到主干，能够更快地发现错误。Docker 的轻量级以及隔离性，使代码集成不影响其它 Docker；
- 可伸缩的云服务：根据应用的负载情况，容易调整 Docker数量；
- 微服务架构：Docker 的轻量级使其适合用于部署、维护、组合微服务；

## 镜像与容器

- 镜像：静态的结构，类比面向对象的类（*Java*）；

- 容器：镜像的一个实例；

**镜像**：包含容器运行所需的代码及组件，是一种分层结构，每一层都是只读的（read-only layers）；构建镜像是逐层构建，前一层是后一层的基础；这种分层结构适合复用、定制；

**容器**：在镜像的基础上添加**可写层（writable layer）**，保存着容器运行过程中的修改；

<img src="C:\Users\xin\AppData\Roaming\Typora\typora-user-images\image-20221106103129975.png" alt="image-20221106103129975" style="zoom: 67%;" />

## 参考资料

- [DOCKER 101: INTRODUCTION TO DOCKER WEBINAR RECAP](https://blog.docker.com/2017/08/docker-101-introduction-docker-webinar-recap/)
- [Docker 入门教程](http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)
- [Docker container vs Virtual machine](http://www.bogotobogo.com/DevOps/Docker/Docker_Container_vs_Virtual_Machine.php)
- [How to Create Docker Container using Dockerfile](https://linoxide.com/linux-how-to/dockerfile-create-docker-container/)
- [理解 Docker（2）：Docker 镜像](http://www.cnblogs.com/sammyliu/p/5877964.html)
- [为什么要使用 Docker？](https://yeasy.gitbooks.io/docker_practice/introduction/why.html)
- [What is Docker](https://www.docker.com/what-docker)
- [持续集成是什么？](http://www.ruanyifeng.com/blog/2015/09/continuous-integration.html)

