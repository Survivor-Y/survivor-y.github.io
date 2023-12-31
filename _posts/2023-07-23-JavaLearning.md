---
layout: post
title: 'JavaLearning'
date: 2023-07-23
author: survivor
tags: Java Mysql Spring
---
「Java学习材料」对自身学习&面试等进行一个系统的整理



- [网络基础](#网络基础)
  - [TCP协议](#tcp协议)
    - [三次握手](#三次握手)
    - [为什么两次不行？](#为什么两次不行)
    - [四次挥手](#四次挥手)
  - [OSI与TCP/IP模型](#osi与tcpip模型)
  - [HTTP/HTTPS](#httphttps)
    - [get和post的区别](#get和post的区别)
    - [cookie和session区别](#cookie和session区别)
    - [浏览器输入URL（Uniform Resource Locator）过程](#浏览器输入urluniform-resource-locator过程)
- [操作系统基础](#操作系统基础)
  - [什么是操作系统？](#什么是操作系统)
    - [进程和线程的区别](#进程和线程的区别)
    - [进程间通信方式IPC（InterProcess Communication）](#进程间通信方式ipcinterprocess-communication)
    - [进程调度算法](#进程调度算法)
    - [用户态和内核态](#用户态和内核态)
  - [内存管理](#内存管理)
    - [常见页面置换算法](#常见页面置换算法)
- [Java基础](#java基础)

## 网络基础

### TCP协议

![]({{site.url}}/assets/img/tcpThreeFour.png)

#### 三次握手

目的是为了建立可靠的通信信道，即全双工的通信，需要确定通信双方都具备完好的接收和发送能力

1. client   发送带有syn标志的数据包， **一次握手**进入syn_sent状态——server端确认：client发送正常，server端接收正常
2. server 发送带有syn、ack标志的数据包， **二次握手**进入syn_rcvd状态——client端确认：client发送接收都正常，server端发送接收都正常
3. client  发送带有ack标志的数据包，**三次握手**连接成功进入established状态——server端确认：client发送接收都正常，server端发送接收都正常

#### 为什么两次不行？

1. 首先要理解三次握手都做了什么，从上面的图解及描述可以看出，如果只有两次，最终client端能够确认双方的接发都没问题，但是server无法确认client的接收和自身的发送是否正常，无法达到全双工通信的要求
2. 在网络拥塞的情况下，如果只有二次握手，只要服务端发出确认就可以建立链接，那客户端一直不发数据，服务端就会一直等待造成资源浪费

#### 四次挥手

1. client 发送带有FIN标志的数据包，关闭与server的连接——client进入FIN-WAIT-1状态
2. server 收到FIN，发回一个ACK，确认序号为收到的序号+1——server 进入了CLOSE-WAIT的状态
3. server 发送一个FIN数据包，关闭与client的连接——client进入FIN-WAIT-2的状态
4. client 收到FIN，发回ACK确认，并将确认序号设置为收到序号+1——client进入TIME-WAIT的状态，server进入CLOSE状态，此时client等待2MSL（**Maximum Segment Lifetime**一个发送和一个回复所需的最大时间）的时间后依然没有收到信息，确认server已关闭，client也关闭了连接

### OSI与TCP/IP模型

![]({{site.url}}/assets/img/OSITCP.png)

### HTTP/HTTPS

HTTP 协议（Hypertext Transfer Protocol），主要是来规范浏览器和服务器端的行为的，HTTP 是一个无状态（stateless）协议，也就是说服务器不维护任何有关客户端过去所发请求的消息。默认端口80，明文传输，数据未加密，安全性差，响应速度快，消耗资源少。HTTP协议是运行再TCP之上。

HTTPS 协议（Hyper Text Transfer Protocol Secure），是 HTTP 的加强安全版本。HTTPS 是基于 HTTP 的，也是用 TCP 作为底层协议，并额外使用 SSL/TLS 协议用作加密和安全认证。默认端口443，传输过程ssl加密，安全性较好，响应速度较慢，消耗资源多，需要用到CA证书。HTTPS协议运行再SSL/TLS上

#### get和post的区别

- get入参在URL中对所有人可见，post数据不会在URL中
- get与post相比，安全性较差，post的参数不会被保存在浏览器历史或web服务器日志中
- get可以传送的数据不能大于2kb，post数据理论上不受限制
- get请求可以缓存，并且作为书签

#### cookie和session区别

- cookie数据保存在客户端（浏览器），session数据保存在服务器端

- cookie不是很安全，浏览器的cookie可以被伪造，安全性更高的是session
- cookie一般用来保存用户信息，session主要作用就是通过服务端记录用户的状态

#### 浏览器输入URL（Uniform Resource Locator）过程

1. 浏览器查找DNS（Domain Name System, 域名系统）的IP地址（DNS：获取域名对应的IP）
2. 根据IP建立TCP链接（TCP：与服务器建立连接）
3. 浏览器向服务器发送HTTP请求（HTTP：发送请求）
4. 服务器响应HTTP请求，把数据传回浏览器
5. 浏览器解析HTML内容并渲染

## 操作系统基础

### 什么是操作系统？

OS（Operating System）的本质是一个软件程序，屏蔽了硬件层的复杂性，用来管理和调用计算机硬件和软件资源。主要是进程和线程的管理，存储管理，文件管理，设备管理，网络管理，安全管理等

![]({{site.url}}/assets/img/os.png)

- Kernel操作系统核心属于操作系统层面，屏蔽了对硬件的操作
- CPU（Central Processing Unit）属于硬件，主要提供运算，处理各种指令的能力。

#### 进程和线程的区别

**进程**是资源分配的最小单位，指计算机中正在运行的一个程序实例。

**线程**是任务调度和执行的最小单位，更加轻量，多个线程可以在同一个进程中同时执行。

![]({{site.url}}/assets/img/jincheng.png)

#### 进程间通信方式IPC（InterProcess Communication）

- **管道Pipes**：用于具有亲缘关系的父子进程间和兄弟进程之间的通信
- **信号**：一种比较复杂的通信方式，通知接收进程某个事件已经发生。
- **消息队列**：消息队列克服了信号承载信息量少，管道只能承载无格式字 节流以及缓冲区大小受限等缺点。
- **信号量**：是一个计数器，用于多进程对共享数据的访问，信号量的意图在于进程间同步。这种通信方式主要用于解决与同步相关的问题并避免竞争条件。
- **共享内存**：多进程可以直接读写同一块内存空间，最有用的进程间通信方式
- **套接字**：客户端和服务器通过网络进行通信，套接字是双方的一种约定，套接字是支持TCP/IP的网络通信的基本通信单元。

#### 进程调度算法

- **FCFS**（Fisrt Come，First Served）：先到先服务
- **SJF**（Shortest Job First）：短作业优先
- RR（Round-Robin）：时间片轮转
- **MFQ**（Multi-Level FeedBack Queue）：公认较好的进程调度算法，Unix采用的

#### 用户态和内核态

**用户态**：只能受限的访问内存，运行所有应用程序

**核心态**：运行操作系统程序，cpu可以访问内存的所有数据，包括外围设备，主要是为了保证计算机系统的安全性、稳定性和性能。

### 内存管理

**内部内存碎片**：是指已经分配给进程的内存块中，存在着未被充分利用的空间。例如，当为一个固定大小的数据结构分配内存，但实际使用的空间小于分配的大小时，就会产生内部碎片。

**外部内存碎片**：是指由于动态内存分配和释放过程中，导致剩余的未分配内存块被零散占据，无法满足大块内存的需求。虽然总的空闲内存足够，但无法分配连续的内存空间。

**页式管理**：将程序的逻辑地址分为固定大小的page，物理内存划分为页框，页框不必连续，这样就没有外碎片，但是会产生内碎片

**段式管理**：将程序的地址空间划分为若干段，段大小可变，没有内碎片，但段换入换出时，会产生外碎片

**段页式管理**：结合段和页的有点，先把驻村分成若干段，每个段分成若干页

#### 常见页面置换算法

- 最佳页面置换算法（OPT）：优先淘汰未来长时间不被访问的或不再被使用的页面，过于理想化无法实现，但可做为评判其他算法的标准
- 先进先出页面置换算法（FIFO）：最简单的算法
- 最近最久未使用页面置换算法（LRU）：选择最近且最久未使用的页面进行淘汰
- 最少使用页面置换算法（LFU）：和LRU类似，只不过标准是一段时间内页面使用次数

## Java基础

面相对象