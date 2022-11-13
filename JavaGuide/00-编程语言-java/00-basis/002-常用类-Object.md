**Object 类**：所有类的父类。主要方法：

```java
public final native Class<?> getClass() // 返回当前对象的 Class 对象，不允许子类重写(final关键字)

public native int hashCode() // 返回对象的哈希码，用在哈希表，如HashMap

public boolean equals(Object obj) // 对象的内存地址是否相等; String 类重写了该方法 ---- 比较字符串的值是否相等

protected native Object clone() throws CloneNotSupportedException

public String toString() // 返回类名的哈希码；建议重写 

public final native void notify() // 唤醒一个在此对象监视器上等待的线程(监视器相当于锁)。不能重写。

public final native void notifyAll()
// 暂停线程的执行(注：sleep 方法不释放锁，而wait释放锁 ，timeout是等待时间。)
public final native void wait(long timeout) throws InterruptedException
// nanos为额外时间（以毫微秒为单位，范围是 0-999999）; 超时时间timeout需加上 nanos 毫秒
public final void wait(long timeout, int nanos) throws InterruptedException
// 一直等待，没有超时时间这个概念
public final void wait() throws InterruptedException

protected void finalize() throws Throwable { } // 实例被垃圾回收时触发的操作
```

#### == 和 equals() 

- 基本数据类型:`==` 比较的是值。
- 引用数据类型:`==` 比较对象的内存地址。

**`equals()`** ：比较对象的内存地址（和 == 一样，但String 类重写该方法）；

- `equals()`方法存在于`Object`类:

```java
public boolean equals(Object obj) {
     return (this == obj);
}
```

`equals()` 方法两种情况：

- **类没有重写 `equals()`方法** ：`equals()`等价于“==”；
- **类重写了 `equals()`方法** ：一般重写 `equals()`方法来比较两个对象中的属性是否相等；

```java
String a = new String("ab"); // a 为一个引用
String b = new String("ab"); // b为另一个引用,对象的内容一样
String aa = "ab"; // 放在常量池中
String bb = "ab"; // 先从常量池中查找
System.out.println(aa == bb);// true
System.out.println(a == b);// false
System.out.println(a.equals(b));// true
System.out.println(42 == 42.0);// true
```

`String`类`equals()`方法：

```java
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```



#### hashCode() ？

作用：获取哈希码（`int` 整数）/散列码。表示该对象在哈希表中的索引。

注： `Object` 的 `hashCode()` 方法是本地(native)方法，即用 C 语言实现的，将对象的内存地址转换为整数；

散列表 HashMap 存储键值对(key-value)：**根据“键”快速的检索“值”。利用了散列码来快速查找；**



#### 为什么要有 hashCode？

以“`HashSet` 查重”为例；摘自我的 Java 启蒙书《Head First Java》:

> 当你把对象加入 `HashSet` 时，`HashSet` 会**先计算对象的 `hashCode` 值**来判断对象加入的位置，同时也会与其他已经加入的对象的 `hashCode` 值作比较，如果没有相符的 `hashCode`，`HashSet` 会假设对象没有重复出现。但是如果发现有相同 `hashCode` 值的对象，这时会**调用 `equals()` 方法**来检查 `hashCode` 相等的对象是否真的相同。如果两者相同，`HashSet` 就不会让其加入操作成功。如果不同的话，就会重新散列到其他位置。这样我们就大大减少了 `equals` 的次数，相应就大大提高了执行速度。

 `hashCode()` 和 `equals()`都可用于比较两个对象是否相等；**为什么 JDK 要同时提供这两个方法？**

- 在一些容器（如 `HashMap`、`HashSet`）中，用 `hashCode()` 效率更高（参考添加元素进`HashSet`的过程）！大大**缩小了查找成本**；

**为什么不只提供 `hashCode()` 方法？**

- 因为两个对象的`hashCode` 值相等并不代表两个对象就相等；

**为什么两个对象有相同的 `hashCode` 值，也不一定相等？**

- 因为不同对象可能会有相同的哈希值（哈希碰撞）

总结：

- 如果两个对象的`hashCode` 值相等：两个对象不一定相等（哈希碰撞）；
- 如果两个对象的`hashCode` 值相等并且`equals()`方法返回 `true`：两个对象相等；
- 如果两个对象的`hashCode` 值不相等：直接认为这两个对象不相等；



#### 为什么重写 equals() 时必须重写 hashCode() 方法？

因为两个相等的对象的 `hashCode` 值必须是相等。即如果 `equals` 方法判断两个对象是相等的，那这两个对象的 `hashCode` 值也要相等。

如果重写 `equals()` 时没有重写 `hashCode()` 方法的话就可能会导致 `equals` 方法判断是相等的两个对象，`hashCode` 值却不相等。

**思考** ：重写 `equals()` 时没有重写 `hashCode()` 方法的话，使用 `HashMap` 可能会出现什么问题？

**总结** ：

- `equals` 方法判断两个对象是相等的：两个对象的 `hashCode` 值也要相等；
- 两个对象有相同的 `hashCode` 值：不一定是相等的（哈希碰撞）；
