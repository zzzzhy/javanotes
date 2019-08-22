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
