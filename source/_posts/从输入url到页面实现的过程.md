---
title: 从输入url到页面加载完成的过程
date: 2018-04-5 12:03:12
tags:
---
大部分参考来自[从输入 URL 到页面加载完成的过程中都发生了什么事情？](https://link.zhihu.com/?target=https%3A//www.jianshu.com/p/71cf7f69eca8)

## 总过程：

1. 输入网址URL
2. 缓存机制
3. DNS解析
4. TCP连接
5. 发送http请求
6. 接受响应，判断状态码选择处理方式
7. 判断缓存
8. 解码
9. 渲染
10. 连接结束

## 1.输入网址Url

端口默认为80，一共有6要素:协议，域名，路径，查询参数，锚点，端口
<img src="https://pic3.zhimg.com/v2-24f77b31189d486d7948452308e53aa7_b.jpg" data-caption="" data-size="normal" data-rawwidth="1272" data-rawheight="344" class="origin_image zh-lightbox-thumb" width="1272" data-original="https://pic3.zhimg.com/v2-24f77b31189d486d7948452308e53aa7_r.jpg">![](https://pic3.zhimg.com/80/v2-24f77b31189d486d7948452308e53aa7_hd.jpg)
URI：**统一资源标识符**（英语：Uniform Resource Identifier）

URL:**统一资源定位符**(英语：Uniform Resource Locator）

URN:**统一资源名称**（英语：Uniform Resource Name）
<img src="https://pic4.zhimg.com/v2-b9108d75dd65e82adc31581dddcd222e_b.jpg" data-caption="" data-size="normal" data-rawwidth="257" data-rawheight="147" class="content_image" width="257">![](https://pic4.zhimg.com/80/v2-b9108d75dd65e82adc31581dddcd222e_hd.jpg)
URI可被视为定位符（URL），名称（URN）或两者兼备。

URL代表一个人的住址，如网址[http://www.wikipedia.org/](https://link.zhihu.com/?target=http%3A//www.wikipedia.org/)

URN如同一个人的名称，如书号[ISBN](https://link.zhihu.com/?target=https%3A//zh.wikipedia.org/wiki/urn%3AISBN) 0-395-36341-1 

> 换言之，URN定义某事物的身份，而URL提供查找该事物的方法

## 2.经过缓存机制

将URL与缓存进行比对如果请求的页面在缓存中且未过期，则直接进行**第7步**

**2.1 彻底缓存的机制（http首部字段）：cache-control，Expires**

--Expires是一个绝对时间，即服务器时间。浏览器检查当前时间，如果还没到失效时间就直接使用缓存文件。但是该方法存在一个问题：服务器时间与客户端时间可能不一致。因此该字段已经很少使用，现在基本用cache-control进行判断。

--cache-control中的max-age保存一个相对时间。例如Cache-Control: max-age = 484200，表示浏览器收到文件后，缓存在484200s内均有效。 如果同时存在cache-control和Expires，浏览器总是优先使用cache-control。

**2.2 当缓存过期时，浏览器会向服务器发起请求询问资源是否真正过期，这就是缓存协商。对应http首部字段：last-modified，Etag**

--last-modified是第一次请求资源时，服务器返回的字段，表示最后一次更新的时间。下一次浏览器请求资源时就发送if-modified-since字段。服务器用本地Last-modified时间与if-modified-since时间比较，如果不一致则认为缓存已过期并返回新资源给浏览器；如果时间一致则发送304状态码，让浏览器继续使用缓存。当然，用该方法也存在问题，比如修改时间有变化但实际内容没有变化，而服务器却再次将资源发送给浏览器。所以，使用Etag进行判断更好。

--Etag：资源的实体标识（哈希字符串），当资源内容更新时，Etag会改变。服务器会判断Etag是否发生变化，如果变化则返回新资源，否则返回304。

> 缓存协商的过程需要发起一起HTTP请求，如果返回304则继续使用缓存。对于移动端一次请求还是有代价的，所以我们需要避免304。
> 对于很少进行更改的静态文件，可以在文件名中加入版本号，如get.v1.js，并且把Cache-Control的max-age设置成一年半载，这样就不会发送请求。
> 需要注意的是，当这些文件更新的时候，我们需要更新其版本号，这样浏览器才会到服务器下载新资源。

<img src="https://pic1.zhimg.com/v2-b25c1588fb03898fccd8d4a1cc4931c1_b.jpg" data-caption="" data-size="normal" data-rawwidth="692" data-rawheight="1031" class="origin_image zh-lightbox-thumb" width="692" data-original="https://pic1.zhimg.com/v2-b25c1588fb03898fccd8d4a1cc4931c1_r.jpg">![](https://pic1.zhimg.com/80/v2-b25c1588fb03898fccd8d4a1cc4931c1_hd.jpg)

## 3.DNS解析得到ip：

由于ip比较难记，所以一般输入域名，通过dns解析得到ip。

網域名稱系統（英文：Domain Name System，縮寫：DNS）是互联网的一项服务。它作为将域名和IP地址相互映射的一个分布式数据库，能够使人更方便地访问互联网。

**DNS查找缓存顺序：浏览器缓存——>hosts文件——>路由器缓存——>网络服务商缓存——>根域名服务器缓存**

## 4.建立连接--三次握手

得到 ip 地址后，知道了服务器的 IP 地址，下面便开始与服务器建立连接了。

TCP是互联网中的传输层协议，提供可靠的链接服务，采用三次握手确认一个连接：

1. 浏览器向服务器发送建立连接的请求。

2. 服务器接收到浏览器发送的请求后，想浏览器发送统一连接的信号。

3. 浏览器接受到服务器发出的同意连接的信号后，再次向服务器发出确认连接的信号。

当三次握手返程，TCP客户端和服务端成功的建立连接，就可以开始传输数据了。

**第三次是防止了服务器端的一直等待而浪费资源**

三次握手的过程：
主机向服务器发送一个建立连接的请求（您好，我想认识您）；
服务器接到请求后发送同意连接的信号（好的，很高兴认识您）；
主机接到同意连接的信号后，再次向服务器发送了确认信号（我也很高兴认识您），自此，主机与服务器两者建立了连接。

TCP/IP协议层次 模型共分为四层，分别是：应用层、传输层、网络层、数据链路层

应用层 http、ftp、DNS    比如浏览器登录界面

传输层tcp/udp  传的快慢

网络层ip        路由器，指导传送路径

数据链路层    交换机，处理帧数据

tcp协议：发送信息以后，可以确认信息是否到达，也就是有应答的方式。先确认有没有人，传到之后也会告诉你成功了。可以比喻成很慢的打电话，可以有效确认。但是速度上是比udp慢的。

UDP协议：发送以后就不管了，不去确认信息是否到达。说什么就传什么的机制。可以比喻成飞快的写信，发了不知道有没有收到，速度上比tcp快。

[TCP 和 UDP 的区别是什么](https://link.zhihu.com/?target=https%3A//www.nowcoder.com/questionTerminal/63c8b45c91a544bd8febc1f1ff02e3b5%3FtoCommentId%3D73766)
TCP 可靠、面向连接、相对 UDP 较慢；

UDP 不可靠，不面向连接、相对 TCP 较快。搞定。
<img src="https://pic1.zhimg.com/v2-d23102b52df5cd3fe37ba61bba89f5b2_b.jpg" data-size="normal" data-rawwidth="700" data-rawheight="372" class="origin_image zh-lightbox-thumb" width="700" data-original="https://pic1.zhimg.com/v2-d23102b52df5cd3fe37ba61bba89f5b2_r.jpg">![](https://pic1.zhimg.com/80/v2-d23102b52df5cd3fe37ba61bba89f5b2_hd.jpg)osi模型
## 5.网页请求

浏览器会开始构造一个**应用层**的 http 请求，把**传输层**tcp 包被封到**网络层**的 ip 包里面，ip 包再被封装到**数据链路层**的数据帧结构中，再通过**物理层**的比特流送出去。

http规定如何请求响应的书写。应用层

tcp规定如何传输。传输层

ip规定如何连接以及对应位置。网络层

port对应ip不同服务

一个典型的 http 请求的 header 一般需要包括请求的方法，例如 GET 或者 POST 等。

浏览器向 web 服务器发送一个 HTTP 请求，请求中包含请求文件的位置、请求文件的方式等等

## 6.浏览器接受 HTTP 响应，检查 HTTP header 里的状态码，并做出不同的处理方式

> 比如404显示错误页面，304使用缓存，200下一步解码和渲染， 204页面不会发生更新。
> 常见状态码：200 ok, 204 no content, 206 partial content
> 301 moved permanently(资源已分配新的uri)，302 found(本次使用新uri访问)，303 see other(以get定向到另一个uri)，304 not modified, 307 temporary redirect(不会从post改为get)
> 400 bad request，402 unauthorized，403 forbidden, 404 not found
> 500 internal server error，503 service unavailable

## 7.如果是可以缓存的，这个响应则会被存储起来

根据首部字段判断是否进行缓存。例如，Cache-Control, no-cache(每次使用缓存前和服务器确认)，no-store 绝对禁止缓存

## 8.解码

9.1 浏览器拿到index.html文件后，就开始解析其中的html代码，遇到js/css/image等静态资源时，就向服务器端去请求下载

9.2 解析成对应的树形数据结构DOM树、CSS规则树，Javascript脚本通过DOM API和CSSOM API来操作DOM树、CSS规则树。

## 9.渲染

10.1 计算CSS样式（JS可动态修改dom或css,进一步改变渲染树和分布）

10.2 构建渲染树（Repaint：屏幕的一部分要重画，比如某个CSS的背景色变了，元素的几何尺寸没有变。Reflow：几何尺寸变了，我们需要重新验证并计算Render Tree。）

10.3 确认布局（定位坐标和大小，是否换行，各种position, overflow, z-index属性 ……）

10.4 绘制（调用操作系统Native GUI的API绘制，将每个节点转化为实际像素绘制到视口上）

## 10.断开连接--四次挥手

四次挥手的过程：
主机向服务器发送一个断开连接的请求（不早了，我该走了）；
服务器接到请求后发送确认收到请求的信号（知道了）；
服务器向主机发送断开通知（我也该走了）；
主机接到断开通知后断开连接并反馈一个确认信号（嗯，好的），服务器收到确认信号后断开连接；