# Iaas 基础设施及服务

# paas 平台及服务

# saas 软件及服务







MESOS APACHE 分布式资源管理框架  2019-5 》 kubernates

Docker Swarm 2019-07 阿里云宣布 Docker Swarn 剔除

Kubernetes Google 10年容器化基础架构 borg



# kubernetes的特点

- 轻量级：消耗资源少

- 开源

- 弹性伸缩

- 负载均衡：IPVS（实现了传输层负载均衡，`ipvs`可以将基于`TCP`和`UDP`的服务请求转发到真实服务器上）

  ipvs 为大型集群提供了更好的可扩展性和性能

  ipvs 支持比 iptables 更复杂的复制均衡算法（最小负载、最少连接、加权等等）

  ipvs 支持服务器健康检查和连接重试等功能

介绍说明：前世今生   kubernetes 框架   kubernetes关键字含义

基础概念：什么是pod   控制器类型  k8s 网络通讯模式

KUbernetes： 构建K8s集群

资源清单： 资源  掌握资源清单的语法  编写Pod   掌握Pod的生命周期***

Pod 控制器：掌握各种控制器的特点以及使用定义方式

服务发现：掌握 SVC 原理及其构建方式  





etcd的官方姜它定义为成一个可信赖的分布式键值存储服务，它能够为整个分布式集群存储一些关键数据，协助分布式集群的正常运转

etcd有v2版和v3版本，v2版本会把所有信息存在内存中，v3版会把信息持久化到磁盘中，意味着关机不会损坏信息
etcd架构图

![1623318270(1)](D:\技术学习\learning\笔记\学习\我的笔记图片\1623318270(1).jpg)



服务发现： 掌握  svc  原理及其构建方式

存储：掌握多种存储类型特点 并且能够在不同环境中选择合适的存储方案

调度器：掌握调度器原理   能够根据要求把pod定义到想要的节点运行

安全：集群的认证 鉴权  访问控制  原理及其流程

HELM：linux yum 掌握 HELM原理  HELM模板自定义  HELM 部署一些常用插件

运维： 修改KUbeadm 达到证书可用期限为10年  能够构建高可用KUbernetes集群





服务分类

​        有状态服务：DBMS

​		无状态服务：LVS  APACHE



APISERVER:所有服务访问统一入口

CrontrollerManager：维持副本期望数

Scheduler：负责介绍任务，选择合适的节点进行分配任务

ETCD:键值对数据库，存储K8S集群所有重要信息

Kubelet:直接跟容器引擎交互实现容器的生命周期管理

kube-proxy：负责写入规则至IPTABLES，IPVS实现服务映射访问

CoreDNS：可以为集群中的SVC创建一个域名IP的对应关系解析

DASHBOARD：给K8s集群提供一个B/S结构访问体系

