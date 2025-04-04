---
title: 'Java的URLDNS利用链'
date: 2025-01-23T23:15:28+08:00
lastmod: 2025-01-23T23:15:28+08:00
draft: false
slug: 315c5592
tags: ["Java"]
---

## 利用链

- Java 反序列化漏洞的难点不在于发现，而在于如何利用

为了完成最终的危险操作，实战中的反序列化攻击往往需要结合很多 serialize 接口，形成复杂的调用链，这一过程非常繁琐。

著名 Java 反序列化利用工具 ysoserial 集成了很多利用链，可以直接使用。

```bash
java -jar ysoserial-all.jar
```

### ysoserial 的 payloads

稍微解释一下 ysoserial 源码中的 payloads 文件夹，之后分析利用链会用到。

payloads 文件夹中每个文件就是一个 payload （一个公共类），每个 payload 类中都会定义一个`getObject` 方法，这个方法会返回一个**对应 payload 的对象**，该对象会在之后被 ysoserial 工具进一步处理，最终生成**序列化的字节流**。

![](../../../../img/Pasted%20image%2020250123194017.png)

另外，每个 payload 文件（比如后文调试了 URLDNS.java ），都会写一个 main：

![](../../../../img/Pasted%20image%2020250123194916.png)

其中这个 `PayloadRunner.run` 会干3件事：
1. 调用 `getObject`
2. 生成序列化数据作为输出的 payload
3. 本地反序列化测试生成的 payload 是否有效

所以，可以单独调试一个 payload.java 文件，来看这个 payload 实际反序列触发利用的过程。


---
## URLDNS 链

参数是一个 URL，结果是触发⼀次 DNS 请求。

优点：
- 使⽤ Java 内置的类构造，对第三⽅库**没有依赖**
- 在⽬标没有回显的时候，能够通过 DNS 请求得知是否存在反序列化漏洞

### 调用链

从第一个 readObject 的反序列化进入，顺着函数中其他方法找下去，最终找到一个可能造成危害的函数进行注入利用。（建议直接看源码分析）

```java
 *     HashMap.readObject()
 *       HashMap.putVal()
 *         HashMap.hash()
 *           URL.hashCode()
```


### 源码分析

从 ysoserial payloads 中的 `URLDNS.java` 一步步调试分析。

反序列化的入口点是 `HashMap.readObject()`，直接去看这个函数，其中 `hash`函数是关键的利用点，先打个断点，开始调试：

![](../../../../img/Pasted%20image%2020250123175832.png)

- 注意，之前讲到 payload 文件主函数的 `PayloadRunner` 会干三件事，这边断点拦截到的是第三件事：反序列化，反序列化一定会触发 `readObject`。

步入 `hash` ：

![](../../../../img/Pasted%20image%2020250123175916.png)

步入 `hashCode`:

![](../../../../img/Pasted%20image%2020250123180030.png)

步入 `handler.hashCode`：

![](../../../../img/Pasted%20image%2020250123180129.png)

步入 `getHostAddress`：

![](../../../../img/Pasted%20image%2020250123180225.png)

最终找到 `InetAddress.getByName(host)` ，这个方法进行了 DNS 查询操作。

单步执行之后可以在反连平台看到 DNS 查询：

![](../../../../img/Pasted%20image%2020250123180534.png)



URL 类的 hashCode 很简单。如果 hashcode 不为 -1，则返回 hashcode。在序列化构造 payload 的时候，需要设置 hashcode 为 -1 的原因，就是防止进入到 `hashcode` 方法中，进而发送 DNS 请求，影响判断。

