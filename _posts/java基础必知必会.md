

# java基础必知必会

## 数据基础

### 8种基本数据类型

#### 分类

![image-20201118162237335](../../../AppData/Roaming/Typora/typora-user-images/image-20201118162237335.png)

#### 整数类型

| **类  型** | **占用存储空间** | **表数范围**  |
| ---------- | ---------------- | ------------- |
| byte       | 1字节=8bit       | -128  ~ 127   |
| short      | 2字节            | -215 ~215-1   |
| int        | 4字节            | -231 ~  231-1 |
| long       | 8字节            | -263 ~  263-1 |

#### 浮点数类型

| **类 型**    | **占用存储空间** | **表数范围**            |
| ------------ | ---------------- | ----------------------- |
| 单精度float  | 4字节            | -3.403E38  ~ 3.403E38   |
| 双精度double | 8字节            | -1.798E308  ~ 1.798E308 |

**注:**float 和 long 后面定义市要跟 f 和 l

#### 数据类型转换

![image-20201118164132334](https://i.loli.net/2020/11/18/xqbgJsG8VOdIt3A.png)

```java
System.out .println(3+4+“Hello!”);      //输出：7Hello!
System.out.println(“Hello!”+3+4);      //输出：Hello!34
System.out.println(‘a’+1+“Hello!”);    //输出：98Hello!
System.out.println(“Hello!”+‘a’+1);    //输出：Hello!a1
// 字符串以前的按数字运算,以后的按字符串拼接运算
```

### HashCode 与 equals 

当你把对象加入 HashSet 时，HashSet 会先计算对象的 hashcode 值来判断对象加入的位置，同时也会与该位置其他已经加入的对象的 hashcode 值作比较，如果没有相符的 hashcode，HashSet 会假设对象没有重复出现。但是如果发现有相同 hashcode 值的对象，这时会调用 `equals()`方法来检查 hashcode 相等的对象是否真的相同。如果两者相同，HashSet 就不会让其加入操作成功。如果不同的话，就会重新散列到其他位置。

**hashCode（）与 equals（）的相关规定**

1. 如果两个对象相等，则 hashcode 一定也是相同的
2. 两个对象相等,对两个对象分别调用 equals 方法都返回 true
3. 两个对象有相同的 hashcode 值，它们也不一定是相等的
4. **因此，equals 方法被覆盖过，则 hashCode 方法也必须被覆盖**
5. hashCode() 的默认行为是对堆上的对象产生独特值。如果没有重写 hashCode()，则该 class 的两个对象无论如何都不会相等（即使这两个对象指向相同的数据）

### String

#### String s = "aaa"

如果一个 String 对象已经被创建过了，那么就会从 String Pool 中取得引用

#### new String("abc")

使用这种方式一共会创建两个字符串对象（前提是 String Pool 中还没有 "abc" 字符串对象）。

- "abc" 属于字符串字面量，因此编译时期会在 String Pool 中创建一个字符串对象，指向这个 "abc" 字符串字面量；
- 而使用 new 的方式会在堆中创建一个字符串对象。

#### String, StringBuffer and StringBuilder

**1. 可变性**

- String 不可变
- StringBuffer 和 StringBuilder 可变

**2. 线程安全**

- String 不可变，因此是线程安全的
- StringBuilder 不是线程安全的
- StringBuffer 是线程安全的，内部使用 synchronized 进行同步

### 位运算



## 方法

### 值传递与引用传递

分析下面代码输出结果:

```java
    public static void main(String[] args) {
        int[] arr = { 1, 2, 3, 4, 5 };
        System.out.println(arr[0]);
        change(arr);
        System.out.println(arr[0]);
    }

    public static void change(int[] array) {
        // 将数组的第一个元素变为0
        array[0] = 0;
    }
// 1,0
```

```java
public class Test {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        Student s1 = new Student("小张");
        Student s2 = new Student("小李");
        Test.swap(s1, s2);
        System.out.println("s1:" + s1.getName());
        System.out.println("s2:" + s2.getName());
    }

    public static void swap(Student x, Student y) {
        Student temp = x;
        x = y;
        y = temp;
        System.out.println("x:" + x.getName());
        System.out.println("y:" + y.getName());
    }
}

// x:小李 y:小张 s1:小张  s2:小李
```

```java
public class Test {
    public static void main(String[] args)  {
        StringBuffer sb = new StringBuffer("Hello ");
        System.out.println("Before change, sb = " + sb);
        changeData(sb);
        System.out.println("After change, sb = " + sb);
    }
    public static void changeData(StringBuffer strBuf) {
        StringBuffer sb2 = new StringBuffer("Hi，I am ");
        strBuf = sb2;
        sb2.append("World!");
    }
}

// Before change, sb = Hello 
// After change, sb = Hello 
```

**下面再总结一下 Java 中方法参数的使用情况：**

- 一个方法不能修改一个基本数据类型的参数（即数值型或布尔型）。
- 一个方法可以改变一个对象参数的状态。
- 一个方法不能让对象参数引用一个新的对象。

### 深拷贝与浅拷贝

1. **浅拷贝**：对基本数据类型进行值传递，对引用数据类型进行引用传递般的拷贝，此为浅拷贝。
2. **深拷贝**：对基本数据类型进行值传递，对引用数据类型，创建一个新的对象，并复制其内容，此为深拷贝。

![deep and shallow copy](https://my-blog-to-use.oss-cn-beijing.aliyuncs.com/2019-7/java-deep-and-shallow-copy.jpg)



## 抽象类与接口

### 抽象类和接口的主要区别

- 抽象类中可以没有抽象方法，也可以抽象方法和非抽象方法共存
- 接口中的方法在JDK8之前只能是抽象的，JDK8版本开始提供了接口中方法的default实现
- 抽象类和类一样是单继承的；接口可以实现多个父接口
- 抽象类中可以存在普通的成员变量；接口中的变量必须是static final类型的，必须被初始化，接口中只有常量，没有变量

> 抽象类可以有非抽象方法,接口可以多继承

所以关于选择:
根据抽象类和接口的不同之处，当我们仅仅需要定义一些抽象方法而不需要其余额外的具体方法或者变量的时候，我们可以使用接口。反之，则需要使用抽象类，因为抽象类中可以有非抽象方法和变量。



## 反射机制与注解

### 反射机制

JAVA 反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为 java 语言的反射机制。

eg:

```java
public class Robot {
    private String name;
    public void sayHi(String helloSentence){
        System.out.println(helloSentence + " " + name);
    }
    private String throwHello(String tag){
        return "Hello " + tag;
    }
    static {
        System.out.println("Hello Robot");
    }
}
```

```java
public class ReflectSample {
    public static void main(String[] args) throws ClassNotFoundException, IllegalAccessException, InstantiationException, InvocationTargetException, NoSuchMethodException, NoSuchFieldException {
        Class rc = Class.forName("com.interview.javabasic.reflect.Robot");
        Robot r = (Robot) rc.newInstance();
        System.out.println("Class name is " + rc.getName());
        Method getHello = rc.getDeclaredMethod("throwHello", String.class);
        getHello.setAccessible(true);
        Object str = getHello.invoke(r, "Bob");
        System.out.println("getHello result is " + str);
        Method sayHi = rc.getMethod("sayHi", String.class);
        sayHi.invoke(r, "Welcome");
        Field name = rc.getDeclaredField("name");
        name.setAccessible(true);
        name.set(r, "Alice");
        sayHi.invoke(r, "Welcome");
        System.out.println(System.getProperty("java.ext.dirs"));
        System.out.println(System.getProperty("java.class.path"));

    }
}
```

​                                                                  

### 注解

**理解:**类似于注释,不对代码逻辑起到实际作用,但注释是写给人看的,注解是写给编译器看的

**原理:**注解本质是一个继承了Annotation 的特殊接口，其具体实现类是Java 运行时生成的动态代理类。而我们通过反射获取注解时，返回的是Java 运行时生成的动态代理对象$Proxy1。通过代理对象调用自定义注解（接口）的方法，会最终调AnnotationInvocationHandler 的invoke 方法。该方法会从memberValues 这个Map 中索引出对应的值。而memberValues 的来源是Java 常量池。

**常用注解:**

 1.）Override:覆盖重写
 2.）Deprecated:声明过时
 3.）`SuppressWarnings`:抑制编译器警告

**自定义注解:**

```java
 /**
 8  * 水果名称注解
 9  */
10 @Target(FIELD)
11 @Retention(RUNTIME)
12 @Documented
13 public @interface FruitName {
14     String value() default "";
15 }
```



## 泛型

```java
public class Box<T> {
    // T stands for "Type"
    private T t;
    public void set(T t) { this.t = t; }
    public T get() { return t; }
}
```



## 异常

### 异常分类

![img](https://snailclimb.gitee.io/javaguide/docs/java/basis/images/Java%E5%BC%82%E5%B8%B8%E7%B1%BB%E5%B1%82%E6%AC%A1%E7%BB%93%E6%9E%84%E5%9B%BE.png)

error 和 exception 的主要区别是程序能否处理

### 异常处理

**异常处理原则**

- 尽可能捕获比较详细的异常，而不是使用Exception一起捕获。
- 当本模块不知道捕获之后该怎么处理异常时，可以将其抛给上层模块。上层模块拥有更多的业务逻辑，可以进行更好的处理。
- 捕获异常后至少应该有日志记录，方便之后的排查。
- 不要使用一个很大的try – catch包住整段代码，不利于问题的排查。

**异常处理例子:**

![image-20201118182008872](https://i.loli.net/2020/11/18/cAX9s6zOlZSqN1h.png)

**Throwable 类常用方法**

- **`public string getMessage()`**:返回异常发生时的简要描述
- **`public string toString()`**:返回异常发生时的详细信息
- **`public string getLocalizedMessage()`**:返回异常对象的本地化信息。使用 `Throwable` 的子类覆盖这个方法，可以生成本地化信息。如果子类没有覆盖该方法，则该方法返回的信息与 `getMessage（）`返回的结果相同
- **`public void printStackTrace()`**:在控制台上打印 `Throwable` 对象封装的异常信息

**try-catch-finally**

- **try 块：** 用于捕获异常。其后可接零个或多个 catch 块，如果没有 catch 块，则必须跟一个 finally 块。
- **catch 块：** 用于处理 try 捕获到的异常。
- **finally 块：** 无论是否捕获或处理异常，finally 块里的语句都会被执行。当在 try 块或 catch 块中遇到 return 语句时，finally 语句块将在方法返回之前被执行。

**在以下 4 种特殊情况下，finally 块不会被执行：**

1. 在 finally 语句块第一行发生了异常。 因为在其他行，finally 块还是会得到执行
2. 在前面的代码中用了 System.exit(int)已退出程序。 exit 是带参函数 ；若该语句在异常语句之后，finally 会执行
3. 程序所在的线程死亡。
4. 关闭 CPU。

下面这部分内容来自 issue:https://github.com/Snailclimb/JavaGuide/issues/190。

**注意：** 当 try 语句和 finally 语句中都有 return 语句时，在方法返回之前，finally 语句的内容将被执行，并且 finally 语句的返回值将会覆盖原始的返回值。如下：

```java
public class Test {
    public static int f(int value) {
        try {
            return value * value;
        } finally {
            if (value == 2) {
                return 0;
            }
        }
    }
}Copy to clipboardErrorCopied
```

如果调用 `f(2)`，返回值将是 0，因为 finally 语句的返回值覆盖了 try 语句块的返回值。

## IO流



### 获取用键盘输入常用的两种方法

方法 1：通过 Scanner

```java
Scanner input = new Scanner(System.in);
String s  = input.nextLine();
input.close();Copy to clipboardErrorCopied
```

方法 2：通过 BufferedReader

```java
BufferedReader input = new BufferedReader(new InputStreamReader(System.in));
String s = input.readLine();
```