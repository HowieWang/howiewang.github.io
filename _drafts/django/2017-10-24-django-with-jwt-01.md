---
layout: post
title:  all python references
category: 
- reference  
stickie: true
---

* content
{:toc}


目录
[什么是jwt汤逊？](https://krzysztofzuraw.com/blog/2016/jwt-in-django-application-part-one.html#what-is-jwt)
[您如何使用jwt，并为什么？](https://krzysztofzuraw.com/blog/2016/jwt-in-django-application-part-one.html#how-can-you-use-jwt-and-why)
[资源](https://krzysztofzuraw.com/blog/2016/jwt-in-django-application-part-one.html#resources)
[本系列的其他博客文章:](https://krzysztofzuraw.com/blog/2016/jwt-in-django-application-part-one.html#other-blog-posts-in-this-series)

[什么是jwt汤逊？](https://krzysztofzuraw.com/blog/2016/jwt-in-django-application-part-one.html#id1)
jwt aka JSONweb令牌是一种身份验证方法。它做了什么:您作为用户首先向服务器发送一个请求，说:我要登录！服务器为您提供了一个长的字符序列。当你得到这个序列时，你可以用它来告诉服务器你是你真正的人。
在一个更技术的意义上:您发送一个请求，它记录您使用登录和密码的示例头服务。作为响应，您得到了加密令牌。然后，您希望获取有关服务器上需要身份验证的其他资源的信息。因此，对于您的请求，您只需添加一个之前收到的令牌，这就是所有！你被证实了。
JSON web标记看起来像这样:
HEADER.PAYLOAD.SIGNATURE

header是一个JSON，它包含一个类型的令牌( jwt )和将使用的散列算法( hmac sha256或RSA)。hmac代表密钥哈希消息身份验证代码。消息认证码( MAC)用于确认消息来自好的发送者，其完整性没有被改变。密钥哈希代表哈希MAC与密钥结合使用。
有效载荷包含索赔。声称存储信息用户希望传输和服务器可以使用来正确处理身份验证。有很多已登记的索赔，但我们将只使用:
“exp”(过期时间)索赔
" nbf "(不在时间之前)索赔
" ISS "(发行人)索赔
“澳元”(观众)索赔
" IAT "(发出)索赔

有效载荷将如下所示:
{ "exp": "1234567890", "name": "Krzysztof Zuraw",}

最后一部分是签名。它基本上是所有前面提到的base64 +secret编码的部分的总和。

[您如何使用jwt，并为什么？](https://krzysztofzuraw.com/blog/2016/jwt-in-django-application-part-one.html#id2)
当您从带有JSON web令牌的服务器返回响应时，您可以在这样的标题中使用它:
Authorization: Bearer <JWT token>

与另一种身份验证方法相比，沙姆尔jwt更紧凑。JSON格式在编程word中被广泛使用，因此对于该格式的解析器没有问题。
这是今天的所有，并继续关注的下一个博客系列关于jwt汤逊！

[资源](https://krzysztofzuraw.com/blog/2016/jwt-in-django-application-part-one.html#id3)
[https : / / www . jwt.io . org /](https://jwt.io/)

[本系列的其他博客文章:](https://krzysztofzuraw.com/blog/2016/jwt-in-django-application-part-one.html#id4)
[django应用程序中的JSON web标记--第二部分](https://krzysztofzuraw.com/blog/2016/jwt-in-django-application-part-two.html)
[django应用程序中的JSON web标记--第三部分](https://krzysztofzuraw.com/blog/2016/jwt-in-django-application-part-three.html)
[django应用程序中的JSON web标记--第四部分](https://krzysztofzuraw.com/blog/2016/jwt-in-django-application-part-four.html)