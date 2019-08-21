```java
    package let;
    
    import java.util.concurrent.locks.AbstractQueuedSynchronizer;
    
    /**
     * Created by z18485 on 2019/8/21.
     * 参考书籍java并发编程的艺术，锁的代码
     * @author zhy
     */
    public class TwinsLock {
        final Sync sync = new Sync(2);
    
        private static final class Sync extends AbstractQueuedSynchronizer {
            Sync(int count) {
                if (count <= 0) {
                    throw new IllegalArgumentException("the length should be large 0");
                }
                setState(count);
            }
    
            @Override
            public int tryAcquireShared(int reduceCount) {
                for (; ; ) {
                    int current = getState();
                    int result = current - reduceCount;
                    if (result < 0 || compareAndSetState(current, result)) {
                        return result;
                    }
                }
    
            }
    
            @Override
            public boolean tryReleaseShared(int returnCount) {
                for (; ; ) {
                    int current = getState();
                    int result = current + returnCount;
                    if (compareAndSetState(current, result)) {
                        return true;
                    }
                }
            }
        }
    
        public void lock() {
            sync.acquireShared(1);
        }
    
        public void unLock() {
            sync.releaseShared(1);
        }
    }

```
```java
    package let;
    
    
    /**
     * Created by z18485 on 2019/8/21.
     *  测试代码
     * @author zhy
     */
    public class TwinsLockTest {
        public static void test() {
            final TwinsLock lock = new TwinsLock();
            class Worker extends Thread {
                @Override
                public void run() {
                    while (true) {
                        try {
                            lock.lock();
                            try {
                                Thread.sleep(1000);
                                System.out.println(Thread.currentThread().getName());
                                Thread.sleep(1000);
                            } catch (InterruptedException e) {
                                e.printStackTrace();
                            }
                        } finally {
                            lock.unLock();
                        }
                    }
                }
            }
            for (int i = 0; i < 10; i++) {
                Worker w = new Worker();
                w.setDaemon(true);
                w.start();
            }
            for (int i = 0; i < 10; i++) {
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println();
            }
        }
    
        public static void main(String[] args) {
            test();
        }
    }

```
