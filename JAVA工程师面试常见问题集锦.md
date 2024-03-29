来源：https://www.cnblogs.com/wwmiert/p/9451546.html
#集锦一：　

#### 一、面试题基础总结
##### 1、 JVM结构原理、GC工作机制详解

答：具体参照：JVM结构、GC工作机制详解，说到GC，记住两点：
1、GC是负责回收所有无任何引用对象的内存空间。 
注意:垃圾回收回收的是无任何引用的对象占据的内存空间而不是对象本身，
2、GC回收机制的两种算法，a、引用计数法  b、可达性分析算法（  这里的可达性，大家可以看基础2 Java对象的什么周期），至于更详细的GC算法介绍，大家可以参考：Java GC机制算法

##### 2、Java对象的生命周期

答：创建阶段 、 应用阶段 、不可见阶段 、不可达阶段 、收集阶段 、终结阶段、 对象空间重新分配阶段等等，具体参照：Java 对象的生命周期

##### 3、Map或者HashMap的存储原理

答：HashMap是由数组+链表的一个结构组成，具体参照：HashMap的实现原理

##### 4、当数据表中A、B字段做了组合索引，那么单独使用A或单独使用B会有索引效果吗？（使用like查询如何有索引效果）

答：看A、B两字段做组合索引的时候，谁在前面，谁在后面，如果A在前，那么单独使用A会有索引效果，单独使用B则没有，反之亦然。同理，使用like模糊查询时，如果只是使用前面%，那么有索引效果，如果使用双%号匹配，那么则无索引效果

##### 5、数据库存储日期格式时，如何考虑时区转换问题？

答：使用TimeStamp ,  
UTC和GMT几乎是同一概念，两者的区别是GMT是一个天文上的概念，UTC是基于原子钟。
GMT=UTC
GMT + 8 = UTC + 8 = CST
UTC+时间差=本地时间 （时间差东为正，西为负，东八区记为 +0800）

##### 6、Java Object类中有哪些方法？

protected Object clone() 创建并返回此对象的一个副本。
boolean equals(Object obj) 指示某个其他对象是否与此对象“相等”。
protected void finalize() 当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。
Class<? extendsObject> getClass() 返回一个对象的运行时类。
int hashCode() 返回该对象的哈希码值。
void notify() 唤醒在此对象监视器上等待的单个线程。
void notifyAll() 唤醒在此对象监视器上等待的所有线程。
String toString() 返回该对象的字符串表示。
void wait() 导致当前的线程等待，直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法。
void wait(long timeout) 导致当前的线程等待，直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法，或者超过指定的时间量。
void wait(long timeout, int nanos) 导致当前的线程等待，直到其他线程调用此对象的 notify()

##### 7、HTTP协议，GET和POST 的区别

答：https://www.cnblogs.com/hyddd/archive/2009/03/31/1426026.html

####二、线程、设计模式、缓存方面
##### 1、SimpleDataFormat是非线程安全的，如何更好的使用而避免风险呢

答：因为 DateFormat 和 SimpleDateFormat 类不都是线程安全的，在多线程环境下调用 format() 和 parse() 方法应该使用同步代码来避免问题。
    https://www.cnblogs.com/peida/archive/2013/05/31/3070790.html

##### 2、如何看待设计模式，并简单说说你对观察者模式的理解

答：1、设计模式有神马用 https://blog.csdn.net/lovelion/article/details/7420866    
    2、观察者模式类图及实现 https://www.cnblogs.com/wangjq/archive/2012/07/12/2587966.html

##### 3、集群环境中，session如何实现共享

答：

1、Java集群之session共享   
 http://blog.sina.com.cn/s/blog_5c69350401014zgm.html 

2、session多服务器共享方案，还有一种方案就是使用一个固定的服务器专门保持session，其他服务器共享
https://www.cnblogs.com/wangtao_20/archive/2013/10/29/3395518.html
##### 4、分布式、集群环境中，缓存如何刷新，如何保持同步？

答：

A、缓存如何刷新？ 1、定时刷新  2、主动刷新覆盖，每个缓存框架都有自带的刷新机制，或者说缓存失效机制，就拿Redis和 Ehcache举例， 他们都有自带的过期机制，另外主动刷新覆盖时，只需获取对应的key进行数据的覆盖即可

B、缓存如何保持同步？  这个redis有自带的集群同步机制，即复制功能，具体参考：基于Redis分布式缓存实现，Ehcache也有分布式缓存同步的配置，只需要配置不同服务器地址即可，参照：Ehcache分布式缓存同步

##### 5、一条sql执行过长的时间，你如何优化，从哪些方面？

答：

1、查看sql是否涉及多表的联表或者子查询，如果有，看是否能进行业务拆分，相关字段冗余或者合并成临时表（业务和算法的优化）

2、涉及链表的查询，是否能进行分表查询，单表查询之后的结果进行字段整合

3、如果以上两种都不能操作，非要链表查询，那么考虑对相对应的查询条件做索引。加快查询速度

4、针对数量大的表进行历史表分离（如交易流水表）

5、数据库主从分离，读写分离，降低读写针对同一表同时的压力，至于主从同步，mysql有自带的binlog实现 主从同步

6、explain分析sql语句，查看执行计划，分析索引是否用上，分析扫描行数等等

7、查看mysql执行日志，看看是否有其他方面的问题

个人理解：从根本上来说，查询慢是占用mysql内存比较多，那么可以从这方面去酌手考虑

#### 三、三大框架方面问题
##### 1、Spring 事务的隔离性，并说说每个隔离性的区别
解答：Spring事务详解
##### 2、Spring事务的传播行为，并说说每个传播行为的区别
解答：Spring事务详解
##### 3、hibernate跟Mybatis/ ibatis 的区别，为什么选择？
解答：Hibernate与Mybatis的比较
##### 4、Struts跟Spring mvc的优缺点，让你选会如何选
解答：Spring MVC 与 Struts的区别
##### 5、简单说说Spring 事务机制
解答：Spring事务机制
##### 6、Spring 4.0新特性
解答：Spring4新特性

#### 四、负载均衡、集群相关
1、weblogic 负载均衡的原理和集群的配置
解答：a、WEBLOGIC负载均衡原理    b、负载均衡和集群的配置（参考）
2、Nginx+Tomcat+Redis实现负载均衡、资源分离、session共享 
解答：配置参考
3、nginx配置文件详解——nginx.conf
解答：Nginx配置文件详细说明

#### 五、项目优化相关
1、web如何项目优化
解答：这个我整理过一次，web项目性能优化（整理）
2、单例模式有几种？ 如何优化？
解答：单例模式的7中用法
3、简单说说线程池的原理和实现
解答：线程原理及实现

#### 六、并发和安全方面
1、项目并发如何处理？（我们是web项目）
解答：高并发量网站解决方案，另外，还有数据库乐观锁，数据库读写分离、使用消息队列、多用存储过程等等
2、简单说说功能权限存在的水平权限漏洞和垂直权限漏洞的场景和解决办法（因为我们目前权限级别就是功能权限）
解答：
A、水平权限漏洞，如下图

假设机构有 用户A和用户B 两个用户，其中A有1、2和3权限 ，  用户B有 2 和3 的权限，这时候假设用户B 知道1，并给自己添加1的权限，这时候就是水平权限漏洞。
目前解决办法：1、限制入口，让用户B无法编辑自己的权限   2、对用户B无法进行向上扩展。最根本的解决办法是深入到数据权限
解答：水平权限漏洞和解决办法

B、垂直权限漏洞
解答：垂直权限漏洞案例和解决方案
3、平台上的图片如何防盗链
解答：http下载防盗链原理：http协议的字段referer记录来实现
4、如何区分上传的图片是不是木马？
解答：1、看上传的图片后缀  2、如何后缀是篡改的，那么每个文件有个魔术数字  文件上传-魔术数字
5、消息队列的原理和实现
解答：1、消息队列原理     2、深入浅出 消息队列 ActiveMQ
#### 七、数据库方面
1、mysql查询字段区不区分大小写？
解答：不区分，哪怕值也不区分（我当时还反问了，区不区分大小的应用含义有哪些，面试官没说得出来）
2、简单说说数据库集群和负载均衡、分布式（我不懂这块）
解答：数据库负载均衡和集群参考 ，参考2
3、存储过程的结构和优点
解答：大概结构  
存储过程的优缺点
4、触发器的原理和作用
解答：参考
#### 八、Java底层基础题
1、SpringMVC的原理以及返回数据如何渲染到jsp/html上？

答：Spring MVC的核心就是 DispatcherServlet ， 一个请求经过 DispatcherServlet ，转发给HandlerMapping ,然后经反射，对应 Controller及其里面方法的@RequestMapping地址，最后经ModelAndView和ViewResoler返回给对应视图  。  具体可参考：Spring MVC的工作原理

2、一个类对象属性发生改变时，如何让调用者知道？

答：Java event时间监听  ，即在set方法改变属性时，触发 ，这种模式也可以理解为观察者模式，具体查看：观察者模式简单案例和说明

3、重写equals为何要重写hashCode？

答：判断两个对象是否相等，比较的就是其hashCode, 如果你重载了equals，比如说是基于对象的内容实现的，而保留hashCode的实现不变，那么很可能某两个对象明明是“相等”，而hashCode却不一样。  hashcode不一样，就无法认定两个对象相等了

4、谈谈你对JVM的理解？

答： Java语言的一个非常重要的特点就是与平台的无关性。而使用Java虚拟机是实现这一特点的关键。Java编译器只要面向JVM，生成JVM能理解的代码或字节码文件。Java源文件经编译成字节码程序，通过JVM将每一条指令翻译成不同平台机器码，通过特定平台运行。

JVM执行程序的过程 ：I.加载。class文件   ，II.管理并分配内存  ，III.执行垃圾收集

JRE（java运行时环境）由JVM构造的java程序的运行环境 



 

具体详情：JVM原理和调优

5、Mysql的事物隔离级别？

答：Mysql的事物隔离级别 其实跟 Spring的事物隔离级别一样，都是1、Read Uncommitted（读取未提交内容）， 2、Read Committed（读取提交内容），3、Repeatable Read（可重读），4、Serializable（可串行化）    具体参照：mysql事物隔离级别

6、Spring的原理

答：Spring的核心是IOC和AOP  ，IOC是依赖注入和控制反转， 其注入方式可分为set注入、构造器注入、接口注入等等。IOC就是一个容器，负责实例化、定位、配置应用程序中的对象及建立这些对象间的依赖。简单理解就是：JAVA每个业务逻辑处理至少需要两个或者以上的对象协作进行工作，但是每个对象在使用它的合作对象的时候，都需要频繁的new 对象来实现，你就会发现，对象间的耦合度高了。而IOC的思想是：Spring容器来管理这些，对象只需要处理本身业务关系就好了。至于什么是控制反转，就是获得依赖对象的方式反转了。
AOP呢，面向切面编程，最直接的体现就是Spring事物管理。至于Spring事物的相关资料，就不细说了，参考：Spring注解式事物管理

7、谈谈你对NIO的理解

答：IO是面向流，NIO是面向缓冲 ，这里不细讲了，具体参照：Java NIO和IO的区别

8、ArrayList和LinkedList、Vector的区别？

答：总得来说可以理解为：.

     1.ArrayList是实现了基于动态数组的数据结构，LinkedList基于链表的数据结构。 
     2.对于随机访问get和set，ArrayList觉得优于LinkedList，因为LinkedList要移动指针。 
     3.对于新增和删除操作add和remove，LinedList比较占优势，因为ArrayList要移动数据

Vector和ArrayList类似,但属于强同步类，即线程安全的，具体比较参照：比较ArrayList、LinkedList、Vector

9、随便说说几个单例模式，并选择一种线程安全的

答：单例的类别：懒汉、饿汉、枚举、静态内部类、双重校验锁 等等 ， 选择线程安全我选最后一种，双重校验锁。  具体实现方式参照：Java：单例模式的七种写法

10、谈谈红黑树

答：算法和数据结构一直是我薄弱之处，这方面说自己补吧，成效不大，这里我就推荐一个：红黑树

11、举例说说几个排序，并说明其排序原理

答：这里我就不细说了，大家自己看看 Java实现几种常见的排序算法

12、Mysql索引的原理

答：索引的作用大家都知道，就是加快查询速度，但是原理，我说不上来，这里直接看吧：Mysql索引工作原理

13、序列化的原理和作用

答：Serialization（序列化）是一种将对象以一连串的字节描述的过程；反序列化deserialization是一种将这些字节重建成一个对象的过程，主要用于HTTP或者WebService接口传输过程中对象参数的传播，具体可参看：Java序列化机制和原理

九、并发及项目调优
1、说说线程安全的几种实现方式？

答：什么是线程安全？ 我的理解是这样的，一个对象被多个线程同时访问，还能保持其内部属性的顺序性及同步性，则认定为线程安全。实现线程安全的三种方式：被volatile、synchronized等关键字修饰，或者使用java.util.concurrent下面的类库。  至于前两者的关系，参考：synchronized和volatile的用法区别

2、方法内部，如何实现更好的异步？

答：我们知道异步其实就是让另一个线程去跑，那么如何创建线程？  第一种直接new Thread ，第二种new 一个实现Runnable接口的实现类。 第三种，通过线程池来管理创建等 ，这里说到更好的实现异步，那就是说我们在方法内部避免频繁的new 线程，就可以考虑线程池了。 那么线程池如何创建？ 这里可以new 一个线程池，但是需要考虑单例，或者在程序初始启东时，就创建一个线程池，让他跑着，然后在具体方法的时候，通过线程池来创建线程，实现异步

3、项目中为何要用缓存？如何理解nginx + tomcat + redis 集群缓存？

答1：最直接的表现就是减轻数据库的压力。避免因为数据读取频繁或过大而影响数据库性能，降低程序宕机的可能性

答2：nginx常用做静态内容服务和代理服务器，直面外来请求转发给后面的应用服务。nginx本身也能做缓存，比如静态页面的缓存什么的。而tomcat是应用服务器，处理JAVA WEB程序功能等等 。你也可以这么理解，假设把用户的请求当做是一条河流，那么nginx就相当于一个水利工程，tomcat相当于一条条分流的支流，而redis 相当于支流旁边的一个个水库。 当你洪水来了，nginx根据你每条支流的承受力度分发不同的水流量，在确保程序正常运行的情况下，分发给每条支流(tomcat）不同的水流量。而redis相当于一个个支流的水库，存储水源，降低压力，让后面的水量平稳。

4、日常项目中，如果你接手，你准备从哪些方面调优？

答：这个呢首先是了解哪些需要优化，需要优化肯定是项目性能遭遇瓶颈或者猜测即将遭遇了，我们才会去考虑优化。那么怎么优化？

a、扩容 ，扩容的理解，就是扩充服务器并行处理的能力，简单来说就是加服务器，增加处理请求的能力，例如增加nginx 、tomcat等应用服务器的个数，或者物理服务器的个数，还有加大服务器带宽等等，这里考虑的是硬件方面

b、调优 ，调优，包括系统调优和代码调优 。 系统调优就是说加快处理速度，比如我们所提到的CDN、ehcache、redis等缓存技术，消息队列等等，加快服务间的响应速度，增加系统吞吐量，避免并发，至于代码调优，这些就需要多积累了，比如重构、工厂等， 数据库调优的话这个我不是很懂，只知道索引和存储过程，具体参考：Mysql数据库调优21个最佳实践  ，其他数据库调优方面就各位自己找找吧

5、谈谈你对分布式的理解

答：个人理解：分布式就是把一个系统/业务 拆分成多个子系统/子业务 去协同处理，这个过程就叫分布式，具体的演变方式参考：Java分布式应用技术架构介绍

6、Redis实现消息队列

答：Redis实现消息队列     、参考2

7、另总结多线程相关面试题50道

8、分享一个调优工具和方案：如何利用 JConsole观察分析Java程序的运行，进行排错调优

十、手写代码题（包含sql题）
1、假设商户表A（id , city ）  ,交易流水表B （aid, amount , time）   这里的time代表交易时间，  请用sql写出查询每个城市每个月的销售业绩（答案可在评论里回复）

2、假设有一个数组 A ，int[] A = { 1 , 3 , -1 ,0 , 2 , 1 , -4 , 2 , 0 ,1 ...  N};   原来是需要查出大于0的数组，但是由于传参错误或者其他原因，导致查出0和负数了，现在要求在不使用新数组和新集合的情况下（即只使用这个A数组，因数组数据比较大，且只能用一次循环） 实现正数放到数组的前面，小于等于0的数放到数组的末尾（答案可在评论里回复）

十一、设计方案相关
面试还会问到一些关于设计方案相关的问题，比如

1、你的接口服务数据被人截包了，你如何防止数据恶意提交？

答：我们可以在接口传输参数里面设置一个业务编号，这个编号用来区分是否重复提交。这样即使数据被抓包了，对方也无法区分每个字段你的含义，这时，这个业务编号的作用就来了

2、假设服务器经常宕机，你从哪些方面去排查问题？

答：这个就留个各位看官补充了，可评论回复

 

#集锦二：

一、Java基础

1. String类为什么是final的。

2. HashMap的源码，实现原理，底层结构。

3. 说说你知道的几个Java集合类：list、set、queue、map实现类咯。。。

4. 描述一下ArrayList和LinkedList各自实现和区别

5. Java中的队列都有哪些，有什么区别。

6. 反射中，Class.forName和classloader的区别

7. Java7、Java8的新特性(baidu问的,好BT)

8. Java数组和链表两种结构的操作效率，在哪些情况下(从开头开始，从结尾开始，从中间开始)，哪些操作(插入，查找，删除)的效率高

9. Java内存泄露的问题调查定位：jmap，jstack的使用等等

10. string、stringbuilder、stringbuffer区别

11. hashtable和hashmap的区别

13 .异常的结构，运行时异常和非运行时异常，各举个例子

14. String a= “abc” String b = “abc” String c = new String(“abc”) String d = “ab” + “c” .他们之间用 == 比较的结果

15. String 类的常用方法

16. Java 的引用类型有哪几种

17. 抽象类和接口的区别

18. java的基础类型和字节大小。

19. Hashtable,HashMap,ConcurrentHashMap 底层实现原理与线程安全问题（建议熟悉 jdk 源码，才能从容应答）

20. 如果不让你用Java Jdk提供的工具，你自己实现一个Map，你怎么做。说了好久，说了HashMap源代码，如果我做，就会借鉴HashMap的原理，说了一通HashMap实现

21. Hash冲突怎么办？哪些解决散列冲突的方法？

22. HashMap冲突很厉害，最差性能，你会怎么解决?从O（n）提升到log（n）咯，用二叉排序树的思路说了一通

23. rehash

24. hashCode() 与 equals() 生成算法、方法怎么重写

二、Java IO

1. 讲讲IO里面的常见类，字节流、字符流、接口、实现类、方法阻塞。

2. 讲讲NIO。

3. String 编码UTF-8 和GBK的区别?

4. 什么时候使用字节流、什么时候使用字符流?

5. 递归读取文件夹下的文件，代码怎么实现

三、Java Web

1. session和cookie的区别和联系，session的生命周期，多个服务部署时session管理。

2. servlet的一些相关问题

3. webservice相关问题

4. jdbc连接，forname方式的步骤，怎么声明使用一个事务。举例并具体代码

5. 无框架下配置web.xml的主要配置内容

6. jsp和servlet的区别

四、JVM

1. Java的内存模型以及GC算法

2. jvm性能调优都做了什么

3. 介绍JVM中7个区域，然后把每个区域可能造成内存的溢出的情况说明

4. 介绍GC 和GC Root不正常引用。

5. 自己从classload 加载方式，加载机制说开去，从程序运行时数据区，讲到内存分配，讲到String常量池，讲到JVM垃圾回收机制，算法，hotspot。反正就是各种扩展

6. jvm 如何分配直接内存， new 对象如何不分配在堆而是栈上，常量池解析

7. 数组多大放在 JVM 老年代（不只是设置 PretenureSizeThreshold ，问通常多大，没做过一问便知）

8. 老年代中数组的访问方式

9. GC 算法，永久代对象如何 GC ， GC 有环怎么处理

10. 谁会被 GC ，什么时候 GC

11. 如果想不被 GC 怎么办

12. 如果想在 GC 中生存 1 次怎么办

五、开源框架

1. hibernate和ibatis的区别

2. 讲讲mybatis的连接池。

3. spring框架中需要引用哪些jar包，以及这些jar包的用途

4. springMVC的原理

5. springMVC注解的意思

6. spring中beanFactory和ApplicationContext的联系和区别

7. spring注入的几种方式（循环注入）

8. spring如何实现事物管理的

9. springIOC

10. spring AOP的原理

11. hibernate中的1级和2级缓存的使用方式以及区别原理（Lazy-Load的理解）

12. Hibernate的原理体系架构，五大核心接口，Hibernate对象的三种状态转换，事务管理。

六、多线程

1. Java创建线程之后，直接调用start()方法和run()的区别

2. 常用的线程池模式以及不同线程池的使用场景

3. newFixedThreadPool此种线程池如果线程数达到最大值后会怎么办，底层原理。

4. 多线程之间通信的同步问题，synchronized锁的是对象，衍伸出和synchronized相关很多的具体问题，例如同一个类不同方法都有synchronized锁，一个对象是否可以同时访问。或者一个类的static构造方法加上synchronized之后的锁的影响。

5. 了解可重入锁的含义，以及ReentrantLock 和synchronized的区别

6. 同步的数据结构，例如concurrentHashMap的源码理解以及内部实现原理，为什么他是同步的且效率高

7. atomicinteger和Volatile等线程安全操作的关键字的理解和使用

8. 线程间通信，wait和notify

9. 定时线程的使用

10. 场景：在一个主线程中，要求有大量(很多很多)子线程执行完之后，主线程才执行完成。多种方式，考虑效率。

11. 进程和线程的区别

12. 什么叫线程安全？举例说明

13. 线程的几种状态

14. 并发、同步的接口或方法

15. HashMap 是否线程安全，为何不安全。 ConcurrentHashMap，线程安全，为何安全。底层实现是怎么样的。

16. J.U.C下的常见类的使用。 ThreadPool的深入考察； BlockingQueue的使用。（take，poll的区别，put，offer的区别）；原子类的实现。

17. 简单介绍下多线程的情况，从建立一个线程开始。然后怎么控制同步过程，多线程常用的方法和结构

18. volatile的理解

19. 实现多线程有几种方式，多线程同步怎么做，说说几个线程里常用的方法

七、网络通信

1. http是无状态通信，http的请求方式有哪些，可以自己定义新的请求方式么。

2. socket通信，以及长连接，分包，连接异常断开的处理。

3. socket通信模型的使用，AIO和NIO。

4. socket框架netty的使用，以及NIO的实现原理，为什么是异步非阻塞。

5. 同步和异步，阻塞和非阻塞。

6. OSI七层模型，包括TCP,IP的一些基本知识

7. http中，get post的区别

8. 说说http,tcp,udp之间关系和区别。

9. 说说浏览器访问www.taobao.com，经历了怎样的过程。

10. HTTP协议、  HTTPS协议，SSL协议及完整交互过程；

11. tcp的拥塞，快回传，ip的报文丢弃

12. https处理的一个过程，对称加密和非对称加密

13. head各个特点和区别

14. 说说浏览器访问www.taobao.com，经历了怎样的过程。

八、数据库MySql

1. MySql的存储引擎的不同

2. 单个索引、联合索引、主键索引

3. Mysql怎么分表，以及分表后如果想按条件分页查询怎么办(如果不是按分表字段来查询的话，几乎效率低下，无解)

4. 分表之后想让一个id多个表是自增的，效率实现

5. MySql的主从实时备份同步的配置，以及原理(从库读主库的binlog)，读写分离

6. 写SQL语句。。。

7. 索引的数据结构，B+树

8. 事务的四个特性，以及各自的特点（原子、隔离）等等，项目怎么解决这些问题

9. 数据库的锁：行锁，表锁；乐观锁，悲观锁

10. 数据库事务的几种粒度；

11. 关系型和非关系型数据库区别

九、设计模式

1. 单例模式：饱汉、饿汉。以及饿汉中的延迟加载,双重检查

2. 工厂模式、装饰者模式、观察者模式。

3. 工厂方法模式的优点（低耦合、高内聚，开放封闭原则）

十、算法

1. 使用随机算法产生一个数，要求把1-1000W之间这些数全部生成。（考察高效率，解决产生冲突的问题）

2. 两个有序数组的合并排序

3. 一个数组的倒序

4. 计算一个正整数的正平方根

5. 说白了就是常见的那些查找、排序算法以及各自的时间复杂度

6. 二叉树的遍历算法

7. DFS,BFS算法

9. 比较重要的数据结构，如链表，队列，栈的基本理解及大致实现。

10. 排序算法与时空复杂度（快排为什么不稳定，为什么你的项目还在用）

11. 逆波兰计算器

12. Hoffman 编码

13. 查找树与红黑树

十一、并发与性能调优

1. 有个每秒钟5k个请求，查询手机号所属地的笔试题(记得不完整，没列出)，如何设计算法?请求再多，比如5w，如何设计整个系统?

2. 高并发情况下，我们系统是如何支撑大量的请求的

3. 集群如何同步会话状态

4. 负载均衡的原理

5 .如果有一个特别大的访问量，到数据库上，怎么做优化（DB设计，DBIO，SQL优化，Java优化）

6. 如果出现大面积并发，在不增加服务器的基础上，如何解决服务器响应不及时问题“。

7. 假如你的项目出现性能瓶颈了，你觉得可能会是哪些方面，怎么解决问题。

8. 如何查找 造成 性能瓶颈出现的位置，是哪个位置照成性能瓶颈。

9. 你的项目中使用过缓存机制吗？有没用用户非本地缓存

十二、其他

1.常用的linux下的命令
