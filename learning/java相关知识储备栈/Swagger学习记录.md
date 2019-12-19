# Swagger学习记录

#### 响应消息头

X-Rate-Limit-Remaining 可以用来表示API可调用的剩余次数

~~~~yaml
X-Rate-Limit-Remaining:
  type: integer
~~~~

X-Rate-Limit-Reset 可以用来表示API的有效截至时间

~~~~yaml
X-Rate-Limit-Reset:
  type: string
  format: date-time
~~~~

#### API安全相关

基础鉴权(三种)

~~~~yaml
securityDefinitions:
  UserSecurity:
  	type: basic
  AdminSecurity:
  	type: basic
  MediaSecurity:
  	type: basic
~~~~

API密钥鉴权

~~~~yaml
securityDefinitions:
  UserSecurity:
    type: apiKey
    in: header
    name: SIMPLE-API-KEY
  AdminSecurity:
    type: apiKey
    in: header
    name: ADMIN-API-KEY
  MediaSecurity:
    type: apiKey
    in: query
    name: MEDIA-API-KEY
~~~~

Oauth2鉴权

| 流程        | 所需要的URL                   |
| ----------- | ----------------------------- |
| implicit    | authorizationUrl(鉴权地址)    |
| password    | tokenUrl(令牌地址)            |
| application | tokenUrl                      |
| accessCode  | authorizationUrl and tokenUrl |

example:

~~~~yaml
securityDefinitions:
  OathSecurity:
    type: oauth2
    flow: accessCode
    authorizationUrl: 'https://oauth.simple.api/authorization'
    tokenUrl: 'https://oauth.simple.api/token'
~~~~

作用范围(scope)

键值对 键值对的键表示作用范围名称; 值是它的相关描述

~~~~yaml
securityDefinitions:
  OauthSecurity:
    type: oauth2
    flow: accessCode
    authorizationUrl: 'https://oauth.simple.api/authorization'
    tokenUrl: 'https://oauth.simple.api/token'
    scopes:
      admin: Admin scope
      user: User scope
      media: Media scope
~~~~

安全定义:

~~~~yaml
securityDefinitions:
  OauthSecurity:
    type: oauth2
    flow: accessCode
    authorizationUrl: 'https://oauth.simple.api/authorization'
    tokenUrl: 'https://oauth.simple.api/token'
    scopes:
      admin: Admin scope
      user: User scope
  MediaSecurity:
    type: apiKey
    in: query
    name: media-api-key
  LegacySecurity:
    type: basic
~~~~

全局安全配置:

~~~~yaml
security:
  - OauthSecurity:
    - user
  - LegacySecurity: []
~~~~

覆盖全局配置: 此时用户可以通过admin 的Oauth2 或者 legacySecurity来鉴权使用这个操作。

~~~~yaml
	post:
      summary: Creates a person
      description: Adds a new person to the persons list.
      security:
        - OauthSecurity:
          - admin
        - LegacySecurity: []
~~~~

用户只能通过 MediaSecurity 鉴权使用这个操作。

~~~~yaml
  /images:
    parameters:
      - $ref: '#/parameters/userAgent'
    post:
      summary: Uploads an image
      security:
        - MediaSecurity: []
~~~~

#### 编辑器报错

把上面的代码放到Swagger Editor中编辑，编辑器预览中可能会报错：

提示找不到person.yaml文件。这是因为我们没有指定正确的编辑器的指针解析基础路径（`Pointer Resolution Base Path`） 。

解决的办法是：进入编辑器的 Preferences -> Preferences 菜单，修改`Pointer Resolution Base Path`为：

考虑到缓存的原因，如果错误依然存在，请刷新浏览器。

引用外部yaml

`$ref: "person.yaml#/Person"`

引用子文件夹下的文件

`$ref: "folder/person.yaml#/Person"`

引用上级文件夹下的文件

`$ref: "../folder/person.yaml#/Person"`

引用远程文件

`$ref: https://myserver.com/mypath/myfile.yaml#/example`

注:服务器必须提供跨域（CORS）访问服务

引用本地服务器上的8080端口引用文件

`$ref: "http://localhost:8080/folder/person.yaml#/Person"`

远端文件引用本地文件

`$ref: "http://localhost:8080/another-folder/persons.yaml#/Persons"`

##### 实战切分的思路

- 结构化切分思路： 先把API文档的头部`info`切下来，放在`info.yaml`，然后分别切分 `paths.yaml`，`definitions.yaml`，`responses.yaml`，`parameters.yaml`等文件，最后合并到`main.yaml`；
- 分层切分思路：分别按照功能和层次，将文件切分为`base.yaml`或`common.yaml`，然后是各个模块的`xxx-paths.yaml`，然后是各个定义的`xxx-definitions.yaml`。
- 其他思路……

##### swagger常用注解

与Controller注解并列使用

| 属性名称       | 备注                                             |
| -------------- | ------------------------------------------------ |
| value          | url的路径值                                      |
| tags           | 如果设置这个值、value的值会被覆盖                |
| description    | 对api资源的描述                                  |
| basePath       | 基本路径可以不配置                               |
| position       | 如果配置多个Api想改变显示的顺序位置              |
| produces       | For example，“application/json，application/xml” |
| consumes       | For example，“application/json，application/xml” |
| protocols      | Possible values:http,https,ws,wss                |
| authorizations | 高级特性认证时配置                               |
| hidden         | 配置为true将在文档中隐藏                         |

##### Api标记

Api用在类上，说明该类的作用。可以标记一个Controller类作为swagger文档资源，使用方式：@Api(value="/user",description="Operations about user")

##### ApiOperation标记

用在方法上，说明方法的作用，每个url资源的定义，使用方法：

@ApiOperation(

​		value = "Find purchase order by ID",

​		notes = "For valid response try integer IDs with value <=5 or >10. Other values will generated exceptions",

​		response = Order,

​		tags = {"Pet Store"})

##### ApiParam标记

ApiParam请求属性,使用方式

~~~~java
public PesponseEntity<User> createUser(@RequestBody @ApiParam(value = "Created user object",required = "true") User user)
~~~~

##### ApiResponse

响应配置,使用方式:

@ApiResponse(code = 400,message = "Invalid user supplied")

##### ApiResponses

响应集配置,使用方式:

@ApiResponses({@ApiResponse(code = 400,message = "Invalid Order")})

##### ResponseHeader

响应头设置,使用方法

@ResponseHeader(name = "head1",description = "response head conf")

##### @Api()用于类

表示标识这个类是swagger的资源

tags–表示说明 

value–也是说明，可以使用tags替代 

但是tags如果有多个值，会生成多个list

~~~~java
@Api(value="用户controller",tags={"用户操作接口"})

@RestController 

public class UserController {

}
~~~~



##### @ApiOperation()用于方法

表示一个http请求的操作

value用于方法描述 

notes用于提示内容 

tags可以重新分组（视情况而用） 

##### @ApiParam()用于方法,参数,字段说明

表示对参数的添加元数据(说明或是否必填等)

name–参数名 

value–参数说明 

required–是否必填

##### @ApiModel()用于类

表示对类进行说明,用于参数用实体类接收

value–表示对象名 

description–描述 

都可省略

##### @ApiModelProperty()用于方法,字段

表示对model属性的说明或者数据操作更改

value–字段说明 

name–重写属性名字 

dataType–重写属性类型 

required–是否必填 

example–举例说明 

hidden–隐藏

##### @Apilgnore()用于类,方法,方法参数

表示这个方法或者类被忽略

##### @ApilmplicitParam()用于方法

表示单独的请求参数

##### @ApilmplicitParams()用于方法

包含多个@ApilmplicitParam

name–参数名 

value–参数说明 

dataType–数据类型 

paramType–参数类型 

example–举例说明



