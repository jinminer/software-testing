## 性能测试服务
> **[PTS : performance testing service](https://help.aliyun.com/product/29260.html?spm=a2c4g.11186623.6.540.1b0e76ddplfcOQ)**  



## 测试概述

### 为什么需要测试

* 开发人员
  * 自己不知道自己做的到底对不对
  * 为什么系统没有按照预定的结果出现



### 测试到底是什么

* 懂业务 -> 功能验证
  * 测试人员知道用户价值到底是要什么
  * 开发人员可以通过参加需求讨论，从而知道用户到底需要什么？
    * 那么问题是：性能测试参加需求讨论到底讨论啥？
* 懂技术 -> 拓展验证
  * 普通的测试人员只能告诉你这里不对，但是不能告诉你不对在哪里
* 懂沟通 -> 质量保证
  * 让一个小问题被大家觉得是一个大问题（包括不能复现的问题）
  * 让开发人员觉得改 `bug` 是理所当然的，能够欣然接受的



## 解决性能问题

### 概述

*  解决软件/系统/服务性能问题的关键在于找到 `平衡点` 

  * 成本：硬件/软件开销，运维/开发支出
  * 性能：用户体验
*  脱离性价比提升性能都是耍流氓 -> 任何性能都是基于性价比的

  - 绝大多数性能问题都是人为制造的
  - 绝大多数性能问题都可以通过业务来解决



### 举例

* 如何提高京沪高铁运营性能

  * I. 提升处理能力/单位内容（时间）
    * 列车提速：降低每趟列车的响应时间 -> `降低请求的响应时间`
      * 高铁速度到达某一个峰值(350公里)后，运营成本随着速度的提升呈倍数型增长 -> `增加了运维成本`
    * 列车扩容：增加车长/座位，增大每趟列车的载客量 -> `在一个TCP包或者请求中包含更多的数据` 
      * 增加座位：车长不变，增加座位数 -> `会降低用户体验` 
      * 增加车长：对车头的动力系统负载较大 ->  `提高运维成本` 
  * II. 修复线
    * 多修几条线路，实现多线并行运载 -> `并发、多线程` 
* 12306的系统到底做的好不好
  * 导致12306用户体验差的原因是 架构设计有问题？代码写的烂？当然不是，其根本原因是  -> `需求变态`
  * 为啥春运的时候觉得12306不好，而平时却很好？

    * 用户：春节车票供不应求，没抢到票，所以用户体验差
    * 方案I：不买二等座，而是直接花高价买商务座/找黄牛/抢票软件等 -> `增加成本`
    * 方案II：每个人在抢票的时候需要多支付1￥的购票竞争费用，抢到票的人直接扣除，没有抢到票的人瓜分奖金池中的金额。 -> `提升用户的心理体验` 



### 剖析

* 需求分析
  * 全世界只有中国人有春节这个节日，而且在春节放10多天假，而且在春节大家都要回家，而且。。。
  * 春节假期：1线城市的本地人都出国旅游，1线城市打工的人都悻悻然回家。
* 透过现象看本质
  * 春节回家 -> 用户在某个特定的时间端必须要做某些事情 ->  `人为制造性能问题` 
  * 如果每个月都放7天长假，大家就不会凑一块抢票 -> `通过调整业务弱化性能问题` 
    * 天猫双十一：在活动打折的那一天，并发访问量呈几何倍数增长，系统性能遇到空前的挑战(限流/熔断/降级：收货地址修改服务被降级)
    * 京东/苏宁：活动打折时间延长为一周，均分峰值，平摊用户访问量，大大降低了系统性能要求。
* 集中式峰值处理能力
  * 天猫双十一：使用用阿里云服务器，提供高性能处理能力，峰值过后，将云服务器租借给用户(比如自己用ECS搭建博客)
  * 12306：春运期间，租用阿里云服务器，峰值过后释放归还
  * 用户真正想要的是实实在在的便宜优惠，每天/一段时间都优惠不是就很ok？当然从现实业务角度出发，这样做是非常不科学的，但是这种单一的集中式营销活动方式也有待商榷。
  * 为了某一天的销售额去堆几倍服务器，浪费成本，劳民伤财。
  * IT仔对 `搞活动` 这玩意又爱又恨，好多公司没事就要搞活动，一搞活动大家都得忙的半死，动不动就上个 `光明顶` 。新人对它充满好奇，热爱技术的码农对某个新技术的运用充满期待，若干个有责任心的同事兢兢业业的盯着大屏幕。
* 终极奥义
  * 真正好的平台：速率是非常重要的
  * 真正好的平台：应该是稳定的
  * 真正好的平台：性能每天都很高，而不是烧钱高一下



## 性能测试

> 是为了验证在一定环境下，系统满足性能需求的一种测试。验证性能指标：响应时间、吞吐量、资源利用率、并发用户数。

### 性能测试难点

性价比 -> 如何平衡性能和成本

* 成本（需求）
* 性能（实际能力）

### 衡量标准

* 一切测试都基于需求，没有需求就无法得到性能测试结果。

* 系统/软件/服务的快慢不应该由人的主观意识来决定，而是需要有相应的标准来衡量，这个标准就是需求。
  * 比如高铁票价，如果都翻好几倍来卖，那么可能都没有人去买票，更没有抢票一说了。这个时候12306系统的衡量指标也会随着改变。



### 影响因素

* 一个线程处理一个业务是需要一定的开销的，随着负载并发的增加，单个线程的处理能力会到上限，逐步触发进程的瓶颈，最后导致硬件资源不足或者无法管理的问题

* 给定并发用户数，验证性能指标：
  * 响应时间
  * 吞吐量
  * 资源利用率
* 简单粗暴的解决性能问题：烧钱堆硬件？
  * 硬件资源不可能达到100%的利用率
  * 对于大多数人来说烧钱就是烧命

* 真香警告 -> 本片纯属虚构，部分内容可能会使码农稍度不适，请勿效仿，或直接忽略。
  * 象征性的指标数据 -> 系统run起来，压一堆用户上去，瞧瞧是否宕机，性能指标是多少
  * `sleep` -> 如果性能指标还不错，在关键业务中 `add sleep` ，为以后的调优提供空间
  * 没啥好调的 -> 大多时候，到某一个阶段后，系统性能很难进行调优。



## 事物处理性能

> [**TPC(Transaction processing Performance Council)**](http://www.tpc.org/information/who/whoweare.asp)



### 约定规范

* 完全不 `care` 具体的程序代码
* 给出了基准程序的标准性能规范



### TPC-C

> **TPC-C** **-> TPC-Client/Server**
>
> **单位时间内业务的处理能力**



* 特点
  * 基于业务 -> 给定一个性能衡量标准，不会在乎用那种语言，那种程序去实现。
  * `TPC` 针对某一种业务来做，但是这个业务却不一定能适合所有情况。
    * 增删改查
    * 纯运算
  * 对相同的代码/程序/服务进行改造后，性能会有相应的提升。

* 缺点
  * 通常我们只是针对自己非常烂、不合理的需求来做的 `TPC-C` ，毫无比较价值可言。
  * 很多时候在非常好的硬件上，拿到的性能指标数据可能是很差的。



## 标准性能评估

> [**SPEC(Standard Performance Evaluation Corporation)**](https://www.spec.org/)  



### 概述

* 基于硬件
  * 同样的系统和业务，硬件配置较高的，就应该得到更好的性能测试结果
  * 实际情况是，硬件条件较好，系统的性能不一定高。
* 工具
  * **[aida](https://www.aida64.com/)** -> 硬件指标

![aida-testing](https://raw.githubusercontent.com/jinminer/docs/master/software-testing/performance-testing-service/aida-testing.png)



## TPC VS SPEC

* `TPC` 
  * 侧重点
    * 面向测试人员
    * 强调业务处理能力
  * 特点
    * 省钱
    * 对开发人员要求较高，需要有立竿见影的落地效果
* `SPEC` 
  * 侧重点
    * 面向运维人员 
      * `CPU` 要满了就加`CPU` 
      * 系统处理能力不足，就加服务器，加资源
    * 强调资源处理能力
  * 特点
    * 充钱就能解决问题
    * 有上限，`1 + 1 <= 2` 
    * 对运维人员要求较高

* 现状
  * 大多数性能问题都可以通过增加服务器来解决
  * 大多数框架的水平扩展能力都不错，能够很好的支撑服务器扩展
  * 大多数开源框架自身已经很好的处理了性能问题，按照相应的规范去开发即可，性能优化空间不大。



## 需求 -> 性能之始

### 重谈铁路售票系统

* 12306可以通过不断的烧钱堆服务器解决性能问题？
  * 这种方法已经在用，春运期间租用阿里的服务器。
  * 服务器堆积到一定程度，这种办法就显得捉襟见肘了。
  * 当然非要头铁继续烧钱可能也ok，但付出的代价超乎想象。
* 追本溯源
  * 12306系统的性能瓶颈在于需求(业务复杂度)
    * 记录的特征导致的 ->  `数据一致性要求非常高` 
      * 每一个订单都是独立的
      * 每一张票都是秒杀
      * 火车票是不能超卖的
    * 火车票会涉及到分票的问题 -> `业务极其复杂` 
      * 比如一列高铁： `上海 -> 北京` ，如果有人买了`南京 -> 天津` 的票，就要多出额外的两张票
        * `上海 -> 南京` 
        * `天津 -> 北京` 
      * 所以购买火车票时，推荐买更长路途的票
    * 12306如何解决/缓解分票问题？
      * 所有出票记录都是 `实时运算` 的结果？ 运算量太大，系统肯定扛不住。
      * 在列车运营一段时间后，统计售票记录，分析不同时期，始发站到各终点站的售票情况
      * 预先进行车票划分 -> 按照每种线路售票数量的高低，提前分配车票比例，比如：
        * 80%的票为固定线路，已经分配好，不会进行 `实时运算` 
        * 预留20%执行分票规则，进行 `实时运算` 
      * 最好的办法：所有的票都是 `上海 -> 北京` 的，这样既赚钱又省力。

* 拨云见日
  * 12306真的很吊，特别是它的核心系统，要解决诸多难题：
    * 中短途分票问题
    * 老人、孕妇、儿童、残疾人座位分配问题
      * 老弱病残的票不会是上铺或者中铺
    * 连续买票，票号要连在一起



### 需求决定性能

透过现象看本质，通过12306系统实例的分析，得出结论：

* 需求/业务复杂度是导致系统性能问题的唯一因素
* 烧钱/堆服务器是提高系统性能的非必要的、有效的手段。



## 性能测试实现原理

### 如何进行性能测试

* 模拟客户端对服务器进行多连接
* 伪造报文欺骗服务器认证机制
  * I.了解服务器认证机制
  * II.了解 `客户 <-> 服务器` 之间的交流报文结构
  * III.合理的技术构造报文结构



### 性能测试手段

* 切入方向
  * 代码层面
    * Junit 并行测试 
    * 得到一个方法执行的时间和资源消耗的代价，再乘以负载数，估算出大致的系统性能指标

  * 单机（极少数）
  * 基于网络架构（大多数）
    * B/S -> 客户端对服务端的调用（HTTP）
      * [RFC2616](https://tools.ietf.org/html/rfc2616) 
      * restful
* 实施效果
  * 确保负载准确的落到对应的接口和代码上，否则结果无效
  * 确保负载是按照(模拟)真实的情况去做的，否则结果无效



### 实例剖析

* HTTP

  * HTTP Request

    * 连接方式 -> 对性能的影响
      * 长连接 `Connection: Keep-Alive` -> 打电话：一次只能和一个人通信
      * 短连接 `Connection:close`   -> 发短信：一次能和多个人通信
    * 浏览器页面是如何渲染整个页面
      * 刷新/请求一个页面，实际产生了多个请求

  * HTTP Response

    *  `200 OK` -> 服务器收到合法的请求并且对前端进行了应答，但是返回数据不一定是前端真实需要的

      * 需要进一步判断 -> 客户端和服务端约定报文中的业务返回码

    *  `Last-modified: Thu, 17 Jan 2019 08:00:03 GMT`  -> 文件的最后修改时间

      * 做缓存标记 -> 提高响应速度，节省流量

        * 以 `Last-modified` 为标记，比较该请求应答和本地相同 `hash` 的文件时间是否相同，如果是，那么就不再重复下载 `body`  

        * 页面图片处理 -> 访问一个网站第一次比较慢，后面会很快。

    *  `ETag` -> 防止缓存

      * 在某些请求上添加 `token` 

* HTTP 验证方式
  * 接口调用 -> 验证请求的发送和返回
  * 抓包
    * 获得 `HAR(HTTP Archive)` -> `firefox` 
  * 快速捕获响应时间







































