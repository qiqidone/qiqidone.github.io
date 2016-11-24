# Jmeter 简易介绍

## 一、性能测试简介
在介绍Jmeter之前，说明下几个性能测试方面的概念。
1. 性能测试
一般的性能测试是基于真实生产环境的模拟，一般会把流量和压力设置在线上环境相当或者2倍左右，使用常规的测试配置。
所有有点规模的功能改动，都应该经过性能测试这一环节。
2. 负载测试
这个测试的目的是为了获得系统的承载极限，会不断地往上施压，直到系统负载能力不能再提高。
这种测试可以获得系统的极限，尤其是可以通过测试获得系统的瓶颈所在。
3. 压力测试
压力实测的出发点就是要超越系统的负载极限，使用尽可能大的流量使系统资源超载，
从而判断：当系统遇到突发情况的时候，会造成什么样的后果。

## 二、Jmeter简介
Jmeter是Apache旗下的开源软件，基于Java的性能测试工具，支持Mac、Linux、Windows等系统，带有GUI。久经考验，方便易用。
一般可以用来测试：
    * Web（Http/Https）
    * SOAP/Rest接口服务
    * FTP
    * 消息中间件（基于JMS）
    * 数据库（基于JDBC）
另外，Jmeter是基于协议层的，它没有实现浏览器的协议，没有浏览器内核，所以不能像浏览器一样调用JS以及AJAX动态数据。
只要安装好JAVA（版本>1.7），下载Jmeter，运行就好了，配置比较简单。

## 三、使用Jmeter
使用Jmeter还是比较简单，最主要的是了解它的几个概念就好了。

1. Thread Group
Thread Group是所有测试的起点，它是一个线程组，也就是一组测试的集合。
给Jmeter增加一个Thread Group（右键Test Plan，然后Add）。

2. Controllers
Controllers最重要的就是Logic Controllers和Samplers。
###### Sampler
就是Request，一般我们测http请求比较多，所以就用HTTP Request。
###### Logic Controller
Logic Controller是用来组合逻辑的，比如once only controller就是只会执行一次的请求，
通过Logic Controller可以进行if、for、random等等逻辑组合，有效模拟真实环境。

3. Listeners
Listener主要是看结果的，包括图、列表等形式可视化结果。

4. Timers
Jmeter有默认的timer，如果不用timer组合，jmeter就使用默认的timer。

5. Assertions
提示错误。

6. Configuration Elements
配置文件，对于要导入数据的测试就很有用。还有Header、Cookie的管理等。

7. Pre-processor Elements & Post-processor Elements
预处理和后处理。

##### Jmeter执行次序
Jmeter对上面这些元素有自己的执行次序：
 Configuration elements
 Pre-Processors
 Timers
 Sampler
 Post-Processors 
 Assertions 
 Listeners 

## 四、开始使用
开始使用吧！
如果有什么不知道的就上官网找吧：http://jmeter.apache.org/