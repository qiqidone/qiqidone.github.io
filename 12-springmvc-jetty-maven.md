# 基于Spring Boot的Java服务

## 一、技术栈简介
Java比较成熟了，市面上也有很多成熟的web框架、容器、项目管理解决方案，这里就不一一介绍，本文以
spring boot作为例子，IDE为Intellij。Java、maven的安装配置比较简单，参见各自官网就可以了。

我使用的是(没有涉及DB)：
1. Java 1.8
2. Maven 3.3

## 二、Hello World
### I 新建项目
Spring boot还是非常方便，只要用引入`spring-boot-starter-parent`和`spring-boot-starter-web`就可以了。
我使用了Intellij的spring initializr，很方便地建出了工程。


### II 部署
很方便，打包成jar包，就可以了：
`mvn package`

### III 启动
也很方便，因为有main入口并且内嵌了web容器，默认是tomcat，直接：
`java -jar hello.jar`

启动之后可以用访问localhost:8080验证（除非在application.properties设置了端口）

## 三、常见问题
Hello world是不是很简单？不过随着需求的加入，就会有源源不断的问题出现。
比如绕开maven使用自己的jar包会出现NoClassDefFoundError，以及比如一些端口、安全的问题。

### 1. NoClassDefFoundError
与ClassNotFoundException不同，ClassNotFoundException是ClassLoader找不到class，这种错一般编译阶段就能找出来；
NoClassDefFoundError一般就是因为找不到依赖的包，而造成的找不到class的情况。
Spring Boot 使用的spring-boot-maven-plugin插件支持参数includeSystemScope，可以显性设置将system引进来的lib打包进jar。

## 四、隐患
Spring Boot的确在开发java项目上做到了方便易用，避免了很多的xml配置，使用了很多注解来实现bean的初始化、注入等操作，
但这种自动化很高的解决方案都存在一个问题，就是技术隐患。一旦遇到问题，可能就很难排查。
对于Spring Boot这样的方案比较合适的场景就是：经验丰富的工程师准备开始一个新项目&新手想快速搭建一个java服务应对简单的需求。
和其他技术方案一样，不断深入了解内部细节的功课一定也是绕不开的。