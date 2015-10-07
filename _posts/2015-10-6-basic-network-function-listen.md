---
layout : post
time : 2015-10-6
title : Some basic functions in Network Programming
category : network programming 
keywrods : network programming
tags : network programming
description : basic network programming function
---

## Socket

### 简介

位于＜sys/socket.h＞,用于指定期望的通信协议类型．

### 函数原型

int socket(int family, int type, int protocol);

  * 成功返回非负描述符，失败返回-1.
  * family为协议簇:AF_INET ipv4, AF_INET6 ipv6 ....
  * type为socket类型:SOCK_STREAM　字节流socket ....
  * protocol为协议类型:TCP,UDP,STCP.可以设置为０来选择默认类型

----

## Connect

###　简介

TCP客户端使用connect与服务器建立连接．

客户端调用connect前不必非得调用bind，内核可以确定源IP地址，并选择一个临时端口作为源端口．

###　函数原型

int connect(int sockfd, const struct sockaddr *servaddr, socklen_t addrlen);

如果是TCP socket，调用connect仅在成功或失败时才返回．失败的原因：

  1. 客户端超时未收到SYN相应．(EDIMEOUT)
  
  2. 对客户端的相应是RST.(ECOUNNERFUSED).
  
    * 目的端口无正在监听的服务器.
    
    * TCP想取消已有连接．
    
    * TCP收到不存在的连接上的分组．

　3. 客户发出的SYN在中间路由器引发"destination unreachable"的ICMP错误．

### 效果

  * connect函数导致socket从CLOSED状态转为SYN_SENT，成功后转为ESTABLISHED．

  * 若失败，必须close，然后重新调用socket．

----


## Bind

### 简介

位于＜sys/socket.h＞中,把一个本地协议地址赋予socket.可以指定IP address或端口．

### 函数原型

int bind(int socketfd, const struct sockaddr *myaddr, socklen_t addrlen);

  * 服务器在启动时绑定周知端口．客户端可以**不调用**bind，让内核选择临时端口．
 
### 效果

|*IP address*|*Port*|*Effect*|
|-----------|:------------:|:---------|
|通配地址|0|内核选定IP地址和端口|
|通配地址|非0|内核选定IP地址|
|本地IP地址|0|内核选择端口|
|本地IP地址|非0|进程指定|

对于IPV4,通配地址由INADDR_ANY指定．　htonl(INADDR_ANY)

如果让内核选择，需调用getsocketname来返回地址．

----

## Listen

### 简介

位于＜sys/socket.h＞中，仅由TCP服务器在bind和accept函数之间调用．

### 函数原型

int listen(int  sockfd, int backlog);

  * backlog设定了内核为socket排队的最大连接数． 
    
    *内核为给定的**监听socket**维护两个队列．
    
      1. 位于SYN_RCVD的未完成连接队列 (收到客户端SYN,发送SYN和ACK,等待ACK）．队列中的元素一直保留到**完成三次握手**或**超时**.
      
      2. 位于ESTABLISHED的已完成连接队列．若三次握手正常完成，连接就移到已完成队列尾部．
      
      两个队列之和 <= backlog(与syn flood相关,伪造的SYN装入未完成队列，使合法的SYN排不上队）．

    ＊当不希望任何客户连接到监听socket时，不要设置为０，不同的实现有不同解释．应该关闭该socket．
    
    ＊当客户端SYN到达时，若队列已满，TCP就忽略该分组(不发送RST).因为这种情况可能时短暂的，客户端超时重发SYN,可能就会在队列中找到可用空间．如果服务器相应RST,客户端的connect会返回错误，**强制**应用程序处理，而非让TCP**正常**重传处理．此外，客户端无法区别RST是＂**端口没有服务器在监听**＂还是＂**端口虽有服务器监听，但是队列满了**＂．

### 效果

  * 当socket函数创建一个套接字时，它被假设为一个**主动socket**（将调用connect发起连接的客户端socket）．
  * listen函数将未连接的套接字转换为**被动socket**，指示内核应接受**指向该socket**的连接请求．


-----

## Accept

### 简介

accept位于＜sys/socket.h＞中,仅由TCP服务器调用．用于从**ESTABLISHED 队列**头返回下一个已完成的连接．如果已完成队列的**队头**会返回,若队列为空，进程睡眠，直到TCP在队列中放入一项.

###　函数原型
int accept(int sockfd, struct sockaddr *cliaddr, socklen_t *addrlen);

  * cliaddr和addrlen用于获取已连接的客户端协议地址．

  * 在这里存在两个不同类型的socket的概念．
  
>sockfd为**监听socket(listening socket）**描述符，它是由socket创建，用作bind和listen第一个参数的描述符).

>如果返回成功，则返回值是内核自动生成的**已连接套接字(connected socket）**，代表与所返回客户的TCP连接．

>一个服务器通常只创建一个listening socket，它在服务器的生命周期内一直存在．内核为每个由服务器进程接受的客户端连接创建一个connected socket.当服务器完成对某给定客户的服务后，相应的connected socket被关闭．

----

## Close

### 简介

位于＜unistd.h＞,可用于关闭socket，并终止TCP连接．

每个文件都有引用计数，它是打开这的引用该文件的描述符的个数．close将引用计数减一．如果socket的引用计数变为０，则发送FIN.

如果要在TCP上发送FIN,可用shutdown函数．

----


