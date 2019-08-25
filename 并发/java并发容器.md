- **_ConcurrentHashMap_**
1. 线程不安全的HashMap：在多线程环境下，使用HashMap进行put操作会引起死循环，导致CPU利用率接近100%，所
   以在并发情况下不能使用HashMap。是因为多线程会导致HashMap的Entry链表形成环形数据结构，一旦形成环形数
   据结构，Entry的next节点永远不为空，就会产生死循环获取Entry。
```java

final HashMap<String, String> map = new HashMap<String, String>(2);
Thread t = new Thread(new Runnable() {
    @Override
    public void run() {
        for (int i = 0; i < 10000; i++) {
            new Thread(new Runnable() {
                @Override
                public void run() {
                    map.put(UUID.randomUUID().toString(), "");
                }
            }, "ftf" + i).start();
        }
    }
}, "ftf");
t.start();
t.join();

```
3. 效率低下的HashTable：
   HashTable容器使用synchronized来保证线程安全，但在线程竞争激烈的情况下HashTable
   的效率非常低下。因为当一个线程访问HashTable的同步方法，其他线程也访问HashTable的同
   步方法时，会进入阻塞或轮询状态。如线程1使用put进行元素添加，线程2不但不能使用put方
   法添加元素，也不能使用get方法来获取元素，所以竞争越激烈效率越低。
2. ConcurrentHashMap使用锁分段技术可有效提升并发访问率，首先将数据分成一段一段地存
    储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据的时候，其他段的数
    据也能被其他线程访问。
3. ConcurrentHashMap是由Segment数组结构和HashEntry数组结构组成。Segment是一种可重
   入锁（ReentrantLock），在ConcurrentHashMap里扮演锁的角色；HashEntry则用于存储键值对数
   据。一个ConcurrentHashMap里包含一个Segment数组。Segment的结构和HashMap类似，是一种
   数组和链表结构。一个Segment里包含一个HashEntry数组，每个HashEntry是一个链表结构的元
   素，每个Segment守护着一个HashEntry数组里的元素，当对HashEntry数组的数据进行修改时，
   必须首先获得与它对应的Segment锁，
4. **_扩容_**：Segment扩容是先判断元素是先判断是否需要扩容，再插入元素。在扩容的时候，首先会创建一个容量是原来容量两倍
的数组，然后将原数组的元素进行再散列后插入到新的数组里。为了高效，ConcurrentHashMap不会对整个容器进行扩容，而只对某个segment
扩容。
5. size操作，ConcurrentHashMap的做法是先尝试2次通过不锁住Segment的方式来统计各个Segment的大小，如果统计的过程中，容器的
count发生了变化，再采用加锁的方式来统计所有Segment的大小。
- **_ConcurrentLinkedQueue_**
1. 入队列: 如果tail节点的next节点不为空，则将入队节点设置成tail节点，如果tail节点的next节点为空，则将tail入队节点设置为tail节点
的next节点。所以tail节点不总是尾结点。这样做的目的是，减少cas更新tail节点的次数，提升入队效率。
2. 出队列: 当head节点有元素时，直接弹出head节点的元素，而不会更新head节点，只有当head节点里没有元素时，出队操作才会更新
head节点。这种做法也是通hops变量来减少使用CAS更新head节点的消耗。
- **_java阻塞队列_**
1. ArrayBlockingQueue: 数组结构的有界阻塞队列。
2. LinkedBlockingQueue: 链表结构
3. PriorityBlockingQueue: 支持优先级排序的无界阻塞队列
4. DelayQueue: 使用优先级队列实现的无界阻塞队列
5. SynchronousQueue: 不存储元素的阻塞队列
6. LinkedTransferQueue: 链表组成的无界阻塞队列。
7. LinkedBlockingDeque: 链表结构的双向阻塞队列，双向队列因为多了操作队列的入口，可以从队列的两端插入和取出元素，在多线程
同时入队时，也就减少了一半的竞争。双向阻塞队列可用运用在“工作窃取”模式中。
- Fork/Join框架
1. Fork/Join框架是java7提供的一个用于并行执行任务的框架，是一个把大任务分割成若干个小任务，最终汇总每个小任务结果后得到
大任务结果的框架.
1. 工作窃取算法: 已经干完活的线程窃取未干完活的线程的剩余任务，这就是工作窃取。为了减少窃取任务线程和被窃取任务线程之间
的竞争，通常会使用双端队列。被窃取线程从头部拿任务，窃取线程从尾部拿任务。优点: 充分利用线程并行计算，减少了线程间的竞争。


