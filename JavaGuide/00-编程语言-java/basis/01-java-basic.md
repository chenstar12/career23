### 标识符和关键字？

**标识符** ：简单来说， **标识符就是一个名字** ；程序、类、变量、方法的名字；

**关键字** ：特殊的标识符；Java 已赋予其特殊的含义；比如我们想开一家店，起的“名字”就是标识符。但是店名不能叫“警察局”，因为“警察局”这个名字已有特殊的含义；



### Java 的关键字？

> Tips：所有的关键字都是小写的；
>
> `default` 关键字很特殊，可用于：程序控制、类、方法、变量修饰符、访问控制。
>
> - 程序控制： `switch` 匹配不到任何情况时，用 `default` 表示默认情况。
> - 类、方法、变量修饰符：用 `default` 关键字表示默认实现。
> - 访问控制：如果一个方法没有修饰符，则默认 `default`，但加上这个修饰符了会报错。

⚠️注 ： `true`, `false`, 和 `null` 看起来像关键字，但实际上是字面值，也不可以作为标识符。

### 自增自减运算

++ 和 -- 运算符：放在变量之前时，先自增/减，再赋值；放在变量之后时，先赋值，再自增/减。

-  `b = ++a` ：先自增（a 增加 1），再赋值给 b；
-  `b = a++` ：先赋值给 b，再自增（a增加 1）；

#### 静态方法：为什么不能调用非静态成员?

结合 JVM 的知识：

1. 静态方法属于类，在类加载时分配内存，通过类名直接访问。而非静态成员属于实例对象，只有在对象实例化之后用对象名访问；
2. 非静态成员不存在时，静态成员就已经存在了，此时调用非静态成员属于非法操作。

#### 重载和重写的区别

**重载**：同一个类（或子类）：方法名相同；参数类型/个数/顺序不同；返回值和访问修饰符可以不同。

> 如 `StringBuilder` 的构造方法：相同的名字、不同的参数
>
> ```java
> StringBuilder sb = new StringBuilder();
> StringBuilder sb2 = new StringBuilder("HelloWorld");
> ```
>

综上：**重载是同一个类中的同名方法，根据不同的传参来执行不同的处理。**

**重写**：子类对父类的方法的重新实现；

1. 方法名、参数列表必须相同；子类方法返回类型 <= 父类，抛出的异常范围 <= 父类，访问修饰符范围 >= 父类。
2. 父类`private/final/static`方法不能重写，但 `static` 修饰的方法能被再次声明。
3. 构造方法无法重写

综上：**重写是子类对父类方法的重新改造，外观不变，内部逻辑改变。**

**重写：遵循“两同两小一大”**（摘录自《疯狂 Java 讲义》）：

- “两同”：方法名相同、形参列表相同；
- “两小”：子类方法返回类型 <= 父类，抛出的异常范围 <= 父类；
- “一大”：子类方法的访问权限 >= 父类方法;

### 可变长参数？

从 Java5 开始支持可变长参数，接受 0 或多个参数：

```java
public static void method1(String... args) 
```

- 可变参数只能作为最后一个参数；

```java
public static void method2(String arg1, String... args) 
```

**方法重载时，匹配固定参数还是可变参数？**

答：优先匹配固定参数的方法，因为固定参数的匹配度更高。

- 注：可变参数编译后会转换成数组

## 基本数据类型

Java 有 8 种基本数据类型：

- 6 种数字类型：
   - 4 种整数型：`byte`、`short`、`int`、`long`
   - 2 种浮点型：`float`、`double`
- 字符类型：`char`
- 布尔型：`boolean`。

**注：**

1.  `long` 类型：一定要在数值后加 **L**，否则将解析为整型。
2. `char a = 'h'`用单引号，`String a = "hello"` 用双引号。

八种基本类型对应的包装类：`Byte`、`Short`、`Integer`、`Long`、`Float`、`Double`、`Character`、`Boolean` 。

### 基本类型和包装类型？

- 包装类型默认值为 `null` ，而基本类型的默认值不是 `null`。
- 包装类型可用于泛型，基本类型不可以。
- 包装类型属于对象类型，java所有对象实例存在于堆。

### 包装类型的缓存机制？

用缓存机制来提升性能；`Byte`,`Short`,`Integer`,`Long` 默认创建数值 **[-128，127]** 的相应类型的缓存数据，`Character` 创建 **[0,127]** 范围的缓存数据，`Boolean` 直接返回 `True` or `False`。

**Integer 缓存源码：**

```java
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}
private static class IntegerCache {
    static final int low = -128;
    static final int high;
    static {
        // high value may be configured by property
        int h = 127;
    }
}
```

**`Character` 缓存源码:**

```java
public static Character valueOf(char c) {
    if (c <= 127) { // must cache
      return CharacterCache.cache[(int)c];
    }
    return new Character(c);
}

private static class CharacterCache {
    private CharacterCache(){}
    static final Character cache[] = new Character[127 + 1];
    static {
        for (int i = 0; i < cache.length; i++)
            cache[i] = new Character((char)i);
    }

}
```

**`Boolean` 缓存源码：**

```java
public static Boolean valueOf(boolean b) {
    return (b ? TRUE : FALSE);
}
```

如果超出对应范围仍然会去创建新的对象，缓存的范围区间的大小只是在性能和资源之间的权衡。

两种浮点数类型的包装类 `Float`,`Double` 并没有实现缓存机制。

```java
Integer i1 = 33;
Integer i2 = 33;
System.out.println(i1 == i2);// 输出 true

Float i11 = 333f;
Float i22 = 333f;
System.out.println(i11 == i22);// 输出 false

Double i3 = 1.2;
Double i4 = 1.2;
System.out.println(i3 == i4);// 输出 false
```

下面我们来看一下问题。下面的代码的输出结果是 `true` 还是 `false` 呢？

```java
Integer i1 = 40;
Integer i2 = new Integer(40);
System.out.println(i1==i2);
```

`Integer i1=40` 这一行代码会发生装箱，也就是说这行代码等价于 `Integer i1=Integer.valueOf(40)` 。因此，`i1` 直接使用的是缓存中的对象。而`Integer i2 = new Integer(40)` 会直接创建新的对象。

因此，答案是 `false` 。你答对了吗？

记住：**所有整型包装类对象之间值的比较，全部使用 equals 方法比较**。

![](https://img-blog.csdnimg.cn/20210422164544846.png)

### 自动装箱与拆箱了解吗？原理是什么？

**什么是自动拆装箱？**

- **装箱**：将基本类型用它们对应的引用类型包装起来；
- **拆箱**：将包装类型转换为基本数据类型；

举例：

```java
Integer i = 10;  //装箱
int n = i;   //拆箱
```

上面这两行代码对应的字节码为：

```java
   L1

    LINENUMBER 8 L1

    ALOAD 0

    BIPUSH 10

    INVOKESTATIC java/lang/Integer.valueOf (I)Ljava/lang/Integer;

    PUTFIELD AutoBoxTest.i : Ljava/lang/Integer;

   L2

    LINENUMBER 9 L2

    ALOAD 0

    ALOAD 0

    GETFIELD AutoBoxTest.i : Ljava/lang/Integer;

    INVOKEVIRTUAL java/lang/Integer.intValue ()I

    PUTFIELD AutoBoxTest.n : I

    RETURN
```

从字节码中，我们发现装箱其实就是调用了 包装类的`valueOf()`方法，拆箱其实就是调用了 `xxxValue()`方法。

因此，

- `Integer i = 10` 等价于 `Integer i = Integer.valueOf(10)`
- `int n = i` 等价于 `int n = i.intValue()`;

注意：**如果频繁拆装箱的话，也会严重影响系统的性能。我们应该尽量避免不必要的拆装箱操作。**

```java
private static long sum() {
    // 应该使用 long 而不是 Long
    Long sum = 0L;
    for (long i = 0; i <= Integer.MAX_VALUE; i++)
        sum += i;
    return sum;
}
```
