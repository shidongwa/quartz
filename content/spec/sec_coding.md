<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

# Java 安全编码规范-1.1.9 by k4n5ha0

<!-- code_chunk_output -->

- [Java 安全编码规范-1.1.9 by k4n5ha0](#java-安全编码规范-119-by-k4n5ha0)
  - [编写依据与参考文件：](#编写依据与参考文件)
    - [1.《信息安全技术 应用软件安全编程指南》（国标 GBT38674-2020）](#1信息安全技术-应用软件安全编程指南国标-gbt38674-2020)
    - [2.《Common Weakness Enumeration》 - 国际通用计算机软件缺陷字典](#2common-weakness-enumeration---国际通用计算机软件缺陷字典)
    - [3.《OWASP Top 10 2017》 - 2017 年十大 Web 应用程序安全风险](#3owasp-top-10-2017---2017-年十大-web-应用程序安全风险)
    - [4.《fortify - 代码审计规则》](#4fortify---代码审计规则)
    - [5.《java 开发手册》（阿里巴巴出品）](#5java-开发手册阿里巴巴出品)
- [第一条 设计开发必须符合概要设计及安全防护方案](#第一条-设计开发必须符合概要设计及安全防护方案)
  - [项目管理要求 ↓：](#项目管理要求-)
- [第二条 上线代码必须进行严格的安全测试并进行软著备案](#第二条-上线代码必须进行严格的安全测试并进行软著备案)
  - [项目管理要求 ↓：](#项目管理要求--1)
- [第三条 严格限制帐号访问权限](#第三条-严格限制帐号访问权限)
  - [程序设计要求 ↓：](#程序设计要求-)
- [第四条 提供完备的安全审计功能](#第四条-提供完备的安全审计功能)
  - [程序设计要求 ↓：](#程序设计要求--1)
- [第五条 采取有效措施保证认证安全](#第五条-采取有效措施保证认证安全)
  - [程序设计要求 ↓：](#程序设计要求--2)
- [第六条 保证代码简洁、注释明确](#第六条-保证代码简洁-注释明确)
  - [安全编码要求 ↓：](#安全编码要求-)
- [第七条 使用安全函数和安全规范验证所有外部输入](#第七条-使用安全函数和安全规范验证所有外部输入)
  - [安全编码要求 ↓：](#安全编码要求--1)
  - [HTTP 参数污染](#http-参数污染)
  - [不受信任的查询字符串](#不受信任的查询字符串)
  - [不受信任的 HTTP 请求头](#不受信任的-http-请求头)
    - [1.不受信任的 Cookie 头](#1不受信任的-cookie-头)
    - [2.不受信任的 Content-Type 请求头](#2不受信任的-content-type-请求头)
    - [3.不受信任的 HOST 请求头](#3不受信任的-host-请求头)
    - [4.不受信任的 User-Agent 请求头](#4不受信任的-user-agent-请求头)
    - [5.不受信任的 IP 请求头](#5不受信任的-ip-请求头)
  - [缺少 CSRF 攻击的防护](#缺少-csrf-攻击的防护)
  - [客户端请求伪造](#客户端请求伪造)
  - [服务端请求伪造](#服务端请求伪造)
  - [恶意的命令注入](#恶意的命令注入)
  - [不安全的反序列化](#不安全的反序列化)
    - [1.配置全局反序列化白名单](#1配置全局反序列化白名单)
    - [2.jackson 安全反序列化编码示例](#2jackson-安全反序列化编码示例)
    - [3.fastjson 安全反序列化编码示例](#3fastjson-安全反序列化编码示例)
      - [使用 fastjson 1.x 系列注意事项](#使用-fastjson-1x-系列注意事项)
      - [使用 fastjson 2.x 系列注意事项](#使用-fastjson-2x-系列注意事项)
    - [4.XStream 安全反序列化编码示例](#4xstream-安全反序列化编码示例)
    - [5.shiro 安全反序列化编码示例](#5shiro-安全反序列化编码示例)
  - [XML 外部实体（XXE）攻击](#xml-外部实体xxe攻击)
    - [1.XStream](#1xstream)
    - [2.javax.xml.parsers.DocumentBuilderFactory](#2javaxxmlparsersdocumentbuilderfactory)
    - [3.org.jdom2.input.SAXBuilder](#3orgjdom2inputsaxbuilder)
    - [4.javax.xml.parsers.SAXParserFactory](#4javaxxmlparserssaxparserfactory)
    - [5.org.dom4j.io.SAXReader](#5orgdom4jiosaxreader)
    - [6.org.xml.sax.XMLReader](#6orgxmlsaxxmlreader)
    - [7.javax.xml.transform.sax.SAXTransformerFactory](#7javaxxmltransformsaxsaxtransformerfactory)
    - [8.javax.xml.validation.SchemaFactory](#8javaxxmlvalidationschemafactory)
    - [9.javax.xml.transform.TransformerFactory](#9javaxxmltransformtransformerfactory)
  - [XPath 注入](#xpath-注入)
  - [EL 表达式引擎代码注入](#el-表达式引擎代码注入)
  - [JS 脚本引擎代码注入](#js-脚本引擎代码注入)
  - [JavaBeans 属性注入](#javabeans-属性注入)
  - [不安全的对象绑定](#不安全的对象绑定)
  - [正则表达式 DOS（ReDOS）](#正则表达式-dosredos)
  - [跨站脚本攻击（XSS）](#跨站脚本攻击xss)
- [第八条 必须过滤上传文件](#第八条-必须过滤上传文件)
  - [安全编码要求 ↓：](#安全编码要求--2)
  - [潜在的路径遍历（读取文件）](#潜在的路径遍历读取文件)
  - [潜在的路径遍历（写入文件）](#潜在的路径遍历写入文件)
- [第九条 确保多线程编程的安全性](#第九条-确保多线程编程的安全性)
  - [安全编码要求 ↓：](#安全编码要求--3)
  - [竞争条件](#竞争条件)
- [第十条 设计错误、异常处理机制](#第十条-设计错误-异常处理机制)
  - [安全编码要求 ↓：](#安全编码要求--4)
- [第十一条 数据库操作使用参数化请求方式](#第十一条-数据库操作使用参数化请求方式)
  - [安全编码要求 ↓：](#安全编码要求--5)
  - [SQL 注入](#sql-注入)
    - [1.使用 Druid 进行 sql 注入防护](#1使用-druid-进行-sql-注入防护)
    - [2.jdbc 安全编码规范](#2jdbc-安全编码规范)
    - [3.Mybatis 安全编码规范](#3mybatis-安全编码规范)
  - [LDAP 注入](#ldap-注入)
- [第十二条 禁止在源代码中写入口令、服务器 IP 等敏感信息](#第十二条-禁止在源代码中写入口令-服务器-ip-等敏感信息)
  - [安全编码要求 ↓：](#安全编码要求--6)
  - [硬编码密码](#硬编码密码)
  - [硬编码密钥](#硬编码密钥)
- [第十三条 为所有敏感信息采用加密传输](#第十三条-为所有敏感信息采用加密传输)
  - [程序设计要求 ↓：](#程序设计要求--3)
    - [名称类信息脱敏规则：](#名称类信息脱敏规则)
    - [联系类信息脱敏规则：](#联系类信息脱敏规则)
    - [证件号脱敏规则：](#证件号脱敏规则)
  - [安全编码要求 ↓：](#安全编码要求--7)
  - [接受任何证书的 TrustManager](#接受任何证书的-trustmanager)
  - [接受任何签名证书的 HostnameVerifier](#接受任何签名证书的-hostnameverifier)
- [第十四条 使用可信的密码算法](#第十四条-使用可信的密码算法)
  - [程序设计要求 ↓：](#程序设计要求--4)
  - [禁止使用弱加密](#禁止使用弱加密)
  - [安全编码要求 ↓：](#安全编码要求--8)
  - [可预测的伪随机数生成器](#可预测的伪随机数生成器)
  - [错误的十六进制串联](#错误的十六进制串联)
- [第十五条 禁止在日志、表单、cookie 等文件中记录口令、银行账号、通信内容等敏感数据](#第十五条-禁止在日志-表单-cookie-等文件中记录口令-银行账号-通信内容等敏感数据)
  - [安全编码要求 ↓：](#安全编码要求--9)
  - [Cookie 中的潜在敏感数据](#cookie-中的潜在敏感数据)
  - [日志伪造](#日志伪造)
  - [HTTP 响应截断](#http-响应截断)
- [第十六条 禁止高风险的服务及协议](#第十六条-禁止高风险的服务及协议)
  - [程序设计要求 ↓：](#程序设计要求--5)
  - [安全编码要求 ↓：](#安全编码要求--10)
  - [不安全的 HTTP 动词](#不安全的-http-动词)
- [第十七条 避免异常信息泄漏](#第十七条-避免异常信息泄漏)
  - [安全编码要求 ↓：](#安全编码要求--11)
  - [意外的属性泄露](#意外的属性泄露)
  - [不安全的 SpringBoot Actuator 暴露](#不安全的-springboot-actuator-暴露)
  - [不安全的 Swagger 暴露](#不安全的-swagger-暴露)
- [第十八条 严格会话管理](#第十八条-严格会话管理)
  - [安全编码要求 ↓：](#安全编码要求--12)
  - [缺少 HttpOnly 标志的 Cookie](#缺少-httponly-标志的-cookie)
  - [不安全的 CORS 策略](#不安全的-cors-策略)
  - [不安全的永久性 Cookie](#不安全的永久性-cookie)
  - [不安全的广播（Android）](#不安全的广播android)

<!-- /code_chunk_output -->

---

## 编写依据与参考文件：

### 1.《信息安全技术 应用软件安全编程指南》（国标 GBT38674-2020）

### 2.《Common Weakness Enumeration》 - 国际通用计算机软件缺陷字典

### 3.《OWASP Top 10 2017》 - 2017 年十大 Web 应用程序安全风险

### 4.《fortify - 代码审计规则》

### 5.《java 开发手册》（阿里巴巴出品）

---

# 第一条 设计开发必须符合概要设计及安全防护方案

## 项目管理要求 ↓：

- 所有项目必须参照《概要设计》编写《安全防护方案》，两者均评审通过后**才能**启动编码工作。
- java8 版本应不低于 jdk-1.8_291
- java11 版本应不低于 jdk-11.0.11

---

# 第二条 上线代码必须进行严格的安全测试并进行软著备案

## 项目管理要求 ↓：

- 所有项目必须完成安全自测和第三方安全测试，并完成软著相关工作才能上线运行，上线运行版本必须与测试通过版本一致。

---

# 第三条 严格限制帐号访问权限

## 程序设计要求 ↓：

- 应用程序除公共功能外，禁止不同角色之间可以跨角色访问其他角色的功能。例如：
  - 某互斥业务名为“发票打印”涉及三个子菜单，该业务是角色“会计”的专有功能。
  - 角色“会计”可以看到发票打印相关的三个子菜单并正常操作。
  - 角色“出纳”无法看到三个子菜单并无法访问该三个子菜单中对应的所有后端接口。
  - 如果“出纳”可以访问或操作“会计”的“发票打印”或其他“会计”专有的功能则应判定为越权。
- 当用户访问无权限的菜单 url 或者接口 url：
  - 后台的 HTTP 响应码禁止返回 200。
  - HTTP 的响应包 body 内容必须返回“无权限”。
- 原则禁止存在“记住密码”的功能。

---

# 第四条 提供完备的安全审计功能

## 程序设计要求 ↓：

- 用户在系统中只要在页面中存在点击、输入、拖拽等操作行为，日志记录中应当针对操作行为产生日志。
- 一条日志所包含的字段应包括：
  - 事件的日期（年月日）
  - 时间（时分秒）
  - 事件类型（系统级、业务级二选一）
  - 登录 ID
  - 姓名
  - IP 地址
  - 事件描述（用户主体对什么客体执行了什么操作？该操作的增删改查的内容又是什么？）
  - 事件结果（成功、失败）

---

# 第五条 采取有效措施保证认证安全

## 程序设计要求 ↓：

- 如果用户连续登录失败（最多失败 10 次），应将该用户锁定，禁止其登陆。
- 外网系统用户登录时，应使用短信进行二次验证可以保证用户登录的安全性。
- 用户登录失败时，应提示“用户名或口令错误”，禁止提示“用户名不存在”或“登录口令错误”。
- 用户登录时，必须使用合规的加密方案加密传输用户的登录名和密码。

```
合规的双向加密数据的传输方案：
   1）后端生成非对称算法（国密SM2、RSA2048）的公钥B1、私钥B2，前端访问后端获取公钥B1。
   2）前端每次发送请求前，随机生成对称算法（国密SM4、AES256）的密钥A1。
   3）公钥、私钥可以全系统固定为一对，前端可以储存公钥，但私钥不能保存在后端数据库中。
   4）前端用步骤2的密钥A1加密所有业务数据生成en_data，用步骤1获取的公钥B1加密密钥A1生成en_key。
   5）前端用哈希算法对en_data + en_key的值形成一个校验值check_hash。
   6）前端将en_data、en_key、check_hash三个参数包装在同一个http数据包中发送到后端。
   7）后端获取三个参数后先判断哈希值check_hash是否匹配en_data + en_key以验证完整性。
   8）后端用私钥B2解密en_key获取本次请求的对称算法的密钥A1。
   9）后端使用步骤8获取的密钥A1解密en_data获取实际业务数据。
  10）后端处理完业务逻辑后，将需要返回的信息使用密钥A1进行加密后回传给前端。
  11）加密数据回传给前端后，前端使用A1对加密的数据进行解密获得返回的信息。
  12）步骤2随机生成的密钥A1已经使用完毕，前端应将其销毁。
```

- 前端发送请求时必须设计防篡改和防重放攻击的安全逻辑，后端必须开展对应的校验。

```
合规的防篡改和防重放攻击的传输方案：
   1）客户端获取公钥时应同时获取后端服务器时间，保证客户端和服务器时间一致。
   2）前端每次发送请求前，应在header请求头中添加时间戳字段。
   3）通过 url + 时间戳 + 用户token + http请求体（可以为空） 产生 sign 签名。
   4）将 sign 签名添加到header请求头中后发送请求。
   5）后端校验 sign 签名是否正确，如果签名校验不通过应提示"数据被篡改"并丢弃当前请求。
   6）如果时间戳的时间和服务器时间相差大于10秒，应丢弃当前请求。
   7）后台应缓存每一个sign值10秒，在10秒内如果出现包含同一个sign值的请求，应丢弃当前请求。
   8）用户token中部分数据（禁止全部）应存储在sessionStorage中，以保证页面关闭后登录失效。
```

---

# 第六条 保证代码简洁、注释明确

## 安全编码要求 ↓：

- 应持续执行代码审计工作。
- 代码中禁止出现 goto 语句。
- 应禁止使用递归并及时去除程序中冗余的功能代码。

---

# 第七条 使用安全函数和安全规范验证所有外部输入

## 安全编码要求 ↓：

## HTTP 参数污染

如果应用程序未正确校验用户输入的数据，则恶意用户可能会破坏应用程序的逻辑以执行针对客户端或服务器端的攻击。

**脆弱代码 1：**

```
// 攻击者可以提交 lang 的内容为：
// en&user_id=1#
// 致使攻击者可以随意篡改 user_id 的值

String lang = request.getParameter("lang");
GetMethod get = new GetMethod("http://www.host.com");

// 攻击者提交 lang=en&user_id=1#&user_id=123 可覆盖原始 user_id 的值
get.setQueryString("lang=" + lang + "&user_id=" + user_id);
get.execute();
```

**解决方案 1：**

```
// 参数化绑定
URIBuilder uriBuilder = new URIBuilder("http://www.host.com/viewDetails");
uriBuilder.addParameter("lang", input);
uriBuilder.addParameter("user_id", userId);

HttpGet httpget = new HttpGet(uriBuilder.build().toString());
```

**脆弱逻辑 2：**

```
订单系统计算订单的价格
步骤1:
订单总价 = 商品1单价 * 商品1数量 + 商品2单价 * 商品2数量 + ...
步骤2:
钱包余额 = 钱包金额 - 订单总价

当攻击者将商品数量都篡改为负数，导致步骤1的订单总价为负数。而负负得正，攻击者不仅买入了商品并且钱包金额也增长了。
```

**解决方案 2：**

```
应在后台严格校验订单中每一个输入参数的长度、格式、逻辑、特殊字符。
```

**整体解决方案：**

- 应按照长度、格式、逻辑以及特殊字符 4 个维度对**每一个**输入参数进行安全校验，然后再将其传递给敏感的 API。
- 原则上数据库主键不能使用自增纯数字，应使用 uuid 或雪花算法作为数据库表主键以保证唯一性和不可预测性。
- 身份信息应使用当前请求的用户 session 或 token 安全的获取，禁止直接信任用户提交的身份信息。
- 安全获取用户身份后，应对请求的数据资源进行逻辑判断，防止用户操作无权限的数据资源。

## 不受信任的查询字符串

查询字符串是 GET 参数名称和值的串联，可以传入非预期参数。

**风险：**

例如 URL 请求 `/app/servlet.htm?a=1&b=2` 则对应查询字符串提取为 `a=1&b=2`
那么 `HttpServletRequest.getParameter() HttpServletRequest.getQueryString()` 获取的值都可能是不安全的。

**解决方案：**

- 查询字符串**只能**在页面渲染时使用，**禁止**将查询字符串关联**任何**业务请求。
- 应按照长度、格式、逻辑以及特殊字符 4 个维度对**每一个**输入的查询字符串参数进行安全校验，然后再将其传递给敏感的 API。

## 不受信任的 HTTP 请求头

攻击者可以恶意篡改或伪造所有 http 请求头中的参数，达到破坏应用程序的逻辑以执行针对客户端或服务器端的目的。

### 1.不受信任的 Cookie 头

- HttpServletRequest.getRequestedSessionId() 可以返回 JSESSIONID 的值 。
- 该值通常是字母、数字的组合值。（例如 JSESSIONID=jp6q31lq2myn）
- 攻击者向后端发起请求时可以恶意伪造或篡改该值。例如：

```
GET /somePage HTTP/1.1
Host: yourwebsite.com
User-Agent: Mozilla/5.0
Cookie: JSESSIONID=Any value of the user's choice!!??'''">
```

**脆弱代码：**

```
Cookie[] cookies = request.getCookies();
for (int i =0; i< cookies.length; i++) {
	Cookie c = cookies[i];
	if (c.getName().equals("authenticated") && Boolean.TRUE.equals(c.getValue())) {
	authenticated = true;
	}
}
```

以上代码直接从 cookie 中而不是 session 中提取了参数作为登录状态的判断，导致攻击者可以伪造登录状态。

**解决方案：**

- 身份信息应使用当前请求的用户 session 或 token 安全的获取，而不是直接采用用户提交的身份信息。
- 身份信息应仅用于查看其值是否与请求的资源权限（包括菜单 URL、接口 URL、业务数据）是否匹配。
- 如果权限不匹配，应停止业务逻辑并立刻告警，同时在审计日志中记录一条越权日志。
- 身份信息的值禁止记录到日志中，否则内部人员可以劫持处于活动状态的用户权限。

### 2.不受信任的 Content-Type 请求头

HTTP 请求头 Content-Type 可以由恶意的攻击者控制。因此，HTTP 的 Content-Type 值不应在任何重要的逻辑流程中使用。

### 3.不受信任的 HOST 请求头

ServletRequest.getServerName() 和 HttpServletRequest.getHeader("Host") 具有相同的逻辑，即提取 Host 请求头。

```
GET /testpage HTTP/1.1
Host: www.example.com
```

因为恶意的攻击者可以伪造 Host 请求头，所以 HTTP 的 Host 值不应在任何重要的逻辑流程中使用。

### 4.不受信任的 User-Agent 请求头

请求头 User-Agent 很容易被客户端伪造，不建议基于 User-Agent 的值采用不同的安全校验逻辑。

### 5.不受信任的 IP 请求头

以下 IP 请求头，很容易被客户端伪造，可能导致 IP 地址欺骗。

- X-Forwarded-For
- X-Originating-IP
- X-Real-IP
- x-Remote-addr
- x-Remote-IP
- 等其他

**解决方案 1：**

应用程序与用户间无代理时, 应使用 getRemoteAddr 函数获取用户 ip。

**解决方案 2：**

应用程序与用户间存在代理时, 应使用代理头获取用户 ip。

```
private String getIpAddr(HttpServletRequest request) {
	String ip = request.getHeader("x-forwarded-for");
	System.out.println("x-forwarded-for ip: " + ip);
	if (ip != null && ip.length() != 0 && !"unknown".equalsIgnoreCase(ip)) {
		// 多次反向代理后会有多个ip值，第一个ip才是真实ip
		if( ip.indexOf(",")!=-1 ){
			ip = ip.split(",")[0];
		}
	}
	if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
		ip = request.getHeader("Proxy-Client-IP");
		System.out.println("Proxy-Client-IP ip: " + ip);
	}
	if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
		ip = request.getHeader("WL-Proxy-Client-IP");
		System.out.println("WL-Proxy-Client-IP ip: " + ip);
	}
	if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
		ip = request.getHeader("HTTP_CLIENT_IP");
		System.out.println("HTTP_CLIENT_IP ip: " + ip);
	}
	if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
		ip = request.getHeader("HTTP_X_FORWARDED_FOR");
		System.out.println("HTTP_X_FORWARDED_FOR ip: " + ip);
	}
	if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
		ip = request.getHeader("X-Real-IP");
		System.out.println("X-Real-IP ip: " + ip);
	}
	if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
		ip = request.getRemoteAddr();
		System.out.println("getRemoteAddr ip: " + ip);
	}
	System.out.println("获取客户端ip: " + ip);
	return ip;
}
```

## 缺少 CSRF 攻击的防护

**风险 1：**

- 恶意用户可以将恶意值分配给 Referer 请求头进行 Referer 请求伪造攻击。
- 如果请求是从另一个安全的来源（HTTPS）发起的，则 Referer 将不存在。

**脆弱代码 2：**

```
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

	@Override
	protected void configure(HttpSecurity http) throws Exception {

		// 禁用 csrf 保护会导致 csrf 攻击的产生
		http.csrf().disable();
	}
}
```

**解决方案：**

- 禁用 Spring Security 的 CSRF 保护对于标准 Web 应用程序是不安全的。
- 任何用户权限判读都应基于 Session 或 Token。
- 如果后台判断当前用户 Session 或 Token 是否成功登录，如登录成功则该用户的 Referer 或 Origin 来源值两者其一不能为空，最后判断不为空的来源值是否与可请求地址白名单逻辑匹配。
- 应避免来源值欺骗攻击，例如 sec_coding.com.evil.com 部分匹配 sec_coding.com 的情况。
- 任何 CSRF 防护都不应仅仅只基于 Referer 或 Origin 来源值，应采用一次性的表单 token 防止重放攻击。

## 客户端请求伪造

当 Web 应用程序将用户重定向并转发到其他页面或其他外部网站，如果不验证这些页面的可信度，攻击者可以将受害者重定向到网络钓鱼或恶意软件站点，或者恶意利用转发来访问未经授权的页面。

**脆弱代码 1：**

```
@RequestMapping("/redirect")
public String redirect(@RequestParam("url") String url) {

	// 不执行校验就直接跳转
	return "redirect:" + url;
}
```

**脆弱代码 2：**

```
protected void doGet(HttpServletRequest req,
	HttpServletResponse resp) throws ServletException, IOException {

	// 不执行校验就直接跳转
	resp.sendRedirect(req.getParameter("redirectUrl"));
}
```

1. 诱使用户访问恶意 URL（将 website 伪造成 vvebsite）：
   - http://website.com/login?redirect=http://evil.vvebsite.com/fake/login
2. 将用户重定向到伪造的登录页面，该页面看起来像他们信任的站点：
   - http://evil.vvebsite.com/fake/login
3. 用户输入其凭据。
4. 恶意站点窃取用户的凭据，并将其重定向到原始网站。

**解决方案：**

完整 URL 格式： `protocol://hostname[:port]/path/[;parameters][?query]#fragment`

- **禁止**直接接受来自用户的 URL 目标，所有 URL 请求应使用白名单校验。
- 原则上应接受相对路径请求。而绝对路径的 URL 应尽可能使用白名单校验**每一个**格式参数。
- 验证 URL 的域名部分时应避免 sec_coding.com.evil.com 部分匹配 sec_coding.com 的恶意伪造。
- 可以使用哈希映射到目标地址，并使用哈希在 URL 白名单中查找合法目标。

## 服务端请求伪造

当 Web 应用程序根据用户请求对数据资源发起请求，如果不对该数据资源执行安全校验，攻击者可能获取敏感数据资源。

**脆弱代码：**

```
@WebServlet( "/downloadServlet" )
public class downloadServlet extends HttpServlet {
	protected void doPost( HttpServletRequest request,
			HttpServletResponse response ) throws ServletException, IOException{
		this.doGet( request, response );
	}
	protected void doGet( HttpServletRequest request,
			HttpServletResponse response ) throws ServletException, IOException{
		String filename = "1.txt";

		// 没有校验 url 变量的安全性
		String url = request.getParameter( "url" );
		response.setHeader( "content-disposition", "attachment;fileName=" + filename );
		int len;
		OutputStream outputStream = response.getOutputStream();

		// 直接使用 url 变量导致任意文件读取
		URL file = new URL( url );
		byte[] bytes = new byte[1024];
		InputStream inputStream = file.openStream();
		while ( (len = inputStream.read( bytes ) ) > 0 )
		{
			outputStream.write( bytes, 0, len );
		}
	}
}
```

使用以下请求可以下载服务器硬盘上的文件

```
http://localhost:8080/downloadServlet?url=file:///c:\1.txt
```

**解决方案：**

完整 URL 格式： `protocol://hostname[:port]/path/[;parameters][?query]#fragment`

- **禁止**直接接受来自用户的 URL 目标，所有 URL 请求应使用白名单校验。
- 原则上应接受相对路径请求。而绝对路径的 URL 应尽可能使用白名单校验**每一个**格式参数。
- 验证 URL 的域名部分时应避免 sec_coding.com.evil.com 部分匹配 sec_coding.com 的恶意伪造。
- 可以使用哈希映射到目标地址，并使用哈希在 URL 白名单中查找合法目标。

## 恶意的命令注入

如果将未经过滤的命令参数传递给执行命令的 API，可以导致任意命令执行。

**脆弱代码：**

```
import java.lang.Runtime;

Runtime r = Runtime.getRuntime();
r.exec("/bin/sh -c some_tool" + input)
```

如果将 input 的内容从 `1.txt` 篡改为 `1.txt && reboot` ，则可以导致服务器重启。

**解决方案：**

- 应禁止用户输入危险的特殊符号 `` & | ; $ > < ` ' " ! ? * # ``
- 应按照长度、格式、逻辑以及特殊字符 4 个维度对命令内容进行**白名单**校验。

## 不安全的反序列化

攻击者通过将恶意数据传递到反序列化 API，可导致：读写任意文件、执行系统命令、探测或攻击内网等危害。

**解决方案：**

确保使用安全的组件和安全的编码执行反序列化操作。

### 1.配置全局反序列化白名单

所有 java 应用程序应配置全局反序列化白名单，保证只有必要的类才能被反序列化。

```
// 针对每一次反序列化的输入数据，配置安全限制
maxdepth=value // 每一个内置对象的最大深度（一次反序列化可能会有多个内置对象）
maxrefs=value  // 对象引用的最大上限
maxbytes=value // 输入数据的字节数上限
maxarray=value // 反序列化数组时的最大上限

// 以下示例介绍了限制反序列化的类名称的配置方法

// 允许输入字节数最大为100，允许唯一类 org.example.Teacher ，并阻止其它一切的类
jdk.serialFilter=maxbytes=100;org.example.Teacher;!*

// 允许输入字节数最大为100，允许 org.example. 下的所有类，并阻止其它一切的类
jdk.serialFilter=maxbytes=100;org.example.*;!*

// 允许输入字节数最大为100，允许 org.example. 下的所有类和子类，并阻止其它一切的类
jdk.serialFilter=maxbytes=100;org.example.**;!*

// 允许一切类
jdk.serialFilter=*;

;    作为表达式的分隔符
.*   代表当前包下的所有类
.**  代表当前包下所有类和所有子类
！   代表取反，禁止匹配符号后的表达式被反序列化
*    通配符
```

- 使用配置文件配置白名单

```
jdk11+：%JAVA_HOME%\conf\security\java.security
jdk8： %JAVA_HOME%\jre\lib\security\java.security
```

- 使用启动参数配置白名单

```
java -Djdk.serialFilter=maxbytes=100;org.example.**;!*
```

- 使用代码配置白名单

```
Properties props = System.getProperties();
props.setProperty("jdk.serialFilter", "maxbytes=100;org.example.**;!*");
```

参考链接：

- https://www.anquanke.com/post/id/238085
- http://openjdk.java.net/jeps/290
- https://docs.oracle.com/javase/9/docs/api/java/io/ObjectInputFilter.FilterInfo.html#references--

### 2.jackson 安全反序列化编码示例

- jackson 版本应不低于 2.11.x
- 禁用 enableDefaultTyping 方法
- 禁用 JsonTypeInfo 注解
- 如需使用 jackson 快速存储数据到 redis 中应使用 activateDefaultTyping + 白名单过滤器

```
// jackson白名单过滤
ObjectMapper om = new ObjectMapper();
BasicPolymorphicTypeValidator validator = BasicPolymorphicTypeValidator.builder()

	// 信任 com.xxxx. 包下的类
	.allowIfBaseType("com.xxxx.")
	.allowIfSubType("com.xxxx.")

	// 信任 Collection、Map 等基础数据结构
	.allowIfSubType(Collection.class)
	.allowIfSubType(Number.class)
	.allowIfSubType(Map.class)
	.allowIfSubType(Temporal.class)
	.allowIfSubTypeIsArray()
	.build();
om.activateDefaultTyping(validator,ObjectMapper.DefaultTyping.NON_FINAL, JsonTypeInfo.As.PROPERTY);
```

- jackson 能稳定处理 json 和 xml 数据，应尽量只引入和使用 jackson 进行序列化和反序列化操作，降低安全管理成本。

### 3.fastjson 安全反序列化编码示例

#### 使用 fastjson 1.x 系列注意事项

- 使用 fastjson 1.x 系列时，版本应不低于 1.2.83
- 需要使用@type 时，应在系统启动阶段添加以下代码，防止被出网扫描方式探测

```
ParserConfig.getGlobalInstance().addAutoTypeCheckHandler((typeName, expectClass, features) -> {
    if (null != typeName && typeName.contains("java.net.")) {
        throw new JSONException("not support autoType : " + typeName);
    }
    return null;
});
```

- 无需使用@type 时，应开启 fastjson 的 safeMode 模式完全禁用@type

```
1. 在代码中配置
    ParserConfig.getGlobalInstance().setSafeMode(true);
    如果使用new ParserConfig的方式，需要注意单例处理，否则会导致低性能full gc

2. 加上JVM启动参数
    -Dfastjson.parser.safeMode=true

3. 通过fastjson.properties文件配置。
    通过类路径的fastjson.properties文件配置，配置方式如下：
    fastjson.parser.safeMode=true
```

#### 使用 fastjson 2.x 系列注意事项

- @type 安全机制
  - 2.x 开始必须显式打开才能使用 @type 功能，打开后禁止暴露到公网的场景下使用
  - 应使用内置过滤器对 @type 功能进行白名单限制

```
public class FastJsonRedisSerializer<T> implements RedisSerializer<T> {
    static final Filter autoTypeFilter = JSONReader.autoTypeFilter(
            // 按需加上需要支持自动类型的类名前缀，范围越小越安全
            "org.springframework.security.core.authority.SimpleGrantedAuthority"
    );

    private Class<T> clazz;

    public FastJsonRedisSerializer(Class<T> clazz) {
        super();
        this.clazz = clazz;
    }

    @Override
    public byte[] serialize(T t) {
        if (t == null) {
            return new byte[0];
        }
        return JSON.toJSONBytes(t, JSONWriter.Feature.WriteClassName);
    }

    @Override
    public T deserialize(byte[] bytes) {
        if (bytes == null || bytes.length <= 0) {
            return null;
        }

        return JSON.parseObject(bytes, clazz, autoTypeFilter);
    }
}
```

- 启动 SafeMode 会完全禁用 @type 功能，就算程序中显示指定也无法使用该功能

```
-Dfastjson2.parser.safeMode=true
```

参考地址： https://github.com/alibaba/fastjson2/wiki/fastjson2_autotype_cn

### 4.XStream 安全反序列化编码示例

- XStream 版本应不低于 1.4.20
- 初始化 XStream 应使用默认解析器
- 使用 XStream 的 setupDefaultSecurity 安全模式限制输入的数据

```
// 使用默认解析器
XStream xStream = new XStream();

// 必须开启安全模式，安全模式采用白名单限制输入的数据类型
XStream.setupDefaultSecurity(xStream);

// 在白名单内添加一些基本数据类型
xstream.addPermission(NullPermission.NULL);
xstream.addPermission(PrimitiveTypePermission.PRIMITIVES);
xstream.allowTypeHierarchy(Collection.class);

// 在白名单中添加可信任包内所有的子类
xstream.allowTypesByWildcard(new String[] {
	Blog.class.getPackage().getName()+".*"
});
```

官方参考：

- http://x-stream.github.io/security.html#implicit
- http://x-stream.github.io/security.html#framework

### 5.shiro 安全反序列化编码示例

- 原则应禁止使用 shiro
- shiro 版本应不低于 1.13.0
- 初始化 RememberMe 的密钥应使用 192 位或 256 位
- 初始化 RememberMe 的密钥禁止外泄

**脆弱代码：**

```
public CookieRememberMeManager rememberMeManager() {
	CookieRememberMeManager cookieRememberMeManager = new CookieRememberMeManager();
	cookieRememberMeManager.setCookie(rememberMeCookie());

	// rememberMe cookie加密的密钥
	// 默认AES算法，密钥可选长度(128 192 256 位)
	// 不应使用外泄的128位密钥
	cookieRememberMeManager.setCipherKey(Base64.decode("1QWLxg+NYmxraMoxAXu/Iw=="));
	return cookieRememberMeManager;
}
```

**解决方案：**

```
public CookieRememberMeManager rememberMeManager() {
	CookieRememberMeManager cookieRememberMeManager = new CookieRememberMeManager();
	cookieRememberMeManager.setCookie(rememberMeCookie());

	// rememberMe cookie加密的密钥，建议每个项目都不一样
	// 应重新生成 192 位或 256 位密钥，或从配置中心统一获取密钥
	cookieRememberMeManager.setCipherKey(GenerateCipherKey.generateNewKey());
	return cookieRememberMeManager;
}

public static class GenerateCipherKey {

	public static byte[] generateNewKey() {
	KeyGenerator kg;
	try {
	    kg = KeyGenerator.getInstance("AES");
	} catch (NoSuchAlgorithmException var5) {
	    String msg = "Unable to acquire AES algorithm.  This is required to function.";
	    throw new IllegalStateException(msg, var5);
	}

	// 密钥应选长度(192 或 256)位
	kg.init(256);
	SecretKey key = kg.generateKey();
	return key.getEncoded();
	}
}
```

## XML 外部实体（XXE）攻击

当 XML 解析器处理从不受信任的来源接收到的 XML 时支持 XML 实体，可能会发生 XML 外部实体（XXE）攻击。

**脆弱代码：**

```
public void parseXML(InputStream input) throws XMLStreamException {

	XMLInputFactory factory = XMLInputFactory.newFactory();
	XMLStreamReader reader = factory.createXMLStreamReader(input);
	[...]
}
```

**解决方案：**

读取外部传入 XML 文件时，XML 解析器初始化过程中配置关闭 DTD 解析。

```
"http://apache.org/xml/features/nonvalidating/load-external-dtd", false
"http://xml.org/sax/features/external-general-entities", false
"http://xml.org/sax/features/external-parameter-entities", false
```

如果不需要 inline DOCTYPE 声明，应使用以下配置将其完全禁用。

```
"http://apache.org/xml/features/disallow-doctype-decl", true
```

以上配置等价于：

```
XMLConstants.ACCESS_EXTERNAL_DTD, ""
XMLConstants.ACCESS_EXTERNAL_STYLESHEET, ""
```

### 1.XStream

- XStream 版本应不低于 1.4.20
- 初始化 XStream 应使用默认解析器
- 使用 XStream 的 setupDefaultSecurity 安全模式限制输入的数据

```
// 使用默认解析器
XStream xStream = new XStream();

// 必须开启安全模式，安全模式采用白名单限制输入的数据类型
XStream.setupDefaultSecurity(xStream);

// 在白名单内添加一些基本数据类型
xstream.addPermission(NullPermission.NULL);
xstream.addPermission(PrimitiveTypePermission.PRIMITIVES);
xstream.allowTypeHierarchy(Collection.class);

// 在白名单中添加可信任包内所有的子类
xstream.allowTypesByWildcard(new String[] {
	Blog.class.getPackage().getName()+".*"
});
```

### 2.javax.xml.parsers.DocumentBuilderFactory

```
DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
try {
	dbf.setFeature("http://javax.xml.XMLConstants/feature/secure-processing", true);
	dbf.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
	dbf.setFeature("http://xml.org/sax/features/external-general-entities", false);
	dbf.setFeature("http://xml.org/sax/features/external-parameter-entities", false);
	dbf.setFeature("http://apache.org/xml/features/nonvalidating/load-external-dtd", false);
	dbf.setXIncludeAware(false);
	dbf.setExpandEntityReferences(false);
	DocumentBuilder builder = dbf.newDocumentBuilder();
	[...]
}
```

### 3.org.jdom2.input.SAXBuilder

```
SAXBuilder builder = new SAXBuilder();
builder.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
builder.setFeature("http://xml.org/sax/features/external-general-entities", false);
builder.setFeature("http://xml.org/sax/features/external-parameter-entities", false);
builder.setFeature("http://apache.org/xml/features/nonvalidating/load-external-dtd", false);
Document doc = builder.build(InputSource);
[...]
```

### 4.javax.xml.parsers.SAXParserFactory

```
SAXParserFactory spf = SAXParserFactory.newInstance();
spf.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
spf.setFeature("http://xml.org/sax/features/external-general-entities", false);
spf.setFeature("http://xml.org/sax/features/external-parameter-entities", false);
spf.setFeature("http://apache.org/xml/features/nonvalidating/load-external-dtd", false);
SAXParser parser = spf.newSAXParser();
parser.parse(InputSource, (HandlerBase) null);
[...]
```

### 5.org.dom4j.io.SAXReader

```
SAXReader saxReader = new SAXReader();
saxReader.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
saxReader.setFeature("http://xml.org/sax/features/external-general-entities", false);
saxReader.setFeature("http://xml.org/sax/features/external-parameter-entities", false);
saxReader.setFeature("http://apache.org/xml/features/nonvalidating/load-external-dtd", false);
saxReader.read(InputSource);
[...]
```

### 6.org.xml.sax.XMLReader

```
XMLReader reader = XMLReaderFactory.createXMLReader();
reader.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
reader.setFeature("http://xml.org/sax/features/external-general-entities", false);
reader.setFeature("http://xml.org/sax/features/external-parameter-entities", false);
reader.setFeature("http://apache.org/xml/features/nonvalidating/load-external-dtd", false);
reader.parse(new InputSource(InputSource));
[...]
```

### 7.javax.xml.transform.sax.SAXTransformerFactory

```
SAXTransformerFactory sf = (SAXTransformerFactory)SAXTransformerFactory.newInstance();
sf.setAttribute(XMLConstants.ACCESS_EXTERNAL_DTD, "");
sf.setAttribute(XMLConstants.ACCESS_EXTERNAL_STYLESHEET, "");
StreamSource source = new StreamSource(InputSource);
sf.newTransformerHandler(source);
[...]
```

### 8.javax.xml.validation.SchemaFactory

```
SchemaFactory factory = SchemaFactory.newInstance("http://www.w3.org/2001/XMLSchema");
factory.setProperty(XMLConstants.ACCESS_EXTERNAL_DTD, "");
factory.setProperty(XMLConstants.ACCESS_EXTERNAL_SCHEMA, "");
StreamSource source = new StreamSource(InputSource);
Schema schema = factory.newSchema(source);
[...]
```

### 9.javax.xml.transform.TransformerFactory

```
TransformerFactory tf = TransformerFactory.newInstance();
tf.setAttribute(XMLConstants.ACCESS_EXTERNAL_DTD, "");
tf.setAttribute(XMLConstants.ACCESS_EXTERNAL_STYLESHEET, "");
StreamSource source = new StreamSourceInputSource);
tf.newTransformer().transform(source, new DOMResult());
[...]
```

## XPath 注入

XPath 注入类似于 SQL 注入，如果不校验而直接拼接用户输入，可能导致应用程序执行恶意的 XPath 语句，而攻击者则可以越权读取或恶意篡改目标 XML 数据。

**下面以登录验证中的模块为例，说明 XPath 注入攻击的实现原理。**

在应用程序的登录验证程序中，一般有用户名（username）和密码（password） 两个参数，程序会通过用户所提交输入的用户名和密码来执行授权操作。若验证数据存放在 XML 文件中，其原理是通过查找 user 表中的用户名 （username）和密码（password）的结果进行授权访问。

例如存在 user.xml 文件：

```
<user>
	<firstname>Ben</firstname>
	<lastname>Elmore</lastname>
	<loginID>abc</loginID>
	<password>test123</password>
</user>
<user>
	<firstname>Shlomy</firstname>
	<lastname>Gantz</lastname>
	<loginID>xyz</loginID>
	<password>123test</password>
</user>
```

在 XPath 中典型的查询语句如下：

```
//users/user[loginID/text()='xyz'and password/text()='123test']
```

正常用户传入 login 和 password，例如 loginID = 'xyz' 和 password = '123test'，则该查询语句将返回 true。但如果恶意用户传入类似 ' or 1=1 or ''=' 的值，那么该查询语句也会得到 true 返回值，因为 XPath 查询语句最终会变成如下代码：

```
//users/user[loginID/text()=''or 1=1 or ''='' and password/text()='' or 1=1 or ''='']
```

**脆弱代码：**

```
public int risk(HttpServletRequest request,
		Document doc, XPath xpath ,org.apache.log4jLogger logger) {
	int len = 0;
	String path = request.getParameter("path");
	try {
		XPathExpression expr = xpath.compile(path);
		Object result = expr.evaluate(doc, XPathConstants.NODESET);
		NodeList nodes = (NodeList) result;
		len = nodes.getLength();
	} catch (XPathExpressionException e) {
		logger.warn("Exception", e);
	}
	return len;
}
```

**解决方案：**

```
public int fix(HttpServletRequest request,
		Document doc, XPath xpath ,org.apache.log4j.Logger logger) {
	int len = 0;
	String path = request.getParameter("path");
	try {
		// 使用过滤函数 filterForXPath 过滤用户输入
		String filtedXPath = filterForXPath(path);
		XPathExpression expr = xpath.compile(filtedXPath);
		Object result = expr.evaluate(doc, XPathConstants.NODESET);
		NodeList nodes = (NodeList) result;
		len = nodes.getLength();
	} catch (XPathExpressionException e) {
		logger.warn("Exception", e);
	}
	return len;
}
// 限制用户的输入数据，尤其应限制特殊字符
public String filterForXPath(String input) {
	if (input == null) {
		return null;
	}
	StringBuilder out = new StringBuilder();
	for (int i = 0; i < input.length(); i++) {
		char c = input.charAt(i);
		if (c >= 'A' && c <= 'Z') {
			out.append(c);
		} else if (c >= 'a' && c <= 'z') {
			out.append(c);
		} else if (c >= '0' && c <= '9') {
			out.append(c);
		} else if (c == '_' || c == '-') {
			//限制特殊字符的使用
			out.append(c);
		} else if (c >= 0x4e00 && c <= 0x9fa5) {
			//允许汉字使用
			out.append(c);
		}
	}
	return out.toString();
}
```

## EL 表达式引擎代码注入

攻击者可以构造恶意 EL 代码注入到 EL 表达式引擎执行恶意代码。

**脆弱代码 1：**

```
public void parseExpressionInterface(Person personObj, String property) {
	ExpressionParser parser = new SpelExpressionParser();

	// property 变量内容不做限制可能导致任意的EL表达式执行
	Expression exp = parser.parseExpression(property+" == 'Albert'");
	StandardEvaluationContext testContext = new StandardEvaluationContext(personObj);
	boolean result = exp.getValue(testContext, Boolean.class);
}
```

**脆弱代码 2：**

```
public void evaluateExpression(String expression) {
	FacesContext context = FacesContext.getCurrentInstance();
	ExpressionFactory expressionFactory = context.getApplication().getExpressionFactory();
	ELContext elContext = context.getELContext();

	// expression 变量不做任何处理就传入表达式引擎执行可能导致任意的EL表达式执行
	ValueExpression vex = expressionFactory.createValueExpression(elContext, expression, String.class);
	return (String) vex.getValue(elContext);
}
```

**解决方案：**

禁止使用动态的 EL 表达式编写复杂的业务逻辑，也应禁止用户输入的 EL 表达式使用 StandardEvaluationContext 执行，可以使用安全的 SimpleEvaluationContext 进行执行。

## JS 脚本引擎代码注入

攻击者可以构造恶意 js 注入到 js 引擎执行恶意代码。

**脆弱代码：**

```
public void runCustomTrigger(String script) {
	ScriptEngineManager factory = new ScriptEngineManager();
	ScriptEngine engine = factory.getEngineByName("JavaScript");

	// 不执行安全校验，直接eval执行可能造成恶意的js代码执行
	engine.eval(script);
}
```

**解决方案：**

在 java 中使用 js 引擎应使用安全的沙盒模式执行 js 代码。

- java 8 或者 8 以上版本使用 delight-nashorn-sandbox 组件

```
<dependency>
    <groupId>org.javadelight</groupId>
    <artifactId>delight-nashorn-sandbox</artifactId>
    <version>[insert latest version]</version>
</dependency>

// 创建沙盒
NashornSandbox sandbox = NashornSandboxes.create();

// 沙盒内默认禁止js代码访问所有的java类对象
// 沙盒可以手工授权js代码能访问的java类对象
sandbox.allow(File.class);

// eval执行js代码
sandbox.eval("var File = Java.type('java.io.File'); File;")
```

- java 7 使用 Rhino 引擎

```
public void runCustomTrigger(String script) {
	// 启用 Rhino 引擎的js沙盒模式
	SandboxContextFactory contextFactory = new SandboxContextFactory();
	Context context = contextFactory.makeContext();
	contextFactory.enterContext(context);
	try {
		ScriptableObject prototype = context.initStandardObjects();
		prototype.setParentScope(null);
		Scriptable scope = context.newObject(prototype);
		scope.setPrototype(prototype);

		context.evaluateString(scope,script, null, -1, null);
	} finally {
		context.exit();
	}
}
```

## JavaBeans 属性注入

如果系统设置 bean 属性前未进行严格的校验，攻击者可以设置能影响系统完整性的任意 bean 属性。
例如 BeanUtils.populate 函数或类似功能函数允许设置 Bean 属性或嵌套属性。攻击者可以利用此功能来访问特殊的 Bean 属性 class.classLoader，从而可以覆盖系统属性并可能执行任意代码。

**脆弱代码：**

```
MyBean bean = ...;
HashMap map = new HashMap();
Enumeration names = request.getParameterNames();
while (names.hasMoreElements()) {
	String name = (String) names.nextElement();
	map.put(name, request.getParameterValues(name));
}
BeanUtils.populate(bean, map);
```

**解决方案：**

Bean 属性的成分复杂，用户输入的数据应**严格校验**后才能填充到 Bean 的属性。

## 不安全的对象绑定

将用户输入数据绑定到对象时如不做限制，可能造成攻击者恶意覆盖用户数据。

**脆弱代码：**

```
@javax.persistence.Entity
class UserEntity {
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;

	private String username;

	private String password;

	private Long role;
}
@Controller
class UserController {

	@PutMapping("/user/")
	@ResponseStatus(value = HttpStatus.OK)
	public void update(UserEntity user) {

		// 攻击者可以构造恶意user对象，将id字段构造为管理员id，将password字段构造为弱密码
		// 如果鉴权不完整，接口读取恶意user对象的id字段后会覆盖管理员的password字段成为弱密码
		userService.save(user);
	}
}
```

**解决方案 1：**

使用 @InitBinder 局部限制 Controller 的数据绑定

```
@Controller
class UserController {

	@InitBinder
	public void initBinder(WebDataBinder binder, WebRequest request){

		// 对允许绑定的字段设置白名单，阻止其他所有字段
		binder.setAllowedFields(["username","firstname","lastname"]);
		// 或者
		// 对不允许绑定的字段设置黑名单，允许其他所有字段
		binder.setDisallowedFields(["home_address","password"]);
	}
}
```

**解决方案 2：**

使用 @ControllerAdvice + @InitBinder 全局限制所有 Controller 的数据绑定

```
@ControllerAdvice
public final class GlobalDataBinder
{
	@InitBinder
	public void initBinder(WebDataBinder binder, WebRequest request) {

		// 对允许绑定的字段设置白名单，阻止其他所有字段
		binder.setAllowedFields(["username","firstname","lastname"]);
		// 或者
		// 对不允许绑定的字段设置黑名单，允许其他所有字段
		binder.setDisallowedFields(["home_address","password"]);
	}
}
```

**解决方案 3：**

使用 jackson 注解 @JsonIgnore 限制数据绑定。

```
@Controller
class UserController {

	// 参数业务应使用POST传递
	@PostMapping("/user")
	public UserEntity updateUser(@RequestBody JSONObject requestJson) {

		// 敏感字段 password 已受限
		return userService.updateById(requestJson).update();
	}
}
class UserEntity {
	@Id
	private Long id;
	private String username;

	@JsonIgnore
	private String password;
}
```

- 使用 jackson 时，可以使用@JsonIgnore 同时禁止某字段执行序列化和反序列化
  - 只在字段的 get 方法使用@JsonIgnore 可以禁止字段执行序列化（防止敏感信息泄露）
  - 只在字段的 set 方法使用@JsonIgnore 可以禁止字段执行反序列化（防止恶意数据绑定）
- 敏感数据传输时，应采用加密手段
- @InitBinder 对 @RequestBody 无效

**解决方案 4：**

使用 jackson 注解 @JsonIgnoreProperties 限制数据绑定。

```
@Controller
class UserController {

	// 参数业务应使用POST传递
	@PostMapping("/user")
	public UserEntity updateUser(@RequestBody JSONObject requestJson) {

		// 敏感字段 password secretKey 已受限
		return userService.updateById(requestJson).update();
	}
}

@JsonIgnoreProperties({"password","secretKey"})
class UserEntity {
	@Id
	private Long id;
	private String username;

	private String password;
	private String secretKey;
}
```

- 敏感数据传输时，应采用加密手段
- @InitBinder 对 @RequestBody 无效

## 正则表达式 DOS（ReDOS）

当存在缺陷的正则表达式处理某些字符串时，正则表达式引擎可能会花费大量时间甚至导致宕机，此类风险称为 ReDOS。

**脆弱代码：**

```
符号 | 符号 [] 符号 + 三者联合使用可能受到 ReDOS 攻击：
表达式：  (\d+|[1A])+z
需求: 会匹配任意数字或任意（1或A）字符串加上字符z
匹配字符串: 111111111 (10 chars)
计算步骤数: 46342

如果两个重复运算符过近，那么有可能收到攻击。请看以下例子：
例子1：
表达式：  .*\d+\.jpg
需求: 会匹配任意字符加上数字加上.jpg
匹配字符串: 1111111111111111111111111 (25 chars)
计算步骤数: 9187

例子2：
表达式：  .*\d+.*a
需求: 会匹配任意字符串加上数字加上任意字符串加上a字符
匹配字符串: 1111111111111111111111111 (25 chars)
计算步骤数: 77600

例子3：
表达式： ^(a+)+$ 处理 aaaaaaaaaaaaaaaaX
重复运算符嵌套将使正则表达式引擎分析65536个不同的匹配路径。
```

**解决方案：**

- 对正则表达式处理的内容应进行长度限制。
- 消除正则表达式的歧义，避免重复运算符嵌套。例如表达式`^(a+)+$`应替换成`^a+$`

## 跨站脚本攻击（XSS）

攻击者嵌入恶意脚本代码到正常用户会访问到的页面中，当正常用户访问该页面时，则可导致嵌入的恶意脚本代码的执行，从而达到恶意攻击用户的目的。

**常见的攻击向量：**

```
<Img src = x onerror = "javascript: window.onerror = alert; throw XSS">
<Video> <source onerror = "javascript: alert (XSS)">
<Input value = "XSS" type = text>
<applet code="javascript:confirm(document.cookie);">
<isindex x="javascript:" onmouseover="alert(XSS)">
"></SCRIPT>”>’><SCRIPT>alert(String.fromCharCode(88,83,83))</SCRIPT>
"><img src="x:x" onerror="alert(XSS)">
"><iframe src="javascript:alert(XSS)">
<object data="javascript:alert(XSS)">
<isindex type=image src=1 onerror=alert(XSS)>
<img src=x:alert(alt) onerror=eval(src) alt=0>
<img  src="x:gif" onerror="window['al\u0065rt'](0)"></img>
```

**解决方案：**

- 禁止简单的正则过滤，浏览器存在容错机制，可能会将攻击者精心构造的变形前端代码渲染成攻击向量。
- 原则上禁止用户输入特殊字符，或者转义用户输入的特殊字符。
- 富文本输出内容应进行白名单校验，只能对用户渲染安全的 HTML 标签和安全的 HTML 属性，请参照以下链接。
  - https://github.com/cure53/DOMPurify
  - https://github.com/leizongmin/js-xss/blob/master/README.zh.md

- 禁止上传包含JavaScript代码的pdf文件
```
// 引入pdfbox依赖包
<dependency>
    <groupId>org.apache.pdfbox</groupId>
    <artifactId>pdfbox</artifactId>
    <version>2.0.30</version>
</dependency>

// 使用pdfbox限制包含js代码的pdf上传
public static void main(String[] args) throws IOException {
        String s1 = "poc.pdf";
        File file = new File(s1);
        boolean b = containsJavaScript(file);
        if (b) {
            System.out.println("pdf文件包含，js脚本代码");
        }
    }

    /**
     * 校验pdf文件是否包含js脚本
     **/
    public static boolean containsJavaScript(File file) throws IOException {
        PDDocument document = PDDocument.load(file);
        return containsJavaScript(document);
    }

    /**
     * 校验pdf文件是否包含js脚本
     **/
    public static boolean containsJavaScript(InputStream input) throws IOException {
        PDDocument document = PDDocument.load(input);
        return containsJavaScript(document);
    }

    /**
     * 校验pdf文件是否包含js脚本，这里使用分页解析，因为对于大pdf文件（文字较多），会出现加载问题。
     **/
    public static boolean containsJavaScript(PDDocument document) {
        //Getting the PDDocumentInformation object

        PDPageTree pages = document.getPages();
        return IntStream.range(0, pages.getCount()).anyMatch(i -> {
            //return str.contains("JavaScript") || str.contains("COSName{JS}");
            return pages.get(i).getCOSObject().toString().contains("COSName{JS}");
        });
    }
```
---

# 第八条 必须过滤上传文件

## 安全编码要求 ↓：

## 潜在的路径遍历（读取文件）

当应用程序读取文件名打开对应的文件以读取其内容，而该文件名来自于用户的输入数据。那么将未经过滤的文件名数据传递给文件 API，则攻击者可以从系统中读取任意文件。

**脆弱代码：**

```
@GET
@Path("/images/{image}")
@Produces("images/*")
public Response getImage(@javax.ws.rs.PathParam("image") String image) {

	// image变量中未校验 ../ 或 ..\
	File file = new File("resources/images/", image);

	if (!file.exists()) {
		return Response.status(Status.NOT_FOUND).build();
	}
	return Response.ok().entity(new FileInputStream(file)).build();
}
```

**解决方案：**

```
import org.apache.commons.io.FilenameUtils;

@GET
@Path("/images/{image}")
@Produces("images/*")
public Response getImage(@javax.ws.rs.PathParam("image") String image) {

	// 首先进行逻辑校验，判断用户是否有权限访问接口 以及 用户对访问的资源是否有权限

	// 过滤image变量中的 ../ 或 ..\
	File file = new File("resources/images/", FilenameUtils.getName(image));

	if (!file.exists()) {
		return Response.status(Status.NOT_FOUND).build();
	}
	return Response.ok().entity(new FileInputStream(file)).build();
}
```

- 如果对用户发起请求的文件参数校验不严格，会导致潜在的路径遍历漏洞。
- 应对所有文件的上传操作进行权限判断，无上传权限应直接提示无权限。
- 危险的文件后缀应严禁上传，包括： .jsp .jspx .war .jar .exe .bat .js .vbs .html .shtml
- 应依照业务逻辑对文件后缀进行前后端的白名单校验，禁止白名单之外的文件上传。
  - 图片类型 .jpg .png .gif .jpeg
  - 文档类型 .doc .docx .ppt .pptx .xls .xlsx .pdf
  - 以此类推
- 上传的文件保存时**应**使用 uuid、雪花算法等算法进行强制重命名，以保证文件的不可预测性和唯一性。
- 上传的文件**禁止**保存在 web 路径中，防止上传文件可以被直接下载。
- 应对所有文件的下载操作依照 “除了公共文件，只有上传者才能下载” 的原则进行权限判断，防止越权攻击。

## 潜在的路径遍历（写入文件）

当应用程序打开文件并写入数据，而该文件名来自于用户的输入数据。那么将未经过滤的文件名数据传递给文件 API，则攻击者可以写入任意数据到系统中。

**脆弱代码：**

```
@RequestMapping("/MVCUpload")
public String MVCUpload(@RequestParam( "description" ) String description, @RequestParam("file") MultipartFile file) throws IOException {

	// 首先进行逻辑校验，判断用户是否有权限访问接口 以及 用户对访问的资源是否有权限

	InputStream inputStream=file.getInputStream();
	String fileName=file.getOriginalFilename();

	// 文件名fileName未校验 ../ 或 ..\  并且也未校验文件后缀
	OutputStream outputStream=new FileOutputStream("/tmp/"+fileName);
	byte[] bytes=new byte[10];
	int len=-1;

	// 将文件写入系统中
	while((len=inputStream.read(bytes))!=-1){
		outputStream.write(bytes,0,len);
	}
	outputStream.close();
	inputStream.close();

	// 记录审计日志

	return "success";
}
```

**解决方案：**

```
import org.apache.commons.io.FilenameUtils;

@RequestMapping("/MVCUpload")
public String MVCUpload(@RequestParam( "description" ) String description, @RequestParam("file") MultipartFile file) throws IOException {

	// 首先进行逻辑校验，判断用户是否有权限访问接口 以及 用户对访问的资源是否有权限

	InputStream inputStream=file.getInputStream();
	String fileInput;
	if(file.getOriginalFilename() == null){
		return "error";
	}

	// 获取上传文件名后强制转化为小写并过滤空白字符
	fileInput=file.getOriginalFilename().toLowerCase().trim();

	// 对变量fileInput所代表的文件路径去除目录和后缀名，可以过滤文件名中的 ../ 或 ..\
	String fileName=FilenameUtils.getBaseName(fileInput);

	// 获取文件后缀
	String ext=FilenameUtils.getExtension(fileInput);

	// 文件名应大于等于 1 并且小于等于 30
	if ( 1 > fileName.length() || fileName.length() > 30 ) {
		return "error";
	}

	// 文件名只能包含大小写字母、数字和中文
	if(fileName.matches("0-9a-zA-Z\u4E00-\u9FA5]+")){
		return "error";
	}

	// 依据业务逻辑使用白名单校验文件后缀
	if(!"jpg".equals(ext)){
		return "error";
	}

	// 将文件写入系统时，应确保文件不写入web路径中
	OutputStream outputStream=new FileOutputStream("/tmp/"+ fileName + "." + ext);
	byte[] bytes=new byte[10];
	int len=-1;
	while((len=inputStream.read(bytes))!=-1){
		outputStream.write(bytes,0,len);
	}
	outputStream.close();
	inputStream.close();

	// 记录审计日志

	return "success";
}
```

- 如果对用户发起请求的文件参数校验不严格，会导致潜在的路径遍历漏洞。
- 应对所有文件的上传操作进行权限判断，无上传权限应直接提示无权限。
- 危险的文件后缀应严禁上传，包括： .jsp .jspx .war .jar .exe .bat .js .vbs .html .shtml
- 应依照业务逻辑对文件后缀进行前后端的白名单校验，禁止白名单之外的文件上传。
  - 图片类型 .jpg .png .gif .jpeg
  - 文档类型 .doc .docx .ppt .pptx .xls .xlsx .pdf
  - 以此类推
- 上传的文件保存时**应**使用 uuid、雪花算法等算法进行强制重命名，以保证文件的不可预测性和唯一性。
- 上传的文件**禁止**保存在 web 路径中，防止上传文件可以被直接下载。
- 应对所有文件的下载操作依照 “除了公共文件，只有上传者才能下载” 的原则进行权限判断，防止越权攻击。

---

# 第九条 确保多线程编程的安全性

## 安全编码要求 ↓：

## 竞争条件

当两个或两个以上的线程对同一个数据进行操作的时候，可能会产生“竞争条件”的现象。这种现象产生的根本原因是因为多个线程在对同一个数据进行操作，此时对该数据的操作是非“原子化”的，可能前一个线程对数据的操作还没有结束，后一个线程又开始对同样的数据开始进行操作，这就可能会造成数据结果的变化未知。

**解决方案：**

- HashMap、HashSet 是非线程安全的。
- Vector、HashTable 内部的方法基本都是 synchronized，所以是线程安全的。
- 在高并发下应使用 Concurrent 包中的集合类，同时在单线程下禁止使用 synchronized。

---

# 第十条 设计错误、异常处理机制

## 安全编码要求 ↓：

1. java 类库中定义的可以通过预检查方式规避的 RuntimeException 异常不应该通过 catch 的方式来处理，比如：NullPointerException，IndexOutOfBoundsException 等等。

   说明：无法通过预检查的异常除外，比如，在解析字符串形式的数字时，可能存在数字格式错误，应通过 catch NumberFormatException 来实现。

- 正例：

```
if (obj != null) {
	...
}
```

- 反例：

```
try {
	obj.method();
} catch ( NullPointerException e ) {
	...
}
```

2. 异常捕获后不要用来做流程控制，条件控制。

   说明：异常设计的初衷是解决程序运行中的各种意外情况，且异常的处理效率比条件判断方式要低很多。

3. catch 时请分清稳定代码和非稳定代码，稳定代码指的是无论如何不会出错的代码。对于非稳定代码的 catch 尽可能进行区分异常类型，再做对应的异常处理。

   说明：对大段代码进行 try-catch，使程序无法根据不同的异常做出正确的应激反应，也不利于定位问题，这是一种不负责任的表现。

- 正例：
  用户注册的场景中，如果用户用户名称已存在或用户输入密码过于简单，在程序上作出"用户名或密码错误"，并提示给用户。

- 反例：
  用户提交表单场景中，如果用户输入的价格为感叹号，系统不做任何提示，系统在后台提示报错。

4. 捕获异常是为了处理它，不要捕获了却什么都不处理而抛弃之，如果不想处理它，请将该异常抛给它的调用者。最外层的业务使用者，必须处理异常，将其转化为用户可以理解的内容。

5. 事务场景中，抛出异常被 catch 后，如果需要回滚，一定要注意手动回滚事务。

6. finally 块中必须对临时文件、资源对象、流对象进行资源释放，有异常也要做 try-catch。

   说明：如果 JDK7 及以上，可以使用 try-with-resources 方式。

7. 不要在 finally 块中使用 return。

   说明：try 块中的 return 语句执行成功后，并不马上返回，而是继续执行 finally 块中的语句，如果此处存在 return 语句，则在此直接返回，无情丢弃掉 try 块中的返回点。

- 反例：

```
private int x = 0;
public int checkReturn(){
	try {
		/* x 等于 1，此处不返回 */
		return(++x);
	} finally {
		/* 返回的结果是 2 */
		return(++x);
	}
}
```

8. 捕获异常与抛异常，必须是完全匹配，或者捕获异常是抛异常的父类。

   说明：如果预期对方抛的是绣球，实际接到的是铅球，就会产生意外情况。

9. 在调用 RPC、二方包、或动态生成类的相关方法时，捕捉异常必须使用 Throwable 类来进行拦截。

   说明：通过反射机制来调用方法，如果找不到方法，抛出 NoSuchMethodException。什么情况会抛出 NoSuchMethodError 呢？二方包在类冲突时，仲裁机制可能导致引入非预期的版本使类的方法签名不匹配，或者在字节码修改框架（比如：ASM）动态创建或修改类时，修改了相应的方法签名。这些情况，即使代码编译期是正确的，但在代码运行期时，会抛出 NoSuchMethodError。

10. 方法的返回值可以为 null，不强制返回空集合，或者空对象等，必须添加注释充分说明什么情况下会返回 null 值。

    说明：本手册明确防止 NPE 是调用者的责任。即使被调用方法返回空集合或者空对象，对调用者来说，也并非高枕无忧，必须考虑到远程调用失败、序列化失败、运行时异常等场景返回 null 的情况。

11. 防止 NPE，是程序员的基本修养，注意 NPE 产生的场景：

    - 数据库的查询结果可能为 null。
    - 集合里的元素即使 isNotEmpty，取出的数据元素也可能为 null。
    - 远程调用返回对象时，一律要求进行空指针判断，防止 NPE。
    - 对于 Session 中获取的数据，建议进行 NPE 检查，避免空指针。
    - 级联调用 obj.getA().getB().getC()；一连串调用，易产生 NPE。应使用 JDK8 的 Optional 类来防止 NPE 问题。
    - 返回类型为基本数据类型，return 包装数据类型的对象时，自动拆箱有可能产生 NPE。

- 反例：

```
public int f() {
	return Integer; // 对象
} // 如果为 null，自动解箱抛 NPE。
```

12. 定义时区分 unchecked / checked 异常，避免直接抛出 new RuntimeException()，更不允许抛出 Exception 或者 Throwable，应使用有业务含义的自定义异常。推荐业界已定义过的自定义异常，如：DAOException / ServiceException 等。

13. 对于公司外的 http/api 开放接口必须使用 errorCode；而应用内部推荐异常抛出；跨应用间 RPC 调用优先考虑使用 Result 方式，封装 isSuccess()方法、errorCode、errorMessage；而应用内部直接抛出异常即可。

    说明：关于 RPC 方法返回方式使用 Result 方式的理由：

    - 使用抛异常返回方式，调用方如果没有捕获到就会产生运行时错误。
    - 如果不加栈信息，只是 new 自定义异常，加入自己的理解的 error message，对于调用端解决问题的帮助不会太多。如果加了栈信息，在频繁调用出错的情况下，数据序列化和传输的性能损耗也是问题。

14. 系统应具有全局性的异常处理机制，避免未知异常直接抛出，防止攻击者探查系统内部敏感信息。

```
@ControllerAdvice
public class GlobalException {

    @ExceptionHandler(RuntimeException.class)
    @ResponseBody
    public ResultData<String> runtimeException(RuntimeException e) {
        log.error("运行异常", e);
        return ResultData.fail(ResultCode.INTERNAL_SERVER_ERROR);
    }

    @ExceptionHandler(Exception.class)
    @ResponseBody
    public ResultData<String> exception(Exception e) {
        log.error("系统未知异常", e);
        return ResultData.fail(ResultCode.INTERNAL_SERVER_ERROR);
    }
}
```

---

# 第十一条 数据库操作使用参数化请求方式

## 安全编码要求 ↓：

## SQL 注入

SQL 注入是指应用程序对用户输入数据的合法性没有判断或过滤不严，攻击者可以在应用程序中事先定义好的 SQL 语句中添加额外的 SQL 语句。

### 1.使用 Druid 进行 sql 注入防护

Druid 对 sql 语句进行"词法解析"，把完整的 sql 语句切分成单独的 sql token，解析成"词法树"，再将词法树中的 token 节点进行 sql 语义识别，将其解析成符合 sql 规范的"语法树"，最后对语法树进行安全校验。该技术是目前攻击者最难绕过的 sql 注入防护技术。

```
  <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
      ...
      <property name="filters" value="wall"/>
  </bean>
```

### 2.jdbc 安全编码规范

**脆弱代码：**

```
public void risk(HttpServletRequest request, Connection c, org.apache.log4j.Logger logger) {
	String text = request.getParameter("text");

	// 使用拼接导致SQL注入
	String sql = "select * from tableName where columnName = '" + text + "'";
	try {
		Statement s = c.createStatement();
		s.executeQuery(sql);
	} catch (SQLException e) {
		logger.warn("Exception", e);
	}
}
```

**解决方案：**

```
public void fix(HttpServletRequest request, Connection c, org.apache.log4j.Logger logger) {
	String text = request.getParameter("text");

	// 使用 PreparedStatement 预编译并使用占位符防止SQL注入
	String sql = "select * from tableName where columnName = ?";
	try {
		PreparedStatement s = c.prepareStatement(sql);
		s.setString(1, text);
		s.executeQuery();
	} catch (SQLException e) {
		logger.warn("Exception", e);
	}
}
```

- 应使用 jdbc 原生的预编译技术执行 SQL 语句，禁止使用 SQL 拼接。
- 应按照长度、格式、逻辑以及特殊字符 4 个维度对每一个输入参数进行安全校验，然后再将其传递给 SQL 函数。

### 3.Mybatis 安全编码规范

- 在 Mybatis 中除了极为特殊的情况，应禁止使用 $ 拼接 sql 。
- 所有 Mybatis 的实体 bean 对象字段都应使用包装类。

**1) Mybatis 执行 like 语句的安全编码**

**脆弱代码：**

```
模糊查询like
Select * from news where title like '%#{title}%'

但由于这样写程序会报错，研发人员将SQL查询语句修改如下：
Select * from news where title like '%${title}%'

修改SQL语句之后，程序停止报错。但是却引入了 SQL 语句拼接的问题，极有可能引发 SQL 注入漏洞。
```

**解决方案：**

可使用 concat 函数解决 SQL 语句动态拼接的问题

```
select * from news where tile like concat('%', #{title}, '%')

注意！对搜索的内容必须进行严格的逻辑校验：
1）例如搜索用户手机号，应限制输入数据只能输入数字，防止出现搜索英文或中文的无效搜索
2）mybatis预编译不会转义 % 符号，应阻止用户输入 % 符号以防止全表扫描
3）输入数据长度和搜索频率应进行限制，防止恶意搜索导致的数据库拒绝服务
```

**2) Mybatis 执行 in 语句的安全编码**

**脆弱代码：**

```
在对同条件多值查询的时候，如当用户输入1001,1002,1003…100N时，如果考虑安全编码规范问题，其对应的SQL语句如下：
Select * from news where id in (#{id})

但由于这样写程序会报错，研发人员将SQL查询语句修改如下：
Select * from news where id in (${id})

修改SQL语句之后，程序停止报错。但是却引入了SQL语句拼接的问题，极有可能引发SQL注入漏洞。
```

**解决方案：**

可使用 Mybatis 自带的 foreach 指令解决 SQL 语句动态拼接的问题

```
select * from news where id in
<foreach collection="ids" item="item" open="(" separator="," close=")">#{item}</foreach>
```

**3) Mybatis 执行 order by 或 group by 的安全编码**

**脆弱代码：**

```
当根据发布时间、点击量等信息进行排序的时候，如果考虑安全编码规范问题，其对应的SQL语句如下：
Select * from news where title = '安全' order by #{time} asc

但由于发布时间time不是用户输入的参数，无法使用预编译。研发人员将SQL查询语句修改如下：
Select * from news where title = '安全' order by ${time} asc

修改SQL语句之后，程序停止报错。但是却引入了SQL语句拼接的问题，极有可能引发SQL注入漏洞。
```

**解决方案：**

可使用 Mybatis 自带的 choose 指令解决 SQL 语句动态拼接的问题

```
ORDER BY
<choose>
<when test="orderBy == 1">
	id desc
</when>
<when test="orderBy == 2">
	date desc
</when>
<otherwise>
	time desc
</otherwise>
</choose>
```

## LDAP 注入

LDAP 注入是指应用程序对用户输入数据的合法性没有判断或过滤不严，攻击者可以在应用程序中事先定义好的 LDAP 语句中添加额外的 LDAP 语句。

**脆弱代码：**

```
String username = request.getParameter("username");
// 未对 username 进行校验直接拼接
NamingEnumeration answers = context.search("dc=People,dc=example,dc=com","(uid=" + username + ")", ctrls);
```

**解决方案：**

因为 LDAP 没有类似于 SQL 的预编译函数，所以针对 LDAP 注入的主要防御措施是依照 LDAP 数据库字段设计围绕长度、格式、逻辑、特殊字符 4 个维度对**每一个**输入参数进行安全校验。

---

# 第十二条 禁止在源代码中写入口令、服务器 IP 等敏感信息

## 安全编码要求 ↓：

## 硬编码密码

**脆弱代码：**

密码不应保留在源代码中。源代码只能在企业环境中**受限**的共享，禁止在互联网中共享。
为了安全管理，密码和密钥应存储在单独的加密配置文件或密钥库中。

```
private String SECRET_PASSWORD = "letMeIn!";

Properties props = new Properties();
props.put(Context.SECURITY_CREDENTIALS, "password");
```

## 硬编码密钥

**脆弱代码：**

密钥不应保留在源代码中。源代码只能在企业环境中**受限**的共享，禁止在互联网中共享。
为了安全管理，密码和密钥应存储在单独的加密配置文件或密钥库中。

```
byte[] key = {1, 2, 3, 4, 5, 6, 7, 8};
SecretKeySpec spec = new SecretKeySpec(key, "AES");
Cipher aes = Cipher.getInstance("AES");
aes.init(Cipher.ENCRYPT_MODE, spec);
return aesCipher.doFinal(secretData);
```

---

# 第十三条 为所有敏感信息采用加密传输

## 程序设计要求 ↓：

### 名称类信息脱敏规则：

- 客户编号信息应显示前 3 位和最后 3 位，其余用\*代替
- 客户姓名信息显示规则：
  - 客户姓名信息小于等于 3 个字，第 1 个字应用\*代替，显示后面的字
  - 客户姓名信息大于 3 小于 6 个字，前 2 个字应用\*代替，显示后面的字
  - 客户姓名信息大于等于 6 个字，第 3-6 个字应用\*替代，显示前面的字
  - 客户姓名信息用“.”分隔位多部分的，每部分应采用上述 1)-3)
- 企业类客户信息显示规则如下:
  - 企业类客户信息小于等于 4 个字，首尾应各保留 1 字，其余用\*替代;
  - 企业类客户信息大于 4 小于 7 个字，首尾应各保留 2 字，其余用\*替代;
  - 企业类客户信息大于等于 7 个字且为奇数的，中间 3 个字应用\*代替，显示其他的字；
  - 企业类客户信息大于等于 8 个字且为偶数的，中间 4 个字应用\*代替，显示其他的字;
- 网站账户信息应分段显示，每隔 2 位用\*代替显示。

### 联系类信息脱敏规则：

- 手机信息应显示前 3 位和最后 3 位，其余用\*替代
- 固定电话号信息应显示区号和最后 3 位，其余用\*替代
- 电子邮箱显示规则：
  - @前小于等于 5 位的，前 2 位用\*替代，显示其他
  - @前大于等于 5 位的，显示前 3 位，其他用\*替代
- 微信账号信息应用分段显示，每隔 2 位用\*代替显示
- qq 号信息应保留前 2 位后最后 1 位，其余用\*替代

### 证件号脱敏规则：

- 身份证号信息应显示前 6 位和最后 4 位，其余用\*替代
- 驾驶证号应显示前 6 位和最后 4 位，其余用\*替代
- 军人证号应显示前 3 个汉字的军种名和最后 3 位数字，其余用\*替代
- 护照号信息应显示前 1 位字母和最后 3 位数字，其余用\*替代
- 台胞证号信息应显示第 5-8 位，其余用\*替代

## 安全编码要求 ↓：

## 接受任何证书的 TrustManager

空的 TrustManager 通常用于实现直接连接到未经根证书颁发机构签名的主机。同时，如果客户端将信任所有的证书会导致应用程序很容易受到中间人攻击。

**脆弱代码：**

```
class TrustAllManager implements X509TrustManager {

	@Override
	public void checkClientTrusted(X509Certificate[] x509Certificates, String s) throws CertificateException {
		//Trust any client connecting (no certificate validation)
	}

	@Override
	public void checkServerTrusted(X509Certificate[] x509Certificates, String s) throws CertificateException {
		//Trust any remote server (no certificate validation)
	}

	@Override
	public X509Certificate[] getAcceptedIssuers() {
		return null;
	}
}
```

**解决方案：**

```
KeyStore ks = //加载包含受信任证书的密钥库
SSLContext sc = SSLContext.getInstance("TLS");
TrustManagerFactory tmf = TrustManagerFactory.getInstance("SunX509");
tmf.init(ks);
sc.init(kmf.getKeyManagers(), tmf.getTrustManagers(),null);
```

应该构建允许特定证书（例如基于信任库）的 TrustManager，并创建通配符证书，保证可以在多个子域上重用。

## 接受任何签名证书的 HostnameVerifier

由于许多主机上都重复使用了证书，因此很多开发人员编码 HostnameVerifier 时经常接受任何主机的请求。但是这很容易受到中间人攻击，因为客户端将信任所有证书。

**脆弱代码：**

```
public class AllHosts implements HostnameVerifier {
	public boolean verify(final String hostname, final SSLSession session) {
		return true;
	}
}
```

**解决方案：**

```
KeyStore ks = //加载包含受信任证书的密钥库
SSLContext sc = SSLContext.getInstance("TLS");
TrustManagerFactory tmf = TrustManagerFactory.getInstance("SunX509");
tmf.init(ks);
sc.init(kmf.getKeyManagers(), tmf.getTrustManagers(), null);
```

应该构建允许特定证书（例如基于信任库）的 TrustManager，并创建通配符证书，保证可以在多个子域上重用。

---

# 第十四条 使用可信的密码算法

## 程序设计要求 ↓：

## 禁止使用弱加密

- 应用程序可以使用 rsa2048 和 aes256 的组合作为等保二级系统对重要数据传输的加密算法。
- 应用程序必须使用国密系列算法的组合作为等保三级系统对重要数据传输的加密算法。
- 重要的敏感数据在传输过程中必须加密传输，如该类数据需要保存在数据库中，必须二次加密后才能保存。
- 应用程序前端 js 代码必须使用混淆和加密手段，保证 js 源代码无法被轻易的逆向分析。

```
	1）使用 webpack-obfuscator 混淆js代码时，不论混淆级别多少，都应设定以下参数为启用状态：
	// 限制使用开发者工具的控制台选项卡
	debugProtection: true

	// 使用间隔强制调试模式，从而更难使用“开发人员工具”的其他功能。
	debugProtectionInterval: true

	// 禁用console.log，console.info，console.error和console.warn 阻碍攻击者调试
	disableConsoleOutput: true

	// 压缩,无换行
	compact: true
	// 混淆后的代码,不能使用代码美化,需要配置 cpmpat:true;
	selfDefending: true

	2）使用js调试探测器 https://www.npmjs.com/package/devtools-detector
	当探测到调试器时，应限制攻击者继续渗透，例如：清除cookie或token、执行死循环、破坏逻辑等等

	3）productionSourceMap应设置为false关闭调试，前端部署禁止在生产环境开启调试模式。
```

- 可信的算法必须结合可信的加密流程形成加密方案，例如用户登录时必须使用合规的加密方案加密用户名和密码：

```
合规的双向加密数据的传输方案：
   1）后端生成非对称算法（国密SM2、RSA2048）的公钥B1、私钥B2，前端访问后端获取公钥B1。
   2）前端每次发送请求前，随机生成对称算法（国密SM4、AES256）的密钥A1。
   3）公钥、私钥可以全系统固定为一对，前端可以储存公钥，但私钥不能保存在后端数据库中。
   4）前端用步骤2的密钥A1加密所有业务数据生成en_data，用步骤1获取的公钥B1加密密钥A1生成en_key。
   5）前端用哈希算法对en_data + en_key的值形成一个校验值check_hash。
   6）前端将en_data、en_key、check_hash三个参数包装在同一个http数据包中发送到后端。
   7）后端获取三个参数后先判断哈希值check_hash是否匹配en_data + en_key以验证完整性。
   8）后端用私钥B2解密en_key获取本次请求的对称算法的密钥A1。
   9）后端使用步骤8获取的密钥A1解密en_data获取实际业务数据。
  10）后端处理完业务逻辑后，将需要返回的信息使用密钥A1进行加密后回传给前端。
  11）加密数据回传给前端后，前端使用A1对加密的数据进行解密获得返回的信息。
  12）步骤2随机生成的密钥A1已经使用完毕，前端应将其销毁。
```

## 安全编码要求 ↓：

## 可预测的伪随机数生成器

当在某些安全性极为关键的上下文中使用可预测的随机值时，可能会导致漏洞。

例如，当该值用作：

- CSRF 令牌：可预测的令牌可能导致 CSRF 攻击，因为攻击者将知道令牌的值
- 密码重置令牌（通过电子邮件发送）：可预测的密码令牌可能会导致帐户被接管
- 任何其他敏感值

**脆弱代码：**

```
String generateSecretToken() {
	Random r = new Random();
	return Long.toHexString(r.nextLong());
}
```

**解决方案：**

```
import org.apache.commons.codec.binary.Hex;

String generateSecretToken() {
	SecureRandom secRandom = new SecureRandom();

	byte[] result = new byte[32];
	secRandom.nextBytes(result);
	return Hex.encodeHexString(result);
}
```

使用随机性更高的 java.security.SecureRandom 替换 java.util.Random

## 错误的十六进制串联

将包含哈希签名的字节数组转换为人类可读的字符串时，如果逐字节读取该数组，则可能会发生转换错误。

**脆弱代码：**

```
MessageDigest md = MessageDigest.getInstance("SHA-256");
byte[] resultBytes = md.digest(password.getBytes("UTF-8"));

StringBuilder stringBuilder = new StringBuilder();
for(byte b :resultBytes) {
	stringBuilder.append( Integer.toHexString( b & 0xFF ) );
}

return stringBuilder.toString();
```

以上代码当 resultBytes 等于 “0x0679” 和 “0x6709” 都将输出为 “679”

**解决方案：**

```
stringBuilder.append(String.format("%02X", b));
```

所有对于数据格式化的操作应优先使用规范的数据格式化处理机制。

---

# 第十五条 禁止在日志、表单、cookie 等文件中记录口令、银行账号、通信内容等敏感数据

## 安全编码要求 ↓：

## Cookie 中的潜在敏感数据

cookie 中的数据如果直接存储具体的明文敏感数据，可以导致攻击者窃取敏感数据。

**脆弱代码：**

```
response.addCookie(new Cookie("userAccountID", acctID));
```

以上代码直接设置 cookie 中 userAccountID 的具体数值，导致攻击者可以窃取 acctID。

**解决方案：**

cookie 中的数据只能使用标准的哈希值，不能存储具体的数据内容，原则应使用 session 进行存取身份信息。

## 日志伪造

日志注入攻击是将未经验证的用户输入写到日志文件中，可以允许攻击者伪造日志内容或将恶意内容注入到日志中。

**日志伪造的实现原理**

如果用户提交 val 的字符串"twenty-one"，则会记录以下条目：

```
INFO: Failed to parse val=twenty-one
```

如果攻击者提交包含换行符%0d 和%0a 的字符串”twenty-one%0d%0aHACK:+User+logged+in%3dbadguy”，会记录以下条目：

```
INFO: Failed to parse val=twenty-one
HACK: User logged in=badguy
```

显然，攻击者可以利用以上手法插入任意日志内容。

**脆弱代码：**

```
public void risk(HttpServletRequest request, HttpServletResponse response) {
	String val = request.getParameter("val");
	try {
		int value = Integer.parseInt(val);
		out = response.getOutputStream();
	}
	catch (NumberFormatException e) {
		e.printStackTrace(out);
		log.info("Failed to parse val = " + val);
	}
}
```

**解决方案：**

```
public void risk(HttpServletRequest request, HttpServletResponse response) {
	String val = request.getParameter("val");
	try {
		int value = Integer.parseInt(val);
	}
	catch (NumberFormatException e) {
		val = val.replace("\r", "");
		val = val.replace("\n", "");
		log.info("Failed to parse val = " + val);
		//不要直接 printStackTrace 输出错误日志
	}
}
```

**所有**写入日志的数据必须去除 \r 和 \n 字符。

## HTTP 响应截断

攻击者任意构造 HTTP 响应数据并传递给应用程序可以构造：缓存中毒（Cache Poisoning），跨站点脚本（XSS） 和页面劫持（Page Hijacking）等攻击。

**HTTP 响应截断的实现原理**

下面的代码从 HTTP 请求中读取用户输入的作者姓名，并将其设置为 HTTP 响应的 cookie 头。

```
String author = request.getParameter(AUTHOR_PARAM);
[...]
Cookie cookie = new Cookie("author", author);
cookie.setMaxAge(cookieExpiration);
response.addCookie(cookie);
```

假设一个用户名：

```
Jane Smith
```

用户在登陆成功后包括 cookie 在内的 HTTP 响应值：

```
HTTP/1.1 200 OK

Set-Cookie: author=Jane Smith
[...]
```

如果 cookie 的值校验不严格，当攻击者提交恶意字符串：

```
Wiley Hacker \r\n Content-Length：999 \r\n \r\n
```

则 HTTP 响应将被分割成伪造的响应，导致原始响应被忽略掉：

```
HTTP/1.1 200 OK

Set-Cookie: author=Wiley Hacker
Content-Length: 999

malicious content... (to 999th character in this example)
Original content starting with character 1000, which is now ignored by the web browser...
```

**脆弱代码：**

```
public void risk(HttpServletRequest request, HttpServletResponse response) {
	String key = request.getParameter("key");
	String value = request.getParameter("value");
	response.setHeader(key, value);
}
```

**解决方案 1：**

```
public void fix(HttpServletRequest request, HttpServletResponse response) {
	String key = request.getParameter("key");
	String value = request.getParameter("value");
	key = key.replace("\r", "");
	key = key.replace("\n", "");
	value = value.replace("\r", "");
	value = value.replace("\n", "");
	response.setHeader(key, value);
}
```

**解决方案 2：**

```
public void fix(HttpServletRequest request, HttpServletResponse response) {
	String key = request.getParameter("key");
	String value = request.getParameter("value");
	if (Pattern.matches("[0-9A-Za-z]+", key) && Pattern.matches("[0-9A-Za-z]+", value)) {
		response.setHeader(key, value);
	}
}
```

**修复建议**

- 在操作 HTTP 响应报头（即 Head 部分）时，**所有**写入该区域的值必须去除\r 和\n 字符。
- 创建一份安全字符白名单，只接受白名单限制内的输入数据出现在 HTTP 响应头中，例如只允许字母和数字。

---

# 第十六条 禁止高风险的服务及协议

## 程序设计要求 ↓：

HTTPS 应使用 TLS 1.2 或 以上版本，旧版本协议存在中间人劫持漏洞。

## 安全编码要求 ↓：

## 不安全的 HTTP 动词

RequestMapping 默认情况下映射到所有 HTTP 动词，应强制要求只能使用 GET 和 POST。

**脆弱代码：**

```
@Controller
public class UnsafeController {

	// RequestMapping 默认情况下映射到所有HTTP动词
	@RequestMapping("/path")
	public void writeData() {
		return "";
	}
}
```

**解决方案：**

```
// 基于Spring Framework 4.3及更高版本
@Controller
public class SafeController {

	// 只接受GET动词，不执行数据修改操作
	@GetMapping("/path")
	public String readData() {
		return "";
	}

	// 只接受POST动词，执行数据修改操作
	@PostMapping("/path")
	public void writeData() {
		return "";
	}
}
```

使用 GetMapping 和 PostMapping 进行 HTTP 动词限制。

---

# 第十七条 避免异常信息泄漏

## 安全编码要求 ↓：

## 意外的属性泄露

应用程序未限制返回用户侧的数据字段，导致敏感内容泄露。

**脆弱代码：**

```
@javax.persistence.Entity
class UserEntity {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;

	private String username;
	private String password;
	[...]
}

@Controller
class UserController {

	// 在查询字符串中关联业务逻辑是不规范的
	@RequestMapping("/user/{id}")
	public UserEntity getUser(@PathVariable("id") String id) {

		// 返回用户所有字段内容，可能导致 password home_address 等敏感字段泄露
		return userService.findById(id).get();
	}
}
```

**解决方案 1：**

禁止在 SQL 语句中使用 `select * from ` 语句，应只查询必需的数据库字段以兼顾性能和安全性。

**解决方案 2：**

使用 jackson 注解 @JsonIgnore 限制数据执行序列化。

```
@Controller
class UserController {

	// 参数业务应使用POST传递
	@PostMapping("/user")
	public UserEntity getUser(@RequestBody JSONObject requestJson) {

		// 敏感字段 password 已受限
		return userService.findById(requestJson).get();
	}
}
class UserEntity {
	@Id
	private Long id;
	private String username;

	@JsonIgnore
	private String password;
}
```

- 使用 jackson 时，可以使用@JsonIgnore 同时禁止某字段执行序列化和反序列化
  - 只在字段的 get 方法使用@JsonIgnore 可以禁止字段执行序列化（防止敏感信息泄露）
  - 只在字段的 set 方法使用@JsonIgnore 可以禁止字段执行反序列化（防止恶意数据绑定）

**解决方案 3：**

使用 jackson 注解 @JsonIgnoreProperties 限制数据执行序列化。

```
@Controller
class UserController {

	// 参数业务应使用POST传递
	@PostMapping("/user")
	public UserEntity getUser(@RequestBody JSONObject requestJson) {

		// 敏感字段 password secretKey 已受限
		return userService.findById(requestJson).get();
	}
}

@JsonIgnoreProperties({"password","secretKey"})
class UserEntity {
	@Id
	private Long id;
	private String username;

	private String password;
	private String secretKey;
}
```

## 不安全的 SpringBoot Actuator 暴露

SpringBoot Actuator 如果不进行任何安全限制**直接**对外暴露访问接口，可导致敏感信息泄露甚至恶意命令执行。

**解决方案：**

```
// 参考版本 springboot 2.3.2
// pom.xml 配置参考
<!-- 引入 actuator -->
	<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-actuator</artifactId>
	</dependency>

<!-- 引入 spring security -->
	<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-security</artifactId>
	</dependency>

// application.properties 配置参考
#路径映射
management.endpoints.web.base-path=/sec_coding
#允许访问的ip列表
management.access.iplist = 127.0.0.1,192.168.1.100,192.168.2.3/24,192.168.1.6
#指定端口，应保证监控信息端口与应用业务端口不同
management.server.port=8081
#关闭默认打开的endpoint
management.endpoints.enabled-by-default=false
#需要访问的endpoint在这里打开
management.endpoint.info.enabled=true
management.endpoint.health.enabled=true
management.endpoint.env.enabled=true
management.endpoint.metrics.enabled=true
management.endpoint.mappings.enabled=true
#sessions需要spring-session包的支持
#management.endpoint.sessions.enabled=true
#允许查询所有列出的endpoint
management.endpoints.web.exposure.include=info,health,env,metrics,mappings
#显示所有健康状态
management.endpoint.health.show-details=always
```

参考链接：

- https://xz.aliyun.com/t/9763
- https://www.cnblogs.com/icez/p/Actuator_heapdump_exploit.html

## 不安全的 Swagger 暴露

Swagger 如果不进行任何安全限制**直接**对外暴露端访问路径，可导致敏感接口以及接口的参数泄露。

**解决方案：**

```
// 测试环境配置文件 application.properties 中
swagger.enable=true

// 生产环境配置文件 application.properties 中
swagger.enable=false

// java代码中变量 swaggerEnable 通过读取配置文件设置swagger开关
@Configuration
public class Swagger {
	@Value("${swagger.enable}")
	private boolean swaggerEnable;

	@Bean
	public Docket createRestApi() {
		return new Docket(DocumentationType.SWAGGER_2)
			//  变量 swaggerEnable 控制是否开启 swagger
			.enable(swaggerEnable)
			.apiInfo(apiInfo())
			.select()
			.apis(RequestHandlerSelectors.basePackage("com.tao.springboot.action"))
			//controller路径
			.paths(PathSelectors.any())
			.build();
    }
```

---

# 第十八条 严格会话管理

## 安全编码要求 ↓：

## 缺少 HttpOnly 标志的 Cookie

如果 Cookie 缺失 HttpOnly 标志，攻击者可以利用 XSS 攻击窃取用户的 Cookie。

**脆弱代码：**

```
Cookie cookie = new Cookie("email",userName);
response.addCookie(cookie);
```

**解决方案：**

```
Cookie cookie = new Cookie("email",userName);
cookie.setSecure(true);
cookie.setHttpOnly(true); //开启HttpOnly
```

强制保持 cookie 开启 HttpOnly 以保护用户鉴权。

## 不安全的 CORS 策略

所有涉及跨域的请求如不进行限制，可以导致攻击者窃取用户敏感信息。

**脆弱代码：**

```
response.addHeader("Access-Control-Allow-Origin", "*");
```

**解决方案：**

Access-Control-Allow-Origin 字段的值应依照部署情况进行白名单限制。

## 不安全的永久性 Cookie

Cookie 的如果不限制过期时间，可能导致攻击者窃取 Cookie 后执行越权操作。

**脆弱代码：**

```
Cookie cookie = new Cookie("email", email);
cookie.setMaxAge(60*60*24*365); // 设置一年的cookie有效期
```

**解决方案：**

- 应用程序禁止使用永久性 Cookie，并限制其最长使用期限为**30 分钟**。
- 应用程序登录前、登录后、退出后三个状态下的 cookie 不能一致。

## 不安全的广播（Android）

在未指定广播者权限的情况下注册的接收者将接收来自任何广播者的消息。如果这些消息包含恶意数据或来自恶意广播者，可能会对应用程序造成危害。应禁止 app 应用无条件接受广播。

**脆弱代码：**

```
Intent i = new Intent();
i.setAction("com.insecure.action.UserConnected");
i.putExtra("username", user);
i.putExtra("email", email);
i.putExtra("session", newSessionId);

this.sendBroadcast(v1);
```

**解决方案：**

配置（接收器）

```
<manifest>
	<!-- 权限宣告 -->
	<permission android:name="my.app.PERMISSION" />
	<receiver
		android:name="my.app.BroadcastReceiver"
		android:permission="my.app.PERMISSION"> <!-- 权限执行 -->
		<intent-filter>
			<action android:name="com.secure.action.UserConnected" />
		</intent-filter>
	</receiver>
</manifest>
```

配置（发送方）

```
<manifest>
	<!-- 声明拥有上述接收器发送广播的许可 -->
	<uses-permission android:name="my.app.PERMISSION"/>

	<!-- 使用以下配置，发送方和接收方应用程序都需要由同一个开发人员证书签名 -->
	<permission android:name="my.app.PERMISSION" android:protectionLevel="signature"/>
</manifest>
```

或者禁止响应一切外部广播

```
<manifest>
	<!-- 权限宣告 -->
	<permission android:name="my.app.PERMISSION" android:exported="false" />
	<receiver
		android:name="my.app.BroadcastReceiver"
		android:permission="my.app.PERMISSION">
		<intent-filter>
			<action android:name="com.secure.action.UserConnected" />
		</intent-filter>
	</receiver>
</manifest>
```

---
