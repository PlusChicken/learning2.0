# PageHelper

依赖:
~~~java
		<dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.2.12</version>
        </dependency>
~~~

配置:

~~~properties
pagehelper.helper-dialect=mysql
pagehelper.support-methods-arguments=true
pagehelper.params=pageNum=pageNum;pageSize=pageSize
~~~

只需要创建实体类Page,或者继承Page

~~~java
package com.page.Entity;

import lombok.Data;

/**
 * @Author: ZhouYuJi
 * @Description: 可以根据继承从而获取页面数量
 * @Date: 16:23 2019/11/7
 */
@Data
public class Page {

    private Integer pageNum;

    private Integer pageSize;
}

~~~

传入Page实体类就能实现分页