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