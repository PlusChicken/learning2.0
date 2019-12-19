# Bean注入实例

现阶段发生的情况是

前提：在common包中有@bean multipartConfigElement配置

1. 在启动类里加入@bean multipartConfigElement ，启动类的bean不能加载，common包中的bean不能加载
2. 重新写一个配置类@Configuration @bean multipartConfigElement ，结果会与common包的报错
3. 如果两个@bean的名字一样，则结果为(1)情况【备注，@bean("设置名字")】
4. 在(1)情况的基础上加@Order(10),结果相同。得：bean扫描的时候扫描到相同name的bean，启动类的先加载，不加载其他相同bean名字的
5. 如果两个@bean的名字不相同，程序启动的时候报错
6. 如果两个@bean使用相同的名字，不是默认名字，结果为(1)情况
7. 当两个都在为@Configuration时是没有优先级，两个就会报错

总结是：@bean在注入的时候会扫描bean名，不同的相同的bean名，启动类的bean优先，不加载其他相同的@bean，如果两个是相同优先级的就会报错，但是swagger配置不同的包配置相同的内容不会报错，另一个假设是multipartConfigElement有对bean相同的值进行判断，或者是swagger上的enable注解能够避免重复的注入