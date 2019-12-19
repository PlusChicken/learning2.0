# HttpClient学习记录

HttpClient是一个客户端的HTTP通信实现库。HttpClient的目标是发送和接收HTTP报文。

##### HTTP请求：

客户端发送一个HTTP请求到服务器的请求消息包括一下格式：请求行(request line)、请求头部(header)、空行和请求数据四个部分组成。

~~~~
GET /562f25980001b1b106000338.jpg HTTP/1.1
Host    img.mukewang.com
User-Agent    Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko)

Accept    image/webp,image/*,*/*;q=0.8
Referer    http://www.imooc.com/
Accept-Encoding    gzip, deflate, sdch
Accept-Language    zh-CN,zh;q=0.8

~~~~

第一部分：请求行，用来说明请求类型，要访问的资源以及所使用的HTTP版本。

第二部分：请求头部，紧接着请求行(即第一行)之后的部分，用来说明服务器要使用的附加信息

HOST将指出请求的目的地

User-Agent,服务器端和客户端脚本都能访问它,它是浏览器类型检测逻辑的重要基础。该消息由你的浏览器来定义

第三部分：空行，请求头部后面的空行是必须的

即使第四部分的请求数据为空，也必须有空行

第四部分：请求数据也叫主体，可以添加任意的其他数据。

这个例子的请求数据为空

##### POST请求例子：

~~~~
POST / HTTP1.1
Host:www.wrox.com
User-Agent:Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727
Content-Type:application/x-www-form-urlencoded
Content-Length:40
Connection: Keep-Alive
 
name=Professional%20Ajax&publisher=Wiley
~~~~

第一部分：请求行，第一行是post请求，以及http1.1版本

第二部分：请求头部，第二行至第六行

第三部分：空行，第七行的空行

第四部分：请求数据，第八行

##### HTTP之响应消息Response：

HTTP响应也由四个部分组成，分别是：状态行、消息报头、空行和响应正文

~~~~
HTTP/1.1 200 OK
Date: Fri, 22 May 2009 06:07:21 GMT
Content-Type: text/html; charset=UTF-8
 
<html>
      <head></head>
      <body>
            <!--body goes here-->
      </body>
</html>
~~~~

第一部分：状态行，由HTTP协议版本号，状态码，状态消息三部分。第一行为状态行，(HHTP/1.1)表明HTTP版本为1.1版本，状态码为200，状态消息为(ok)

第二部分：消息报头，用来说明客户端要使用的一些附加信息

第二行和第三行为消息报头，Date：生成响应的日期和时间；Content-Type：指定了MIME类型的HTML(text/html)，编码类型是UTF-8

第三部分：空行，消息报头后面的空行是必须的

第四部分：响应正文，服务器返回给客户端的文本信息。

空行后面的html部分为响应正文

##### HTTP之状态码

状态代码有三位数字组成，第一个数字定义了响应的类别，共分五种类别:

1xx：指示信息--表示请求已接收，继续处理
2xx：成功--表示请求已被成功接收、理解、接受
3xx：重定向--要完成请求必须进行更进一步的操作
4xx：客户端错误--请求有语法错误或请求无法实现
5xx：服务器端错误--服务器未能实现合法的请求

~~~~
200 OK //客户端请求成功
400 Bad Request //客户端请求有语法错误，不能被服务器所理解
401 Unauthorized //请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用
403 Forbidden //服务器收到请求，但是拒绝提供服务
404 Not Found //请求资源不存在，eg：输入了错误的URL
500 Internal Server Error //服务器发生不可预期的错误
503 Server Unavailable //服务器当前不能处理客户端的请求，一段时间后可能恢复正常
~~~~

##### HTTP请求方法

GET 请求指定的页面信息，并返回实体主体。

HEAD 类似于get请求，只不过返回的响应中没有具体的内容，用于获取报头

POST 向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。

PUT 从客户端向服务器传送的数据取代指定的文档的内容。

DELETE 请求服务器删除指定的页面。

CONNECT HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。

OPTIONS 允许客户端查看服务器的性能。

TRACE 回显服务器收到的请求，主要用于测试或诊断。

### HttpClient正文

HttpClient很好的支持http方法：

HttpGet，HttpHead，HttpPost，HttpPut，HttpDelete，HttpTrace和HttpOptions

HTTP请求URL包含了一个协议名称，主机名，可选端口，资源路径，可选的参数，可选的片段

`HttpGet httpget = new HttpGet("http://www.google.com/search?hl=en&q=httpclient&btnG=Google+Search&aq=f&oq=");`

~~~~java
URI uri = new URIBuilder()
			.setScheme("http")
			.setHost("www.google.com")
			.setPath("/search")
			.setParameter("q","httpclient")
			.setParameter("btnG","Google Search")
			.setParameter("aq","f")
			.setParameter("oq","")
			.build();
HttpGet httpget = new HttpGet(uri);
System.out.println(httpget.getURI());
~~~~

##### HTTP响应(Response)

HTTP相应是服务器接收并解析请求信息后返回给客户端的信息，它的起始行包含了一个协议版本，一个状态码和描述状态的短语。

~~~~java
HttpResponse response = new BasicHttpResponse(HttpVersion.HTTp_1_1,HttpStatus.SC_OK,"OK");
System.out.println(response.getProtocolVersion());
System.out.println(response.getStatusLine().getStatusCode());
System.out.println(response.getStatusLine().getReasonPhrase());
System.out.println(response.getStatusLine().toString());

// 输出：
// HTTP/1.1
// 200
// OK
// HTTP/1.1 200 OK
~~~~

##### 处理报文首部(Headers)

一个HTTP报文包含了许多描述报文的首部，比如内容长度，内容类型等。HttpClient提供了一些方法来取出，添加，移除，枚举首部。

~~~~java
HttpResponse response = new BasicHttpResponse(HttpVersion.HTTP_1_1,HttpStatus.SC_OK,"OK");
response.addHeader("Set-Cookie","c1=a; path=/; domain=localhost");
response.addHeader("Set-Cookie","c2=b; path\"/\",c3=c; domain=\"localhost\"");
Header h1 = response.getFirstHeader("Set-Cookie");
System.out.println(h1);
Header h2 = response.getLastHeader("Set-Cookie");
System.out.println(h2);
Header[] hs = response.getHeaders("Set-Cookie");
System.out.println(hs.length);
~~~~

~~~~java
//输出
Set-Cookie: c1=a; path=/; domain=localhost
Set-Cookie: c2=b; path="/", c3=c; domain="localhost"
2
~~~~

获取所有指定类型首部最有效的方式是使用HeaderIterator接口
~~~~java
HttpResponse response = new BasicHttpResponse(HttpVersion.HTTP_1_1,HttpStatus.SC_OK, "OK");
response.addHeader("Set-Cookie","c1=a; path=/; domain=localhost");
response.addHeader("Set-Cookie","c2=b; path=\"/\", c3=c; domain=\"localhost\"");
HeaderIterator it = response.headerIterator("Set-Cookie");
while (it.hasNext()) {
     System.out.println(it.next());
}
~~~~

~~~~java
//输出
Set-Cookie: c1=a; path=/; domain=localhost
 
Set-Cookie: c2=b; path="/", c3=c; domain="localhost"
~~~~

HttpClient也提供了其他便利的方法把HTTP报文转化为单个的HTTP元素

~~~~java
HttpResponse response = new BasicHttpResponse(HttpVersion.HTTP_1_1,HttpStatus.SC_OK, "OK");
response.addHeader("Set-Cookie","c1=a; path=/; domain=localhost");
response.addHeader("Set-Cookie","c2=b; path=\"/\", c3=c; domain=\"localhost\"");
HeaderElementIterator it = new BasicHeaderElementIterator(
response.headerIterator("Set-Cookie"));
while (it.hasNext()) {
     HeaderElement elem = it.nextElement();
     System.out.println(elem.getName() + " = " + elem.getValue());
     NameValuePair[] params = elem.getParameters();
     for (int i = 0; i < params.length; i++) {
          System.out.println(" " + params[i]);
     }
}
~~~~

~~~~
//输出
c1 = a
path=/
domain=localhost
c2 = b
path=/
c3 = c
domain=localhost
~~~~

##### HTTP实体(HTTP Entity)

使用实体的请求被称为内含实体请求

HttpClient区分出三种实体：

流体实体(Streamed) : 内容来源于一个流，或者在运行中产生。特别的，这个类别包括从响应中接收到的实体。流式实体不可重复。

自包含实体(self-contained) : 在内存中的内容或者通过独立的连接/其他实体获得的内容。自包含实体是可重复的。这类实体大部分是HTTP内容实体请求。

包装实体(wrapping) ：从另外一个实体中获取内容。

##### 可重复实体

一个实体能够被重复，意味着它的内容能够被读取多次。它仅可能是自包含实体(像ByteArrayEntity或StringEntity)

##### 使用HTTP实体

~~~~java
StringEntity myEntity = new StringEntity("important message",
                          ContentType.create("text/plain", "UTF-8"));
System.out.println(myEntity.getContentType());
System.out.println(myEntity.getContentLength());
System.out.println(EntityUtils.toString(myEntity));
System.out.println(EntityUtils.toByteArray(myEntity).length);
~~~~

~~~~
//输出
Content-Type: text/plain; charset=utf-8
17
important message
17
~~~~

##### 确保释放低级别的资源

为了确保正确的释放系统资源，你必须关掉与实体与实体相关的内容流，还必须关掉响应本身。

~~~~java
CloseableHttpClient httpclient = HttpClients.createDefault();
HttpGet httpget = new HttpGet("http://localhost/");
CloseableHttpResponse response = httpclient.execute(httpget);
try {
     HttpEntity entity = response.getEntity();
     if (entity != null) {
        InputStream instream = entity.getContent();
        try {
            // do something useful
        } finally {
            instream.close();
        }
   }
} finally {
     response.close();
}
~~~~

关闭内容流和关闭响应的不同点是：前者将会通过消费实体内容保持潜在的连接，而后者迅速的关闭并丢弃连接。
请注意，一旦实体被HttpEntity#writeTo(OutputStream)方法成功地写入时，也需要确保正确地释放系统资源。如果方法获得通过HttpEntity#getContent()，它也需要在一个finally子句中关闭流。
当使用实体时，你可以使用EntityUtils#consume(HttpEntity)方法来确保实体内容完全被消费并且使潜在的流关闭。
某些情况，整个响应内容的仅仅一小部分需要被取出，会使消费其他剩余内容的性能代价和连接可重用性代价太高，这时可以通过关闭响应来终止内容流（例子中可以看出，实体输入流仅仅读取了两个字节，就关闭了响应，也就是按需读取，而不是读取全部响应）。

~~~~java
CloseableHttpClient httpclient = HttpClients.createDefault();
HttpGet httpget = new HttpGet("http://localhost/");
CloseableHttpResponse response = httpclient.execute(httpget);
try {
HttpEntity entity = response.getEntity();
if (entity != null) {
        InputStream instream = entity.getContent();
    int byteOne = instream.read();
        int byteTwo = instream.read();
    // Do not need the rest
}
} finally {
    response.close();
}
~~~~

这样，连接将不会被重用，它分配的所有级别的资源将会被解除。

##### 消费实体内容

##### 生产实体内容

HttpClient提供了StringEntity，ByteArrayEntity，InputStreamEntity，and  FileEntity 类

请注意InputStreamEntity是不可重复的，因为它仅仅能够从数据流中读取一次。一般建议实现一个定制的HttpEntity类来代替使用一般的InputStreamEntity。FileEntity将会是一个好的出发点。

##### HTML表单模仿

~~~~java
List<NameValuePair> formparams = new ArrayList<NameValuePair>();
formparams.add(new BasicNameValuePair("param1", "value1"));
formparams.add(new BasicNameValuePair("param2", "value2"));
UrlEncodedFormEntity entity = new UrlEncodedFormEntity(formparams,
                                                          Consts.UTF_8);
HttpPost httppost = new HttpPost("http://localhost/handler.do");
httppost.setEntity(entity);
~~~~

##### 响应处理器

~~~~java
CloseableHttpClient httpclient = HttpClients.createDefault();
HttpGet httpget = new HttpGet("http://localhost/json");
ResponseHandler<MyJsonObject> rh = new ResponseHandler<MyJsonObject>() {
    @Override
    public JsonObject handleResponse(final HttpResponse response) throws IOException {
        StatusLine statusLine = response.getStatusLine();
        HttpEntity entity = response.getEntity();
        if (statusLine.getStatusCode() >= 300) {
            throw new HttpResponseException(statusLine.getStatusCode(),
                statusLine.getReasonPhrase());
        }
        if (entity == null) {
             throw new ClientProtocolException("Response contains no content");
        }
        Gson gson = new GsonBuilder().create();
        ContentType contentType = ContentType.getOrDefault(entity);
        Charset charset = contentType.getCharset();
        Reader reader = new InputStreamReader(entity.getContent(), charset);
        return gson.fromJson(reader, MyJsonObject.class);
    }
};
MyJsonObject myjson = client.execute(httpget, rh);
~~~~

### HttpClientUtil(工具类的创建)

使用HttpClient发送请求、接收响应很简单，一般需要如下几步即可：

1. 创建HttpClient对象
2. 创建请求方法的实例，并指定请求URL。如果需要发送GET请求，创建HttpGet对象；如果需要发送POST请求，创建HttpPost对象。
3. 如果需要发送请求参数，可调用HttpGetsetParams方法来添加请求参数；对于HttpPost对象而言，可调用setEntity(HttpEntity entity)方法来设置请求参数
4. 调用HttpClient对象的execute(HttpUriRequest request)发送请求，该方法返回一个HttpResponse对象
5. 调用HttpResponse的getAllHeaders()、getHeaders(String name)等方法可获取服务器的响应头；调用HttpResponse的getEntity()方法可获取HttpEntity对象，该对象包装了服务器的响应内容。程序可通过该对象获取服务器的响应内容。
6. 释放连接。无论执行方法是否成功，都必须释放连接

