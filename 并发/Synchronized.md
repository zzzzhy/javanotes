1. synchronized锁定同步块使用的是monitorenter 和monitorexit指令，而同步方法则是依靠方法修饰符上的ACC_SYNCHRONIZED来完成的。
无论采用哪种方式，其本质是对一个对象的监视器进行获取，而这个获取过程是排他的。