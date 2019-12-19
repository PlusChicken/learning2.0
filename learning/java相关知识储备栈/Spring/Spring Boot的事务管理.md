# Spring Boot的事务管理

通过添加@Transactional注解来声明一个函数需要被事务管理。就是当出现异常时，之前对于数据库的操作将回滚到调用这个方法之前。

当使用多数据源的时候,可以添加value属性来判断事务管理的是哪个数据库.

~~~java
//范例
@Transactional(value="transactionalManagerPrimary")
~~~

在业务开发逻辑中，我们通常在Service层接口中使用@Transactional来对各个业务逻辑进行事务管理的配置。

#### 隔离级别：

隔离级别是指若干个并发的事务之间的隔离程度,与我们开发时候主要相关的场景包括:脏读取\重复读\幻读.

~~~java
public enum Isolation{
	DEFAULT(-1),
	READ_UNCOMMITTED(1),
	READ_COMMITTED(2),
	REPEATABLE_READ(4),
	SERIALIZABLE(8);
}
~~~

`DEFAULT`:这是默认值，表示使用底层数据库的默认隔离级别。对大多数数据库而言，通常这值就是：READ_COMMITTED

`READ_UNCOMMITTED`:该隔离级别表示一个事务可以读取另一个事务修改但还没有提交的数据。该级别不能防止脏读和不可重复读，因此很少使用该隔离级别。

`READ_COMMITTED`:该隔离级别表示一个事务只能读取另一个事务已经提交的数据。该级别可以防止脏读，这也是大多数情况下的推荐值。

`REPEATABLE_READ`:该隔离级别表示一个事务在整个过程中可以多次重复执行某个查询，并且每次返回的记录都相同。即使在多次查询之间有新增的数据满足该查询，这些新增的记录也会被忽略。该级别可以防止脏读和不可重复读。