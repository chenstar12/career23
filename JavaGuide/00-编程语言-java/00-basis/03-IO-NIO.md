**I**/O（**I**nput/**O**utpu） 即**输入／输出** ；

### 键盘输入

方法 1： `Scanner`

```java
Scanner input = new Scanner(System.in);
String s  = input.nextLine();
input.close();
```

方法 2： `BufferedReader`

```java
BufferedReader input = new BufferedReader(new InputStreamReader(System.in));
String s = input.readLine();
```

### Java 中 IO 流分类方式?

- 流向：输入流、输出流；
- 操作单元：字节流、字符流；
- 流的角色：节点流、处理流；

Java IO 流都是从如下 4 个抽象类基类中派生：

- InputStream（字节输入流）、Reader（字符输入流）: 所有输入流的基类；
- OutputStream（字节输出流）、Writer（字符输出流）: 所有输出流的基类；



**为什么 I/O 流操作要分：字节流、字符流？**

- 字符流是由字节转换得到，过程非常耗时；
- 如果不知道编码类型，容易出现乱码问题；java提供直接操作字符的接口（音频、图片等媒体文件用字节流）



### 有哪些常见的 IO 模型?

UNIX 系统的 IO 模型有 5 种： **同步阻塞 I/O**、**同步非阻塞 I/O**、**I/O 多路复用**、**信号驱动 I/O** 和**异步 I/O**；

### Java 的 3 种 IO 模型

- **BIO (Blocking I/O)：**同步阻塞 IO 模型；应用程序调用 read() 后，会一直阻塞（直到内核把数据拷贝到用户空间）；

  <img src="C:\Users\xin\AppData\Roaming\Typora\typora-user-images\image-20221112184317289.png" alt="image-20221112184317289" style="zoom:50%;" />

- **NIO (Non-blocking I/O)**：`java.nio` 包，提供 `Channel` , `Selector`，`Buffer`；支持面向缓冲、基于通道的 I/O ； 适合高负载、高并发的（网络）应用；

Java 的 NIO 可看作 **I/O 多路复用模型**。也有人认为 NIO 属于**同步非阻塞 IO 模型**；

先看 **同步非阻塞 IO 模型**：

<img src="C:\Users\xin\AppData\Roaming\Typora\typora-user-images\image-20221112184700476.png" alt="image-20221112184700476" style="zoom:50%;" />

应用程序可一直调用 read() ，通过轮询操作，避免阻塞；但**数据从内核空间拷贝到用户空间时，线程是阻塞的**;

缺点：**应用程序不断进行 I/O 系统调用轮询**数据是否已经准备好的是十分消耗 CPU 的；

改进 ---- **I/O 多路复用模型**：

<img src="C:\Users\xin\AppData\Roaming\Typora\typora-user-images\image-20221112185030020.png" alt="image-20221112185030020" style="zoom:50%;" />

线程首先发起 select 调用，询问内核是否数据已准备就绪；等内核把数据准备好，用户线程再发起 read 调用；

**read() 过程（数据从内核空间 -> 用户空间）还是阻塞的**；

IO 多路复用模型**减少了无效的系统调用，减少了对 CPU 资源的消耗**；

注：NIO 的**选择器 ( Selector )** ，即 **多路复用器**。通过 **Selector 只需一个线程管理多个客户端**连接；

<img src="C:\Users\xin\AppData\Roaming\Typora\typora-user-images\image-20221112190355637.png" alt="image-20221112190355637" style="zoom: 80%;" />

- **AIO (Asynchronous I/O)**：即 **NIO 2**；**异步 IO** 模型；

异步 IO 基于事件和回调机制实现，即应用操作后会直接返回（不会堵塞），当后台处理完成，OS会通知线程；

<img src="C:\Users\xin\AppData\Roaming\Typora\typora-user-images\image-20221112185753217.png" alt="image-20221112185753217" style="zoom:50%;" />

AIO 应用较少，性能并没有多少提升。

总结 Java 的 BIO、NIO、AIO：

<img src="https://images.xiaozhuanlan.com/photo/2020/33b193457c928ae02217480f994814b6.png" style="zoom: 50%;" />

## 参考

- 如何完成一次 IO：https://llc687.top/126.html
- 程序员应该这样理解 IO：[https://www.jianshu.com/p/fa7bdc4f3de7](https://www.jianshu.com/p/fa7bdc4f3de7)
- 10 分钟看懂， Java NIO 底层原理：https://www.cnblogs.com/crazymakercircle/p/10225159.html
- IO 模型知多少 | 理论篇：https://www.cnblogs.com/sheng-jie/p/how-much-you-know-about-io-models.html

