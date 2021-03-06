# Rails 异步任务

## 一、异步任务

异步任务的适用场景是什么？即什么情况下使用异步任务？

1. 长业务
即需要较长处理时间的业务，这样的请求会导致响应时间的大大增加，进而导致系统容量下降。当大量请求堆积的时候会导致堵塞，违背了快速响应的设计原则。
把长业务分配到其他的进程中去，某种程度上和多线程/分布式的设计原理是相近的，就是利用更多的CPU/IO资源处理同一任务，而异步任务很好地分割了业务逻辑，
从而实现了多线/进程——只不过异步任务的设计带来的容量的提升，是一种**垂直拓展**，而我们常说的**可拓展性**更多地在指**水平拓展**，后者不需要很多额外的努力。

2. 业务耦合
原则上，在程序设计的时候应该更多地使用“短逻辑”，就好象在对话/写作中更提倡短句的道理是一样的——更容易被人理解。
单一函数如果行数多于50行，一般认为是不好的代码。同样地，在业务逻辑层面，过长的逻辑使得业务更多地耦合在一起，当需求不断变化时会导致维护成本过高。
异步任务的设计使得切分逻辑链成为了必然要求，可以迫使程序员思考解耦的问题，从而加强系统的可维护性和拓展性。

3. 流量高峰
异步任务设计是一个提高单一系统容量的有效办法。在现在的web服务器系统中，最大的瓶颈一般来源于数据库/网络等IO上，
尤其是数据库的访问容量一般都是整个系统容量的最大瓶颈。当出现访问高峰的时候（例如秒杀等活动），大量的访问请求可能击中系统的容量上限，
导致大量的请求堵塞，最后使得大量用户完全无法收到回复。异步任务的使用，可以使大量的数据库的操作进入队列，然后慢慢消化（consume）。
这样可以使响应变得快速，用户不会有“网站又挂了”这样的感受以外，系统的容量上限也大幅地提高。

## 二、消息队列
rails4.2之后有了Active Job模块，将异步任务加入了框架标准。 其中可以选择的Adapter包括Resque、Sidekiq、Delayed Job等。以Sidekiq为例。
Sidekiq官方宣称是Resque和Delayed Job速度的20倍。

1. 队列
Sidekiq使用Redis做队列容器，当调用sidekiq的接口`perform_async`的时候相当于把job`push`到了redis的队列里面。
然后woker会去消费这些任务。Redis是有持久化能力的，所以不需要担忧消息的丢失，这一点对于很多业务非常重要。Redis的容量要注意一下。
2. 优先级
异步的任务如果不止一种，需要设置多队列，这样就要设置优先级。
3. 重试与异常
异步任务需要保证线程安全，不管是retry还是异常之后处理，都有可能导致数据库被重复修改，如果服务器较多，那线程安全务必要注意。


## 三、面向服务
集成在Rails里面自然使得整个开发变得轻松快速，但如果任务的量级达到一定规模之后，就会显得很棘手。这个时候要考虑将任务的处理业务独立成单独的服务，
SOA（Service-oriented architecture）这种方式的架构可以使得系统的耦合水准降低、水平拓展能力与可维护性提高，可以独立选择更优的技术栈。
Rails的服务只负责把Job push到Redis里，然后负责Worker的服务可以从redis的队列里消费这些任务，这样可以减少因为rails带来的代码冗余从而导致的性能低下。
一般来说，Sidekiq+redis+rails的组合应对一般的移动应用绰绰有余，只要保证线程安全，配合Loadbalance，水平拓展能力对于一般的需求完全足够。
最后需要做的就是实现数据库的分布式支持，配合面向服务的架构方式这样基本上系统的容量就很可观了。