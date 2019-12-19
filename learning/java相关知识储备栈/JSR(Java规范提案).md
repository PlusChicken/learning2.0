# JSR(Java规范提案)

JSR是Java Specification Requests的缩写，意思是Java规范提案。是指向JCP(Java Community Process)提出新增一个标准化技术规范的正式请求。任何人都可以提交JSR，以向Java平台增添新的API和服务。JSR已成为Java界的一个重要标准

JSR-303是JAVA EE 6中的一项子规范，叫做Bean Validation，Hibernate Validator是Bean Validation的参考实现。Hibernate Validator 提供了JSR 303 规范中所有内置constraint的实现，除此之外还有一些附加的constraint。

Bean Validation中内置的constraint

| Constraint                | 详细信息                                                 |
| ------------------------- | -------------------------------------------------------- |
| @Null                     | 被注释的元素必须为null                                   |
| @NotNull                  | 被注释的元素必须不为null                                 |
| @AssertTrue               | 被注释的元素必须为true                                   |
| @AssertFalse              | 被注释的元素必须为false                                  |
| @Min(value)               | 被注释的元素必须是一个数字，其值必须大于等于指定的最小值 |
| @Max(value)               | 被注释的元素必须是一个数字，其值必须小于等于指定的最大值 |
| @DecimalMin(value)        | 被注释的元素必须是一个数字，其值必须大于等于指定的最小值 |
| @DecimalMax(value)        | 被注释的元素必须是一个数字，其值必须小于等于指定的最大值 |
| @Size(max,min)            | 被注释的元素的大小必须在指定的范围内                     |
| @Digits(integer,fraction) | 被注释的元素必须是一个数字，其值必须在可接受的范围内     |
| @Past                     | 被注释的元素必须是一个过去的日期                         |
| @Future                   | 被注释的元素必须是一个将来的日期                         |
| @Pattern(value)           | 被注释的元素必须符合指定的正则表达式                     |

####主要使用的都是结合Mybatis的代码，不太使用Hibernate，但是以下已经集成进web依赖里面了，可以使用

 Hibernate Validator附加的constraint

| Constraint | 详细信息                               |
| ---------- | -------------------------------------- |
| @Email     | 被注释的元素必须是电子邮箱地址         |
| @Length    | 被注释的字符串的大小必须在指定的范围内 |
| @NotEmpty  | 被注释的字符串必须非空                 |
| @Range     | 被注释的元素必须在合适的范围内         |

更多关于JSR的内容可以参阅官方文档

使用方法：1.在实体类的变量上加@NotNull

​					2.在Controller的引入实体变量前加@Valid