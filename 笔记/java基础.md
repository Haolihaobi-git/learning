# JAVA基础

### 数据类型

- byte/8
- char/16
- short/16
- int/32
- float/32
- long/64
- double/64
- boolean/~
- boolean 只有两个值：true、false，可以使用 1 bit 来存储，但是具体大小没有明确规定。JVM 会在编译时期将 boolean 类型的数据转换为 int（为什么不用byte，对于32位cpu来说，一次处理数据是32位），使用 1 来表示 true，0 表示 false。JVM 支持 boolean 数组，但是是通过读写 byte 数组来实现的。

### 缓存池
new Integer(123) 与 Integer.valueOf(123) 的区别在于：
- new Integer(123) 每次都会新建一个对象；
- Integer.valueOf(123)先判断值是否在缓存池中，如果在的话就直接返回缓存池的内容, 编译器会在自动装箱过程调用 valueOf() 方法。
- 在 Java 8 中，Integer 缓存池的大小默认为 -128~127。

``````s
Integer x = new Integer(123);
Integer y = new Integer(123);
System.out.println(x == y);    // false
Integer z = Integer.valueOf(123);
Integer k = Integer.valueOf(123);
System.out.println(z == k);   //true

Integer m = 123;
Integer n = 123;
System.out.println(m == n); // true

Integer keke = 999;
Integer haha = 999;
System.out.println(keke == haha); //false
``````

### String的最大长度
如果字符串是放在堆中，那String最大长度就是2的32次方，也就是4G，
如果字符串是放在常量池中，如Strins str = “aaa”， 只能是2的16次方减2，减2是因为null值需要2个字节表示 

### String不可变的好处
- 缓存hash值：不可变的特性可以使得 hash 值也不可变，因此只需要进行一次计算。因为 String 的 hash 值经常被使用，例如 String 用做 HashMap 的 key。
- String Pool （字符串常量池）的需要：如果一个String对象已经被创建过了，那么就会从String Pool中取得引用。只有String是不可变的，才可能使用。
-  安全性：String 经常作为参数，String 不可变性可以保证参数不可变。例如在作为网络连接参数的情况下如果 String 是可变的，那么在网络连接过程中，String 被改变，改变 String 对象的那一方以为现在连接的是其它主机，而实际情况却不一定是。
- 线程安全

### StringBuffer 是线程安全的，内部使用 synchronized 进行同步

### 隐式转型

	short s1 = 1;
	//s1 = s1 + 1;编译错误
	s1 += 1;//相当于s1 = (short) (s1 + 1);

### switch
java7开始，swith支持String，enum，byte，short， int， 但还是不支持long；

### 修饰符的区别
	访问权限      类   包  子类  其他包
  　  public           ∨   	∨     ∨         ∨          （对任何人都是可用的）
   　 protect         ∨   	∨     ∨         ×　　　 （继承的类可以访问以及和private一样的权限）
   　 default    	 ∨   	∨      ×         ×　　　 （包访问权限，即在整个包内均可被访问）
   　 private   	  ∨   	×       ×         ×　　　 （除类型创建者和类型的内部方法之外的任何人都不能访问的元素）

### 抽象类与接口
- 抽象类：如果一个类中包含抽象方法，那么这个类必须声明为抽象类。抽象类和普通类最大的区别是，抽象类不能被实例化，需要继承抽象类才能实例化其子类。子类一定要实现抽象方法
- 接口：接口的成员默认都是public的，并且不允许定义位private或者protected。接口的字段默认都是static和final，接口也可以有默认的方法实现


##### 区别
- 抽象类提供了一种IS-A关系，接口更像是一种LIKE-A关系
- 一个类可以实现多个接口，但是不能继承多个抽象类
- 接口的字段只能是public和static和final类型，而抽象类的字段没有这种限制
- 接口的成员只能是public，protect（如果方法有默认实现得话）的，而抽象类的成员可以有多种访问权限。

### clone()
- clone() 是 Object 的 protected 方法，它不是 public，一个类不显式去重写 clone()，其它类就不能直接去调用该类实例的 clone() 方法。

重写方法，如果子类抛出异常，那父类一定要抛出这个异常的自己或者父类，子类重写方法可以不抛出异常。

### 初始化顺序
    父类（静态变量、静态语句块）
    子类（静态变量、静态语句块）
    父类（实例变量、普通语句块）
    父类（构造函数）
    子类（实例变量、普通语句块）
    子类（构造函数）

### 异常

Throwable 可以用来表示任何可以作为异常抛出的类，分为两种： **Error**  和 **Exception**。其中 Error 用来表示 JVM 无法处理的错误，Exception 分为两种：

- **受检异常** ：需要用 try...catch... 语句捕获并进行处理，并且可以从异常中恢复；
- **非受检异常** ：是程序运行时错误，例如除 0 会引发 Arithmetic Exception，此时程序崩溃并且无法恢复。

![](C:\Users\好里好比\Documents\面试\学习\我的笔记图片\68747470733a2f2f63732d6e6f7465732d313235363130393739362e636f732e61702d6775616e677a686f752e6d7971636c6f75642e636f6d2f50506a77502e706e67.png)
## java容器
容器主要包括Collection和map两种，colllection存储着对象的集合。和map存储着键值对的映射
### Collection

Collection 继承了 Iterable 接口，其中的 iterator() 方法能够产生一个 Iterator 对象，通过这个对象就可以迭代遍历 Collection 中的元素

![](C:\Users\好里好比\Documents\面试\学习\我的笔记图片\68747470733a2f2f63732d6e6f7465732d313235363130393739362e636f732e61702d6775616e677a686f752e6d7971636c6f75642e636f6d2f37333430336438342d643932312d343966312d393361392d6438666530353066333439372e706e67.png)

#### 1,Set:
- TreeSet：基于红黑树实现，支持有序性操作，例如根据一个范围查找元素的操作。但是查找效率不如 HashSet，HashSet 查找的时间复杂度为 O(1)，TreeSet 则为 O(logN)。
- HashSet:基于哈希表实现，支持快速查找，但不支持有序性操作。并且失去了元素的插入顺序信息，也就是说使用 Iterator 遍历 HashSet 得到的结果是不确定的。
- LinkedHashSet：具有 HashSet 的查找效率，且内部使用双向链表维护元素的插入顺序。
#### 2, List

- ArrayList：基于动态数组实现，支持随机访问。
- Vector：和 ArrayList 类似，但它是线程安全的。
- LinkedList：基于双向链表实现，只能顺序访问，但是可以快速地在链表中间插入和删除元素。不仅如此，LinkedList 还可以用作栈、队列和双向队列。

#### 3. Queue

- LinkedList：可以用它来实现双向队列。
- PriorityQueue：基于堆结构实现，可以用它来实现优先队列。

### Map
![](C:\Users\好里好比\Documents\面试\学习\我的笔记图片\68747470733a2f2f63732d6e6f7465732d313235363130393739362e636f732e61702d6775616e677a686f752e6d7971636c6f75642e636f6d2f37373464373536622d393032612d343161332d613366642d3831636133656636383864632e706e67.png)

- TreeMap：基于红黑树实现。
- HashMap：基于哈希表实现。
- HashTable：和 HashMap 类似，但它是线程安全的，这意味着同一时刻多个线程可以同时写入 HashTable  并且不会导致数据不一致。它是遗留类，不应该去使用它。现在可以使用 ConcurrentHashMap 来支持线程安全，并且  ConcurrentHashMap 的效率会更高，因为 ConcurrentHashMap 引入了分段锁。
- LinkedHashMap：使用双向链表来维护元素的顺序，顺序为插入顺序或者最近最少使用（LRU）顺序。

### 容器中的设计模式
迭代器设计模式
``````s
List<String> list = new ArrayList<>();
list.add("a");
list.add("b");
for (String item : list) {
    System.out.println(item);
}
``````
适配器设计模式
``````s
Integer[] arr = {1, 2, 3};
List list = Arrays.asList(arr);
``````
### ArrayList
modCount 用来记录 ArrayList 结构发生变化的次数。结构发生变化是指添加或者删除至少一个元素的所有操作，或者是调整内部数组的大小，仅仅只是设置元素的值不算结构发生变化。

在进行序列化或者迭代等操作时，需要比较操作前后 modCount 是否改变，如果改变了需要抛出 ConcurrentModificationException。

#### ArrayList与Vector之间的比较

- Vector 是同步的，因此开销就比 ArrayList 要大，访问速度更慢。最好使用 ArrayList 而不是 Vector，因为同步操作完全可以由程序员自己来控制；

- Vector 每次扩容请求其大小的 2 倍（也可以通过构造函数设置增长的容量），而 ArrayList 是 1.5 倍。

###  替代方案
可以使用 `Collections.synchronizedList();` 得到一个线程安全的 ArrayList。

```s
List<String> list = new ArrayList<>();
List<String> synList =    Collections.synchronizedList(list);
```
  也可以使用 concurrent 并发包下的 CopyOnWriteArrayList 类。
```
  List<String> list = new CopyOnWriteArrayList<>();
```
### CopyOnWriteArrayList
写操作在一个复制的数组上进行，读操作还是在原始数组中进行，读写分离，互不影响。

写操作需要加锁，防止并发写入时导致写入数据丢失。

写操作结束之后需要把原始数组指向新的复制数组。

但是 CopyOnWriteArrayList 有其缺陷：

- 内存占用：在写操作时需要复制一个新的数组，使得内存占用为原来的两倍左右；

- 数据不一致：读操作不能读取实时性的数据，因为部分写操作的数据还未同步到读数组中。

所以 CopyOnWriteArrayList 不适合内存敏感以及对实时性要求很高的场景。

### HashMap

- 内部包含了一个 Entry 类型的数组 table。

```
transient Entry[] table;
```

- HashMap 使用拉链法来解决冲突,同一个链表中存放哈希值和散列桶取模运算结果相同的 Entry。

- HashMap 使用第 0 个桶存放键为 null 的键值对。

- 新的键值对插在链表的头部，而不是链表的尾部。

- 位运算的代价比求模运算小的多,所以每次扩容都是2的次方，可以通过直接与就实现了取模操作

- 从 JDK 1.8 开始，一个桶存储的链表长度大于等于 8 时会将链表转换为红黑树。

- 和扩容相关的参数主要有：capacity、size、threshold 和 load_factor。

  | 参数       | 含义                                                         |
  | ---------- | ------------------------------------------------------------ |
  | capacity   | table 的容量大小，默认为 16。需要注意的是 capacity 必须保证为 2 的 n 次方。 |
  | size       | 键值对数量。                                                 |
  | threshold  | size 的临界值，当 size 大于等于 threshold 就必须进行扩容操作。 |
  | loadFactor | 装载因子，table 能够使用的比例，threshold = (int)(newCapacity * loadFactor)。 |



### ConcurrentHashMap 

ConcurrentHashMap 和 HashMap 实现上类似，最主要的差别是 ConcurrentHashMap 
采用了分段锁（Segment），每个分段锁维护着几个桶（HashEntry），多个线程可以同时访问不同分段锁上的桶，从而使其并发度更高（并发度就是
Segment 的个数，默认创建 16 个 ）。

JDK 1.8 的改动

JDK 1.7 使用分段锁机制来实现并发更新操作，核心类为 Segment，它继承自重入锁 ReentrantLock，并发度与 Segment 数量相等。

JDK 1.8 使用了 CAS 操作来支持更高的并发度，在 CAS 操作失败时使用内置锁 synchronized。

并且 JDK 1.8 的实现也在链表过长时会转换为红黑树。

# 线程
三种方法使用线程

- 实现Runnable接口
- 实现Callable接口
- 继承Thread类

实现runnable和Callable接口的类只能当做一个可以在线程中运行的任务，不是真正意义上的线程，因此最后还需要通过Thread来调用。可以说是任务是通过线程驱动从而执行的。

### 实现runnable接口

需要实现 run() 方法。

通过 Thread 调用 start() 方法来启动线程。

```
public class MyRunnable implements Runnable {
    public void run() {
        // ...
    }
}
```
```
public static void main(String[] args) {
    MyRunnable instance = new MyRunnable();
    Thread thread = new Thread(instance);
    thread.start();
}
```



### 实现Callable接口

Callable有返回值，返回值通过futrueTask进行封装。

```
public class MyCallable implements Callable<Integer> {
    public Integer call() {
        return 123;
    }
}
```
```
public static void main(String[] args) throws ExecutionException, InterruptedException {
    MyCallable mc = new MyCallable();
    FutureTask<Integer> ft = new FutureTask<>(mc);
    Thread thread = new Thread(ft);
    thread.start();
    System.out.println(ft.get());
}
```

### 实现接口 VS 继承 Thread

- Java 不支持多重继承，因此继承了 Thread 类就无法继承其它类，但是可以实现多个接口；
- 类可能只要求可执行就行，继承整个 Thread 类开销过大。



### ThreadPool

1,线程池的创建

- **Exectors.newFixedThreadPool(int size)：**

​     创建一个固定大小的线程池，有任务提交，有空闲线程那么空闲线程去执行，否则任务添加到无界的等待队列中

- **Exectors.newCachedThreadPool():**

当线程池规模当线程池的当前规模超过处理需求时，那么将回收线程，当需求增加时则添加新的线程

**Exectors.newSingleThreadExcutor():**

这个线程异常结束，它会创建另一个线程来代替。

**Exectors.newScheduledThreadPool():**

创建一个固定长度的线程池，而且以延迟或定时的方式来执行任务。

上面都是通过工厂方法来创建线程池，其实它们内部都是通过创建ThreadPoolExector对象来创建线程池的。下面是ThreadPoolExctor的构造函数。

### 为什么不适用线程池的默认实现方式

- newFixedThreadPool和singleThreadExcutor的队列都是LinkedBlockingQueue，会把内存占满
- newCachedThreadPool的线程数量是Integer的max值

### ThreadPoolExctor的参数意义

- ### corePoolSize

  线程池保持线程的最小数量，线程的数量时逐步到达corepoolSize值得，而不是一次性开启全部线程

- ### maxinumPoolSize

线程池中能容纳的最大线程数量，如果超出，则使用RejectedExecutionHandler拒绝策略处理。 

- ### keepAliveTime

线程的最大生命周期，生命周期有两个约束条件：一：该参数针对的是超过corePoolSize数量的线程；二：处于非运行状态的线程。

- ### unit

这是keepAliveTime的时间单位，可以是纳秒，微秒，毫秒，秒，分钟等。

- ### workQueue

当线程池中的线程都处于运行状态，而此时任务数量继续增加，则需要一个容器来容纳这些任务，这就是任务队列

**newFixedThreadPool**和**newSingleThreadExector使用的是LinkedBlockingQueue的无界模式(美团面试题目)。

- ### threadFactory

定义如何启动一个线程，可以设置线程的名称，并且可以确定是否是后台线程等。

- ### handler

拒绝任务处理器。由于超出线程数量和队列容量而对继续增加的任务进行处理的程序。



### 线程的管理过程

**首先创建一个线程池，然后根据任务的数量逐步将线程增大到corePoolSize，如果此时仍有任务增加，则放置到workQueue中，**直到workQueue爆满为止，然后继续增加池中的线程数量（增强处理能力），最终达到maxinumPoolSize。那如果此时还有任务要增加进来呢？这就需要handler来处理了，**或者丢弃新任务，或者拒绝新任务，或者挤占已有的任务（拒绝策略，美团面试）**。在任务队列和线程池都饱和的情况下，一旦有线程处于等待（任务处理完毕，没有新任务）状态的时间超过keepAliveTime，则该线程终止，也就是说池中的线程数量会逐渐降低，直至为corePoolSize数量为止。

### 线程的异常处理方式

```java
public class JAVA线程运行时出现异常的解决方法 {
    public static void main(String[] args) {
        ThreadTest threadTest = new ThreadTest();
        threadTest.setUncaughtExceptionHandler(new ExeceptionHandler());
        threadTest.start();
         
    }
}

class ThreadTest extends Thread{
    @Override
    public void run() {
        int a = 1 / 0;
    }
}

class ExeceptionHandler implements Thread.UncaughtExceptionHandler{

    @Override
    public void uncaughtException(Thread t, Throwable e) {
        System.out.println("出现异常了");
        e.printStackTrace();
    }
}
```



### 线程池中线程发生异常的处理方式

​		当一个线程池里面的线程异常后:执行方式是execute时,可以看到堆栈异常的输出。当执行方式是submit时,堆栈异常没有输出。但是调用Future.get()方法时，可以捕获到异常。不会影响线程池里面其他线程的正常执行。线程池会把这个线程移除掉，并创建一个新的线程放到线程池中。

​	java中的线程池用的是ThreadPoolExecutor，真正执行代码的部分是runWorker方法：final void runWorker(Worker w)，线程池中处理异常的是afterExecute（）。

![1593223946(1)](C:\Users\好里好比\Documents\学习\我的笔记图片\1593223946(1).jpg)

###### 1，自定义线程池，继承ThreadPoolExecutor并复写其afterExecute(Runnable r, Throwable t)方法。

![1593224063(1)](C:\Users\好里好比\Documents\学习\我的笔记图片\1593224063(1).jpg)

##### 2，实现Thread.UncaughtExceptionHandler接口

实现Thread.UncaughtExceptionHandler接口，实现void uncaughtException(Thread t, Throwable e);方法，并将该handler传递给线程池的ThreadFactory

![1593240682](C:\Users\好里好比\Documents\学习\我的笔记图片\1593240682.jpg)

##### 继承ThreadGroup

覆盖其uncaughtException方法。（与第二种方式类似，因为ThreadGroup类本身就实现了Thread.UncaughtExceptionHandler接口)

> 尤其注意：上面三种方式针对的都是通过execute(xx)的方式提交任务，如果你提交任务用的是submit()方法，那么上面的三种方式都将不起作用,而应该使用下面的方式

![1593240909(1)](C:\Users\好里好比\Documents\学习\我的笔记图片\1593240909(1).jpg)

##### 采用Future模式

如果提交任务的时候使用的方法是submit，那么该方法将返回一个Future对象，所有的异常以及处理结果都可以通过future对象获取。 采用Future模式，将返回结果以及异常放到Future中，在Future中处理

![1593240966(1)](C:\Users\好里好比\Documents\学习\我的笔记图片\1593240966(1).jpg)

### runnable

##### 在没有返回值的线程中运行

```java
ExectorService exector = Exectors.newCachedThreadPool();
exector.execute(new Runnable(){
public void run(){
System.out.println("running...");
}
});

```

### callable

```
ExectorService exector = Exectors.newCachedThreadPool();
Callable<Result> task = new Callable<Result>() {
    public Result call() {
        return new Computor().compute();
    }
};
Future<Result> future = exector.submit(task);
result = future.get();  //改方法会一直阻塞，直到提交的任务被运行完毕
```

#### 任务按照什么顺序来执行(FIFO,优先级)

- 如果传入不同的BlockQueue就可以实现不同的执行顺序。传入LinkedBlockingQueue则表示先来先服务，传入PriorityBlockingQueue则使用优先级来处理任务

### 任务队列

这个问题和BlockingQueue相关。  BlockingQueue有三个子类，一个是ArrayBlockingQueue(有界队列),一个是LinkedBlockingQueue(默认无界，但可以配置为有界)，PriorityBlockingQueue(默认无界，可配置为有界)。所以，对于有多少个任务等待执行与传入的阻塞队列有关。

**newFixedThreadPool**和**newSingleThreadExector**使用的是LinkedBlockingQueue的无界模式。而**newCachedThreadPool**使用的是SynchronousQueue，这种情况下线程是不需要排队等待的，SynchronousQueue适用于线程池规模无界。

### 如果系统过载则需要拒绝一个任务，如何通知任务被拒绝？

- 当有界队列被填满或者某个任务被提交到一个已关闭的Executor时将会启动饱和策略，即使用RejectedExecutionHandler来处理。JDK中提供了几种不同的RejectedExecutionHandler的实现：AbortPolicy，CallerRunsPolicy,
  DiscardPolicy和DiscardOldestPolicy。

**AbortPolicy：**默认的饱和策略。该策略将抛出未检查的**RejectedExcutionException**,调用者可以捕获这个异常，然后根据自己的需求来处理。

**DiscardPolicy：**该策略将会抛弃提交的任务

**DiscardOldestPolicy：**该策略将会抛弃下一个将被执行的任务(处于队头的任务)，然后尝试重新提交该任务到等待队列

**CallerRunsPolicy:**该策略既不会抛弃任务也不会抛出异常，而是在调用execute()的线程中运行任务。比如我们在主线程中调用了execute(task)方法，但是这时workQueue已经满了，并且也不会创建的新的线程了。这时候将会在主线程中直接运行execute中的task。



### ThreadLocal

- 每个Thread都有一个TheadLocal.ThreadLocalMap对象，当调用一个ThreadLocal的set（T value)方法时，先得到当前线程的ThreadLocalMap对象，然后将ThreadLocal-》value键值对插入到该map中。

### AQS

​		不同的自定义同步器争用共享资源的方式也不同。**自定义同步器在实现时只需要实现共享资源state的获取与释放方式即可**，至于具体线程等待队列的维护（如获取资源失败入队/唤醒出队等），AQS已经在顶层实现好了。自定义同步器实现时主要实现以下几种方法：

- isHeldExclusively()：该线程是否正在独占资源。只有用到condition才需要去实现它。
- tryAcquire(int)：独占方式。尝试获取资源，成功则返回true，失败则返回false。
- tryRelease(int)：独占方式。尝试释放资源，成功则返回true，失败则返回false。
- tryAcquireShared(int)：共享方式。尝试获取资源。负数表示失败；0表示成功，但没有剩余可用资源；正数表示成功，且有剩余资源。
- tryReleaseShared(int)：共享方式。尝试释放资源，如果释放后允许唤醒后续等待结点返回true，否则返回false。

## 锁优化

这里的锁优化主要是指 JVM 对 synchronized 的优化。



### 自旋锁

主要思想是让一个线程在请求一个共享数据的锁时执行忙循环一段时间，如果在这段时间内能获得锁，就可以避免进入线程阻塞状态减少开销，但是它需要进行忙循环（自旋）占用CPU时间，所以只适合共享数据的锁定状态很短的场景。在JDK1.6引入了自适应的自旋锁，意味着自旋锁的次数不再固定了，而是由前一次在同一个锁上的自旋次数及锁的拥有着的状态来决定的。

### 锁消除

锁消除主要是通过逃逸分析来支持，如果堆上的共享数据不可能逃逸出去被其它线程访问到，那么就可以把它们当成私有数据对待，也就可以将它们的锁进行消除。

### 锁粗化

一系列的连续操作都对同一个对象反复加锁和解锁，频繁的加锁和解锁会导致性能损耗，所以就把锁粗化到整个操作的外部。

### Synchronized

### 偏向锁

偏向锁的思想是偏向于让第一个获取锁对象的线程，这个线程在之后获取该锁就不再需要进行同步操作，甚至莲CAS操作也不再需要。当有另一个线程去尝试获取锁对象时，偏向状态就宣告结束，此时撤销偏向锁后恢复到未锁定状态或者轻量级锁状态。适用于长期只有一个线程访问同步块的场景

### 轻量级锁

使用CAS操作来避免重量级锁使用互斥的开销。对于绝大部分的锁，在整个同步周期内都是不存在竞争，因此也就不需要都使用互斥量进行同步，可以先采用CAS操作进行同步，如果CAS失败了再改用互斥量进行同步。追求响应时间, 且同步块执行速度非常快的场景

### 重量级锁

追求吞吐量, 或者是同步块执行时间较长的时候

#### 对象头的锁状态

001   无锁

00 轻量级 锁

101 偏向锁

## 内存屏障

处理器提供了两个内存屏障指令（Memory Barrier)

### 写内存屏障（Store Memory Barrier）

在指令插入Store Barrier， 能让写入缓存种的最新数据更行写入主内存，让其他线程可见

### 读内存屏障

在指令前插入Load Barrier， 可以让高速缓存中的数据失效，强制从新的之内存加载数据。





# JVM面试题

### JDK、 JRE、JVM 的关系是什么？

```
什么是 JVM ？
英文名称 ( Java Virtual Machine )，就是 JAVA 虚拟机， 它只识别 .class 类型文件，它能够将 class 文件中的字节码指令进行识别并调用操作系统向上的 API 完成动作。
什么是 JRE ？
英文名称（ Java Runtime Environment ），Java 运行时环境。它主要包含两个部分：JVM 的标准实现和 Java 的一些基本类库。相对于 JVM 来说，JRE多出来一部分 Java 类库。
什么是 JDK？ 英文名称（ Java Development Kit ），Java 开发工具包。JDK 是整个 Java 开发的核心，它集成了 JRE 和一些好用的小工具。例如：javac.exe、java.exe、jar.exe 等。
```

这三者的关系：一层层的嵌套关系。JDK > JRE > JVM。

### JVM 的内存模型以及分区情况和作用

![bf54c7d0-3ee8-11e-9993-cdc55e79d144](C:\Users\好里好比\Documents\学习\我的笔记图片\bf54c7d0-3ee8-11e9-9993-cdc55e79d144.png)
    方法区   
    用于存储虚拟机加载的类信息，常量，静态变量等数据。在 JDK 1.8 之后，原来永久代的数据被分到了堆和元空间中。元空间存储类的元信息，静态变量和常量池等放入堆中。  
    堆
    存放对象实例，所有的对象和数组都要在堆上分配。 是 JVM 所管理的内存中最大的一块区域。  
    栈
    Java 方法执行的内存模型：存储局部变量表，操作数栈，动态链接，方法出口等信息。生命周期与线程相同。   
    本地方法栈
    作用与虚拟机栈类似，不同点本地方法栈为 native 方法执行服务，虚拟机栈为虚拟机执行的 Java 方法服务。  
    程序计数器
    当前线程所执行的行号指示器。是 JVM 内存区域最小的一块区域。执行字节码工作时就是利用程序计数器来选取下一条需要执行的字节码指令。

### 如果对象的引用被置为null，垃圾收集器是否会立即释放对象占用的内存？

```
不会，在下一个垃圾回收周期中，这个对象将是可被回收的。	
也就是说当一个对象的引用变为 null 时，并不会被垃圾收集器立刻回收，而是在下一次垃圾回收时才会释放其占用的内存。
```

### Java对象创建过程

1，类加载机制检查：JVM首先检查一个new指令的参数是否能在常量池中定位到一个符号引用，并且检查该符号引用代表的类是否已被加载、解析和初始化过
2，分配内存：把一块儿确定大小的内存从Java堆中划分出来，有以下几种分配方法

- 指针碰撞法（已分配内存和空闲内存分别在不同的一侧，通过一个指针座位分界点，需要分配内存时，仅仅需要把指针往空闲的一端移动与对象大小相等的距离。）
- 空闲列表法（已分配的内存和空闲内存相互交错，JVM通过维护一个列表，记录可用的内存块信息，当分配操作发生时，从列表中找到一个足够大的内存块分配给对象实例，并更新列表上的记录。）
- 内存分配并发问题：（TLAB：也叫本地线程缓冲分配， 为每一个线程预先分配一块内存，JVM在给线程中的对象分配内存时，首先在TLAB分配，当对象大于TLAB中的剩余内存或TLAB的内存已用尽时，再采用的CAS进行内存分配。）
  3，初始化零值：对象的实例字段不需要赋初始值也可以直接使用其默认零值，就是这里起得作用
  4，设置对象头：存储对象自身的运行时数据，类型指针
  5，执行<init %>：为对象的字段赋值

### Java对象结构

- Java对象由三个部分组成：对象头、实例数据、对齐填充。
- 对象头由两部分组成，第一部分存储对象自身的运行时数据：哈希码、GC分代年龄、锁标识状态、线程持有的锁、偏向线程ID（一般占32/64 bit）。第二部分是指针类型，指向对象的类元数据类型（即对象代表哪个类）。如果是数组对象，则对象头中还有一部分用来记录数组长度。
- 实例数据用来存储对象真正的有效信息（包括父类继承下来的和自己定义的）
- 对齐填充：JVM要求对象起始地址必须是8字节的整数倍（8字节对齐）

### 如何判断对象可以被回收

判断对象是否存活一般有两种方式：

-引用计数：每个对象有一个引用计数属性，新增一个引用时计数加1，引用释放时计数减1，计数为0时可以回收。此方法简单，无法解决对象相互循环引用的问题。
-可达性分析（Reachability Analysis）：从GC Roots开始向下搜索，搜索所走过的路径称为引用链。当一个对象到GC Roots没有任何引用链相连时，则证明此对象是不可用的，不可达对象。

### 一个对象可以被回收的条件

1，该对象没有与GC Roots相连

可作为GC Roots对象的包括如下几种：

a,虚拟机栈（栈帧中的本地变量表）中的引用的对象

b,方法区中的类静态属性引用的对象

c,方法区中的常量引用的对象

d,本地方法栈中JNI的引用的对象



2，该对象没有重写finalize()方法或finalize()已经被执行过则直接回收（第一次标记），否则对象会加入回收队列中并在这里执行finalize（）方法，进行第二次标记，如果该对象仍然应该被GC则GC，否则移除队列。（在finalize方法中，对象很可能和其他 GC Roots中的某一个对象建立了关联，finalize方法只会被调用一次，且不推荐使用finalize方法）

### 回收方法区

方法去回收价值低。
如何判断无用的类：
1，该类所有实例都被回收（JAVA堆中没有该类的对象）
2，加载该类的classLoader已经被回收
3，该类对应的java.lang.class对象没有任何地方被引用，无法在任何地方利用反射访问该类

### 永久代会被回收吗

-垃圾回收会发生在永久代，如果永久代满了或者是超过了临界值，会触发完全垃圾回收(Full GC)。
永久代的垃圾回收主要包括类型的卸载和废弃常量池的回收。

- 当没有对象引用一个常量的时候，该常量即可以被回收。
- 而类型的卸载更加复杂，有以下条件：

1，该类所有实例都被回收了

2，该类型的classLoader被回收了，

3，该类型对应的java.lang.class没有任何地方被引用，没有任何地方可以通过反射来访问一个对象。

### 垃圾收集算法

-标记-清除算法，分两步，第一步，先标出出所有需要回收的对象，第二步，在标记完成后统一回收掉所有被标记的对象
-复制算法：就是平时用的eden，surviver1和surviver2
-标记-压缩：分两步，第一步，先标出出所有需要回收的对象，第二步，把所有存活的对象都向一端移动，第三步，直接清理掉边界以外的内存（老年代回收算法）
分代收集算法：把Java堆分为新生代和老年代，这样就可以根据各个年代的特点采用最适当的收集算法。

#### 垃圾收集器

![1590404212(1)](C:\Users\好里好比\Documents\学习\我的笔记图片\1590404212(1).jpg)

### 对象分配规则

- 对象优先分配在Eden区，如果Eden区没有足够的空间时，虚拟机执行一次Minor GC。
- 大对象直接进入老年代（大对象是指需要大量连续内存空间的对象）。这样做的目的是避免在Eden区和两个Survivor区之间发生大量的内存拷贝（新生代采用复制算法收集内存）。
- 长期存活的对象进入老年代。虚拟机为每个对象定义了一个年龄计数器，如果对象经过了1次Minor GC那么对象会进入Survivor区，之后每经过一次Minor GC那么对象的年龄加1，知道达到阀值对象进入老年区。
- 动态判断对象的年龄。如果Survivor区中相同年龄的所有对象大小的总和大于Survivor空间的一半，年龄大于或等于该年龄的对象可以直接进入老年代。
- 老年代空间只有在新生代对象转入及创建大对象、大数组时才会出现不足的现象，当执行Full GC后空间仍然不足，则抛出如下错误：
  java.lang.OutOfMemoryError: Java heap space 
  为避免以上两种状况引起的Full GC，调优时应尽量做到让对象在Minor GC阶段被回收、让对象在新生代多存活一段时间及不要创建过大的对象及数组。

### finalize()方法工作原理

```
一旦垃圾回收器准备好释放对象占用的存储空间，将首先调用其 finalize() 方法（如果如果覆盖了finalize()），并且在下一次垃圾回收动作发生时，才会真正回收对象占用的内存。	
至于为什么在下一次垃圾回收动作发生时才会回收内存，原因是如果一个对象覆盖了 finalize() 方法，那么在真正被宣告死亡的时候，至少需要经过两次标记。第一次被标记的时候会被放在 一个 F-Queue 队列中，finalize() 方法是对象逃脱死亡命运的最后一次机会。在第二次标记的时候，如果该对象成功与引用链（GC-Roots）上的任何一个对象关联，那么它仍然可以存活下来，否则将会被垃圾收集器回收
```

### Java 8 的内存分代改进

```
在 jdk1.8 中对内存模型中方法区的实现永久代（Perm Gen space）进行了移除，取而代之的是元空间（Metaspace）。	
原因是在方法区中实现垃圾回收的条件比较苛刻，因此存在着内存溢出的风险。在 jdk1.8 之后，当方法区内存使用较多时，元空间会使用物理内存，减少了风险。
```

### 调优命令

Sun JDK监控和故障处理命令有jps jstat jmap jhat jstack jinfo

- jps，JVM Process Status Tool,显示指定系统内所有的HotSpot虚拟机进程。
- jstat，JVM statistics Monitoring是用于监视虚拟机运行时状态信息的命令，它可以显示出虚拟机进程中的类装载、内存、垃圾收集、JIT编译等运行数据。
- jmap，JVM Memory Map命令用于生成heap dump文件
- jhat，JVM Heap Analysis Tool命令是与jmap搭配使用，用来分析jmap生成的---dump，jhat内置了一个微型的HTTP/HTML服务器，生成dump的分析结果后，可以在浏览器中查看
- jstack，用于生成java虚拟机当前时刻的线程快照。
- jinfo，JVM Configuration info 这个命令作用是实时查看和调整虚拟机运行参数。

### jvm参数调优

- Xmx：堆内存最大限制。

设定新生代大小。 新生代不宜太小，否则会有大量对象涌入老年代

- -XX:NewSize：新生代大小
- -XX:NewRatio  新生代和老生代占比
- -XX:SurvivorRatio：伊甸园空间和幸存者空间的占比
- 设定垃圾回收器 年轻代用  -XX:+UseParNewGC  年老代用-XX:+UseConcMarkSweepGC

### 调优工具

JDK自带的有jconsole和jvisualvm，第三方有MAT，GChisto

- jconsole, java Monitoring and Management Console，Java监控和管理平台，用于堆JVM中内存，线程快和类的监控；
- jvisualvm, jdk自带全能工具，可以分析内存快照，线程快照；监控内存变化，GC变化
- MAT，Memory Analyzer Tool，一个基于Eclipse的内存分析工具，是一个快速、功能丰富的Java heap分析工具，它可以帮助我们查找内存泄漏和减少内存消耗
- GChisto，一款专业分析gc日志的工具

### 反射中，Class.forName() 和ClassLoader.loadClass()区别

```
加载：加载 class 的二进制流，将字节流存储结构转化为方法区运行的数据结构，生成一个 Class 对象作为这个类的访问入口
验证：保证 class 文件的字节流中包含的信息符合虚拟机的要求，比如文件格式验证，元数据验证等
准备：为类变量分配内存，并设置初始值，并非在堆中分配内存，而是在方法区
解析：将常量池中的符号引用替换为直接引用
初始化：也是类加载的最后一步，执行类构造器 clinit() 方法，按照要求初始化静态变量的值，并执行静态代码块
```

下面是 Class.forName() 和ClassLoader的区别

```
Class.forName() 默认执行类加载过程中的连接与初始化动作，一旦执行初始化动作，静态变量就会被初始化为程序员设置的值，如果有静态代码块，静态代码块也会被执行
ClassLoader.loadClass() 默认只执行类加载过程中的加载动作，后面的动作都不会执行    
比如我们连接 MySQL 数据库会用到的 Class.forName("com.mysql.cj.jdbc.Driver")，在类加载的过程中就会通过静态代码块注册一个驱动对象。
```

### 静态代理和动态代理

#### 描述动态代理的几种实现方式？分别说出相应的优缺点

代理可以分为 "静态代理" 和 "动态代理"，动态代理又分为 "JDK动态代理" 和 "CGLIB动态代理" 实现。

**静态代理**：代理对象和实际对象都继承了同一个接口，在代理对象中指向的是实际对象的实例，这样对外暴露的是代理对象而真正调用的是 Real Object

- **优点**：可以很好的保护实际对象的业务逻辑对外暴露，从而提高安全性。
- **缺点**：不同的接口要有不同的代理类实现，会很冗余

**JDK 动态代理**：

- 为了解决静态代理中，生成大量的代理类造成的冗余；

- JDK 动态代理只需要实现 InvocationHandler 接口，重写 invoke 方法便可以完成代理的实现，

- jdk的代理是利用反射生成代理类 Proxyxx.class 代理类字节码，并生成对象

- jdk动态代理之所以**只能代理接口**是因为**代理类本身已经extends了Proxy，而java是不允许多重继承的**，但是允许实现多个接口

- **优点**：解决了静态代理中冗余的代理实现类问题。

- **缺点**：JDK 动态代理是基于接口设计实现的，如果没有接口，会抛异常。

  实例：

  ```java
  public interface UserService {
      public void select();   
      public void update();
  }
  
  public class UserServiceImpl implements UserService {  
      public void select() {  
          System.out.println("查询 selectById");
      }
      public void update() {
          System.out.println("更新 update");
      }
  }
  
  import java.lang.reflect.InvocationHandler;
  import java.lang.reflect.Method;
  import java.util.Date;
  
  public class LogHandler implements InvocationHandler {
      Object target;  // 被代理的对象，实际的方法执行者
  
      public LogHandler(Object target) {
          this.target = target;
      }
      @Override
      public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
          before();
          Object result = method.invoke(target, args);  // 调用 target 的 method 方法
          after();
          return result;  // 返回方法的执行结果
      }
      // 调用invoke方法之前执行
      private void before() {
          System.out.println(String.format("log start time [%s] ", new Date()));
      }
      // 调用invoke方法之后执行
      private void after() {
          System.out.println(String.format("log end time [%s] ", new Date()));
      }
  }
  
  import proxy.UserService;
  import proxy.UserServiceImpl;
  import java.lang.reflect.InvocationHandler;
  import java.lang.reflect.Proxy;
  
  public class Client2 {
      public static void main(String[] args) throws IllegalAccessException, InstantiationException {
          // 设置变量可以保存动态代理类，默认名称以 $Proxy0 格式命名
          // System.getProperties().setProperty("sun.misc.ProxyGenerator.saveGeneratedFiles", "true");
          // 1. 创建被代理的对象，UserService接口的实现类
          UserServiceImpl userServiceImpl = new UserServiceImpl();
          // 2. 获取对应的 ClassLoader
          ClassLoader classLoader = userServiceImpl.getClass().getClassLoader();
          // 3. 获取所有接口的Class，这里的UserServiceImpl只实现了一个接口UserService，
          Class[] interfaces = userServiceImpl.getClass().getInterfaces();
          // 4. 创建一个将传给代理类的调用请求处理器，处理所有的代理对象上的方法调用
          //     这里创建的是一个自定义的日志处理器，须传入实际的执行对象 userServiceImpl
          InvocationHandler logHandler = new LogHandler(userServiceImpl);
          /*
  		   5.根据上面提供的信息，创建代理对象 在这个过程中，
                 a.JDK会通过根据传入的参数信息动态地在内存中创建和.class 文件等同的字节码
                 b.然后根据相应的字节码转换成对应的class，
                 c.然后调用newInstance()创建代理实例
  		 */
          UserService proxy = (UserService) Proxy.newProxyInstance(classLoader, interfaces, logHandler);
          // 调用代理的方法
          proxy.select();
          proxy.update();
          
          // 保存JDK动态代理生成的代理类，类名保存为 UserServiceProxy
          // ProxyUtils.generateClassFile(userServiceImpl.getClass(), "UserServiceProxy");
      }
  }
  
  
  
  ```

**CGLIB 代理**：

- 由于 JDK 动态代理限制了只能基于接口设计，而对于没有接口的情况，JDK方式解决不了；
- CGLib 采用了非常底层的字节码技术，其原理是通过字节码技术为一个类创建子类，并在子类中采用方法拦截的技术拦截所有父类方法的调用，顺势织入横切逻辑，来完成动态代理的实现。
- 实现方式实现 MethodInterceptor 接口，重写 intercept 方法，通过 Enhancer 类的回调方法来实现。
- 但是CGLib在创建代理对象时所花费的时间却比JDK多得多，所以对于单例的对象，因为无需频繁创建对象，用CGLib合适，反之，使用JDK方式要更为合适一些。
- 同时，由于CGLib由于是采用动态创建子类的方法，对于final方法，无法进行代理。
- **优点**：没有接口也能实现动态代理，而且采用字节码增强技术，性能也不错。
- **缺点**：技术实现相对难理解些。


### 说说强引用、软引用、弱引用、虚引用以及他们之间和 gc 的关系

```
强引用是指在代码中普遍存在的，类似 Object obj = new Object()； 这类的引用，只要强引用还存在，垃圾回收器永远不会回收掉引用的对象
软引用是用来描述一些还有用但并非是必要的对象。对于软引用着的对象，在系统将要发生内存溢出异常之前，将会把这类对象列进回收范围进行第二次的回收。如果这次回收仍然没有足够的内存，就会抛出内存溢出异常。在 jdk1.2 中提供了 SoftReference 类来实现软引用
弱引用也是用来描述非必须对象的，但是它的强度比软引用更弱一些，被弱引用关联的对象只能生存到下一次的垃圾回收之前。当垃圾收集器工作时，无论当前内存是否足够，都会回收掉只被弱引用关联的对象。在 jdk1.2 中提供了 WeakReference 类来实现弱引用
虚引用也被称为幽灵引用或幻影引用，它是最弱的一种引用关系。一个对象是否有虚引用的存在，完全不会对其生存时间造成影响，也无法通过虚引用来取得一个对象的实例。为一个对象设置虚引用关联的唯一目的就是能在这个对象被收集时收到一个系统通知。在 jdk1.2 中提供了 PhantomReference 类来实现虚引用。
```

![1580998609397](C:\Users\好里好比\AppData\Roaming\Typora\typora-user-images\1580998609397.png)





![1581866157445](C:\Users\好里好比\AppData\Roaming\Typora\typora-user-images\1581866157445.png)

### JVM 垃圾回收机制，何时触发 MinorGC 等操作

```
Minor GC 也被称为新生代 GC，指发生在新生代（PSYoungGen）的垃圾收集动作，新生代包括三块内存区域 eden 区，from （From Survivor）区 与 to（To Survivor） 区。对象优先在 eden 创建并区分配内存，当 eden 区内存无法为一个新对象分配内存时，就会触发 Minor GC。至于为什么把新生代分为 3 个区，主要是为了新生代复制算法的实现。
```

### 对象如何晋升到老年代

```
大对象直接进入老年代。比如很长的字符串，或者很大的数组等
长期存活的对象进入老年代。在堆中分配内存的对象，其内存布局的对象头中（Header）包含了 GC 分代年龄标记信息。如果对象在 eden 区出生，那么它的 GC 分代年龄会初始值为 1，每熬过一次 Minor GC 而不被回收，这个值就会增加 1 岁。当它的年龄到达一定的数值时（jdk1.7 默认是 15 岁），就会晋升到老年代中。
动态对象年龄判定。当 Survivor 空间中相同年龄所有对象的大小总和大于 Survivor 空间的一半，年龄大于或等于该年龄的对象就可以直接进入老年代，而不需要达到默认的分代年龄。
```

### 面试官：说说双亲委派模型

```
类加载阶段分为加载、连接、初始化三个阶段，而加载阶段需要通过类的全限定名来获取定义了此类的二进制字节流。
在Java中任意一个类都是由这个类本身和加载这个类的类加载器来确定这个类在JVM中的唯一性。
1、启动类加载器(Bootstrap ClassLoader),它是属于虚拟机自身的一部分，用C++实现的，主要负责加载<JAVA_HOME>\lib目录中或被-Xbootclasspath指定的路径中的并且文件名是被虚拟机识别的文件。它等于是所有类加载器的爸爸。
2、扩展类加载器(Extension ClassLoader),它是Java实现的，独立于虚拟机，主要负责加载<JAVA_HOME>\lib\ext目录中或被java.ext.dirs系统变量所指定的路径的类库。
3、应用程序类加载器(Application ClassLoader),它是Java实现的，独立于虚拟机。主要负责加载用户类路径(classPath)上的类库，如果我们没有实现自定义的类加载器那这玩意就是我们程序中的默认加载器。
双亲委派的意思是如果一个类加载器需要加载类，那么首先它会把这个类请求委派给父类加载器去完成，每一层都是如此。一直递归到顶层，当父加载器无法完成这个请求时，子类才会尝试去加载。这里的双亲其实就指的是父类，没有mother。父类也不是我们平日所说的那种继承关系，只是调用逻辑（调用父类的加载方法）。
双亲委派模型不是一种强制性约束，也就是你不这么做也不会报错怎样的，它是一种JAVA设计者推荐使用类加载器的方式。比如JDBC加载
```

### volatile 的语义，它修饰的变量一定线程安全吗

 	volatile 关键字的作用，一个是保证内存的可见性，还有防止指令重排序
 	被 volatile 关键字修饰的变量不是线程安全的，因为 volatile 不能保证原子性。再另外的说一句，被 synchronized 修饰的代码块具备原子性。

### Java内存模型中的happen-before是什么

```
线程内执行的每个操作，都保证happen-before后面的操作，这就保证了基本的程序顺序规则，这是开发者在书写程序时的基本约定。
对于volatile变量，对它的写操作，保证happen-before在随后对该变量的读取操作。
对于一个锁的解锁操作，保证happen-before加锁操作。
对象构建完成，保证happen-before于fnalizer的开始动作。
甚至是类似线程内部操作的完成，保证happen-before其他Thread.join()的线程等。
```

#### ClassLoader的种类

![19485102d2b4af4f6d7ebc5d71153b1](C:\Users\好里好比\Documents\学习\我的笔记图片\19485102d2b4af4f6d7ebc5d71153b1.png)





# JAVAWEB基础知识

### http和https的区别

与http相比，https先和SSL(SecureSockets Layer)通信，再由SSL和TCP通信，也就是说HTTPS使用了隧道进行通信。

HTTPS具有了加密(防窃听)，认证（防伪装）和完整性保护（防篡改），对称密钥加密，非对称密钥加密

### 认证

​		数字证书认证机构（CA，Certificate Authority）是客户端与服务器双方都可信赖的第三方机构。

​		服务器的运营人员向 CA 提出公开密钥的申请，CA 在判明提出申请者的身份之后，会对已申请的公开密钥做数字签名，然后分配这个已签名的公开密钥，并将该公开密钥放入公开密钥证书后绑定在一起。

