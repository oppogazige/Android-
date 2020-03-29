## Android面试相关的知识汇总
### github上好的Android面试知识项目
[Android高级工程师面试必备](https://github.com/yangkun19921001/Blog/blob/master/%E7%AC%94%E8%AF%95%E9%9D%A2%E8%AF%95/Android%E9%AB%98%E7%BA%A7%E5%B7%A5%E7%A8%8B%E5%B8%88%E9%9D%A2%E8%AF%95%E5%BF%85%E5%A4%87/README.md)    
[stormzhang的Android面试指南](https://github.com/stormzhang/android-interview-questions-cn)

### 我自己整理的Android面试知识
#### 抖音直播Android
**1、超清图片解码，如何局部解码**    
**2、摄像头采集以及渲染**    
**3、java弱应用原理应用场景**    
java中存在四种引用机制，分别是强引用，软引用，弱引用，虚引用

强引用
一般情况下我们用new方式创建的引用就是强引用，比如

    Client client = new Client()
jvm进行GC的时候是不会回收存在强引用的对象的，比如

    Server server = new Server()
    Client client = new Client()
当在第二行时jvm内存耗尽，jvm会报内存溢出的错误，也不会去回收第一行的对象

只有当存在强引用的方法块执行完毕或者手动将强引用设置为null，这样才有可能被垃圾回收器回收掉

软引用
如果一个对象具有软引用，内存空间足够，垃圾回收器就不会回收它；

如果内存空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。

软引用可用来实现内存敏感的高速缓存,比如网页缓存、图片缓存等。使用软引用能防止内存泄露，增强程序的健壮性。

我们使用SoftReference实例来保存一个对象的软引用，比如

    MyObject aRef = new  MyObject();  
    SoftReference aSoftRef=new SoftReference(aRef);  
aSoftRef就是aRef的软引用，如果我们将

    aRef==null
此时当内存不足jvm进行GC时就会将aSoftRef清除掉，使用

    MyObject anotherRef=(MyObject)aSoftRef.get();  
会重新获得强引用对象，如果软引用对象已经被GC清楚，则返回null，我们可以利用软引用实现一些缓存功能

当第一次从数据库加载数据的时候将对象设置一个软引用，当对象使用完毕，GC还没有清理的时候再次加载对象时，

可以先从软引用队列查找是否存在跟对象相关的软应用，如果存在则调用get方法获取该对象的强引用，否则再去数据库加载

说到软引用队列ReferenceQueue，往往是结合软引用使用，当软引用的对象被清除以后往往软引用本身的引用也就没有存在的价值了，此时需要

一个场景来记录所以软引用，然后轮询这清除这些软引用，此时会选择将软引用加入软引用队列，然后如果软引用的get方法返回Null时则清除改软引用

弱引用
弱引用与软引用相似，不同的时jvm在GC时会直接将存在弱引用的对象清除，而不是在内存不足时才开始清除，所以相比软引用被GC的几率更大

虚引用
需引用往往是发生在finalize之后，与前几种引用不同的是，具有虚引用的对象是无法转换为强引用被调用的，它更像是一个finalize方法的记录，可以通过查找虚引用判断对象是否要被GC回收，以及相关资源是否在finalize的时候被关闭等
————————————————
版权声明：本文为CSDN博主「烟火HL」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/sinat_22594643/article/details/80520712    
**4、浏览器内核排版渲染流程**    
**5、css z-index实现原理**    
**6、c/c++实现内存管理方式**    
[《C/C++内存管理详解》](https://chenqx.github.io/2014/09/25/Cpp-Memory-Management/)    
**7、Android消息队列原理，loop和handler的关系**    
[《Handler的原理(Looper，Handler，Message三者的关系)》](https://blog.csdn.net/lowprofile_coding/article/details/72580044)    
**8、线程锁，哪些场景使用到了CAS**    
[《不可不说的JAVA“琐事”》](https://tech.meituan.com/2018/11/15/java-lock.html)    

线程锁中我们常聊到的乐观锁和悲观锁，悲观锁的意思就是线程认为自己要访问的同步资源随时可能被其他线程修改，所以在访问前就对资源进行加锁，以确保访问时只有自己能够修改资源。乐观锁则其实是使用无锁的编程来实现，最常采用的是CAS算法，及比较与交换。Java中java.util.concurrent包里的原子类就是通过CAS来实现的乐观锁，比如AtomicInteger类。

自旋锁是指在线程访问同步资源时，在已经获取到CPU占用的情况下，如果同步资源被其他线程锁定，此时不马上释放CPU占用，而是通过自循环来短暂占用CPU一段时间，然后再查看同步资源是否释放。自旋锁比较适合使用在同步资源访问耗时很短的场景，通过自我循环减少了CPU频繁切换线程带来的损耗。

sleep()方法是Thread的静态方法，而wait是Object实例方法
wait()方法必须要在同步方法或者同步块中调用，也就是必须已经获得对象锁。而sleep()方法没有这个限制可以在任何地方种使用。另外，wait()方法会释放占有的对象锁，使得该线程进入等待池中，等待下一次获取资源。而sleep()方法只是会让出CPU并不会释放掉对象锁；
sleep()方法在休眠时间达到后如果再次获得CPU时间片就会继续执行，而wait()方法必须等待Object.notift/Object.notifyAll通知后，才会离开等待池，并且再次获得CPU时间片才会继续执行



##### 二面面试题目：
**1、Android查询资源文件layout原理**    
**2、设计一个decode bitmap方法**    
**3、启动Activity A后，按home键，再从桌面启动activity A ， Activity A的生命周期**    
**4、handler原理**    
**5、给定一个数组n, K位旋转算法。     如数组1、2、3、4，1位旋转结果为4、1、2、3， 2位旋转为3、4、1、2**    
##### 三面面试题：
**Activity启动流程**    
**Android app签名原理**    







#### 头条-视频方向android职位面试题
>头条面试一般3-4轮，绝大多数是三轮。
第一轮是基础面试，第二轮是更高级别的人给你面试，第三轮是部门负责人，第四轮有些人有有些人没有，
算法只在面试中穿插有问几道，至少我当时备被问的都是无比基础的问题，
大体如下:    

**1. 数组实现队列**   


    class ArrayQueue {
        private int[] arrInt;// 内置数组
        private int front;// 头指针
        private int rear;// 尾指针
        public ArrayQueue(int size) {
            this.arrInt = new int[size];
            front = 0;
            rear = -1;
        }
        /**
        * 判断队列是否为空
        *
        * @return
        */
        public boolean isEmpty() {
            return front == arrInt.length;
        }
        /**
        * 判断队列是否已满
        *
        * @return
        */
        public boolean isFull() {
            return arrInt.length - 1 == rear;
        }
        /**
        * 向队列的队尾插入一个元素
        */
        public void insert(int item) {
            if (isFull()) {
                throw new RuntimeException("队列已满");
            }
            arrInt[++rear] = item;
        }
        /**
        * 获得对头元素
        *
        * @return
        */
        public int peekFront() {
            return arrInt[front];
        }
        /**
        * 获得队尾元素
        *
        * @return
        */
        public int peekRear() {
            return arrInt[rear];
        }
        /**
        * 从队列的对头移除一个元素
        *
        * @return
        */
        public int remove() {
            if (isEmpty()) {
            throw new RuntimeException("队列为空");
            }
            return arrInt[front++];
        }
    }    

**2. gc的流程**    
[《java深入篇之GC》](https://www.jianshu.com/p/b4760ef4b07f)    
**3. java软引用与弱引用区别**    
**4. java中的this编译时的原理**    
**5. final变量用反射修改**    

    field = Comtest.class.getDeclaredField("value1");
    field.setAccessible(true);
    Field modifiersField = Field.class.getDeclaredField("modifiers");
    modifiersField.setAccessible(true);
    modifiersField.setInt(field,field.getModifiers()&~Modifier.FINAL);
    field.set(null, new char[]{'1', '2', '3'});


**6. HashMap的内部结构，给定一个key，如何找到对应的value，使用equal**   
1、首先判断Key是否为Null，如果为null，直接查找Enrty[0]，如果不是Null，先计算Key的HashCode，然后经过二次Hash，得到Hash值。    
2、根据Hash值，对Entry[]的长度length求余，得到的就是Entry数组的index。    
3、根据对应的索引找到对应的数组，就是找到了其所在的链表，然后按照链表的操作对Value进行插入、删除和查询操作    
**7. volatile**    
**8. Java线程池有什么作用**    
**9. Java动态代理**    
**10. handler机制**    
**11. android跨进程通信的方式**    
**12.自定义控件方式**    
**13.Canvas绘制过什么 手写功能**    
**14.断点续传的实现**    
**15.如何设计图片加载库**    
**16.有看过哪些安卓的源码 View树绘制 事件分发 Activity启动 handler**    
**17.看过哪些开源项目 eventbus volley 图片 线程池管理 插件化 资源加载**    
**18.app 启动速度的优化做过哪些**    
**19.fresco加载图片原理 优势是什么**    
**20.写程序时，堆和栈有什么优化点 内存回收时机 如何判断对象可被回收,引用计数法和gc root法**    
**21. 事件分发 cancel事件一般在什么时候被触发**    
**22. touchdelagate**     **一个父view只能设置一个delegate，如何解决设置多个**
**23. App整个架构了解么**    
**24.mvvm data binding**    
**25. webview**    
**26.fragment startactivity**   
**27. 动画的原理**    
**28. 黄油计划 vsync**     
**29. 职业规划 做些有挑战的事儿 有意义的事儿 缺乏实战场景**    
**30.设计一个离线视频下载功能**   
