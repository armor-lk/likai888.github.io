---
layout: post
title: session与cookie
description: 总结一下什么是session,什么是cookie,以及session与cookie的区别
category: blog
---

## session

Session:在计算机中，尤其是在网络应用中，称为“会话控制”。Session 对象存储特定用户会话所需的属性及配置信息。这样，当用户在应用程序的 Web 页之间跳转时，存储在 Session 对象中的变量将不会丢失，而是在整个用户会话中一直存在下去。当用户请求来自应用程序的 Web 页时，如果该用户还没有会话，则 Web 服务器将自动创建一个 Session 对象。当会话过期或被放弃后，服务器将终止该会话。Session 对象最常见的一个用法就是存储用户的首选项。例如，如果用户指明不喜欢查看图形，就可以将该信息存储在 Session 对象中。


session的工作原理

- 当一个session第一次被启用时，一个唯一的标识被存储于本地的cookie中。
- 首先使用session_start()函数，PHP从session仓库中加载已经存储的session变量。
- 当执行PHP脚本时，通过使用session_register()函数注册session变量。
- 当PHP脚本执行结束时，未被销毁的session变量会被自动保存在本地一定路径下的session库中，这个路径可以通过php.ini文件中的session.save_path指定，下次浏览网页时可以加载使用。


Session 通常用于执行以下操作

- 存储需要在整个用户会话过程中保持其状态的信息，例如登录信息或用户浏览 Web应用程序时需要的其它信息。
- 存储只需要在页重新加载过程中或按功能分组的一组页之间保持其状态的对象。
- Session 的作用就是它在 Web服务器上保持用户的状态信息供在任何时间从任何设备上的页面进行访问。因为浏览器不需要存储任何这种信息，所以可以使用任何浏览器


持久性方法的限制

- 随着越来越多用户登录，Session 所需要的服务器内存量也会不断增加。
- 访问 Web应用程序的每个用户都生成一个单独的 Session 对象。每个 Session象的持续时间是用户访问的时间加上不活动的时间。
- 如果每个 Session 中保持许多对象，并且许多用户同时使用 Web应用程序（创建许多 Session），则用于 Session 持久性的服务器内存量可能会很大，从而影响了可伸缩性。


Php如何创建Session

开始介绍如何创建 session。非常简单，真的。
启动 session 会话，并创建一个 $admin变量：

    // 启动 session
    session_start();

    // 声明一个名为 admin 的变量，并赋空值。
    $_session["admin"] = null;

如果你使用了 Session，或者该 PHP 文件要调用 Session变量，那么就必须在调用 Session 之前启动它，使用 session_start() 函数。其它都不需要你设置了，PHP 自动完成 session 文件的创建。
执行完这个程序后，我们可以到系统临时文件夹找到这个 session 文件，一般文件名形如：sess_4c83638b3b0dbf65583181c2f89168ec，后面是 32 位编码后的随机字符串。用编辑器打开它，看一下它的内容：

    admin|N;

Session是什么呢?简单来说就是服务器给客户端的一个编号。当一台WWW服务器运行时，可能有若干个用户浏览正在运行这台服务器上的网站。当每个用户首次与这台WWW服务器建立连接时，他就与这个服务器建立了一个Session，同时服务器会自动为其分配一个SessionID，用以标识这个用户的唯一身份。这个SessionID是由WWW服务器随机产生的一个由24个字符组成的字符串，我们会在下面的实验中见到它的实际样子。
这个唯一的SessionID是有很大的实际意义的。当一个用户提交了表单时，浏览器会将用户的SessionID自动附加在HTTP头信息中，(这是浏览器的自动功能，用户不会察觉到)，当服务器处理完这个表单后，将结果返回给SessionID所对应的用户。试想，如果没有SessionID，当有两个用户同时进行注册时，服务器怎样才能知道到底是哪个用户提交了哪个表单呢。

##cookie

Cookie 是在 HTTP 协议下，服务器或脚本可以维护客户工作站上信息的一种方式。Cookie 是由 Web 服务器保存在用户浏览器（客户端）上的小文本文件，它可以包含有关用户的信息。无论何时用户链接到服务器，Web 站点都可以访问 Cookie 信息。

主要用途

服务器可以利用Cookies包含信息的任意性来筛选并经常性维护这些信息，以判断在HTTP传输中的状态。Cookies最典型的应用是判定注册用户是否已经登录网站，用户可能会得到提示，是否在下一次进入此网站时保留用户信息以便简化登录手续，这些都是Cookies的功用。另一个重要应用场合是“购物车”之类处理。用户可能会在一段时间内在同一家网站的不同页面中选择不同的商品，这些信息都会写入Cookies，以便在最后付款时提取信息。

用户可以改变浏览器的设置，以使用或者禁用Cookies

cookie具体使用方法

简单的：

    SetCookie("MyCookie","Value of MyCookie");

带失效时间的：

    SetCookie("WithExpire","Expire in 1 hour",time()+3600);//3600秒=1小时

如果要设置同名的多个Cookie，要用数组，方法是：

    SetCookie("CookieArray[]","Value 1");
    SetCookie("CookieArray[]","Value 2");

或
    SetCookie("CookieArray[0]","Value 1");
    SetCookie("CookieArray[1]","Value 2");

接收和处理Cookie

##cookie 和session 的区别：

1、cookie数据存放在客户的浏览器上，session数据放在服务器上。

2、cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗
   考虑到安全应当使用session。

3、session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能
   考虑到减轻服务器性能方面，应当使用COOKIE。

4、单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。

5、所以个人建议：
   将登陆信息等重要信息存放为SESSION
   其他信息如果需要保留，可以放在COOKIE中


