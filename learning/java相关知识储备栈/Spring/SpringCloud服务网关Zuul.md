# SpringCloud服务网关Zuul

zuul的工作原理关键在于ZuulServlet和它的内置Filter.

自定义Filter:

​	总共分为4种:pre,Route,Post,Error

1. Pre类型的Filter总是先执行,它可以做到限流和权限控制的功能

2. Route类型的Filter为Zuul内部转发请求到真正的服务的Filter,一般我们不需要实现这种类型

3. Post类型为请求转发完成后的后续动作,可以进行日志等的一些添加

4. Error类型为上述Filter出错后执行的动作,可以进行错误处理等

   执行顺序就是1,2,3,4,5

Zuul内置的Filter:

1,RibbonRoutingFilter

2,SimpleHostRoutingFilter

3,SendForwardFilter