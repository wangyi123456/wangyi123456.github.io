# 操作系统及`java`异步编程面试

**单线程和多线程，进程与线程的区别**

**线程活性故障及其解决方法**

**线程调度方式**

进程间通信方式

虚拟内存

**可见性，原子性以及有序性**

**synchronized，volatile，Atomic等关键字**

**线程池及阻塞队列**



## 进程 与 线程 

### 线程进程区别

进程与线程之间的主要区别可以总结如下。

- 进程是一个“执行中的程序”，是系统进行**资源分配和调度**的一个独立单位
- 线程是进程的一个实体，一个进程中一般拥有多个线程。线程之间**共享地址空间**和其它资源（所以通信和同步等操作,线程比进程更加容易）
- 线程一般不拥有系统资源，但是也有一些必不可少的资源（使用ThreadLocal存储）
- 线程上下文的切换比进程上下文切换要快很多。

**线程上下文切换比进程上下文切换快的原因**，可以总结如下：

- **进程切换时**，涉及到当前进程的CPU环境的保存和新被调度运行进程的CPU环境的设置
- **线程切换时**，仅需要保存和设置少量的寄存器内容，不涉及存储管理方面的操作

### 多线程与单线程之间的关系

- 多线程是指在**一个进程中，并发执行了多个线程**，每个线程都实现了不同的功能
- 在单核CPU中，将CPU分为很小的时间片，在每一时刻只能有一个线程在执行，是一种微观上轮流占用CPU的机制。由于**CPU轮询**的速度非常快，所以看起来像是“同时”在执行一样
- 多线程会存在**线程上下文切换**，会导致程序执行速度变慢
- 多线程不会提高程序的执行速度，反而会降低速度。但是对于用户来说，可以**减少用户的等待响应时间，提高了资源的利用效率**

**解析：**

搞清楚多线程和单线程之间的区别，有助于我们理解为什么要使用多线程并发编程。**多线程并发利用了CPU轮询时间片的特点**，在一个线程进入阻塞状态时，可以快速切换到其余线程执行其余操作，这有利于**提高资源的利用率，最大限度的利用系统提供的处理能力**，**有效减少了用户的等待响应时间**。

但是，多线程并发编程也会带来数据的安全问题，线程之间的竞争也会导致线程死锁和锁死等**活性故障**。线程之间的上下文切换也会带来**额外的开销**等问题

![image-20201115204011726](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201115204011726.png)

![image-20201115204340398](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201115204340398.png)

## 进程间通信方式

- 通过使用套接字Socket来实现不同机器间的进程通信
- 通过映射一段可以被多个进程访问的共享内存来进行通信
- 通过写进程和读进程利用管道进行通信

## 进程调度算法

- **先到先服务(FCFS)调度算法** : 从就绪队列中选择一个最先进入该队列的进程为之分配资源，使它立即执行并一直执行到完成或发生某事件而被阻塞放弃占用 CPU 时再重新调度。
- **短作业优先(SJF)的调度算法** : 从就绪队列中选出一个估计运行时间最短的进程为之分配资源，使它立即执行并一直执行到完成或发生某事件而被阻塞放弃占用 CPU 时再重新调度。
- **时间片轮转调度算法** : 时间片轮转调度是一种最古老，最简单，最公平且使用最广的算法，又称 RR(Round robin)调度。每个进程被分配一个时间段，称作它的时间片，即该进程允许运行的时间。
- **多级反馈队列调度算法** ：前面介绍的几种进程调度的算法都有一定的局限性。如**短进程优先的调度算法，仅照顾了短进程而忽略了长进程** 。多级反馈队列调度算法既能使高优先级的作业得到响应又能使短作业（进程）迅速完成。，因而它是目前**被公认的一种较好的进程调度算法**，UNIX 操作系统采取的便是这种调度算法。
- **优先级调度** ： 为每个流程分配优先级，首先执行具有最高优先级的进程，依此类推。具有相同优先级的进程以 FCFS 方式执行。可以根据内存要求，时间要求或任何其他资源要求来确定优先级。



## 线程的生命周期

### 周期

![image-20201115221042256](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201115221042256.png)

### 内置函数剖析

#### **sleep 和 wait 的区别：**

**基本的差别**

-  sleep 是 Thread类的方法，Wait是Oject类中定义的方法
-  sleep 方法可以在任何地方使用
-  Wait 方法只能在 synchronized，方法或 synchronized块中使用

**最主要的本质区别**

-  Thread. sleep。只会让出CPU，不会导致锁行为的改变
-  `Object.wait`不仅让出CPU，还会释放已经占有的同步资源锁

#### **join 方法：**

当前线程调用，则其它线程全部停止，等待当前线程执行完毕，接着执行。

#### **yield 方法：**

该方法使得线程放弃当前分得的 CPU 时间。但是不使线程阻塞，即线程仍处于可执行状态，随时可能再次分得 CPU 时间。

#### notify 和 `notifyAll` 的区别

## `java`多线程操作：

###  start 和 run 的区别

![image-20201116194950138](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201116194950138.png)

### thread 和 runnable 的关系

#### 用 thread来构造多线程

```java
public class myThread{
    public static void main(String[] args) {
        my_thread myth1 = new my_thread("th01");
        my_thread myth2 = new my_thread("th02");
        my_thread myth3 = new my_thread("th03");
        myth1.start();
        myth2.start();
        myth3.start();
    }
    public static class my_thread extends Thread{
        private String name;
        public my_thread (String name){
            this.name = name;
        }
        @Override
        public void run(){
            for (int i = 0; i<10 ; i++){
                System.out.println(this.name + ":  i = " + i);
            }
        }
    }
```

#### 用 runnable 来构造多线程

```java
public class myRunnable{
    public static void main(String[] args) {
        Thread ru01 = new Thread(new my_Runnable("ru01"));
        Thread ru02 = new Thread(new my_Runnable("ru02"));
        Thread ru03 = new Thread(new my_Runnable("ru03"));
        ru01.start();
        ru02.start();
        ru03.start();

    }
    public static class my_Runnable implements Runnable{
        private String name;
        public my_Runnable(String name){
            this.name = name;
        }
        @Override
        public void run() {
            for (int i = 0; i<5; i++){
                System.out.println(this.name+"  :i="+i);
            }
        }
    }
}
```

#### 二者区别

- Thread是实现了 Runnable接口的类，使得run支持多线程
- 因类的单一继承原则，推荐多使用 Runnable接口

### 如何处理线程返回值

![image-20201116204317388](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201116204317388.png)

## 线程死锁

### 死锁产生条件

死锁是最常见的一种线程活性故障。死锁的起因是多个线程之间相互等待对方而被永远暂停（处于非Runnable）。死锁的产生必须满足如下**四个必要条件：**

- **资源互斥**：一个资源每次只能被一个线程使用
- **请求与保持条件**：一个线程因请求资源而阻塞时，对已获得的资源保持不放
- **不剥夺条件**：线程已经获得的资源，在未使用完之前，不能强行剥夺
- **循环等待条件**：若干线程之间形成一种头尾相接的循环等待资源关系

### 如何避免死锁的发生？

- **粗锁法：**使用一个粒度粗的锁来消除“请求与保持条件”，缺点是会明显降低程序的并发性能并且会导致资源的浪费。
- **锁排序法：（必须回答出来的点）**

**指定获取锁的顺序**，比如某个线程只有获得A锁和B锁，才能对某资源进行操作，在多线程条件下，如何避免死锁？

通过指定锁的获取顺序，比如规定，只有获得A锁的线程才有资格获取B锁，按顺序获取锁就可以避免死锁。这通常被认为是解决死锁很好的一种方法。

- 使用**显式锁**中的`**ReentrantLock.try(long,TimeUnit)**`来申请锁

## 内存管理

### 内存管理方式

#### 分段与分页

简单分为**连续分配管理方式**和**非连续分配管理方式**这两种。连续分配管理方式是指为一个用户程序分配一个连续的内存空间，常见的如 **块式管理** 。同样地，非连续分配管理方式允许一个程序使用的内存分布在离散或者说不相邻的内存中，常见的如**页式管理** 和 **段式管理**。

1. **块式管理** ： 远古时代的计算机操系统的内存管理方式。将内存分为几个固定大小的块，每个块中只包含一个进程。如果程序运行需要内存的话，操作系统就分配给它一块，如果程序运行只需要很小的空间的话，分配的这块内存很大一部分几乎被浪费了。这些在每个块中未被利用的空间，我们称之为碎片。
2. **页式管理** ：把主存分为大小相等且固定的一页一页的形式，页较小，相对相比于块式管理的划分力度更大，提高了内存利用率，减少了碎片。页式管理通过页表对应逻辑地址和物理地址。
3. **段式管理** ： 页式管理虽然提高了内存利用率，但是页式管理其中的页实际并无任何实际意义。 段式管理把主存分为一段段的，每一段的空间又要比一页的空间小很多 。但是，最重要的是段是有实际意义的，每个段定义了一组逻辑信息，例如,有主程序段 MAIN、子程序段 X、数据段 D 及栈段 S 等。 段式管理通过段表对应逻辑地址和物理地址。
4. **段页式管理机制** 。段页式管理机制结合了段式管理和页式管理的优点。简单来说段页式管理机制就是把主存先分成若干段，每个段又分成若干页，也就是说 **段页式管理机制** 中段与段之间以及段的内部的都是离散的

#### 二者区别与联系

1. 共同点
   - 分页机制和分段机制都是为了提高内存利用率，较少内存碎片。
   - 页和段都是离散存储的，所以两者都是离散分配内存的方式。但是，每个页和段中的内存是连续的。
2. 区别
   - 页的大小是固定的，由操作系统决定；而段的大小不固定，取决于我们当前运行的程序。
   - 分页主要用于实现虚拟内存，从而获得更大的地址空间；分段主要是为了使程序和数据可以被划分为逻辑上独立的地址空间并且有助于共享和保护。

#### 快表与多级页表

### 虚拟内存与页面置换算法

#### **虚拟内存** 

虚拟内存使得应用程序认为它拥有连续的可用的内存（一个连续完整的地址空间），而实际上，它通常是被分隔成多个物理内存碎片，还有部分暂时存储在外部磁盘存储器上，在需要时进行数据交换。与没有使用虚拟内存技术的系统相比，使用这种技术的系统使得大型程序的编写变得更容易，对真正的物理内存（例如 RAM）的使用也更有效率。目前，大多数操作系统都使用了虚拟内存，如 Windows 家族的“虚拟内存”；

#### 页面置换算法

地址映射过程中，若在页面中发现所要访问的页面不在内存中，则发生缺页中断 。

> **缺页中断** 就是要访问的**页**不在主存，需要操作系统将其调入主存后再进行访问。 在这个时候，被内存映射的文件实际上成了一个分页交换文件。

当发生缺页中断时，如果当前内存中并没有空闲的页面，操作系统就必须在内存选择一个页面将其移出内存，以便为即将调入的页面让出空间。用来选择淘汰哪一页的规则叫做页面置换算法，我们可以把页面置换算法看成是淘汰页面的规则。

- **OPT 页面置换算法（最佳页面置换算法）** ：最佳(Optimal, OPT)置换算法所选择的被淘汰页面将是以后永不使用的，或者是在最长时间内不再被访问的页面,这样可以保证获得最低的缺页率。但由于人们目前无法预知进程在内存下的若千页面中哪个是未来最长时间内不再被访问的，因而该算法无法实现。一般作为衡量其他置换算法的方法。
- **FIFO（First In First Out） 页面置换算法（先进先出页面置换算法）** : 总是淘汰最先进入内存的页面，即选择在内存中驻留时间最久的页面进行淘汰。
- **LRU （Least Currently Used）页面置换算法（最近最久未使用页面置换算法）** ：LRU算法赋予每个页面一个访问字段，用来记录一个页面自上次被访问以来所经历的时间 T，当须淘汰一个页面时，选择现有页面中其 T 值最大的，即最近最久未使用的页面予以淘汰。
- **LFU （Least Frequently Used）页面置换算法（最少使用页面置换算法）** : 该置换算法选择在之前时期使用最少的页面作为淘汰页。



## `java`线程安全以及synchronize的源码剖析

### **原子性，可见性，有序性；**

![image-20201117105933373](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201117105933373.png)

### 对象锁与类锁

![image-20201117110806125](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201117110806125.png)

![image-20201117111340090](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201117111340090.png)

### **synchronized底层实现**



![image-20201117114201015](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201117114201015.png)

### jmm的内存可见性 与 happeneds-before原则

![image-20201117152614282](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201117152614282.png)

![image-20201117152631467](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201117152631467.png)

![image-20201117153035919](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201117153035919.png)

![image-20201117152508008](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201117152508008.png)



![image-20201117152335588](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201117152335588.png)

### **轻量级锁volatile关键字**

#### volatile变量为何立即可见？

-  当写一个 volatile变量时，JMM会把该线程对应的工作内存中的共享变量值刷新到主内存中

-  当读取一个 volatile变量时，JMM会把该线程对应的工作内存置为无效。

  ![image-20201117155212600](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201117155212600.png)

  ![image-20201117155112338](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201117155112338.png)

#### volatile如何禁止重排优化

- 保证特定操作的执行顺序 
- 保证某些变量的内存可见性
- 通过插入内存屏障指令禁止在内存屏障前后的指令执行重排序优化强制刷出各种CPU的缓存数据，因此任何CPU上的线程都能读取到这些数据的最新版本

![image-20201117154622475](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201117154622475.png)

#### **volatile和 synchronized的区别**

1. volatile本质是在告诉 JVM 当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取； synchronized则是锁定当前变量，只有当前线程可以访问该变量，其他线程被阻塞住直到该线程完成变量操作为止
2. volatile仅能使用在变量级别； synchronized则可以使用在变量、方法和类级别
3. volatile仅能实现变量的修改可见性，不能保证原子性；而synchronized则可以保证变量修改的可见性和原子性
4. volatile。不会造成线程的阻塞； synchronized可能会造成线程的阻塞
5. volatile标记的变量不会被编译器优化； synchronized标记的变量可以被编译器优化

### **显式锁`ReentrantLock**`

#### 定义

-  位于java.uti. concurrent. locks包
-  和 Countdownlatch、 Future Task、 Semaphore一样基于AQS实现
-  能够实现比 synchronized更细粒度的控制，如控制 Fairness
-  调用lock0之后，必须调用 unlock）释放锁
-  性能未必比 synchronized高，并且也是可重入的

![image-20201117120553476](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201117120553476.png)

![image-20201117120649601](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201117120649601.png)

![image-20201117121351298](C:\Users\37779\AppData\Roaming\Typora\typora-user-images\image-20201117121351298.png)

### CAS(乐观锁)







## Java中的线程池以及Atomic等关键字的解析

