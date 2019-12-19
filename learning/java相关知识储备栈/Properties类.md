# Properties类

##### 概述

Properties继承于Hashtable.表示一个持久的属性集,属性列表以key-value的形式存在,key和value都是字符串.

Properties类被许多Java类使用.例如,在获取环境变量时它就作为System.getProperties()方法的返回值.

我们在很多需要避免硬编码的应用场景下需要使用properties文件来加载程序需要的配置信息,比如JDBC、MyBatis框架等.Properties类则是properties文件和程序的中间桥梁,不论是从properties文件读取信息还是写入信息到properties文件都需要经由properties

1. getProperty(String key),用指定的键在此属性列表中搜索属性。也就是通过参数key，得到key所对应的value.
2. load(InputStream inStream),从输入流中读取属性列表(键和元素对).通过对指定的文件(比如说上面的test.properties文件)进行装载来获取改文件中的所有键-值对.以供getProperty(String key)来搜索.
3. setProperty(String key,String value),调用Hashtable的方法put,他通过调用基类的put方法来设置键-值对.
4. store(OutputStream out, String comments), 以适合使用load方法加载到Properties表中的格式,将此Properties表中的属性列表(键和元素对)写入输出流.与load方法相反,改方法将键-值对写入到指定的文件中去.
5. clear(),清除所有装载的键-值对.该方法在基类中提供.

Properties

1. 作用:读写资源配置文件

2. 键与值只能为字符串

3. 方法:

   setProperty(String key,String value)

   getProperty(String key)

   getProperty(String key, String defaultValue)

后缀:.properties

store(OutputStream out, String comments)

store(Write write, String comments)

load(InputStream inStream)

load(Reader reader)

.xml

storeToXML(OutputStream os, String comments) :UTF-8字符集

storeToXML(OutputStream os, String comments, String encoding)

loadFromXML(InputStream in)