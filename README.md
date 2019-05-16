

1、K8S的核心组件
Master/Node：
Master：
apiserver：提供HTTP rest接口，所有资源增删查改的唯一入口。
scheduler：资源调度（最大/最小资源）
controller-manger：管理kubelet健康状态，自动化控制中心。
Node：
kubelet： 用于创建、销毁、创建存储卷等容器管理的操作，容器健康状态检查。
docker：容器引擎
kube-proxy：与apiserver通信，当pod发生改变时，kube-proxy作为关联组件可接接收到改变信息，然后在本地更新到iptables或ipvs中。负载均衡的实现。
Pod：豌豆荚，是一个或多个容器组成的一个容器组，一个容器组中共享网络环境及存储卷。是K8S最小的调度单元。
自主式Pod
控制器管理的Pod
Replication Controllers：负责指定数量的 pod 在同一时间一起运行。已经废弃了被deployment代替了。
ReplicaSet： 
Deployment
StatefulSet： 有状态应用。
DaemonSet： 系统级守护进程类型应用。
Job、Cronjob
HPA
HorizontalPodAutoscaler：水平控制器，用于做类似php-fpm进程管理方式（最大最小空闲进程）。
Label、Label Selector：
Label：标签，key=value类型数据。
Label Selector：标签选择器，用于过滤标签。

2、K8S个级别通信方式。
同一个Pod内多个容器间：lo
各Pod之间通信：Overlay Network，叠加网络

3、插件
CNI：容器网络接口
flannel：网络配置
calico：网络配置，网络策略
canel：使用flannel实现网络配置，calico实现网络策略


