---
layout: post
title: "PHP面试技巧(-) 垃圾回收相关"
subtitle: "PHP interview skills-garbage collection mechanism
"
author: "cuizhazha"
header-img: ""
header-bg-css: "linear-gradient(to right, #24b94a, #38ef7d);"
tags:
  - PHP面试
  - PHP
  - 垃圾回收
  - session
---
PHP
# 一次php请求过程图解
![](https://i.vgy.me/3N5ETC.png)

# php 的垃圾回收机制
PHP 使用了引用计数 (reference counting) GC 机制, PHP 可以自动进行内存管理，清除不需要的对象。

每个对象都内含一个引用计数器 refcount，每个 reference 连接到对象，计数器加 1。当 reference 离开生存空间或被设为 NULL，计数器减 1。当某个对象的引用计数器为零时，PHP 知道你将不再需要使用这个对象，释放其所占的内存空间。

# session的跨域共享
# session 与 cookie 的区别和联系
区别：

存放位置：Session 保存在服务器，Cookie 保存在客户端。

存放的形式：Session 是以对象的形式保存在服务器，Cookie 以字符串的形式保存在客户端。

用途：Cookies 适合做保存用户的个人设置，爱好等，Session 适合做客户的身份验证

路径：Session 不能区分路径，同一个用户在访问一个网站期间，所有的 Session 在任何一个地方都可以访问到。而 Cookie 中如果设置了路径参数，那么同一个网站中不同路径下的 Cookie 互相是访问不到的。

安全性：Cookie 不是很安全，别人可以分析存放在本地的 COOKIE 并进行 COOKIE 欺骗，考虑到安全应当使用 session

大小以及数量限制：每个域名所煲含的 cookie 数：IE7/8,FireFox:50 个，Opera30 个； Cookie 总大小：Firefox 和 Safari 允许 cookie 多达 4097 个字节，Opera 允许 cookie 多达 4096 个字 节，InternetExplorer 允许 cookie 多达 4095 个字节；一般认为 Session 没有大小和数量限制。

关系：

Session 需要借助 Cookie 才能正常工作。如果客户端完全禁止 Cookie，Session 将失效！因为 Session 是由应用服务器维持的一个 服务器端的存储空间，用户在连接服务器时，会由服务器生成一个唯一的 SessionID, 用该 SessionID 为标识符来存取服务器端的 Session 存储空间。而 SessionID 这一数据则是保存到客户端，用 Cookie 保存的，用户提交页面时，会将这一 SessionID 提交到服务器端，来存取 Session 数据。这一过程，是不用开发人员干预的。所以一旦客户端禁用 Cookie，那么 Session 也会失效。

# 如何修改 SESSION 的生存时间
设置浏览器保存的 sessionid 失效时间 setcookie (session_name (), session_id (), time () + $lifeTime, "/");

可以使用 SESSION 自带的 session_set_cookie_params (86400); 来设置 Session 的生存期

通过修改 php.ini 中的 session.gc_maxlifetime 参数的值就可以改变 session 的生存时间

# 类的属性可以序列化后保存到 session 中，从而以后可以恢复整个类，这个要用到的函数是?
函数： string serialize ( mixed $value )

serialize() 返回字符串，此字符串包含了表示 value 的字节流，可以存储于任何地方。 这有利于存储或传递 PHP 的值，同时不丢失其类型和结构。

想要将已序列化的字符串变回 PHP 的值，可使用 unserialize()。serialize()可处理除了 resource之外的任何类型。甚至可以 serialize() 那些包含了指向其自身引用的数组。你正 serialize()的数组／对象中的引用也将被存储。

当序列化对象时，PHP 将试图在序列动作之前调用该对象的成员函数 __sleep()。这样就允许对象在被序列化之前做任何清除操作。类似的，当使用 unserialize() 恢复对象时， 将调用 __wakeup() 成员函数。

# 比较CGI，FastCGI，PHP-CGI与PHP-FPM的区别
CGI

CGI全称是“公共网关接口”(Common Gateway Interface)，HTTP服务器与你的或其它机器上的程序进行“交谈”的一种工具，其程序须运行在网络服务器上。

CGI可以用任何一种语言编写，只要这种语言具有标准输入、输出和环境变量。如php,perl,tcl等。

FastCGI

FastCGI像是一个常驻(long-live)型的CGI，它可以一直执行着，只要激活后，不会每次都要花费时间去fork一次（这是CGI最为人诟病的fork-and-execute 模式）。它还支持分布式的运算，即 FastCGI 程序可以在网站服务器以外的主机上执行并且接受来自其它网站服务器来的请求。FastCGI是语言无关的、可伸缩架构的CGI开放扩展，其主要行为是将CGI解释器进程保持在内存中并因此获得较高的性能。众所周知，CGI解释器的反复加载是CGI性能低下的主要原因，如果CGI解释器保持在内存中并接受FastCGI进程管理器调度，则可以提供良好的性能、伸缩性、Fail- Over特性等等。

FastCGI特点

FastCGI具有语言无关性.FastCGI在进程中的应用程序，独立于核心web服务器运行，提供了一个比API更安全的环境。APIs把应用程序的代码与核心的web服务器链接在一起，这意味着在一个错误的API的应用程序可能会损坏其他应用程序或核心服务器。 恶意的API的应用程序代码甚至可以窃取另一个应用程序或核心服务器的密钥。

FastCGI技术目前支持语言有：C/C++、Java、Perl、Tcl、Python、SmallTalk、Ruby等。相关模块在Apache, ISS, Lighttpd等流行的服务器上也是可用的。

FastCGI的不依赖于任何Web服务器的内部架构，因此即使服务器技术的变化, FastCGI依然稳定不变。

FastCGI的工作原理

Web Server启动时载入FastCGI进程管理器（IIS ISAPI或Apache Module)

FastCGI进程管理器自身初始化，启动多个CGI解释器进程(可见多个php-cgi)并等待来自Web Server的连接。

当客户端请求到达Web Server时，FastCGI进程管理器选择并连接到一个CGI解释器。Web server将CGI环境变量和标准输入发送到FastCGI子进程php-cgi。

FastCGI子进程完成处理后将标准输出和错误信息从同一连接返回Web Server。当FastCGI子进程关闭连接时，请求便告处理完成。FastCGI子进程接着等待并处理来自FastCGI进程管理器(运行在Web Server中)的下一个连接。 在CGI模式中，php-cgi在此便退出了。

在上述情况中，你可以想象CGI通常有多慢。每一个Web请求PHP都必须重新解析php.ini、重新载入全部扩展并重初始化全部数据结构。使用FastCGI，所有这些都只在进程启动时发生一次。一个额外的好处是，持续数据库连接(Persistent database connection)可以工作。

FastCGI的不足

因为是多进程，所以比CGI多线程消耗更多的服务器内存，PHP-CGI解释器每进程消耗7至25兆内存，将这个数字乘以50或100就是很大的内存数。

PHP-CGI

PHP-CGI是PHP自带的FastCGI管理器。

PHP-CGI的不足：

php-cgi变更php.ini配置后需重启php-cgi才能让新的php-ini生效，不可以平滑重启。

直接杀死php-cgi进程，php就不能运行了。(PHP-FPM和Spawn-FCGI就没有这个问题，守护进程会平滑从新生成新的子进程。）

PHP-FPM

PHP-FPM是一个PHP FastCGI管理器，是只用于PHP的，可以在 http://php-fpm.org/download下载得到。

PHP-FPM其实是PHP源代码的一个补丁，旨在将FastCGI进程管理整合进PHP包中。必须将它patch到你的PHP源代码中，在编译安装PHP后才可以使用。

现在我们可以在最新的PHP 5.3.2的源码树里下载得到直接整合了PHP-FPM的分支，据说下个版本会融合进PHP的主分支去。相对Spawn-FCGI，PHP-FPM在CPU和内存方面的控制都更胜一筹，而且前者很容易崩溃，必须用crontab进行监控，而PHP-FPM则没有这种烦恼。

PHP5.3.3已经集成php-fpm了，不再是第三方的包了。PHP-FPM提供了更好的PHP进程管理方式，可以有效控制内存和进程、可以平滑重载PHP配置，所以被PHP官方收录了。

PHP-FPM的使用非常方便，配置都是在PHP-FPM.ini的文件内，而启动、重启都可以从php/sbin/PHP-FPM中进行。更方便的是修改php.ini后可以直接使用PHP-FPM reload进行加载，无需杀掉进程就可以完成php.ini的修改加载

结果显示使用PHP-FPM可以使php有不小的性能提升。PHP-FPM控制的进程cpu回收的速度比较慢,内存分配的很均匀。

而PHP-FPM合理的分配，导致总体响应的提到以及任务的平均。