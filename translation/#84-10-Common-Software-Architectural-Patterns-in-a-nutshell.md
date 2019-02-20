https://towardsdatascience.com/10-common-software-architectural-patterns-in-a-nutshell-a0b47a1e9013
# 10 Common Software Architectural Patterns in a nutshell
# 简述10大通用软件架构模式

Ever wondered how large enterprise scale systems are designed? Before major software development starts, we have to choose a suitable architecture that will provide us with the desired functionality and quality attributes. Hence, we should understand different architectures, before applying them to our design.
大型的企业级系统是如何设计的呢？想必大家都曾经有过这样的疑惑。大型软件开发前，我们必须选择一种合适的架构，它既要提供我们想要的功能，质量上也要过关。因此，在应用不同的架构之前，我们有必要熟悉一下这些架构。

## What is an Architectural Pattern?
## 什么是架构模式？

According to Wikipedia,
> An architectural pattern is a general, reusable solution to a commonly occurring problem in software architecture within a given context. Architectural patterns are similar to software design pattern but have a broader scope.

根据维基百科，
> 针对软件架构中给定上下文的常见问题，架构模式是一种通用的、可复用的解决方案。它与软件设计模式相似，但范围更广。

In this article, I will be briefly explaining the following 10 common architectural patterns with their usage, pros and cons.

在这篇文章中，我将简要地说明一下10个通用的架构模式，以及它们的用法和利弊。

1. Layered pattern
2. Client-server pattern
3. Master-slave pattern
4. Pipe-filter pattern
5. Broker pattern
6. Peer-to-peer pattern
7. Event-bus pattern
8. Model-view-controller pattern
9. Blackboard pattern
10. Interpreter pattern


1. 分层模式
2. 客户端 - 服务端模式(cs模式)
3. 主从模式
4. 管道过滤器模式
5. 代理人模式
6. 点对点模式
7. 事件总线模式
8. 模型 - 视图 - 控制器模式(MVC模式)
9. 黑板模式
10. 解释器模式


## 1. Layered pattern
## 1. 分层模式


This pattern can be used to structure programs that can be decomposed into groups of subtasks, each of which is at a particular level of abstraction. Each layer provides services to the next higher layer.
这种模式

The most commonly found 4 layers of a general information system are as follows.
以下是最常见的通用信息系统中的4个层次。

- Presentation layer (also known as UI layer)
- Application layer (also known as service layer)
- Business logic layer (also known as domain layer)
- Data access layer (also known as persistence layer)

- 表示层（亦称为 UI层）
- 应用层（亦称为 服务层）
- 业务逻辑层（亦称为 领域层）
- 数据访问层（亦称为 持久层）

**Usage**
- General desktop applications.
- Ecommerce web applications.

**用法**
- 通用桌面应用
- 电子商务web应用

![Layered pattern](https://cdn-images-1.medium.com/max/800/1*jMWk_JqqyyloVPhTs_Zd1A.png)

## 2. Client-server pattern
## 2. 客户端-服务端模式

This pattern consists of two parties: a server and multiple clients. The server component will provide services to multiple client components. Clients request services from the server and the server provides relevant services to those clients. Furthermore, the server continues to listen to client requests.
该模式包含一个服务端和多个客户端。服务端组件给多个客户端组件提供服务。客户端向服务端请求服务，服务端提供相关的服务。此外，服务端会继续监听客户端的请求。


**Usage**
- Online applications such as email, document sharing and banking.

**用法**
- 在线应用，例如电子邮件、文件共享和存储。

![Client-server pattern](https://cdn-images-1.medium.com/max/800/1*4xX_WQQuD2u0PMK5bcWFkQ.png)

## 3. Master-slave pattern
## 3. 主从模式

This pattern consists of two parties; master and slaves. The master component distributes the work among identical slave components, and computes a final result from the results which the slaves return.
该模式包含两部分；主和从。主组件分配工作给完全相同的从组件，并根据从从组件中返回的结果计算最终结果.

**Usage**
- In database replication, the master database is regarded as the authoritative source, and the slave databases are synchronized to it.
- Peripherals connected to a bus in a computer system (master and slave drives).

**用法**
- 主服务器是权威来源，从属数据库与其进行同步。
- 在计算机系统中，外围设备连接到总线中（主驱动和从驱动）。

![Master-slave pattern](https://cdn-images-1.medium.com/max/800/1*lsK9QntZl2d5oLojwRGXDg.png)

