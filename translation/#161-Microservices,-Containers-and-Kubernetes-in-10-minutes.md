translator: http://www.jobbole.com/members/wx87925059/
reviewer: http://www.jobbole.com/members/hanxiaomax/
via: https://gravitational.com/blog/microservices-containers-kubernetes/

# Microservices, Containers and Kubernetes in 10 minutes（微服务、容器和 Kubernetes 10分钟教程）

> 转译自： https://gravitational.com/blog/microservices-containers-kubernetes/

## What is a Microservice?（什么是微服务）

What is a microservice? Should you be using microservices? How are microservices related to containers and Kubernetes? If these things keep coming up in your day-to-day and you need an overview in 10 minutes, this blog post is for you.

什么是微服务？你应该使用微服务吗？微服务是如何与容器和 Kubernetes 关联的？如果这些问题在你的日常工作中不断涌现，那么你需要用10分钟时间来阅读为你准备的这篇博文。

Fundamentally, a microservice is just a computer program which runs on a server or a virtual computing instance and responds to network requests.

从根本上来说，微服务是运行在服务器或者虚拟主机实例上并能对网络请求做出响应的计算机程序。

How is this different from a typical Rails/Django/Node.js application? It is not different at all. In fact, you may discover that you already have a dozen of microservices deployed at your organization. There are not any new magical technologies that qualify your application to be called a microservice. *A microservice is not defined by how it is built but by how it fits into the broader system or solution.*

微服务与典型的 Rails/Django/Node.js 应用有什么不同呢？其实并无不同。事实上，你会发现你所在的公司（组织）已经部署了很多微服务。不需要什么新的黑科技，你的应用就可以被称为微服务。微服务并非基于构建方式定义，而是取决于一个应用如何适用更广泛的系统或者解决方案。

So what makes a service a microservice? Generally, microservices have a more narrow scope and focus on doing smaller tasks well. Let’s explore further by looking at an example.

那么，是什么让普通的服务变成了微服务呢？一般来说，微服务在有限的业务范围内专注于执行较小的任务。

## Example: Amazon Product Listing（例：亚马逊产品列表）

Let’s examine the system which serves you this [product page on Amazon](https://www.amazon.com/Thinking-Fast-Daniel-Kahneman-2011-10-25/dp/B01FIYNOKU/). It contains several blocks of information, probably retrieved from different databases:

- The product description, which includes the price, title, photo, etc.
- Recommended items, i.e. similar books other people have bought.
- Sponsored listings that are related to this item.
- Information about the author of the book.
- Customer reviews.
- Your own browsing history of other items on the Amazon store.

我们来研究一下为你提供服务的 [亚马逊书籍详情页](https://www.amazon.com/Thinking-Fast-Daniel-Kahneman-2011-10-25/dp/B01FIYNOKU/) 。该页包含了几个信息模块，这些信息可能基于不同数据库的检索而来：

- 书籍描述，包括价格、标题、图片、等等。
- 推荐书籍，比如其他人购买的书籍
- 该项目的赞助商（发起人）列表
- 作者信息
- 用户评论
- 你在亚马逊商城的书籍浏览记录

If you were to quickly write the code which serves this listing, the simple approach would look something like this:

如果你要很快就写出来为以上列表提供服务的代码，那么简单的方式可能是类似下图这样的：

![](https://ws4.sinaimg.cn/large/006tNc79ly1g27th0vn93j30gx09hmxh.jpg)

When a user’s request comes from a browser, it will be served by a web application (a Linux or Windows process). Usually, the application code fragment which gets invoked is called a request handler. The logic inside of the handler will sequentially make several calls to databases, fetch the required information needed to render a page and stitch it together and render a web page to be returned to the user. Simple, right? In fact, many of Ruby on Rails books feature tutorials and examples that look like this. So, why complicate things, you may ask?

当一个用户通过浏览器发送请求到服务端，服务端将通过一个  web 应用（一个 Linux 或者  Windows 进程）提供服务。通常，被调用的应用代码片段被称为请求处理程序。请求处理程序的内部逻辑将依次多数据库进行多次调用，获取渲染页面所需要的信息并拼接组织，然后渲染页面返回给用户。很简单是吧？其实，很多 Ruby on Rails 书籍都提供了这样的教程和示例。那么你可能会疑惑，为什么事情变得复杂了？

Imagine what happens as the application grows and more and more engineers become involved. The recommendation engine alone in the example above is maintained by a small army of programmers and data scientists. There are dozens of different teams who are responsible for some component of rendering that page. Each of those teams usually wants the freedom to:

1. Change their database schema.
2. Release their code to production quickly and often.
3. Use development tools like programming languages or data stores of their choice.
4. Make their own trade-offs between computing resources and developer productivity.
5. Have a preference for maintenance/monitoring of their functionality.

设想一下，在应用程序不断拓展演化以及越来越多的开发者参与的时候，会发生什么。上述例子的推荐引擎由一小撮开发者和数据科学家维护。为了渲染那个网页，有很多不同的开发团队为各个网页组件提供数据应答服务。每个团队都希望不受限制地做如下工作：

1. 改变他们的数据库架构
2. 快速频繁地发布代码和产品
3. 使用开发工具工作，比如他们选择的编程语言或者数据库
4. 在计算资源和开发者生产力之间做出权衡
5. 腾出更多精力以维护/监控系统功能

As you can imagine, having the teams agree on everything to ship newer versions of the web store application will become more difficult over time.

你可以想象，随着时间的推移，这些团队在传输新版本电商应用时想要在所有事情上达成共识将会变得愈发困难。

The solution is to split up the components into smaller, separate services (aka, microservices).

解决方案是，把这些组件分割成更小的、独立的服务（即微服务）。

![microservices design](https://ws3.sinaimg.cn/large/006tNc79ly1g27tbeo9kzj30mr0a6dgz.jpg)

The application process becomes smaller and dumber. It’s basically a proxy which simply breaks down the incoming page request into several specialized requests and forwards them to corresponding microservices, who are now their own processes and are running elsewhere. The “application microservice” is basically an aggregator of the data returned by specialized services. You may even get rid of it entirely and offload that job to a user’s device, having this code run in a browser as a single-page JavaScript app.

应用的业务流程变得更小和更笨拙。它基于代理，将发送过来的页面请求拦截并拆分成不同的请求转发到对应的微服务上，这些微服务此时属于内部过程，但是可以再任何地方运行。这个「微服务应用」基于一个针对于某些特定服务返回数据的聚合器。你甚至可以完全摆脱它并将这些工作放在用户设备上的浏览器作为一个单页 JavaScript  应用来执行。

The other microservices are now separated out and each development team working on their microservice can:

- Deploy their service as frequently as they wish without disrupting other teams.
- Scale their service the way they see fit. For example, use AWS instance types of their choice or perhaps run on specialized hardware.
- Have their own monitoring, backups and disaster recovery that are specific to their service.

其它的微服务现在都被分离出来了，每个开发团队在开发他们的微服务时都可以做这些事：

- 在不干扰其它团队的前提下频繁地部署他们的服务
- 按照他们认为合适的方式扩展服务。例如，使用他们选择或者可能在特定硬件上运行的 AWS 实例类型。
- 部署专门针对他们服务的的监视器、备份程序以及灾难恢复

## What is the difference between microservices and containers?（微服务和容器的区别是什么？）

A container is just a method of packaging, deploying and running a Linux program/process. You could have one giant monolithic application as a container and you could have a swarm of microservices that do not user containers, at all.

容器只是打包、部署和运行 Linux 程序/进程的一个方法。你可以将一个庞大的整体应用作为一个容器，当然你也可以使用一大堆不用容器的微服务。

A container is a useful resource allocation and sharing technology. *It’s something devops people get excited about.* A microservice is a software design pattern. *It’s something developers get excited about.*

容器是一种实用的资源分配和共享技术，饱受开发从业者喜爱。微服务是一种软件设计模式，同样广受开发者好评。

Containers and microservices are both useful but not dependent on each other.

容器技术和微服务都很实用，而且并不互相依赖。

## When to use Microservices?（什么时候该用微服务？）

The idea behind microservices is not new. For decades, software architects have been at work trying to decouple monolithic applications into reusable components. The benefits of microservices are numerous and include:

- easier automated testing;
- rapid and flexible deployment models; 
- higher overall resiliency.

微服务这个想法并不新鲜。数十年以来，软件架构师们一直在尝试将高耦合度的整体应用拆分为可复用的组件。微服务的好处很多，包括：

- 使自动化测试变得更容易
- 灵活高效地部署模块
- 更高的整体弹性

Another win of adopting microservices is the ability to pick the best tool for the job. Some parts of your application can benefit from the speed of C++ while others can benefit from increased productivity of higher level languages such as Python or JavaScript.

采用微服务架构的另一个优势在于，使我们能够选择更好工具以胜任工作。你应用中的某些部分可以借助 C++ 提高运行速度，其它部分则可借助高阶编程语言（如 Python 或 JavaScript）提升开发效率。

The drawbacks of microservices include:

- the need for more careful planning;
- higher R&D investment up front; 
- the temptation of over-engineering.

微服务的缺点包括：

- 需要更谨慎的制订开发计划
- 提高研发投入预算
- 过度设计的诱惑

If an application and development team is small enough and the workload isn’t challenging, there is usually no need to throw additional engineering resources into solving problems you do not have yet and use microservices. However, if you are starting to see the benefits of microservices outweigh the disadvantages, here are some specific design considerations:

1. **Separation of computing and storage**. As your needs for CPU power and storage grow, these resources have very different scaling costs and characteristics. Not having to rely on local storage from the beginning will allow you to adapt to future workloads with relative ease. This applies to both simple storage forms like file systems and more complex solutions such as databases.
2. **Asynchronous processing**. The traditional approach of gradually building applications by adding more and more subroutines or objects who call each other stops working as workloads grow and the application itself must be stretched across multiple machines or even data centers. Re-architecting an application around the event-driven model will be required. This means sending an event (and not waiting for a result) instead of calling a function and synchronously waiting for a result.
3. **Embrace the message bus**. This is a direct consequence of having to implement an asynchronous processing model. As your monolithic application gets broken into event handlers and event emitters, the need for a robust, performant and flexible message bus is required. There are numerous options and the choice depends on application scale and complexity. For a simple use case, something like Redis will do. If you need your application to be truly cloud-native and scale itself up and down, you may need the ability to process events from multiple event sources: from streaming pipelines like Kafka to infrastructure and even monitoring events.
4. **API versioning**. As your microservices will be using each other’s APIs to communicate with each other via a bus, designing a schema for maintaining backward compatibility will be critical. Simply by deploying the latest version of one microservice, a developer should not be demanding everyone else to upgrade their code. This will be a step backward towards the monolith approach, albeit separated across application domains. Development teams must agree upon a reasonable compromise between supporting old APIs forever and keeping the higher velocity of development. This also means that API design becomes an important skill. Frequent breaking API changes is one of the reasons teams fail to be productive in developing complex microservices.
5. **Rethink your security**. Many developers do not realize this but migrating to microservices creates an opportunity for a much better security model. As every microservice is a specialized process, it is a good idea to only allow it to access resources it needs. This way a vulnerability in just one microservice will not expose the rest of your system to an attacker. This is in contrast with a large monolith which tends to run with elevated privileges (a superset of what everyone needs) and there is limited opportunity to restrict the impact of a breach.

如果一个应用和它的开发团队足够小并且工作负荷没有挑战性，这个时候往往没必要投入额外的工程师资源去解决还没有遇到的问题，也没必要使用微服务。任何时候，如果你已经意识到了微服务在项目中的优势大于劣势，这几条事项在设计中需要具体考虑：

1. **计算和存储分离**。随着对 CPU 和存储需求的增长，这些资源具有非常不同的扩展开销和特点。一开始就不依赖本地存储会让你更轻松地适应将来的工作负载。此条适用于简单存储形式如文件系统，也适用于复杂的解决方案如数据库。
2. **异步处理**。渐进式开发应用的传统方式为：加入越来越多的子程序或者对象，他们可以在工作负载增长时互相通知对方停止工作，并且应用本身必须在多台机器上平行部署乃至建立数据中心。围绕事件驱动模型的重构是必要的。这意味着发送一个事件（不等待发送结果）以代替调用一个功能同步等待结果。
3. **拥抱消息总线**。这是实现一个异步处理模型的直接结果。随着你的整体应用拆分为事件处理者和事件发送者，你需要一个健壮的、高性能的、灵活的消息总线。基于应用的规模和复杂度，有很多选择。对于一个简单应用场景，类似 Redis 的东西可以做到。如果你需要让你的应用成为真正的云原生应用并且能够自我伸缩，你或许需要处理多个事件源的事件的能力：藉由类似 Kafka 的流管道作为基础设施以对事件进行监听。
4. **API 版本化**。因为你的微服务的 API 被各服务节点通过总线调用以互相通信，设计保持向后兼容性的模式就至关重要。只部署单个微服务的最新版本的话，开发者就不应该要求其他人升级他们的代码。这将是向整体化方式买进的一步，即使是分开的跨应用程序域。在永久支持旧的 API 和保持高效开发之间，开发团队必须接受一个合理的折中方案。这也意味着 API 设计变成一种重要的技能。频繁地变更 API 是团队在开发复杂微服务时效率低下的原因之一。
5. **重新思考你的安全问题**。很多开发者不了解这一点，但是迁移到微服务提供了一个使其可以构建更好的安全模块的机会。因为每个微服务都是一个专门的进程，所以它是一个限制自身只能访问所需要资源的好办法。这样一来，你系统中的一个微服务被漏洞攻击时就不会暴露系统其它部分。这与一个庞大的整体应用相反，后者往往以提升的特权（每个人都需要的超集）运行，并且限制违规影响的机会有限。

## What does Kubernetes have to do with microservices?（Kubernetes 与微服务有什么关系？）

[Kubernetes](https://kubernetes.io/) is too complex to describe in detail here, but it deserves an overview since many people bring it up in conversations about microservices.

[Kubernetes](https://kubernetes.io/) 的细节在此赘述起来相当复杂，但是在此还是要简要介绍一下，毕竟在关于微服务的一些讨论中它被很多人提及。

Strictly speaking, the primary benefit of Kubernetes (aka, K8s) is to increase infrastructure utilization through the efficient sharing of computing resources across multiple processes. Kubernetes is the master of *dynamically allocating computing resources to fill the demand*. This allows organizations to avoid paying for computing resources they are not using. However, there are side benefits of K8s that make the transition to microservices much easier.

严格来说，Kubernetes（即 k8s）的主要好处在于，在跨多个进程的计算资源共享上，它可以提高基础设施的利用率。Kubernetes 是 「动态分配计算资源以满足需求」的大师，它避免了组织产生一些不必要的计算资源开支。无论如何，K8s 附带的好处是让微服务的迁移变得更加容易。

As you break down your monolithic application into separate, loosely-coupled microservices, your teams will gain more autonomy and freedom. However, they still have to closely cooperate when interacting with the infrastructure the microservices must run on.

由于你拆分了你的整体应用为单独的、松散耦合的微服务，你的团队将获得更多的自由度和自治权。然而，他们仍然需要在与供微服务运行的基础设施交互时密切合作。

You will have to solve problems like:

- predicting how much computing resources each service will need;
- how these requirements change under load;
- how to carve out infrastructure partitions and divide them between microservices; 
- enforce resource restrictions.

你将要解决这些问题：

- 预估每个服务需要多少计算资源
- 负载不足时这些需求如何调整
- 如何划分基础设置分区并在微服务之间划分它们
- 强制实施资源限制

Kubernetes solves these problems quite elegantly and provides a common framework to describe, inspect and reason about infrastructure resource sharing and utilization. That’s why adopting Kubernetes as part of your microservice re-architecture is a good idea.

Kubernetes 非常优雅地解决了这些问题，并且提供了一个通用的框架去描述、检查和推理基础设施资源共享、利用。这就是你要接受 Kubernetes 成为你微服务重构的一部分的原因。

Kubernetes, however, is a complex technology to learn and it’s even harder to manage. You should take advantage of a hosted Kubernetes service provided by your cloud provider if you can. However, this is not always viable for companies who need to run their own Kubernetes clusters across multiple cloud providers and enterprise data centers.

不管怎么说，Kubernetes 都是一个需要学习并且难以管理的复杂技术。如果可能，你应该使用云服务供应商提供的托管 Kubernetes 服务。当然，在一些需要跨多个云供应商和多个企业数据中心运行自己的 Kubernetes 集群的公司，上述方法并不总是可行。

For such use cases, we recommend trying out [Gravity](https://github.com/gravitational/gravity), the open source [Kubernetes packaging ](https://github.com/gravitational/gravity)solution, which removes the need for Kubernetes administration. Gravity works by creating Kubernetes clusters from a single image file or “Kubernetes appliances” and can be downloaded, moved, created and destroyed by the hundreds, making it possible to treat Kubernetes clusters [like cattle, not pets](http://cloudscaling.com/blog/cloud-computing/the-history-of-pets-vs-cattle/).

在这种情况下，我们推荐试试 Gravity，这是一个开源的 [Kubernetes 封装 ](https://github.com/gravitational/gravity) 解决方案，它消除了对 Kubernetes 管理的需要。Gravity 的工作方式是：用一个单独的镜像文件或者 「Kubernetes 设备」创建 Kubernetes 集群，并且使其可以被批量下载、创建、移动和销毁。创建 Kubernetes 集群的过程被 Gravity 变得简单，用饲养动物作比方的话，使用 Gravity[「就像养牛一样简单，而不像样宠物一样让人头大」](http://cloudscaling.com/blog/cloud-computing/the-history-of-pets-vs-cattle/) 。

## The Conclusion(写在最后)

To summarize:

1. Microservices are not new. It’s an old software design pattern which has been growing in popularity due to the growing scale of Internet companies.
2. Small projects should not shy from the monolithic design. It offers higher productivity for smaller teams.
3. Kubernetes is a great platform for complex applications comprised of multiple microservices.
4. Kubernetes is also a complex system and hard to run. Consider using hosted Kubernetes if you can.
5. If you must run your own K8s clusters or if you need to publish your K8s applications as downloadable appliances, consider the open source solution, [Gravity](https://github.com/gravitational/gravity).

总结：

1. 微服务并不是一个新概念。它是一种老的软件设计模式，现在被规模不断扩大的互联网公司所追捧。
2. 小型项目不要不好意进行整体化设计，这样可以提升小型团队的生产力。
3. 对于有多个微服务组成的复杂应用来说，Kubernetes 是一个很好的平台。
4. Kubernetes 同时也是一个复杂的系统，并且很难运行。尽可能去使用 Kubernetes 托管服务。
5. 如果你必须运行自己的 K8s 集群 或者必须发布可供下载的 K8s 应用，可以考虑使用开源解决方案：[Gravity](https://github.com/gravitational/gravity)。 