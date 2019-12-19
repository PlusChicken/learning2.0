# @Conditional注解

~~~

~~~

@Confitional太烦了，先等等，先学习衍生类

1.@ConditionalOnBean(仅仅在当前上下文中存在某个对象时，才会实例化一个Bean)

2.@ConditionalOnClass(某个class位于类路径上，才会实例化一个Bean)，该注解的参数对应的类必须存在，否则不解析该注解修饰的配置类

3.@ConditionalOnExpression(当表达式为true的时候,才会实例化一个Bean)

4.@ConditionalOnMissingBean(仅仅在当前上下文中不存在某个对象时,才会实例化一个Bean),该注解表示,如果存在它修饰的类的bean,则不需要再创建这个bean;可以给该注解传入参数

5.@ConditionalOnMissingBean(name = "example"),这个表示如果name为"example"的bean存在,这该注解修饰的代码块不执行.

6.@ConditionalOnMissingClass(某个class类路径上不存在的时候,才会实例化一个Bean)

7.@ConditionalOnNotWebApplication(不是web应用)

8.@ConditionalOnProperty是指在application.yml里配置的属性是否为true,其他的几个都是对class的判断

~~~java
value//数组,获取对应property名称的值,与name不可同时使用
prefix//property名称的前缀,可有可无
name//数组,property完整名称或部分名称(可与prefix组合使用,组成完整的property名称)与value不可同时使用//{"name","flag"}可以判断多元素
havingValue//可与name组合使用,比较获取到的属性值与havingValue给定的值是否相同,相同才加载配置
matchIfMissing//允许缺少,缺少该property时是否可以加载.如果true,没有该property也会正常加载;反之报错
relaxedNames//松散匹配,我这个版本没有
~~~

