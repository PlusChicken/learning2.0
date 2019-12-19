# SpringBoot的ApplicationEvent事件机制

ApplicationEvent：事件，每个实现类表示一类事件，可携带数据。

ApplicationListener：事件监听器，用于接收事件处理时间。

ApplicationEventMulticaster：事件管理者，用于事件监听器的注册和事件的广播。

ApplicationEventPublisher：事件发布者，委托ApplicationEventMulticaster：事件发布者，委托ApplicationEventMulticaster完成事件发布。

在spring中提供的标准事件：

1. ContextRefreshEvent，当ApplicationContext容器初始化完成或者被刷新的时候，就会发布该事件。
2. ContextStartedEvent，当ApplicationContext启动的时候发布事件，即调用ConfigurableApplicationContext接口的start方法的时候
3. ContextStoppedEvent，当ApplicationContext容器停止的时候发布事件，即调用ConfigurableApplicationContext的close方法的时候
4. ContextClosedEvent，当ApplicationContext关闭的时候发布事件，即调用ConfigurableApplicationContext的close方法的时候，关闭指的是所有的单例Bean都被销毁。
5. equestHandledEvent，只能用于DispatcherServlet的web应用，Spring处理用户请求结束后，系统会触发该事件。

Spring事件机制实现

​	事件，ApplicationEvent，继承EventObject。

​	事件监听器，ApplicationListener，是一个接口，继承自Event Listener，实际中需要实现其onApplicationEvent方法

​	事件发布，ApplicationEventPublisher，是一个接口，包含publish Event()方法，ApplicationContext继承了该接口，在AbstractApplicationContext中实现事件的发布接口。

​	Spring是如何通过事件找到对应的监听器的呢？Spring利用反射机制，通过getBeansOfType获取所有继承了Application Listener接口的监听器，然后把监听放到注册表中。

#### 异步是什么？

异步就是不等该结果的代码

Java中，transient变量修饰符，用transient关键字标记的成员变量不参与序列化过程。

序列化与反序列化需要使用serialVersionUID

Java序列化的机制是通过判断类的serialVersionUID来验证的版本一致的。

##### ApplicationEvent

所有的事件必须继承这个抽象类。

##### ApplicationEventMulticaster(构建广播器)