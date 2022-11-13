#### String、StringBuffer、StringBuilder 的区别？

- `String` ：不可变类型；

- `StringBuilder` 与 `StringBuffer` ：继承 `AbstractStringBuilder` 类，用字符数组保存字符串（没有 `final` 和 `private` 修饰），提供修改字符串的方法（如 `append` 方法）；

```java
abstract class AbstractStringBuilder implements Appendable, CharSequence {
    char[] value; // 字符数组: 保存字符串
    public AbstractStringBuilder append(String str) {
        if (str == null)
            return appendNull();
        int len = str.length();
        ensureCapacityInternal(count + len);
        str.getChars(0, len, value, count);
        count += len;
        return this;
```

**线程安全性**

- `String` 对象不可变，可理解为常量，线程安全；

`AbstractStringBuilder` 是 `StringBuilder` 与 `StringBuffer` 的公共父类，定义了一些字符串的基本操作，如 `expandCapacity`、`append`、`insert`、`indexOf` 等方法；

- `StringBuffer` 线程安全：对方法加了同步锁，或对调用的方法加了同步锁；
- `StringBuilder` 非线程安全：没有对方法加同步锁；

**性能**

- 修改 `String` 类型：会生成新的 `String` 对象，指针指向新对象；
- `StringBuffer` ：在对象本身操作（而不是生成新对象并改变对象引用）；
- `StringBuilder` 比 `StringBuffer` 仅有 10%~15% 的性能提升，但有多线程不安全的风险；

**总结：**

1. 操作少量数据: 用 `String`
2. 单线程、操作大量数据: 用 `StringBuilder`
3. 多线程、操作大量数据: 用 `StringBuffer`



#### String 为什么不可变?

`String` 类中： `final` 修饰了字符数组（保存字符串）；

```java
public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
    private final char value[];
```

> 🐛 修正 ：  `final` 修饰的类不能继承，修饰的方法不能重写，修饰的变量是基本数据类型则值不能改变，修饰的变量是引用类型则不能再指向其他对象。因此，`final` 修饰字符数组（引用类型）并不是 `String` 不可变的根本原因；
>
> `String` 不可变的根本原因：
>
> 1. 字符数组 `final` 且 `private`，并且`String` 类没有提供修改字符数组的方法；
> 2. `String` 类被 `final` 修饰（不能继承）：避免子类破坏 `String` 的不可变；
>
> 补充：在 Java 9 之后，`String` 、`StringBuilder` 与 `StringBuffer` 改用 `byte` 数组存储字符串：
>
> ```java
>public final class String implements java.io.Serializable,Comparable<String>, CharSequence {
>     @Stable     // @Stable 注解：变量最多被修改一次，“稳定的”；
>     private final byte[] value;
> 
> abstract class AbstractStringBuilder implements Appendable, CharSequence {
>     byte[] value;
> ```
> 
> **Java 9 为何将 `char[]` 改成 `byte[]` ?**
> 
> `byte` 占一个字节(8 位)，`char` 占用 2 个字节（16），节省一半的内存空间；
> 
> - String 支持两中编码方案： UTF-16和Latin-1；如果字符串仅包含 Latin-1 表示范围，就用 Latin-1 编码方案（绝大部分字符串对象只包含 Latin-1 可表示的字符）
>
> ![](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/github/javaguide/jdk9-string-latin1.png)
>
> - 如果字符串含有超过 Latin-1 表示范围，`byte` 和 `char` 所占空间一样；
>
> 官方介绍：https://openjdk.java.net/jeps/254

#### 字符串拼接：“+” 还是 StringBuilder?

Java 不支持运算符重载，但“+”和“+=”是专门为 String 类重载的运算符；

```java
String str1 = "he";
String str2 = "llo";
String str3 = "world";
String str4 = str1 + str2 + str3;
```

对应的字节码：

![](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/github/javaguide/java/image-20220422161637929.png)

- 字符串“+”拼接：实际上是 `StringBuilder` 调用 `append()` 方法实现，拼接完用 `toString()` 得到 `String` 对象；

- 注：在循环内用“+”拼接字符串，存在明显缺陷：**编译器不会复用 `StringBuilder` ，导致过多的创建 `StringBuilder` 对象**；每循环一次就创建一个 `StringBuilder` 对象；

![](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/github/javaguide/java/image-20220422161320823.png)

直接**用 `StringBuilder` 对象拼接字符串：**

```java
String[] arr = {"he", "llo", "world"};
StringBuilder s = new StringBuilder();
for (String value : arr) {
    s.append(value);
}
System.out.println(s);
```

- IDEA 自带的代码检查机制也会提示修改代码；



#### String的equals() 和 Object的equals() 区别？

`String` 重写了 `equals` 方法：比较字符串的值； `Object` 的 `equals` 方法是比较对象的内存地址；



#### 字符串常量池？

**字符串常量池**：JVM 为了减少内存消耗，针对字符串（String 类）的一块区域，**避免字符串的重复创建**：

- 在**堆中创建字符串对象**；将字符串对象的**引用保存在字符串常量池**；

```java
String aa = "ab";
String bb = "ab"; // 直接返回：字符串常量池中”ab“的引用
System.out.println(aa==bb); // true：相同的引用/地址
```

更多关于字符串常量池的介绍可以看一下 [Java 内存区域详解](https://javaguide.cn/java/jvm/memory-area.html) ;



#### String 类型的变量和常量做“+”运算时发生了什么？

先来看字符串不加 `final` 关键字拼接的情况（JDK1.8）：

```java
String str1 = "str";
String str2 = "ing";
String str3 = "str" + "ing";
String str4 = str1 + str2;
String str5 = "string";
System.out.println(str3 == str4);//false
System.out.println(str3 == str5);//true
System.out.println(str4 == str5);//false
```

> **注意** ：比较 String 字符串的值是否相等，可以使用 `equals()` 方法。 `String` 中的 `equals` 方法是被重写过的。 `Object` 的 `equals` 方法是比较的对象的内存地址，而 `String` 的 `equals` 方法比较的是字符串的值是否相等。如果你使用 `==` 比较两个字符串是否相等的话，IDEA 还是提示你使用 `equals()` 方法替换。

![](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/java-guide-blog/image-20210817123252441.png)

**对于编译期可以确定值的字符串，也就是常量字符串 ，jvm 会将其存入字符串常量池。并且，字符串常量拼接得到的字符串常量在编译阶段就已经被存放字符串常量池，这个得益于编译器的优化。**

在编译过程中，Javac 编译器（下文中统称为编译器）会进行一个叫做 **常量折叠(Constant Folding)** 的代码优化。《深入理解 Java 虚拟机》中是也有介绍到：

![](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/javaguide/image-20210817142715396.png)

常量折叠会把常量表达式的值求出来作为常量嵌在最终生成的代码中，这是 Javac 编译器会对源代码做的极少量优化措施之一(代码优化几乎都在即时编译器中进行)。

对于 `String str3 = "str" + "ing";` 编译器会给你优化成 `String str3 = "string";` 。

并不是所有的常量都会进行折叠，只有编译器在程序编译期就可以确定值的常量才可以：

- 基本数据类型( `byte`、`boolean`、`short`、`char`、`int`、`float`、`long`、`double`)以及字符串常量。
- `final` 修饰的基本数据类型和字符串变量
- 字符串通过 “+”拼接得到的字符串、基本数据类型之间算数运算（加减乘除）、基本数据类型的位运算（<<、\>>、\>>> ）

**引用的值在程序编译期是无法确定的，编译器无法对其进行优化。**

对象引用和“+”的字符串拼接方式，实际上是通过 `StringBuilder` 调用 `append()` 方法实现的，拼接完成之后调用 `toString()` 得到一个 `String` 对象 。

```java
String str4 = new StringBuilder().append(str1).append(str2).toString();
```

我们在平时写代码的时候，尽量避免多个字符串对象拼接，因为这样会重新创建对象。如果需要改变字符串的话，可以使用 `StringBuilder` 或者 `StringBuffer`。

不过，字符串使用 `final` 关键字声明之后，可以让编译器当做常量来处理。

示例代码：

```java
final String str1 = "str";
final String str2 = "ing";
// 下面两个表达式其实是等价的
String c = "str" + "ing";// 常量池中的对象
String d = str1 + str2; // 常量池中的对象
System.out.println(c == d);// true
```

被 `final` 关键字修改之后的 `String` 会被编译器当做常量来处理，编译器在程序编译期就可以确定它的值，其效果就相当于访问常量。

如果 ，编译器在运行时才能知道其确切值的话，就无法对其优化。

示例代码（`str2` 在运行时才能确定其值）：

```java
final String str1 = "str";
final String str2 = getStr();
String c = "str" + "ing";// 常量池中的对象
String d = str1 + str2; // 在堆上创建的新的对象
System.out.println(c == d);// false
public static String getStr() {
      return "ing";
}
```

## 参考

- R 大（RednaxelaFX）关于常量折叠的回答：https://www.zhihu.com/question/55976094/answer/147302764