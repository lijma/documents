1. 请解释一下什么是Nginx?

Nginx是一个web服务器和方向代理服务器，用于HTTP、HTTPS、SMTP、POP3和IMAP协议。

2. Nginx和apache的区别？

轻量级，同样起web 服务，比apache 占用更少的内存及资源
抗并发，nginx 处理请求是异步非阻塞的，而apache 则是阻塞型的，在高并发下nginx 能保持低资源低消耗高性能
高度模块化的设计，编写模块相对简单
最核心的区别在于apache是同步多进程模型，一个连接对应一个进程；nginx是异步的，多个连接（万级别）可以对应一个进程
3. Nginx是如何实现高并发的

答：一个主进程，多个工作进程，每个工作进程可以处理多个请求，每进来一个request，会有一个worker进程去处理。但不是全程的处理，处理到可能发生阻塞的地方，比如向上游（后端）服务器转发request，并等待请求返回。那么，这个处理的worker继续处理其他请求，而一旦上游服务器返回了，就会触发这个事件，worker才会来接手，这个request才会接着往下走。由于web server的工作性质决定了每个request的大部份生命都是在网络传输中，实际上花费在server机器上的时间片不多。这是几个进程就解决高并发的秘密所在。即@skoo所说的webserver刚好属于网络io密集型应用，不算是计算密集型。

4. 请解释Nginx如何处理HTTP请求

Nginx使用反应器模式。主事件循环等待操作系统发出准备事件的信号，这样数据就可以从套接字读取，在该实例中读取到缓冲区并进行处理。单个线程可以提供数万个并发连接。

5. 使用“反向代理服务器”的优点是什么?

反向代理服务器可以隐藏源服务器的存在和特征。它充当互联网云和web服务器之间的中间层。这对于安全方面来说是很好的，特别是当您使用web托管服务时。

6. 请列举Nginx服务器的最佳用途。

Nginx服务器的最佳用法是在网络上部署动态HTTP内容，使用SCGI、WSGI应用程序服务器、用于脚本的FastCGI处理程序。它还可以作为负载均衡器。

7. 请解释Nginx服务器上的Master和Worker进程分别是什么?

Master进程：读取及评估配置和维持
Worker进程：处理请求

8. 在Nginx中，解释如何在URL中保留双斜线?

要在URL中保留双斜线，就必须使用merge_slashes_off;

语法:merge_slashes [on/off]

默认值: merge_slashes on

环境: http，server

9. 请解释ngx_http_upstream_module的作用是什么?

ngx_http_upstream_module用于定义可通过fastcgi传递、proxy传递、uwsgi传递、memcached传递和scgi传递指令来引用的服务器组。

10、请解释什么是C10K问题?

C10K问题是指无法同时处理大量客户端(10,000)的网络套接字。

11、请陈述stub_status和sub_filter指令的作用是什么?

Stub_status指令：该指令用于了解Nginx当前状态的当前状态，如当前的活动连接，接受和处理当前读/写/等待连接的总数
Sub_filter指令：它用于搜索和替换响应中的内容，并快速修复陈旧的数据
12、解释Nginx是否支持将请求压缩到上游?

您可以使用Nginx模块gunzip将请求压缩到上游。gunzip模块是一个过滤器，它可以对不支持“gzip”编码方法的客户机或服务器使用“内容编码:gzip”来解压缩响应。

13、解释如何在Nginx中获得当前的时间?

要获得Nginx的当前时间，必须使用SSI模块、d a t e g m t 和 date_gmt和date
g

mt和date_local的变量。Proxy_set_header THE-TIME $date_gmt;

18、用Nginx服务器解释-s的目的是什么?

用于运行Nginx -s参数的可执行文件。

19、解释如何在Nginx服务器上添加模块?

在编译过程中，必须选择Nginx模块，因为Nginx不支持模块的运行时间选择。
————————————————
版权声明：本文为CSDN博主「ThisLX」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/li521wang/article/details/89337282
