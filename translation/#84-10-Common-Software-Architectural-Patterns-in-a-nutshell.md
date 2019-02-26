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
5. 代理模式
6. 点对点模式
7. 事件总线模式
8. 模型-视图-控制器模式(MVC模式)
9. 黑板模式
10. 解释器模式


## 1. Layered pattern
## 1. 分层模式


This pattern can be used to structure programs that can be decomposed into groups of subtasks, each of which is at a particular level of abstraction. Each layer provides services to the next higher layer.
这种模式可用于构建能分解成子任务组的程序，每个子任务处在特定的抽象级别中。每一层为更高一层提供服务。

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

![分层模式](https://cdn-images-1.medium.com/max/800/1*jMWk_JqqyyloVPhTs_Zd1A.png)

## 2. Client-server pattern
## 2. 客户端-服务端模式

This pattern consists of two parties: a server and multiple clients. The server component will provide services to multiple client components. Clients request services from the server and the server provides relevant services to those clients. Furthermore, the server continues to listen to client requests.
该模式包含一个服务端和多个客户端。服务端组件给多个客户端组件提供服务。客户端向服务端请求服务，服务端提供相关的服务。此外，服务端会继续监听客户端的请求。


**Usage**
- Online applications such as email, document sharing and banking.

**用法**
- 在线应用，例如电子邮件、文件共享和存储。

![客户端-服务端模式](https://cdn-images-1.medium.com/max/800/1*4xX_WQQuD2u0PMK5bcWFkQ.png)

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

![主从模式](https://cdn-images-1.medium.com/max/800/1*lsK9QntZl2d5oLojwRGXDg.png)

## 4. Pipe-filter pattern
## 4. 管道过滤模式

This pattern can be used to structure systems which produce and process a stream of data. Each processing step is enclosed within a filter component. Data to be processed is passed through pipes. These pipes can be used for buffering or for synchronization purposes.
该模式可用于构建生成和处理数据流的系统。每个处理步骤包含在一个过滤组件中。待处理的数据通过管道传递。这些管道可用于数据缓存或同步。

**Usage**
- Compilers. The consecutive filters perform lexical analysis, parsing, semantic analysis, and code generation.
- Workflows in bioinformatics.

**用法**
- 编译器。连续的过滤器执行词法分析，解析，语意分析，和代码生成。
- 生物信息学中的工作流。

![管道过滤模式](https://cdn-images-1.medium.com/max/800/1*qikehZcDhhl_wWsqeI_nvg.png)


## 5. Broker pattern
## 5. 代理模式

This pattern is used to structure distributed systems with decoupled components. These components can interact with each other by remote service invocations. A **broker** component is responsible for the coordination of communication among **components**.

该模式用于构建伴有解耦组件的分布式系统。这些组件通过远程服务调用来和彼此互动。**代理**组件负责协调**组件**之间的通信。

Servers publish their capabilities (services and characteristics) to a broker. Clients request a service from the broker, and the broker then redirects the client to a suitable service from its registry.

服务器将其功能（服务和特性）发布到代理。客户端从代理请求服务，代理根据注册表把客户重定向给合适的服务。

**Usage**
- Message broker software such as **[Apache ActiveMQ](https://en.wikipedia.org/wiki/Apache_ActiveMQ)**, **[Apache Kafka](https://en.wikipedia.org/wiki/Apache_Kafka)**, **[RabbitMQ](https://en.wikipedia.org/wiki/RabbitMQ)** and **[JBoss Messaging](https://en.wikipedia.org/wiki/JBoss_Messaging)**.

**用法**
- 消息代理服务，例如[Apache ActiveMQ](https://en.wikipedia.org/wiki/Apache_ActiveMQ), [Apache Kafka](https://en.wikipedia.org/wiki/Apache_Kafka)，[RabbitMQ](https://en.wikipedia.org/wiki/RabbitMQ)和[JBoss Messaging](https://en.wikipedia.org/wiki/JBoss_Messaging)。

![代理模式](https://cdn-images-1.medium.com/max/800/1*1qRQZjLRAd0yY_T9p2OgBw.png)

## 6. Peer-to-peer pattern
## 6. 点对点模式

In this pattern, individual components are known as peers. Peers may function both as a client, requesting services from other peers, and as a server, providing services to other peers. A peer may act as a client or as a server or as both, and it can change its role dynamically with time.

在该模式中，相同的组件被称为对等组件。对等体既可以作为客户端，请求其他对等体的服务，也可以作为服务端，为其他对等体提供服务。一个对等体可以作为客户端、或者服务端、或者兼任两者，它能随着时间动态变化自己的角色。

**Usage**
- File-sharing networks such as [Gnutella](https://en.wikipedia.org/wiki/Gnutella) and [G2)](https://en.wikipedia.org/wiki/Gnutella2)
- Multimedia protocols such as [P2PTV](https://en.wikipedia.org/wiki/P2PTV) and [PDTP](https://en.wikipedia.org/wiki/Peer_Distributed_Transfer_Protocol).

**用法**
- 文件共享网络，例如[Gnutella](https://en.wikipedia.org/wiki/Gnutella) 和 [G2)](https://en.wikipedia.org/wiki/Gnutella2)
- 多媒体协议，例如[P2PTV](https://en.wikipedia.org/wiki/P2PTV) 和 [PDTP](https://en.wikipedia.org/wiki/Peer_Distributed_Transfer_Protocol)。

![点对点模式](https://cdn-images-1.medium.com/max/800/1*ROvkckSTw1UncrbQSmUJUQ.png)

## 7. Event-bus pattern
## 7. 事件总线模式

This pattern primarily deals with events and has 4 major components; event source, event listener, channel and event bus. Sources publish messages to particular channels on an event bus. Listeners subscribe to particular channels. Listeners are notified of messages that are published to a channel to which they have subscribed before.

该模式主要处理事件，并且有4个主要组件：事件源，事件监听者，事件通道和事件总线。事件源发布消息到事件总线上的特定通道。监听者订阅特定通道。如果监听者订阅的通道有消息发布，那么监听者就会得到通知。

**Usage**
- Android development
- Notification services

**用法**
- 安卓开发
- 通知服务

![事件总线模式](https://cdn-images-1.medium.com/max/800/1*DOZ4nVR9zkJm-EnXT3KOGQ.png)

## 8. Model-view-controller pattern
## 8. 模型-视图-控制器模式(MVC模式)

This pattern, also known as MVC pattern, divides an interactive application in to 3 parts as,
该模式亦被称为MVC模式，它将交互式应用分成3个部分，

1. model — contains the core functionality and data
2. view — displays the information to the user (more than one view may be defined)
3. controller — handles the input from the user
**This is done to separate internal representations of information from the ways information is presented to, and accepted from, the user. It decouples components and allows efficient code reuse.**

1. 模型 - 包含核心功能和数据
2. 视图 - 给用户展示信息（可能不止一个视图）
3. 控制器 - 处理用户的输入
这样做的目的是将 **信息的内部表示** 和 **信息呈现给用户并且从用户获取的方式** 分离开。这样能解耦组件并且有效重用代码。

**Usage**
- Architecture for World Wide Web applications in major programming languages.
- Web frameworks such as [Django](https://en.wikipedia.org/wiki/Django_%28web_framework%29) and [Rails](https://en.wikipedia.org/wiki/Ruby_on_Rails).

**用法**
- 主要编程语言的万维网应用体系结构。
- web框架，例如[Django](https://en.wikipedia.org/wiki/Django_%28web_framework%29)和[Rails](https://en.wikipedia.org/wiki/Ruby_on_Rails)。

![MVC模式](https://cdn-images-1.medium.com/max/800/1*OP0CS6O5Sb66jpc-H-IuRQ.png)

## 9. Blackboard pattern
## 9. 黑板模式

This pattern is useful for problems for which no deterministic solution strategies are known. The blackboard pattern consists of 3 main components.
该模式可用于没有已知的确定性的解决方案策略的问题。黑板模式由3个主要组件组成。

- blackboard — a structured global memory containing objects from the solution space
- knowledge source — specialized modules with their own representation
- control component — selects, configures and executes modules.
All the components have access to the blackboard. Components may produce new data objects that are added to the blackboard. Components look for particular kinds of data on the blackboard, and may find these by pattern matching with the existing knowledge source.

- 黑板 - 一块结构化的全局内存，包含解决方案空间的对象。
- 知识源 - 具有各自代表性的专业模块。
- 控制组件 - 选择，配置和执行模块。
所有组件都可以访问黑板。组件可能生产添加进黑板的新数据对象。组件在黑板上寻找特定类型的数据，并且可能利用已有的知识源，通过模式匹配的方式来寻找数据。

**Usage**
- Speech recognition
- Vehicle identification and tracking
- Protein structure identification
- Sonar signals interpretation.

**用法**
- 语音识别
- 车辆识别和追踪
- 蛋白质结构识别
- 海纳信号解析

!(黑板模式)[https://cdn-images-1.medium.com/max/800/1*ArbMx7A21I47llvwUTiSDg.png]

