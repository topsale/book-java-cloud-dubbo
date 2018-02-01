# Zookeeper 安装

---

Zookeeper安装方式有三种

* 单机模式：Zookeeper 只运行在一台服务器上，适合测试环境
* 伪集群模式：就是在一台物理机上运行多个 Zookeeper 实例
* 集群模式：Zookeeper 运行于一个集群上，适合生产环境，这个计算机集群被称为一个“集合体”（ensemble）

Zookeeper 通过复制来实现高可用性，只要集合体中半数以上的机器处于可用状态，它就能够保证服务继续。为什么一定要超过半数呢？这跟 Zookeeper 的复制策略有关：Zookeeper 确保对 znode 树的每一个修改都会被复制到集合体中超过半数的机器上。

* [单机模式](/chapter10/Zookeeper安装/单机模式.md)
* [伪集群模式](/chapter10/Zookeeper安装/伪集群模式.md)
* [集群模式](/chapter10/Zookeeper安装/集群模式.md)