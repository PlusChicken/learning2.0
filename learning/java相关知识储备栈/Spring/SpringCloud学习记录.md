# SpringCloud学习记录

1. Eureka

   Eureka是SpringCloud的注册中心，其主要职能就是提供注册，充当项目之间调用的中间件，注册之后，项目的调用都将经过注册中心，避免了一个项目修改，多个项目重启的尴尬境地。每个项目只需从注册中心去调用即可。

   Eureka的基本架构，由3个角色组成：

   1. Eureka Server

      ·提供服务注册和发现

   2. Service Provider

      ·服务提供方

      ·将自身服务注册到Eureka，从而使服务消费方能够找到

   3. Service Consumer

      ·服务消费方

      ·从Eureka获取注册服务列表，从而能够消费服务

   ~~~~java
   //启动类备注
   @EnableEurekaServer
   ~~~~

   ~~~~properties
   #单个节点的Eureka的配置
   spring.application.name=spring-cloud-eureka
   
   server.port=8000
   #是否将自己注册到Eureka Server，默认为true
   eureka.client.register-with-eureka=false
   #表示是否从EurekaServer获取注册信息，默认为true
   eureka.client.fetch-registry=false
   
   #设置与Eureka Server交互的地址，查询服务和注册服务都需要依赖这个地址。默认是http://localhost:8761/eureka;多个地址可以使用，分隔
   eureka.client.service-url.defaultZone=http://localhost:${server.port}/eureka/
   ~~~~

   多个节点组成集群(不太了解)

http://www.ityouknow.com/springcloud/2017/05/10/springcloud-eureka.html

2. FeignClients

   搭建一个简单的服务端注册服务，客户端去调用服务使用的案例。案例中有三个角色：服务注册中心、服务提供者、服务消费者。流程是首先启动注册中心，服务提供者生产服务并注册到服务中心中，消费者从服务中心获取服务并执行。

   服务的提供者：

   ​	提供服务(Controller)

   服务的消费者：

   ​	启动加上(@EnableFeignClients)

   ​	在接口上加入

   ​	@FeignClient(name="spring-cloud-producer")

   ​	加上Controller调用接口

   负载均衡：就是当你调用相同的url时，如果有两个结果，请求就会自动轮询到每个服务端来处理。

3. Hystrix

   断路器的机制就是当Hystrix Command请求后端服务失败数量超过一定比例(默认50%),断路器会切换到开路状态(Open).这时所有请求会直接失败而不会发送到后端服务.断路器保持在开路状态一段时间后(默认5秒),自动切换到半开路状态(HALF-OPEN).这时会判断下一次请求的返回情况,如果请求成功,断路器切回闭路状态(CLOSE),否则重新切换到开路状态(OPEN).

   Hystrix主要是通过线程池来实现资源隔离,通常在使用的时候我们会根据调用的远程服务划分出多个线程池.

   Feign Hystrix