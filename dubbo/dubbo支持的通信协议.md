1. 为什么 PB 的效率是最高的？

可能有一些同学比较习惯于 JSON or XML 数据存储格式，对于 Protocol Buffer 还比较陌生。Protocol Buffer 其实是 Google 出品的
一种轻量并且高效的结构化数据存储格式，性能比 JSON、XML 要高很多。其实 PB 之所以性能如此好，主要得益于两个：第一，它使用 
proto 编译器，自动进行序列化和反序列化，速度非常快，应该比 XML 和 JSON 快上了 20~100 倍；第二，它的数据压缩效果好，就是
说它序列化后的数据量体积小。因为体积小，传输起来带宽和速度上会有优化。