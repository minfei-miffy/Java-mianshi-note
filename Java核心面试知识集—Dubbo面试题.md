# Contanct Me

如果觉得看起来比较麻烦，需要PDF版本，或是需要更多学习资料，都可以加上QQ群领取

>本群由我创立，目前已将群主权限交由合作方便于进行日常管理，介意的朋友们在GitHub上看最新版就好了
>
>> 这份笔记资料是会免费提供的，特地向你们保证…毕竟还是要恰饭的嘛…

祝愿每一位有追求的Java开发同胞都能进大厂拿高薪！

## QQ群

Java架构交流QQ群：**578486082** （备注一下GitHub，免得被认成打无良广告的）

快捷加群方式：[点击此处加入群聊：java高级程序猿①](https://jq.qq.com/?_wv=1027&k=oE5kCnMu)

![image.png](https://upload-images.jianshu.io/upload_images/24613101-931262091ba7ed2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> PS：
>
> > 平常很忙，找miffy小姐姐领取就好了，免费获取的！

![image.png](https://upload-images.jianshu.io/upload_images/24613101-4b0507ab7ef34106.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 1、为什么要用Dubbo？

随着服务化的进一步发展，服务越来越多，服务之间的调用和依赖关系也越来越复杂，诞生了面向服务的架构体系(SOA)，

也因此衍生出了一系列相应的技术，如对服务提供、服务调用、连接处理、通信协议、序列化方式、服务发现、服务路由、日志输出等行为进行封装的服务框架。

就这样为分布式系统的服务治理框架就出现了，Dubbo也就这样产生了。

## 2、Dubbo 的整体架构设计有哪些分层?

接口服务层（Service）：该层与业务逻辑相关，根据 provider 和 consumer 的业务设计对应的接口和实现

配置层（Config）：对外配置接口，以 ServiceConfig 和 ReferenceConfig 为中心

服务代理层（Proxy）：服务接口透明代理，生成服务的客户端 Stub 和 服务端的 Skeleton，以 ServiceProxy 为中心，扩展接口为 ProxyFactory

服务注册层（Registry）：封装服务地址的注册和发现，以服务 URL 为中心，扩展接口为 RegistryFactory、Registry、RegistryService

路由层（Cluster）：封装多个提供者的路由和负载均衡，并桥接注册中心，以Invoker 为中心，扩展接口为 Cluster、Directory、Router和LoadBlancce

监控层（Monitor）：RPC调用次数和调用时间监控，以 Statistics 为中心，扩展接口为 MonitorFactory、Monitor和MonitorService

远程调用层（Protocal）：封装 RPC 调用，以 Invocation 和 Result 为中心，扩展接口为 Protocal、Invoker和Exporter

信息交换层（Exchange）：封装请求响应模式，同步转异步。以 Request 和 Response 为中心，扩展接口为 Exchanger、ExchangeChannel、ExchangeClient和ExchangeServer

网络传输层（Transport）：抽象 mina 和 netty 为统一接口，以 Message 为中心，扩展接口为Channel、Transporter、Client、Server和Codec

数据序列化层（Serialize）：可复用的一些工具，扩展接口为Serialization、 ObjectInput、ObjectOutput和ThreadPool

## 3、默认使用的是什么通信框架，还有别的选择吗?

默认也推荐使用netty框架，还有mina。

## 4、服务调用是阻塞的吗？

默认是阻塞的，可以异步调用，没有返回值的可以这么做。

Dubbo 是基于 NIO 的非阻塞实现并行调用，客户端不需要启动多线程即可完成并行调用多个远程服务，相对多线程开销较小，异步调用会返回一个 Future 对象。

## 5、一般使用什么注册中心？还有别的选择吗？

推荐使用 Zookeeper 作为注册中心，还有 Redis、Multicast、Simple 注册中心，但不推荐。

## 6、默认使用什么序列化框架，你知道的还有哪些？

推荐使用Hessian序列化，还有Duddo、FastJson、Java自带序列化。

## 7、服务提供者能实现失效踢出是什么原理？

服务失效踢出基于zookeeper的临时节点原理。

## 8、服务上线怎么不影响旧版本？

采用多版本开发，不影响旧版本。

## 9、如何解决服务调用链过长的问题？

可以结合zipkin实现分布式服务追踪。

## 10、说说核心的配置有哪些？

| 配置 | 配置说明 |

| --- | --- |

| dubbo:service | 服务配置 |

| dubbo:reference | 引用配置 |

| dubbo:protocol | 协议配置 |

| dubbo:application | 应用配置 |

| dubbo:module | 模块配置 |

| dubbo:registry | 注册中心配置 |

| dubbo:monitor | 监控中心配置 |

| dubbo:provider | 提供方配置 |

| dubbo:consumer | 消费方配置 |

| dubbo:method | 方法配置 |

| dubbo:argument | 参数配置 |

## 11、Dubbo 推荐用什么协议？

· dubbo://（推荐）

· rmi://

· hessian://

· http://

· webservice://

· thrift://

· memcached://

· redis://

· rest://

## 12、同一个服务多个注册的情况下可以直连某一个服务吗？

可以点对点直连，修改配置即可，也可以通过telnet直接某个服务。

## 13、画一画服务注册与发现的流程图？

![image.png](https://upload-images.jianshu.io/upload_images/11474088-be34bef59137b1aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 14、Dubbo 集群容错有几种方案？

| 集群容错方案 | 说明 |

| --- | --- |

| Failover Cluster | 失败自动切换，自动重试其它服务器（默认） |

| Failfast Cluster | 快速失败，立即报错，只发起一次调用 |

| Failsafe Cluster | 失败安全，出现异常时，直接忽略 |

| Failback Cluster | 失败自动恢复，记录失败请求，定时重发 |

| Forking Cluster | 并行调用多个服务器，只要一个成功即返回 |

| Broadcast Cluster | 广播逐个调用所有提供者，任意一个报错则报错 |

## 15、Dubbo 服务降级，失败重试怎么做？



## 16、Dubbo 使用过程中都遇到了些什么问题？



## 17、Dubbo Monitor 实现原理？



## 18、Dubbo 用到哪些设计模式？



## 19、Dubbo 配置文件是如何加载到Spring中的？



## 20、Dubbo SPI 和 Java SPI 区别？



## 21、Dubbo 支持分布式事务吗？



## 22、Dubbo 可以对结果进行缓存吗？



## 23、服务上线怎么兼容旧版本？



## 24、Dubbo必须依赖的包有哪些？



## 25、Dubbo telnet 命令能做什么？



## 26、Dubbo 支持服务降级吗？



## 27、Dubbo 如何优雅停机？



## 28、Dubbo 和 Dubbox 之间的区别？



## 29、Dubbo 和 Spring Cloud 的区别？



