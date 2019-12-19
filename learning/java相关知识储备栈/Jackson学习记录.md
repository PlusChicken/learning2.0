# Jackson学习记录JWT

分为三部分:Header(头信息),Payload(荷载信息,实际数据),Signature(由头信息+荷载信息+密钥 组合之后进行加密得到)

Header头信息通常包含两部分

type:代表token的类型,这里使用的是JWT类型.

alg:使用的Hash算法,例如HMAC SHA256或RSA

~~~~json
{
	"alg" : "HS256",
	"typ" : "JWT"
}
//经过Base64Url编码后形成第一部分
~~~~

Payload   一个token的第二部分是荷载信息,它包含一些声明Claim(实体的描述,通常是一个User信息,还包括一些其他的元数据)

声明分三类:

1)Reserved Claims,这是一套预定义的声明,并不是必须的,包括(iss(issuer)、sub(subject)、aud(audience))等

2)Plubic Claims

3)Private Claims,交换信息的双方自定义的声明

~~~~json
{
	"sub" : "1234567890",
    "name" : "Jhon Doe",
    "admin" : true
}
//经过Base64Url编码后形成第二部分
~~~~

~~~~java
iss: jwt签发者
sub: jwt所面向的用户
aud: 接收jwt的一方
exp: jwt的过期时间,这个过期时间必须要大于签发时间
nbf: 定义在什么时间之前,该jwt都是不可用的
iat: jwt的签发时间
jti: jwt的唯一身份标识,主要用来作为一次性token,从而回避重放攻击
~~~~



3)signature 使用header中指定的算法将编码后的header、编码后的payload、一个secret进行加密

例如：使用的是HMAC SHA256算法，大致流程类似于：HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload),secret)

这个signature字段被用来确认JWT信息的发送者是谁，并保证信息没有被修改

JSON Web Tokens包含了必要的用户信息，减少了对数据库进行多次查询的需要。

由于没有使用Cookies，Cross-Origin Resource Sharing(CORS)，跨域的资源访问不会成为问题。

![jwt-diagram](..\picture\jwt-diagram.png)

注:secret是保存在服务器端的，jwt的签发生成也是在服务器端的，secret就是用来进行jwt的签发和jwt的验证，所以，它就是你服务端的私钥，在任何场景都不应该流露出去。一旦客户端得知这个secret, 那就意味着客户端是可以自我签发jwt了。

加密代码

~~~~java
public class JwtToken {
	public static String createToken() throws Exception{
		Map<String, Object> map = new HashMap<String, Object>();
		map.put("alg", "HS256");
		map.put("typ", "JWT");
		String token = JWT.create()
				.withHeader(map)//header
				.withClaim("name", "zwz")//payload
				.withClaim("age", "18")
				.sign(Algorithm.HMAC256("secret"));//加密
		return token;
	}
}
~~~~

验证代码

~~~~java
public static void verifyToken(String token,String key) throws Exception{
		JWTVerifier verifier = JWT.require(Algorithm.HMAC256(key))
		        .build(); 
		    DecodedJWT jwt = verifier.verify(token);
		    Map<String, Claim> claims = jwt.getClaims();
		    System.out.println(claims.get("name").asString());
	}
~~~~

