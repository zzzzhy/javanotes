1. keystone需要完成的事情: 
    - 管理用及其权限
    - 维护OpenStack Services 的Endpoint
    - Authenticattion（认证） Authorization（鉴权）
2. keystone概念
    - 用户： User
    - Credentials：用户用来证明自己身份的信息，可以是：1. 用户名、密码
    2.token 3.API Key 4. 其他高级方式
    - Authentication：Authentication是keystone验证user用户身份的过程，User用户访问OpenStack时向OpenStack提供用户名密码
    形式的Credentials，然后Keystone验证后会给User签发一个Token最为后续访问的Credentials。
    - Token： Token使用数字和字母组成的字符串，User成功Authentication，它由Keystone分配给User。其有效默认时长为24小时。
    - project： 用于将OpenStack的资源（计算，存储，网络）进行分组和隔离。根据OpenStack服务的对象不用，Project可以使一个
    客户（公有云，也叫租户）、部门或者项目组（私有云）。
        1. 资源的所有权属于Project而不是User
        2. 每个User（包括admin）必须挂在Porject里才能访问该Project的资源。
    - Service： 每个Service都会提供若干个Endpoint，User 通过Endpoint访问资源和执行操作。
    - Endpoint： 是一个网络上可访问的地址，通常是一个URL。Service通过Url暴露自己的API
    - Role： 角色，决定一个用户的权限。Service决定每个Role能做什么事情，Service通过各自的policy.json文件对
    Role进行访问控制。
