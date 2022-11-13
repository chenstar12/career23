**Java 泛型（Generics）**：泛型参数可增强代码的可读性、稳定性；通过泛型参数指定可传入的对象类型：

- 如 `ArrayList<Persion> persons = new ArrayList<Persion>()` 指明了只能传入 `Persion` 对象，编译器会检测泛型参数；

```java
ArrayList<E> extends AbstractList<E>
```

- 原生 `List` 返回 `Object` 类型，用泛型后会自动转换为 Person 类型；

### 泛型的使用方式？

**1.泛型类**：

```java
public class GenericClass<T>{ // T可以随便写，常用T、E、K、V；实例化泛型类时，必须指定T的具体类型
    private T key;

    public GenericClass(T key) {
        this.key = key;
    }

    public T getKey(){
        return key;
    }
```

实例化泛型类：

```java
GenericClass<Integer> genericInteger = new GenericClass<Integer>(123456);
```

**2.泛型接口** ：

```java
public interface GeneratorInterface<T> {
    public T method();
}
```

实现接口（1：不指定类型）：

```java
class GeneratorImpl<T> implements GeneratorInterface<T>{
    @Override
    public T method() {
        return null;
    }
```

实现接口（2：指定类型）：

```java
class GeneratorImpl<T> implements GeneratorInterface<String>{
    @Override
    public String method() {
        return "hello";
    }
```

**3.泛型方法** ：

```java
   public static <E> void printArray( E[] inputArray ) // 泛型数组：E[]
   {
         for ( E element : inputArray ){
            System.out.printf( "%s ", element );
         }
         System.out.println();
    }
```

使用：

```java
// 创建不同类型的数组
Integer[] intArray = { 1, 2, 3 };
String[] stringArray = { "Hello", "World" };
printArray( intArray  );
printArray( stringArray  );
```
> 注: `public static < E > void printArray()` 称静态泛型方法; java泛型只是一个占位符；类在实例化时传递类型参数，由于static加载先于类的实例化，此时类的泛型还没有传递类型参数，所以**static泛型方法无法使用类的泛型！**只能自己声明一个`<E>`

### 项目中用了泛型？

- 自定义接口 `CommonResult<T>` ：根据具体的返回类型，动态指定返回结果的数据类型；
- 集合工具类（参考 `Collections` 中）；
