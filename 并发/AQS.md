1. 队列同步器(AQS)是用来构建锁或其他同步组件的基础框架，它使用了一个int成员变量表示同步状态，
通过内置的FIFO双向队列来完成资源获取线程的排队工作。
2. 同步器的主要使用方式是继承，子类推荐被定义为同步组件的静态内部类。
- 同步队列
1. 同步队列中的节点的属性和状态。
```java
    1. int waitStatus
    static final int CANCELLED =  1;  取消状态，节点从同步队列中取消等待，将会置为该状态
    static final int SIGNAL    = -1;  后继节点处于等待状态，而当前节点的线程释放了同步状态或者被取消，将会通知后继节点，
    使后继节点得以运行(状态为-1表示你要去通知的你后继线程，不要忘了)
    static final int CONDITION = -2;  节点在等待队列中，节点线程等待在Condition上，当其他线程对Condition调用了signal（）
    方法后，该节点将会从等待队列转移到同步队列，加入到对同步状态的获取中。
    static final int PROPAGATE = -3;  表示下一次共享式同步状态获取将会无条件地被传播下去。
    static final int INTIAL = -0;   初始状态。
    2. Node prev 前驱节点，当节点加入同步队列时被设置(尾部添加)
    3. Node next 后继节点
    4. Node nextWatier 等待队列中的后继节点，如果当前节点是共享的，那么这个字段将是一个SHARED常量，也就是说节点类型
    和等待队列中的后继节点公用同一个字段
    5. Threaad thread 获取同步状态的线程
```