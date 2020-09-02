### 1.嵌入式系统中经常要用到无限循环，如何用C编写死循环

答：while(1){}或者for(;;)



### 2.程序的局部变量存在于哪里，全局变量存在于哪里，动态申请数据存在于哪里。

答：程序的局部变量存在于栈区；全局变量存在于静态区；动态申请数据存在于堆区。



### 3.关键字const有什么含义？

答：1)只读。2）使用关键字const也许能产生更紧凑的代码。3）使编译器很自然地保护那些不希望被改变的参数，防止其被无意的代码修改。



### 4.请问以下代码有什么问题：
```c
int main() {

char a;

char *str=&a;

strcpy(str,"hello");

printf(str);

return 0;

}
```
答：没有为str分配内存空间，将会发生异常，问题出在将一个字符串复制进一个字符变量指针所指地址。虽然可以正确输出结果，但因为越界进行内在读写而导致程序崩溃。



### 5.已知一个数组table，用一个宏定义，求出数据的元素个数

答： ``` 
    #define NTBL (sizeof(table)/sizeof(table[0]))
    ```



### 6.写一个"标准"宏MIN ，这个宏输入两个参数并返回较小的一个。

答：```
    #define MIN(A,B) （（A） <= (B) ? (A) : (B))
    ```

考点：

1) 标识#define在宏中应用的基本知识。这是很重要的。因为在 嵌入(inline)操作符 变为标准C的一部分之前，宏是方便产生嵌入代码的唯一方法，对于嵌入式系统来说，为了能达到要求的性能，嵌入代码经常是必须的方法。

2) 三重条件操作符的知识。这个操作符存在C语言中的原因是它使得编译器能产生比if-then-else更优的代码，了解这个用法是很重要的。

3) 懂得在宏中小心地把参数用括号括起来。



### 7. do……while和while有什么区别？

答：前一个循环一遍再判断，后一个判断以后再循环。



### 8.什么是预编译，何时需要预编译？

答：

１、总是使用不经常改动的大型代码体。

２、程序由多个模块组成，所有模块都使用一组标准的包含文件和相同的编译选项。在这种情况下，可以将所有包含文件预编译为一个预编译头。

预编译指令指示了在程序正式编译前就由编译器进行的操作，可以放在程序中的任何位置。



### 9.一个32位的机器,该机器的指针是多少位？

答：指针是多少位只要看地址总线的位数就行了。80386以后的机子都是32的数据总线。所以指针的位数就是4个字节了。



### 10.局部变量能否和全局变量重名？

答：能，局部会屏蔽全局。

局部变量可以与全局变量同名，在函数内引用这个变量时，会用到同名的局部变量，而不会用到全局变量。

对于有些编译器而言，在同一个函数内可以定义多个同名的局部变量，比如在两个循环体内都定义一个同名的局部变量，而那个局部变量的作用域就在那个循环体内。



### 11.引用与指针有什么区别？

答：

1) 引用必须被初始化，指针不必。

2) 引用初始化以后不能被改变，指针可以改变所指的对象。

3) 不存在指向空值的引用，但是存在指向空值的指针。



### 12.关键字static的作用是什么？

答：在C语言中，关键字static有三个明显的作用：

1) 在函数体，一个被声明为静态的变量在这一函数被调用过程中维持其值不变。

2) 在模块内（但在函数体外），一个被声明为静态的变量可以被模块内所用函数访问，但不能被模块外其它函数访问。它是一个本地的全局变量。

3) 在模块内，一个被声明为静态的函数只可被这一模块内的其它函数调用。那就是，这个函数被限制在声明它的模块的本地范围内使用。



### 13.static全局变量与普通的全局变量有什么区别？static函数与普通函数有什么区别？

答：全局变量(外部变量)的说明之前再冠以static 就构成了静态的全局变量。

全局变量本身就是静态存储方式，静态全局变量当然也是静态存储方式。这两者在存储方式上并无不同。

这两者的区别虽在于非静态全局变量的作用域是整个源程序， 当一个源程序由多个源文件组成时，非静态的全局变量在各个源文件中都是有效的。而静态全局变量则限制了其作用域，即只在定义该变量的源文件内有效， 在同一源程序的其它源文件中不能使用它。

由于静态全局变量的作用域局限于一个源文件内，只能为该源文件内的函数公用，因此可以避免在其它源文件中引起错误。

从以上分析可以看出，把局部变量改变为静态变量后是改变了它的存储方式即改变了它的生存期。把全局变量改变为静态变量后是改变了它的作用域，限制了它的使用范围。

static函数与普通函数作用域不同。仅在本文件。只在当前源文件中使用的函数应该说明为内部函数(static)，内部函数应该在当前源文件中说明和定义。

对于可在当前源文件以外使用的函数，应该在一个头文件中说明，要使用这些函数的源文件要包含这个头文件。



### 14.进程之间通信的途径有哪些？

答：进程间通信主要通过管道、消息、信号等途径进行。

1、无名管道( pipe )：管道是一种半双工的通信方式，数据只能单向流动，而且只能在具有亲缘关系的进程间使用。进程的亲缘关系通常是指父子进程关系。

2、高级管道(popen)：将另一个程序当做一个新的进程在当前程序进程中启动，则它算是当前程序的子进程，这种方式我们成为高级管道方式。

3、有名管道 (named pipe) ：有名管道也是半双工的通信方式，但是它允许无亲缘关系进程间的通信。

4、消息队列( message queue ) ：消息队列是由消息的链表，存放在内核中并由消息队列标识符标识。消息队列克服了信号传递信息少、管道只能承载无格式字节流以及缓冲区大小受限等缺点。

5、信号量( semophore ) ：信号量是一个计数器，可以用来控制多个进程对共享资源的访问。它常作为一种锁机制，防止某进程正在访问共享资源时，其他进程也访问该资源。因此，主要作为进程间以及同一进程内不同线程之间的同步手段。

6、信号 ( sinal ) ：信号是一种比较复杂的通信方式，用于通知接收进程某个事件已经发生。

7、共享内存( shared memory ) ：共享内存就是映射一段能被其他进程所访问的内存，这段共享内存由一个进程创建，但多个进程都可以访问。共享内存是最快的 IPC 方式，它是针对其他进程间通信方式运行效率低而专门设计的。它往往与其他通信机制，如信号两，配合使用，来实现进程间的同步和通信。

8、套接字( socket ) ：套解口也是一种进程间通信机制，与其他通信机制不同的是，它可用于不同机器间的进程通信。



### 15.产生死锁的原因是什么？

答：多个并发进程因争夺系统资源而产生相互等待的现象。即：一组进程中的每个进程都在等待某个事件发生，而只有这组进程中的其他进程才能触发该事件，这就称这组进程发生了死锁。

产生死锁的本质原因为：

1）、系统资源有限。

2）、进程推进顺序不合理。



### 16.死锁的4个必要条件

答：

1、互斥：某种资源一次只允许一个进程访问，即该资源一旦分配给某个进程，其他进程就不能再访问，直到该进程访问结束。

2、占有且等待：一个进程本身占有资源（一种或多种），同时还有资源未得到满足，正在等待其他进程释放该资源。

3、不可抢占：别人已经占有了某项资源，你不能因为自己也需要该资源，就去把别人的资源抢过来。

4、循环等待：存在一个进程链，使得每个进程都占有下一个进程所需的至少一种资源。

当以上四个条件均满足，必然会造成死锁，发生死锁的进程无法进行下去，它们所持有的资源也无法释放。这样会导致CPU的吞吐量下降。所以死锁情况是会浪费系统资源和影响计算机的使用性能的。那么，解决死锁问题就是相当有必要的了。



### 17.死锁的处理方式有哪些？

答：死锁的处理方式主要从预防死锁、避免死锁、检测与解除死锁这四个方面来进行处理。

预防死锁：

1、资源一次性分配：（破坏请求和保持条件）

2、可剥夺资源：即当某进程新的资源未满足时，释放已占有的资源（破坏不可剥夺条件）

3、资源有序分配法：系统给每类资源赋予一个编号，每一个进程按编号递增的顺序请求资源，释放则相反（破坏环路等待条件）

避免死锁:

预防死锁的几种策略，会严重地损害系统性能。因此在避免死锁时，要施加较弱的限制，从而获得 较满意的系统性能。由于在避免死锁的策略中，允许进程动态地申请资源。因而，系统在进行资源分配之前预先计算资源分配的安全性。若此次分配不会导致系统进入不安全状态，则将资源分配给进程；否则，进程等待。其中最具有代表性的避免死锁算法是银行家算法。

检测死锁：

首先为每个进程和每个资源指定一个唯一的号码；

然后建立资源分配表和进程等待表

解除死锁：

当发现有进程死锁后，便应立即把它从死锁状态中解脱出来，常采用的方法有：

1、剥夺资源：从其它进程剥夺足够数量的资源给死锁进程，以解除死锁状态；

2、撤消进程：可以直接撤消死锁进程或撤消代价最小的进程，直至有足够的资源可用，死锁状态.消除为止；所谓代价是指优先级、运行代价、进程的重要性和价值等。



### 18.进程和线程有什么区别？

答：进程是并发执行的程序在执行过程中分配和管理资源的基本单位。线程是进程的一个执行单元，是比进程还要小的独立运行的基本单位。一个程序至少有一个进程，一个进程至少有一个线程。两者的区别主要有以下几个方面：

1. 进程是资源分配的最小单位。

2. 线程是程序执行的最小单位，也是处理器调度的基本单位，但进程不是，两者均可并发执行。

3. 进程有自己的独立地址空间，每启动一个进程，系统就会为它分配地址空间，建立数据表来维护代码段、堆栈段和数据段，这种操作非常昂贵。而线程是共享进程中的数据，使用相同的地址空间，因此，CPU切换一个线程的花费远比进程小很多，同时创建一个线程的开销也比进程小很多。

4. 线程之间的通信更方便，同一进程下的线程共享全局变量、静态变量等数据，而进程之间的通信需要以通信的方式（IPC)进行。不过如何处理好同步与互斥是编写多线程程序的难点。但是多进程程序更健壮，多线程程序只要有一个线程死掉，整个进程也跟着死掉了，而一个进程死掉并不会对另外一个进程造成影响，因为进程有自己独立的地址空间。

5. 进程切换时，消耗的资源大，效率低。所以涉及到频繁的切换时，使用线程要好于进程。同样如果要求同时进行并且又要共享某些变量的并发操作，只能用线程不能用进程。

6. 执行过程：每个独立的进程有一个程序运行的入口、顺序执行序列和程序入口。但是线程不能独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制。

优缺点：

线程执行开销小，但是不利于资源的管理和保护。线程适合在SMP机器（双CPU系统）上运行。

进程执行开销大，但是能够很好的进行资源管理和保护，可以跨机器迁移。

何时使用多进程，何时使用多线程？

对资源的管理和保护要求高，不限制开销和效率时，使用多进程。

要求效率高，频繁切换时，资源的保护管理要求不是很高时，使用多线程。



### 19. 线程是否具有相同的堆栈?

答：真正的程序执行都是线程来完成的，程序启动的时候操作系统就帮你创建了一个主线程。

每个线程有自己的堆栈。



### 20.TCP与UDP有啥区别？

答：TCP和UDP是OSI模型中的运输层中的协议。TCP提供可靠的通信传输，而UDP则常被用于广播和细节控制交给应用的通信传输，两者主要的不同体现在一下几个方面：

1、TCP面向连接（如打电话要先拨号建立连接）;UDP是无连接的，即发送数据之前不需要建立连接

2、TCP提供可靠的服务。它通过校验和，丢包时的重传控制，序号标识，滑动窗口、确认应答，次序乱掉的分包进行顺序控制实现可靠传输。即通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达; UDP尽最大努力交付，即不保证可靠交付。

3、UDP具有较好的实时性，工作效率比TCP高，适用于对高速传输和实时性有较高要求的通信或广播通信场景。

4、每一条TCP连接只能是点到点的; UDP支持一对一，一对多，多对一和多对多的交互通信方式。

5、TCP对系统资源要求较多，UDP对系统资源要求较少。

UDP有时比TCP更有优势:

UDP以其简单、传输快的优势，在越来越多场景下取代了TCP, 如实时游戏。

（1）网速的提升给UDP的稳定性提供可靠网络保障，丢包率很低，如果使用应用层重传，能够确保传输的可靠性。

（2）TCP为了实现网络通信的可靠性，使用了复杂的拥塞控制算法，建立了繁琐的握手过程，由于TCP在内置的系统协议栈中，极难对其进行改进。

采用TCP，一旦发生丢包，TCP会将后续的包缓存起来，等前面的包重传并接收到后再继续发送，延时会越来越大。

基于UDP对实时性要求较为严格的情况下，采用自定义重传机制，能够把丢包产生的延迟降到最低，尽量减少网络问题造成的影响。