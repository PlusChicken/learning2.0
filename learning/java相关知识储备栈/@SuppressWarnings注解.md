# @SuppressWarnings注解

java.lang.SuppressWarnings是J2SE5.0中标准的Annotation之一。可以标注在类，字段，方法，参数，构造方法，以及局部变量上。

SuppressWarnings主要用于抑制编译器产生警告信息

~~~java
@SuppressWarnings("unchecked")
~~~

告诉编译器忽略unchecked警告信息

```java
@SuppressWarnings("serial")
```

如果编译器出现这样的警告信息：The serializable class WmailCalendar does notdeclare a static final serialVersionUID field of type long使用这个注释将警告信息去掉。

```java
 @SuppressWarnings("deprecation")
```

如果使用了使用@Deprecated注释的方法，编译器将出现警告信息。使用这个注释将警告信息去掉。

告诉编译器同时忽略unchecked和deprecation的警告信息。

```java
@SuppressWarnings(value={"unchecked", "deprecation"})
```

等同于@SuppressWarnings("unchecked", "deprecation")