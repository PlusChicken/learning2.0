# springbootAOP学习记录

##### aop能干嘛

aop能够记录接口运行状态。

记录抛异常的请求

@Aspect：描述一个切面类，定义切面类的时候需要打上这个注解

@Pointcut：声明一个切入点，切入点决定了连接点关注的内容，使得我们可以控制通知什么时候执行。Spring AOP只支持Springbean的方法执行连接点。所以你可以把切入点看作是Springbean上方法执行的匹配。一个切入点声明有两个部分：一个包含名字和任意参数的签名，还有一个切入点表达式，该表达式决定了我们关注那个方法的执行。

注：作为切入点返回的方法必须返回void类型

SpringAOP支持在切入点表达式中使用如下的切入带你指示符：

execution-匹配方法执行的连接点，这是你将会用到的Spring的最主要的切入点指示符。

within-限定匹配特定类型的连接点(在使用SpringAOP的时候，在匹配的类型中定义的方法的执行)

this-限定匹配特定的连接点(使用SpringAOP的时候方法的执行)，其中bean reference(SpringAOP代理)是指定类型的实例

target-限定匹配特定的连接点(使用SpringAOP的时候方法的执行)，其中目标对象(被代理的应用对象)是指定类型的实例

args-限定匹配特定的连接点(使用SpringAOP的时候方法的执行)，其中参数是指定类型的实例

@target-限定匹配特定的连接点(使用SpringAOP的时候方法的执行)，其中正执行对象的类有指定类型的注解

@args-限定匹配特定的连接点(使用SpringAOP的时候方法的执行)，其中实际传入参数的运行时类型持有特定类型的注解

@within-限定匹配特定的连接点，其中连接点所在的类型已指定注解(在使用SpringAOP)的时候，所执行的方法所在类型已指定注解

@annotation-限定匹配特定的连接点(使用SpringAOP的时候方法的执行)，其中连接点的主题持有指定的注解

#### 切入点表达式关键词：

　   1）execution：用于匹配子表达式。

​            //匹配com.cjm.model包及其子包中所有类中的所有方法，返回类型任意，方法参数任意
​            @Pointcut("execution(* com.cjm.model..*.*(..))")
​            public void before(){}

execution(public * *(..))

public可以省略，第一个*代表方法的任意返回值 第二个参数代表任意包+类+方法 (..)任意参数

任何一个以“set”开始的方法的执行：

execution(* set*(..))

UserService接口的任意方法:

execution(* com.coffee.service.UserService.*(..))

定义在com.coffee.service包里的任意方法的执行:

execution(* com.coffee.service.* .*(..))

#第一个 .* 代表任意类,第二个 .*代表人以方法

定义在service包和所有子包里的任意类的任意方法的执行:

execution(* com.coffee.service..* .*(..))

#..*代表任意包或者子包

定义在com.coffee包和所有子包里的UserService类的任意方法的执行:

execution(* com.coffee..UserService.*(..))")

##### 

​      2）within：用于匹配连接点所在的Java类或者包。

​            //匹配Person类中的所有方法
​            @Pointcut("within(com.cjm.model.Person)")
​            public void before(){}

 

​            //匹配com.cjm包及其子包中所有类中的所有方法

​            @Pointcut("within(com.cjm..*)")
​            public void before(){}

 

​     3） this：用于向通知方法中传入代理对象的引用。
​            @Before("before() && this(proxy)")
​            public void beforeAdvide(JoinPoint point, Object proxy){
​                  //处理逻辑
​            }

 

​      4）target：用于向通知方法中传入目标对象的引用。
​            @Before("before() && target(target)
​            public void beforeAdvide(JoinPoint point, Object proxy){
​                  //处理逻辑
​            }

 

​      5）args：用于将参数传入到通知方法中。
​            @Before("before() && args(age,username)")
​            public void beforeAdvide(JoinPoint point, int age, String username){
​                  //处理逻辑
​            }

​      6）@within ：用于匹配在类一级使用了参数确定的注解的类，其所有方法都将被匹配。 

​            @Pointcut("@within(com.cjm.annotation.AdviceAnnotation)") － 所有被@AdviceAnnotation标注的类都将匹配
​            public void before(){}

　　

​      7）@target ：和@within的功能类似，但必须要指定注解接口的保留策略为RUNTIME。
​            @Pointcut("@target(com.cjm.annotation.AdviceAnnotation)")
​            public void before(){}

 

​      8）@args ：传入连接点的对象对应的Java类必须被@args指定的Annotation注解标注。
​            @Before("@args(com.cjm.annotation.AdviceAnnotation)")
​            public void beforeAdvide(JoinPoint point){
​                  //处理逻辑
​            }

​      9）@annotation ：匹配连接点被它参数指定的Annotation注解的方法。也就是说，所有被指定注解标注的方法都将匹配。
​            @Pointcut("@annotation(com.cjm.annotation.AdviceAnnotation)")
​            public void before(){}

​      10）bean：通过受管Bean的名字来限定连接点所在的Bean。该关键词是Spring2.5新增的。
​            @Pointcut("bean(person)")
​            public void before(){}