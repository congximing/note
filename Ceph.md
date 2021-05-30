# ceph

## 1.Crush

​	用于添加集群时，可以让数据在各个节点之间均衡，防止数据倾斜。

​	crush的算法实现。

​	straw1：通过hash（输入为权重、节点号、固定输入）计算每个节点的长度，选出最大的那个节点，将数据存入到这个节点中。

​	straw2：

## 2.BlueStore

​	对象存储。

​	元数据与用户数据分离。与hdfs类似NN和DN，元数据存储在SSD中，采用位式图的方式管理磁盘，比磁盘块的占用存储少8倍。



## 3.纠删码

​	磁盘冗余阵列，保证硬件的数据可靠性。

​	目的：为提升空间利用率。

## 4.PG

​	Placement Group：放置组

## 5.服务质量Qos

​	个别虚拟机或节点占用了集群大部分的io资源，导致其他节点的服务质量差。

## 6.RBD

​	分布式块存储服务。

## 7.RGW

​	分布式对象存储网关。对象存储服务。

## 8.分布式文件系统CephFS

​	文件存储服务。