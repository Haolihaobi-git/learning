# 语法

# go语言特点

自带gc。

静态编译，编译好后，扔服务器直接运行。

简单的思想，没有继承，多态，类等。

丰富的库和详细的开发文档。

语法层支持并发，和拥有同步并发的channel类型，使并发开发变得非常方便。

简洁的语法，提高开发效率，同时提高代码的阅读性和可维护性。

超级简单的交叉编译，仅需更改环境变量。

Go 的速度也非常快，几乎和 C 或 C++ 程序一样快，且能够快速开发应用程序。

# 1，在go语言中，new和make的区别

new

- 函数定义：func new(Type) *Type,传递new函数的是一个类型，返回的是一个零值的指针

make

- 函数定义：func make(Type, size IntegerType) Type,入参是类型，长度，返回的是是类型，仅仅用于创建slice,map,channel

# 2 var和:=的区别

- :=不能再函数外使用，不能定义全局变量

- :=需要初始化，不能定义nil值，一定要初始化，var定义变量，可以初始化也可以不初始化

# 3 数组和切片的区别

数组

- 数组的长度是固定的，长度是数组类型的一部分，[3]int和[4]int是两种不同的数组类型

- 数组需要指定大小，不指定也会根据初始化的自动推算出大小，不可改变
- 数组是值传递，当作为方法的参数传入时姜复制一份数组而不是引用同一指针。

切片

- 切片的长度是可变的，是一种轻量级的数据结构，有三个属性，指针，长度，容量。
- 不需要指定大小
- 切片可以通过数组来初始化var one = []int{}，也可以通过内置函数make()初始化var one = make([]int.3)，在追加元素是如果容量不足时将len的2倍扩容，容量小于1000个时，总是成倍的增长，一旦容量超过1000个，增长因子设为1.25，也就是说每次会增加25%的容量。

# 4 解释以下命令的作用

- go env ：用于查看go的环境变量
- go run:  用于编译并运行go源文件
- go build：用于编译源码文件，代码包，依赖包
- go get：用于动态获取远程代码包
- go install:用于编译go文件，并将编译结构安装到bin,pkg目录
- go clean：#用于清理工作目录，删除编译和安装遗留的目标文件
- go version:用于查看go的版本信息

# go build和go install的区别

- go build：用于编译包生成可执行文件，必须有main包才可以
- go install: go install 的作用有两个：主要用来生成库和工具，(如果有main包)编译后生成的可执行工具文件放到 bin 目录、$GOPATH/bin，编译后的库文件放到 pkg 目录下（$GOPATH/pkg)

# go语言中的协程

- 协程和线程都可以实现程序的并发执行
- 通过channel来进行协程间的通信
- 关键字go并非执行并发任务，而是创建并发任务单元，真正执行的是逻辑处理器从任务单元列表里调用协程，线程运行。

# 通道的实现原理

- 通道的存储在heap堆，堆属于进程拥有，多个线程共同享有里面的。
- 通道的底层数据模型

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190307092928342.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA4NTMyNjE=,size_16,color_FFFFFF,t_70)

- 先获取全局锁，再往channel里添加或者获取元素，然后在释放锁

- 不通过共享内存去通信，而是通过通信（复制）实现共享内存

- 当我们使用make去创建一个channel的时候，实际上返回的是一个指向channel的pointer，所以我们能够在不同的function之间直接传递channel对象，而不用通过指向channel的指针

- 创建和管理协程都是通过Go的runtime，而不是通过OS的thread，但是Go的runtime调度执行goroutime确实基于OS的thread。

  

- 写入流程

  ```
  type hchan struct {
      qcount uint // 当前队列中剩余元素个数
      dataqsiz uint // 环形队列长度，即缓冲区的大小，即make（chan T，N），N.
      buf unsafe.Pointer // 环形队列指针
      elemsize uint16 // 每个元素的大小
      closed uint32 // 表示当前通道是否处于关闭状态。创建通道后，该字段设置为0，即通道打开; 通过调用close将其设置为1，通道关闭。
      elemtype *_type // 元素类型，用于数据传递过程中的赋值；
      sendx uint和recvx uint是环形缓冲区的状态字段，它指示缓冲区的当前索引 - 支持数组，它可以从中发送数据和接收数据。
      recvq waitq // 等待读消息的goroutine队列 recvq和sendq基本上是链表
      sendq waitq // 等待写消息的goroutine队列
      lock mutex // 互斥锁，为每个读写操作锁定通道，因为发送和接收必须是互斥操作。
  }
  ```

  

  ![img](https://raw.githubusercontent.com/guyan0319/golang_development_notes/master/images/9.9.5.png)

写入信息

1、锁定整个通道结构。

2、确定写入。尝试recvq从等待队列中等待goroutine，然后将元素直接写入goroutine。

3、如果recvq为Empty，则确定缓冲区是否可用。如果可用，从当前goroutine复制数据到缓冲区。

4、如果缓冲区已满，则要写入的元素将保存在当前正在执行的goroutine的结构中，并且当前goroutine将在sendq中排队并从运行时挂起。

5、写入完成释放锁

读取信息

1、先获取channel全局锁

2、尝试sendq从等待队列中获取等待的goroutine，

3、 如有等待的goroutine，没有缓冲区，取出goroutine并读取数据，然后唤醒这个goroutine，结束读取释放锁。

4、如有等待的goroutine，且有缓冲区（此时缓冲区已满），从缓冲区队首取出数据，再从sendq取出一个goroutine，将goroutine中的数据存入buf队尾，结束读取释放锁。

5、如没有等待的goroutine，且缓冲区有数据，直接读取缓冲区数据，结束读取释放锁。

6、如没有等待的goroutine，且没有缓冲区或缓冲区为空，将当前的goroutine加入recvq排队，进入睡眠，等待被写goroutine唤醒。结束读取释放锁。

# for 和 for range有什么区别?

主要是使用场景不同

for可以

遍历array和slice

遍历key为整型递增的map

遍历string

for range可以完成所有for可以做的事情，却能做到for不能做的，包括

遍历key为string类型的map并同时获取key和value

遍历channel

# range和select的区别

- range只能读取一个channal，没数据时会阻塞，select监听和channel有关的IO操作，可以读取多个channel，而且select可以有default默认值，不会阻塞。

# for循环

- for循环提供了一个更高级的break，可以选择中断哪一个循环，不支持以逗号为间隔的多个赋值语句

# switch

- 在case中明确添加fallthrough关键字，才会继续执行紧跟的下一个case

# int和unit的区别

```go
package main

import (
	"fmt"
	_ "time"
)

func main() {
	a := byte(255)  //11111111 这是byte的极限， 因为 a := byte(256)//越界报错， 0~255正好256个数，不能再高了
	b := uint8(255) //11111111 这是uint8的极限，因为 c := uint8(256)//越界报错，0~255正好256个数，不能再高了
	c := int8(127)  //01111111 这是int8的极限， 因为 b := int8(128)//越界报错， 0~127正好128个数，所以int8的极限只是256的一半
	d := int8(a)    //11111111 打印出来则是-0000001，int8(128)、int8(255)、int8(byte(255))都报错越界，因为int极限是127，但是却可以写：int8(a)，第一位拿来当符号了
	e := int8(c)    //01111111 打印出来还是01111111

	fmt.Printf("%08b %d \n", a, a)
	fmt.Printf("%08b %d \n", b, b)
	fmt.Printf("%08b %d \n", c, c)
	fmt.Printf("%08b %d \n", d, d)
	fmt.Printf("%08b %d \n", e, e)
}
```



# 循环控制Goto、Break、Continue

```
  1.三个语句都可以配合标签(label)使用
  2.标签名区分大小写，定以后若不使用会造成编译错误
  3.continue、break配合标签(label)可用于多层循环跳出
  4.goto是调整执行位置，与continue、break配合标签(label)的结果并不相同
```

# go语言中没有this

- 方法施加的对象显示传递，没有被隐藏起来

# go语言中的引用类型包含哪些

- 数组切片slice，字典，通道，接口

# go语言的同步锁

- sync.mutex,一个goroutine获得了MUtex后，其他goroutine就只能乖乖的等待
- RWMutex读写锁，读读不排斥，读写排斥，写写排斥

# 说一说go语言的channel特性

- A.给一个nil channel发送数据，造成永远阻塞
- B.从一个nil channel 接收数据，造成永远阻塞
- C.给一个已经关闭的channel发送数据，引起panic
- D.从一个已经关闭的channel接收数据，如果缓冲区为空，则返回一个零值
- E。无缓冲区的channel是同步的，而又缓冲区的channel是非同步的

# 说说go语言的beego框架

- A.beego是一个golang实现的轻量级http框架
- B.beego可以通过注释路由，正则路由等多种方式完成url路由注入
- C.可以使用bee new工具生成空工程，然后使用bee run命令自动热编译

# 说说go语言的goconvey框架



![1622704510(1)](D:\技术学习\笔记\学习\我的笔记图片\1622704510(1).jpg)

# 说说进程，线程，协程之间的区别

- 进程是资源的分配和调度的一个独立单元，而线程是cpu调度的基本单元
- 线程共享整个进程的资源（寄存器，堆栈，上下文），一个进程至少包含一个线程
- 线程执行时一般都要进行同步互斥，因为他们共享同一进程的所有资源
- 协程的本质是就是使用当前进程在不同的函数代码中切换执行，可以理解为并行
- 不同协程的模型实现可能是单线程，也可能是多线程
- 线程拥有自己独立的栈和共享的堆，共享堆，不共享栈，进程有操作系统调度。（全局变量保存在堆中，局部变量级函数保存在栈中）
- 协程和线程一样共享堆，不共享栈
- 一个进程一般有一个主线程和多个辅助线程，线程里面可以开启多个协程，
- 协程避免了无意义的调度，由此可以提高性能，但也因此程序员必须自己承担调度的责任，同时，协程也失去了标准线程使用多cpu的能力，完全由程序（用户态）控制，不会像线程切换那样消耗资源
- 和多线程比，线程数量越多，协程的性能优势就越明显；

# 指针接收和值接收方法的区别

- 指针接收：指针类型的接收者由一个结构体的指针组成，由于指针的特性，调用方法时修改接收者指针的任意成员变量，在方法结束后，修改都是有效的

- 值接收：当方法作用于值类型接收者时，Go语言会在代码运行时将接收者的值复制一份。在值类型接收者的方法中可以获取接收者的成员值，但修改操作只是针对副本，无法修改接收者变量本身。

- 区别：

  值类型实现接口的方法，指针和值都实现了

  指针类型实现接口的方法，只有指针实现了，值类型没有实现

  

  # 闭包的定义

  闭包=函数+引用环境

  # recover使用注意事项

- defer必须放在panic之前定义，另外recover只有在defer调用的函数中




# defer和return的执行顺序

- defer先执行，最后再return

# 指针接收方法和值接收方法的区别

指针接收方法，实现接口的方法，值类型没有实现接口方法

值类型接收方法，实现接口的方法，指针类型实现接口的方法



# 指针

- A. 给一个 nil channel 发送数据，造成永远阻塞
- B. 从一个 nil channel 接收数据，造成永远阻塞
- C. 给一个已经关闭的 channel 发送数据，引起 panic
- D. 从一个已经关闭的 channel 接收数据，如果缓冲区中为空，则返回一个零值

### unitptr和unsafe.Pointer的区别

- unsafe.Pointer只是单纯的通用指针类型，用于转换不同类型指针，它不可以参与指针运算；
- 而uintptr是用于指针运算的，GC 不把 uintptr 当指针，也就是说 uintptr 无法持有对象， uintptr 类型的目标会被回收；
- unsafe.Pointer 可以和 普通指针 进行相互转换；
- unsafe.Pointer 可以和 uintptr 进行相互转换。

# 知识点

- 知识点：常量。常量不同于变量的在运行期分配内存，常量通常会被编译器在预处理阶段直接展开，作为指令数据使用，所以常量无法寻址
- go的defer函数，是以协程为单位，不是以函数为单位，协程退出或异常，所有函数都会执行一遍defer



  