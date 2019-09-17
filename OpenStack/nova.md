1. nova-compute： nova-compute在计算节点上运行，复杂管理节点上的insatance。OpenStack对instance的操作，最后都是交给nova-compute
来完成的。nova-compute与Hypervisor一起实现OpenStack对instance生命周期的管理。功能如下：
    - 定时向OpenStack报告计算节点的状态。
    - 实现instance生命周期的管理。
2. 通过Driver架构支持多种Hypervisor，nova-compute为这些Hypervisor定义了统一的接口，Hypervisor只需要实现这些接口，就可以Driver
的形式即插即用到OpenStack系统中。
3. nova-conductor： nova-comput通过nova-conductor实现数据库的访问，这样做的好处是更高的系统安全性和更好的系统伸缩性。
4. nova的调度过程如下：通过过滤器（filter）选择满足条件的存储节点，通过权重计算（weighting）选择最优（权重值最大）的
存储节点。
