## 头条-视频方向android职位面试题
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
- 先说`强引用`，创建一个对象，并正常使用，这种就属于强引用。如果一个对象持具有强引用，那么GC绝对不会回收它。当内存空间不足时，JVM宁愿抛出OutOfMemoryError错误，也不会轻易回收强引用。
- `软引用SoftReference`，通过例如以下方式进行创建    

      String str=new String("abc");                                     // 强引用
      SoftReference<String> softRef=new SoftReference<String>(str);     // 软引用     

&ensp;&ensp;&ensp;&ensp;系统触发GC时，如果内存足够，则不会回收弱引用，只有内存不足时才会对弱引用执行回收。
- `弱引用WeakReference`，创建方式与SoftReference类似，与SoftReference的区别在于，系统触发GC时，只要发现有弱引用，不管内存是否足够，都会对其进行回收。    
- ``虚引用``，顾名思义，就是形同虚设。与其他几种引用都不同，虚引用并不会决定对象的生命周期。如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收器回收。

&ensp;&ensp;&ensp;&ensp;虚引用主要用来跟踪对象被垃圾回收器回收的活动。虚引用与软引用和弱引用的一个区别在于：虚引用必须和引用队列(ReferenceQueue)联合使用。当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之关联的引用队列中。

    String str = new String("abc");
    ReferenceQueue queue = new ReferenceQueue();
    // 创建虚引用，要求必须与一个引用队列关联
    PhantomReference pr = new PhantomReference(str, queue);
&ensp;&ensp;&ensp;&ensp;复制代码程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要进行垃圾回收。如果程序发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象的内存被回收之前采取必要的行动。


**4. java中的this编译时的原理**  
**5. final变量用反射修改**

    field = Comtest.class.getDeclaredField("value1");
    field.setAccessible(true);
    Field modifiersField = Field.class.getDeclaredField("modifiers");
    modifiersField.setAccessible(true);
    modifiersField.setInt(field,field.getModifiers()&~Modifier.FINAL);
    field.set(null, new char[]{'1', '2', '3'});


**6. HashMap的内部结构，给定一个key，如何找到对应的value，使用equal**  
- 首先判断Key是否为Null，如果为null，直接查找Enrty[0]，如果不是Null，先计算Key的HashCode，然后经过二次Hash，得到Hash值。  
- 根据Hash值，对Entry[]的长度length求余，得到的就是Entry数组的index。  
- 根据对应的索引找到对应的数组，就是找到了其所在的链表，然后按照链表的操作对Value进行插入、删除和查询操作  

**7. volatile**  
**8. Java线程池有什么作用**  
**9. Java动态代理**  
- [10分钟带你了解动态代理](https://juejin.im/post/5a99048a6fb9a028d5668e62)    
- 动态代理的核心知识点：利用Java的反射机制，为你想要代理的类动态创建一个代理类（Proxy），而该代理类真正在执行代理方法时，是交给InvocationHandler类的invoke方法实现的，你可以自由实现invoke方法。在该方法参数里有动态代理类本身、代理类实现的接口列表、动态代理类执行的代理方法（Method），因此你可以在这个接口里自行选择代理某个方法（根据方法名），在执行这个方法的前后添加自己的逻辑，也可以修改该方法的返回值和传入参数来达到面向切面（AOP）编程的目的。    

**10. handler机制**  
- Handler的三大核心类，Looper，Message，MessageQueue     
- 每个Looper持有一个MessageQueue，负责无限循环的从MessageQueue中取出一个Message进行处理，如果MessageQueue中没有Message，则进入阻塞？状态
- Message中持有一个Handler对象，Looper每次取出Message处理时，实际上就是执行这个Handler对象的dispatchMessage方法
- 每个线程只有一个Looper，Looper通过调用Looper.prepare方法进行创建并存入ThreadLocal中，然后调用loop方法让它进入循环
- 获取主线程的Looper可通过Looper.getMainLooper方法获取    

**11. android跨进程通信的方式**  
- [知乎高赞回答：为什么Android使用Binder机制来实现IPC呢？](https://zhuanlan.zhihu.com/p/116440847)
- Android中跨进程通信的方式有：通过Intent启动Activity、发送Broadcast，通过ContentProvider共享，通过SD卡共享，通过Messanger通信，通过开启本地端口然后简历Socket通信，以及aidl
- Intent、ContentProvider、Messanger、aidl底层都是通过Binder实现
- Binder是Android特有的进程间通信机制，它通过Binder驱动实现，相比Socket有更好的安全性与速度
- Binder相对Socket（从A用户空间拷贝到内核空间，再从内核空间拷贝到B用户空间）少一次内存拷贝，Binder利用内存映射的方式，将B用户空间和内核空间映射起来，拷贝的时候是从A用户空间拷贝到内核空间，实现一次拷贝
- Binder相对Socket更安全的机制是Android为每个进程分配了一个UID，这个UID是完全由系统分配，在系统层就决定了哪个UID的进程能和当前进程通信。而不像Socket，端口是暴露的，任何进程都可以尝试建立连接。
- 首先，一个进程使用 BINDER_SET_CONTEXT_MGR 命令通过 Binder 驱动将自己注册成为 ServiceManager；    
Server 通过驱动向 ServiceManager 中注册 Binder（Server 中的 Binder 实体），表明可以对外提供服务。驱动为这个 Binder 创建位于内核中的实体节点以及 ServiceManager 对实体的引用，将名字以及新建的引用打包传给 ServiceManager，ServiceManger 将其填入查找表。     
Client 通过名字，在 Binder 驱动的帮助下从 ServiceManager 中获取到对 Binder 实体的引用，通过这个引用就能实现和 Server 进程的通信。


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
