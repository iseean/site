``````` json
{
  "create_time": "2017-06-25T14:14:21.956306Z",
  "description": "DESCRIPTION",
  "id": "5dcb2e53-89ad-4251-9b58-a3524c154dcb",
  "meta": null,
  "tags": ["Java"],
  "target": "DRAFT",
  "title": "Java volatile 关键字"
}
```````

# Table of Contents

1.  [Java](#org8a4f4aa)
    1.  [Mybatis Mapper.xml 重复生成问题](#orgd898e9d)
    2.  [Equal和==的区别](#orga53c17e)
    3.  [final关键字](#orgc99706f)
        1.  [描述成员变量时](#orgaddb733)
        2.  [描述方法](#org2d13196)
        3.  [描述类](#org8eae40d)
    4.  [Char在Java中占两个字节](#org4735c4e)
    5.  [String，StringBuffer，StringBuilder区别](#orgccf621e)
    6.  [ArrayList、LinkedList、Vector区别](#org9ac9f1d)
    7.  [HashMap、HashTable、ConcurrentHashMap区别](#org2fb935f)
    8.  [HashMap](#orgb313b86)
    9.  [线程状态](#orgc930e1e)
    10. [Static 关键字](#orgfeccf76)
    11. [内部类](#org3e672ac)
        1.  [成员内部类](#org9aa08c8)
        2.  [局部内部类](#orgbe0dbb2)
        3.  [匿名内部类](#orgcf85bdc)
        4.  [静态内部类](#org16d4f6c)
    12. [为什么调用当前类的其他和父类的构造方法（ `this()` `super()` ）需要放在第一行。](#org1ea7340)
    13. [Java CAS和synchronized](#orgcdbf125)
    14. [LinkedHashMap的实现](#orgce1c4c7)
    15. [synchronized关键字](#orga36af11)
    16. [volatile关键字](#org445e71a)
    17. [并发编程的原子性、可见性、有序性](#orga7af3dc)
    18. [Java创建线程的方式](#org96b716e)
    19. [interrupted和isInterrupted方法的区别](#orga30c04f)
    20. [Daemon线程和用户线程](#org874821e)
    21. [Wait/Notify](#orga2fe44a)
    22. [Wait/Join和Sleep区别](#orgbc8ff47)
    23. [管道通信](#orgff4eec2)
    24. [ThreadLocal/InheritableThreadLocal](#orga94ace6)
    25. [Lock 接口](#orgd9f2fc9)
        1.  [方法](#org600894b)
    26. [ReentrantLock，ReentrantReadWriteLock区别](#orgda3845b)
    27. [线程上下文切换](#org2d9dd4e)
        1.  [减少上下文切换](#org0348c65)
    28. [CAS(Compare and swap)算法](#orgbb85b60)
    29. [协程](#orgdfbf37a)
    30. [避免死锁](#orga7ecf95)
    31. [线程池](#orgac9e6d7)
        1.  [FixedThreadPool](#orga7b22ba)
        2.  [SingleThreadExecutor](#orgbf5aea7)
    32. [Runnable和Callable区别](#org1d9f43e)
    33. [Executors类](#orgb0071f3)
    34. [创建ThreadPool](#org1aafe9f)
    35. [Thread 的start和run方法](#orgdcedb02)
    36. [ArrayList](#orgdee9505)
        1.  [ArrayList每次扩容默认为上次的1.5倍。](#org5f08bad)
    37. [HashMap的hash方法](#orgdb33b32)
2.  [数据库](#org128dc0a)
    1.  [事务隔离级别](#orga26bcf9)
        1.  [未提交读](#orga22cc91)
        2.  [已提交读](#org93bbd91)
        3.  [可重复读](#org221e3f6)
        4.  [可串行化](#org8f3694b)
    2.  [幻读](#org2c73932)
3.  [数学](#orgf3960c9)
    1.  [对数](#orgcce2f5a)
        1.  [ln 表示 log(e)n](#org1bc77b1)
        2.  [lg 表示 log(10)n](#orgb1089b2)


<a id="org8a4f4aa"></a>

# Java


<a id="orgd898e9d"></a>

## Mybatis Mapper.xml 重复生成问题

\#1 使用 1.3.7 以上版本
\#2 在 generatorConfig.xml 的 content 节点中添加以下配置

    <plugin type="org.mybatis.generator.plugins.UnmergeableXmlMappersPlugin"/>

\#3 在 pom.xml 中添加如下配置

    <configuration>
        <overwrite>true</overwrite>
    </configuration>

<https://segmentfault.com/a/1190000013038272>
<https://github.com/mybatis/generator/pull/311>


<a id="orga53c17e"></a>

## Equal和==的区别

使用 `==` 比较的是栈中的值。
Object的equal方法初始是比较的内存地址，但一些对象（String，Date等）会重写这个方法。

1.  如果两个对象相等，则hashcode一定也是相同的
2.  两个对象相等,对两个equals方法返回true
3.  两个对象有相同的hashcode值，它们也不一定是相等的
4.  综上，equals方法被覆盖过，则hashCode方法也必须被覆盖
5.  hashCode的默认行为是对堆上的对象产生独特值。如果没有重写hashCode()，则该class的两个对象无论如何都不会相等（即使这两个对象指向相同的数据）。


<a id="orgc99706f"></a>

## final关键字

使用final关键字描述不能被更改的引用。

-   final关键字可以用于成员变量、本地变量、方法以及类。
-   final成员变量必须在声明的时候初始化或者在构造器中初始化，否则就会报编译错误。
-   你不能够对final变量再次赋值。
-   本地变量必须在声明时赋值。
-   在匿名类中所有变量都必须是final变量。
-   final方法不能被重写。
-   final类不能被继承。
-   final关键字不同于finally关键字，后者用于异常处理。
-   final关键字容易与finalize()方法搞混，后者是在Object类中定义的方法，是在垃圾回收之前被JVM调用的方法。
-   接口中声明的所有变量本身是final的。
-   final和abstract这两个关键字是反相关的，final类就不可能是abstract的。
-   final方法在编译阶段绑定，称为静态绑定(static binding)。
-   没有在声明时初始化final变量的称为空白final变量(blank final variable)，它们必须在构造器中初始化，或者调用this()初始化。不这么做的话，编译器会报错“final变量(变量名)需要进行初始化”。
-   将类、方法、变量声明为final能够提高性能，这样JVM就有机会进行估计，然后优化。
-   按照Java代码惯例，final变量就是常量，而且通常常量名要大写。


<a id="orgaddb733"></a>

### 描述成员变量时

final常与static一起使用，表示常量。


<a id="org2d13196"></a>

### 描述方法

表示方法不能被子类重写。final方法会在编译时进行静态绑定（内嵌）。


<a id="org8eae40d"></a>

### 描述类

表示类不能被继承（类似于C#中的密封类）。


<a id="org4735c4e"></a>

## Char在Java中占两个字节


<a id="orgccf621e"></a>

## String，StringBuffer，StringBuilder区别

1.  是否可变

    String内部使用final关键字保存字符数组，所以是不可变的。
    StringBuffer，StringBuilder 可变。

2.  线程安全

    String不可变所以是线程安全的。
    StringBuffer对一些操作添加了同步锁，所以是线程安全的。
    StringBuilder是非线程安全的。


<a id="org9ac9f1d"></a>

## ArrayList、LinkedList、Vector区别

ArrayList、LinkedList是不同步的。
ArrayList内部使用数组保存数据（支持随机访问）。
LinkedList内部使用双向链表保存数据。
Vector是同步的。


<a id="org2fb935f"></a>

## HashMap、HashTable、ConcurrentHashMap区别

HashMap没有实现同步。
HashTable一直是采用的链表数组，Concurrenthashmap在1.8之后的版本采用的时数组/红黑树链表。
HashTable和ConcurrentHashMap实现同步的方式不同，HashTable使用同一把锁。
HashTable不允许key为null。


<a id="orgb313b86"></a>

## HashMap

HashMap内部通过一个链表数组存取数据（ **1.8之后的版本如果链表长度大于8,则将链表转为红黑树** ）。
添加的时候通过 `indexOf(hash(key.getHashCode()))` 确定在数组中的下标，然后比对链表，通过 `==` 和 `equal` 方法判断key是否存在。如果不存在则将元素添加在链表头部。
HashMap的数组长度总是2的n次冪，这样在 `indexOf` 方法中，可以使用 `hash&(length-1)` 代替取模运算。
取数据的时候同理。
HashMap扩容十分缓慢，因为需要对每一项数据重新计算，以确定位置。
多线程操作HashMap，在遇到多个线程同时进行扩容时可能会出现死循环。


<a id="orgc930e1e"></a>

## 线程状态

![img](../Assets/thread_status.jpeg)

-   新建状态
-   可运行状态
-   运行状态
-   阻塞状态
    -   **等待阻塞:** `wait()` 方法，线程会进入等待队列（waiting queue）。
    -   **同步阻塞:** 锁被占用，线程进入锁池（lock pool）。
    -   **其他阻塞:** `sleep()/join()` 或IO请求。
-   死亡状态


<a id="orgfeccf76"></a>

## Static 关键字

static 关键字主要有以下四种使用场景：

1.  修饰成员变量和成员方法: 被 static 修饰的成员属于类，不属于单个这个类的某个对象，被类中所有对象共享，可以并且建议通过类名调用。被static 声明的成员变量属于静态成员变量，静态变量 存放在 Java 内存区域的方法区。调用格式：类名.静态变量名 类名.静态方法名()
2.  静态代码块: 静态代码块定义在类中方法外, 静态代码块在非静态代码块之前执行(静态代码块—>非静态代码块—>构造方法)。 该类不管创建多少对象，静态代码块只执行一次.
3.  静态内部类（static修饰类的话只能修饰内部类）： 静态内部类与非静态内部类之间存在一个最大的区别: 非静态内部类在编译完成之后会隐含地保存着一个引用，该引用是指向创建它的外围内，但是静态内部类却没有。没有这个引用就意味着：1. 它的创建是不需要依赖外围类的创建。2. 它不能使用任何外围类的非static成员变量和方法。
4.  静态导包(用来导入类中的静态资源，1.5之后的新特性): 格式为： `import static` 这两个关键字连用可以指定导入某个类中的指定静态资源，并且不需要使用类名调用类中静态成员，可以直接使用类中静态成员变量和成员方法。


<a id="org3e672ac"></a>

## 内部类


<a id="org9aa08c8"></a>

### 成员内部类

成员内部类是外围类的一个成员，内部会保留一个外围类的引用（ `OuterClass.this` ）。
**成员内部类不能包含静态变量和方法。**


<a id="orgbe0dbb2"></a>

### 局部内部类

局部内部类只在方法和属性中有效。


<a id="orgcf85bdc"></a>

### 匿名内部类

没有访问修饰符。

没有构造函数。


<a id="org16d4f6c"></a>

### 静态内部类

不需要依赖外围类创建。

只能使用外围类的静态成员。


<a id="org1ea7340"></a>

## TODO 为什么调用当前类的其他和父类的构造方法（ `this()` `super()` ）需要放在第一行。


<a id="orgcdbf125"></a>

## TODO Java CAS和synchronized


<a id="orgce1c4c7"></a>

## LinkedHashMap的实现

LinkedHashMap内部使用一个双链表维护顺序，内部的Entry会额外维护 `before` 和 `after` 两个引用。


<a id="orga36af11"></a>

## synchronized关键字

synchronized关键字加到static静态方法和synchronized(class)代码块上都是是给Class类上锁，而synchronized关键字加到非static静态方法上是给对象上锁。
synchronized关键字不能被继承，子类重写方法时需要显式指定synchronized。


<a id="org445e71a"></a>

## volatile关键字

volatile关键字不保证原子性。
volatile关键字，会生成内存屏障：

1.  它确保指令重排序时不会把其后面的指令排到内存屏障之前的位置，也不会把前面的指令排到内存屏障的后面；即在执行到内存屏障这句指令时，在它前面的操作已经全部完成；

2.  它会强制将对缓存的修改操作立即写入主存；

3.  如果是写操作，它会导致其他CPU中对应的缓存行无效。


<a id="orga7af3dc"></a>

## 并发编程的原子性、可见性、有序性

-   **原子性:** 一组操作要么全部完成，要么都不执行。
-   **可见性:** 当一个线程修改一个变量时，其他线程能够立刻看到修改（由于CPU缓存会出现，修改没有刷新到内存的情况）。
-   **有序性:** 编译器会进行指令重排，在没有数据依赖的情况下多线程时会出现意料外的结果
    
         //线程1:
        context = loadContext();   //语句1
        inited = true;             //语句2
        
        //线程2:
        //这里如果语句2先于语句1执行，则会出现content未初始化的情况。
        while(!inited ){
          doSomethingwithconfig(context);
        }


<a id="org96b716e"></a>

## Java创建线程的方式

-   继承Thread类
-   实现Runnable接口


<a id="orga30c04f"></a>

## interrupted和isInterrupted方法的区别

`Thread.interrupted()` 会恢复中断状态。
`Thread.currentThread().isInterrupted()` 只查询中断状态。


<a id="org874821e"></a>

## Daemon线程和用户线程

Daemon 相当于C#中的后台线程。
一旦所有用户线程都结束运行，守护线程会随JVM一起结束工作。


<a id="orga2fe44a"></a>

## Wait/Notify

Wait会释放锁，线程进入等待状态。Notify会唤醒一个Wait的线程，但不会释放锁。


<a id="orgbc8ff47"></a>

## Wait/Join和Sleep区别

Wait/Join会释放锁，进入等待状态。
Join内部使用wait方法实现等待，对象是当前线程。
Sleep不会释放锁，进入阻塞状态。


<a id="orgff4eec2"></a>

## 管道通信

-   面向字节： PipedOutputStream、 PipedInputStream
-   面向字符: PipedWriter、 PipedReader


<a id="orga94ace6"></a>

## ThreadLocal/InheritableThreadLocal

每个线程的THreadLocal值不一样。
Inheritablethreadlocal可以使子线程共享父线程的值。
子线程创建后，对Inheritablethreadlocal的修改将无法应用到子线程。


<a id="orgd9f2fc9"></a>

## Lock 接口


<a id="org600894b"></a>

### 方法

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<tbody>
<tr>
<td class="org-left">void lock()</td>
<td class="org-left">获得锁，如果不可用则休眠，并禁止用以线程调度直到获得锁</td>
</tr>


<tr>
<td class="org-left">void lockInterruptilby()</td>
<td class="org-left">同上，但是获取过程中可以被中断</td>
</tr>


<tr>
<td class="org-left">Condition newCondition()</td>
<td class="org-left">获取等待通知组件</td>
</tr>


<tr>
<td class="org-left">boolean tryLock()</td>
<td class="org-left">如果获得锁，则返回true，如果锁不可用则返回false</td>
</tr>


<tr>
<td class="org-left">boolean tryLock(long time, TimeUnit unit)</td>
<td class="org-left">获得锁返回true，超时返回false，中断触发异常</td>
</tr>


<tr>
<td class="org-left">void unlock()</td>
<td class="org-left">释放锁</td>
</tr>
</tbody>
</table>


<a id="orgda3845b"></a>

## ReentrantLock，ReentrantReadWriteLock区别

ReentrantLock互斥锁。
ReentrantReadWriteLock实现ReadWriteLock接口。
ReentrantLock 维护两个锁：读锁/写锁。读锁是共享锁，写锁是排他锁。读读共享，读写互斥，写写互斥。


<a id="org2d9dd4e"></a>

## 线程上下文切换

-   **让步式上下文切换:** 执行线程主动释放CPU
-   **抢占式上下文切换:** 时间片用完，被其他线程抢占


<a id="org0348c65"></a>

### 减少上下文切换

1.  减少锁的使用。因为多线程竞争锁时会引起上下文切换。
2.  使用CAS算法。这种算法也是为了减少锁的使用。CAS算法是一种无锁算法。
3.  减少线程的使用。人物很少的时候创建大量线程会导致大量线程都处于等待状态。
4.  使用协程。


<a id="orgbb85b60"></a>

## CAS(Compare and swap)算法


<a id="orgdfbf37a"></a>

## 协程


<a id="orga7ecf95"></a>

## 避免死锁

1.  避免一个线程同时获得多个锁
2.  避免一个线程在锁内同时占用多个资源，尽量保证每个锁只占用一个资源
3.  尝试使用定时锁，使用lock.tryLock(timeout)来替代使用内部锁机制
4.  对于数据库锁，加锁和解锁必须在一个数据库连接里，否则会出现解锁失败的情况


<a id="orgac9e6d7"></a>

## 线程池


<a id="orga7b22ba"></a>

### FixedThreadPool

-   固定线程数。
-   使用LinkedBlockingQueue。
-   线程数不会超过corePoolSize和maxinumPoolSize是一样的。
-   keepAliveTime无效（创建的线程不会销毁）。


<a id="orgbf5aea7"></a>

### SingleThreadExecutor

-   corePoolSize为1.
-   其他和


<a id="org1d9f43e"></a>

## Runnable和Callable区别

Runnable不能返回结果。
Callable可以返回结果。


<a id="orgb0071f3"></a>

## Executors类

转换Runnable和Callable对象。


<a id="org1aafe9f"></a>

## 创建ThreadPool

-   通过构造方法。
-   通过Executors实现

FixedThreadPool 和 SingleThreadExecutor ： 允许请求的队列长度为 Integer.MAX<sub>VALUE,可能堆积大量的请求</sub>，从而导致OOM。
CachedThreadPool 和 ScheduledThreadPool ： 允许创建的线程数量为 Integer.MAX<sub>VALUE</sub> ，可能会创建大量线程，从而导致OOM。


<a id="orgdcedb02"></a>

## Thread 的start和run方法

start方法会创建线程然后在新创建的线程上调用run方法。
run会直接在当前线程上执行。


<a id="orgdee9505"></a>

## ArrayList


<a id="org5f08bad"></a>

### ArrayList每次扩容默认为上次的1.5倍。


<a id="orgdb33b32"></a>

## HashMap的hash方法

    (h = key.hashCode()) ^ (h >>> 16);
    // h >>> 16 取hashCode的高位（16位）
    // 然后与 hashCode进行亦或运算。


<a id="org128dc0a"></a>

# 数据库


<a id="orga26bcf9"></a>

## 事务隔离级别


<a id="orga22cc91"></a>

### 未提交读

可以读到其他会话未提交的数据。


<a id="org93bbd91"></a>

### 已提交读

只能读到其他会话已提交的数据。


<a id="org221e3f6"></a>

### 可重复读

事务开启后读了数据，在这个事务结束前读到的数据都保持一致。


<a id="org8f3694b"></a>

### 可串行化

串行化执行，读写都会阻塞。
当两个会话在事务中读了数据，但都未提交时,如果这两个会话都要写入数据，会造成死锁。


<a id="org2c73932"></a>

## 幻读

假设两会话A和B，会话A开启事务，并查询所有行

    set session transaction isolation level repeatable read;
    start transaction;
    select *
    from account;
    -- 未提交事务

得到查询结果：

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-right" />

<col  class="org-right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-right">id</th>
<th scope="col" class="org-right">amount</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-right">1</td>
<td class="org-right">50</td>
</tr>


<tr>
<td class="org-right">2</td>
<td class="org-right">50</td>
</tr>
</tbody>
</table>

此时B会话开启事务，并新增一条记录：

    set session transaction isolation level repeatable read;
    start transaction;
    insert account (amount)
    value (50);
    commit;
    -- 已提交事务

由于隔离级别是 `repeatable read` ，在会话A中是无法查询出这条记录的。
此时如果会话A进行更新操作并进行提交：

    update account
    set amount = 77;
    commit;

会发现会话B新提交的记录也被更新了。
最后数据如下：

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-right" />

<col  class="org-right" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-right">id</th>
<th scope="col" class="org-right">amount</th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-right">1</td>
<td class="org-right">77</td>
</tr>


<tr>
<td class="org-right">2</td>
<td class="org-right">77</td>
</tr>


<tr>
<td class="org-right">3</td>
<td class="org-right">77</td>
</tr>
</tbody>
</table>


<a id="orgf3960c9"></a>

# 数学


<a id="orgcce2f5a"></a>

## 对数

幂的逆运算


<a id="org1bc77b1"></a>

### ln 表示 log(e)n


<a id="orgb1089b2"></a>

### lg 表示 log(10)n

