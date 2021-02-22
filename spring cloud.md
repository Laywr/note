# spring cloud

![preview](https://pic2.zhimg.com/v2-25915a395592b4a88a3a35dff89ab969_r.jpg)

![img](https://img-blog.csdnimg.cn/20200327232316452.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODMwNTQ0MA==,size_16,color_FFFFFF,t_70#pic_center)

Spring Cloud 就是微服务系统架构的一站式解决方案，在平时我们构建微服务的过程中需要做如 **服务发现注册** 、**配置中心** 、**消息总线** 、**负载均衡** 、**断路器** 、**数据监控** 等操作，而 Spring Cloud 为我们提供了一套简易的编程模型，使我们能在 Spring Boot 的基础上轻松地实现微服务项目的构建

### Eureka---服务发现框架

##### **1.服务注册 Register**：

当 `Eureka` 客户端(提供者 `Eureka Client Provider`)向 `Eureka Server` 注册时，它提供自身的**元数据**，比如IP地址、端口，运行状况指示符URL，主页等。

##### **2.服务续约 Renew**：

**`Eureka`** **客户 (提供者 `Eureka Client Provider`)会每隔30秒(默认情况下)发送一次心跳来续约**。 通过续约来告知 `Eureka Server` 该 `Eureka` 客户仍然存在，没有出现问题。 正常情况下，如果 `Eureka Server`在90秒没有收到 `Eureka` 客户的续约，它会将实例从其注册表中删除。

##### **3.获取注册列表信息 Fetch Registries**：

`Eureka` 客户端(消费者 `Eureka Client Consumer`)从服务器获取注册表信息，并将其缓存在本地。客户端会使用该信息查找其他服务，从而进行远程调用。该注册列表信息定期（每30秒钟）更新一次。每次返回注册列表信息可能与 `Eureka` 客户端的缓存信息不同, `Eureka` 客户端自动处理。如果由于某种原因导致注册列表信息不能及时匹配，`Eureka` 客户端则会重新获取整个注册表信息。 `Eureka` 服务器缓存注册列表信息，整个注册表以及每个应用程序的信息进行了压缩，压缩内容和没有压缩的内容完全相同。`Eureka` 客户端和 `Eureka` 服务器可以使用JSON / XML格式进行通讯。在默认的情况下 `Eureka` 客户端使用压缩 `JSON` 格式来获取注册列表的信息。

##### **4.服务下线 Cancel**：

`Eureka`客户端在程序关闭时向Eureka服务器发送取消请求。 发送请求后，该客户端实例信息将从服务器的实例注册表中删除。该下线请求不会自动完成，它需要调用以下内容：`DiscoveryManager.getInstance().shutdownComponent();`

**5.服务剔除 Eviction**：

在默认的情况下，**当Eureka客户端连续90秒(3个续约周期)没有向Eureka服务器发送服务续约，即心跳，Eureka服务器会将该服务实例从服务注册列表删除**，即服务剔除。

### 负载均衡之 Ribbon

##### 1.什么是 RestTemplate?

**`RestTemplate`是`Spring`提供的一个访问Http服务的客户端类**，就是微服务之间的调用使用`RestTemplate` 

##### 2.什么是Ribbon？

`Ribbon` 是 `Netflix` 公司的一个开源的负载均衡 项目，是一个客户端/进程内负载均衡器，**运行在消费者端**。

其工作原理就是 `Consumer` 端获取到了所有的服务列表之后，在其**内部**使用**负载均衡算法**，进行对多个系统的调用。

##### 3.Nginx 和 Ribbon 的对比

**Nginx**是一种**集中式**的负载均衡器。简单理解就是 **将所有请求都集中起来，然后再进行负载均衡**。如下图。

![preview](https://pic1.zhimg.com/v2-4fb6c809e868f2a897f87580f2786d4c_r.jpg)

我们可以看到 `Nginx` 是接收了所有的请求进行负载均衡的，而对于 `Ribbon` 来说它是在消费者端进行的负载均衡。如下图。

![preview](https://pic3.zhimg.com/v2-8f08b87f73a9707c9b25a7d711e4b74e_r.jpg)

请注意 `Request` 的位置，在 `Nginx` 中请求是先进入负载均衡器，而在 `Ribbon` 中是先在客户端进行负载均衡才进行请求的。

##### 4.Ribbon 的几种负载均衡算法

负载均衡，不管 `Nginx` 还是 `Ribbon` 都需要其算法的支持，如果我没记错的话 `Nginx` 使用的是 轮询和加权轮询算法。而在 `Ribbon` 中有更多的负载均衡调度算法，其默认是使用的 `RoundRobinRule` 轮询策略。

- **RoundRobinRule**：轮询策略。`Ribbon` 默认采用的策略。若经过一轮轮询没有找到可用的 `provider`，其最多轮询 10 轮。若最终还没有找到，则返回 null。
- **RandomRule**: 随机策略，从所有可用的 provider 中随机选择一个。
- **RetryRule**: 重试策略。先按照 RoundRobinRule 策略获取 provider，若获取失败，则在指定的时限内重试。默认的时限为 500 毫秒。

还有很多，这里不一一举 了，你最需要知道的是默认轮询算法，并且可以更换默认的负载均衡算法，只需要在配置文件中做出修改就行。

### 分布式系统的延迟和容错库---Hystrix

##### 1.什么是Hystrix

在分布式环境中，不可避免地会有许多服务依赖项中的某些失败。Hystrix是一个库，可通过添加等待时间容限和容错逻辑来帮助您控制这些分布式服务之间的交互。Hystrix通过隔离服务之间的访问点，停止服务之间的级联故障并提供后备选项来实现此目的，所有这些都可以提高系统的整体弹性。

总体来说 `Hystrix` 就是一个能进行 **熔断** 和 **降级** 的库，通过使用它能提高整个系统的弹性。

那么什么是 熔断和降级 呢？再举个 ，此时我们整个微服务系统是这样的。服务A调用了服务B，服务B再调用了服务C，但是因为某些原因，服务C顶不住了，这个时候大量请求会在服务C阻塞。

![img](https://pic2.zhimg.com/80/v2-2caef15d9073ef3e1983846aba5a100d_720w.jpg)

服务C阻塞了还好，毕竟只是一个系统崩溃了。但是请注意这个时候因为服务C不能返回响应，那么服务B调用服务C的的请求就会阻塞，同理服务B阻塞了，那么服务A也会阻塞崩溃。

> 请注意，为什么阻塞会崩溃。因为这些请求会消耗占用系统的线程、IO 等资源，消耗完你这个系统服务器不就崩了么。

![img](https://pic3.zhimg.com/80/v2-ac973d4014e6ab65493a659176ac22e2_720w.jpg)

所谓 **熔断** 就是服务雪崩的一种有效解决方案。当指定时间窗内的请求失败率达到设定阈值时，系统将通过 **断路器** 直接将此请求链路断开。

也就是我们上面服务B调用服务C在指定时间窗内，调用的失败率到达了一定的值，那么 `Hystrix` 则会自动将 服务B与C 之间的请求都断了，以免导致服务雪崩现象。

其实这里所讲的 **熔断** 就是指的 `Hystrix` 中的 **断路器模式** ，你可以使用简单的 `@HystrixCommand` 注解来标注某个方法，这样 `Hystrix` 就会使用 **断路器** 来“包装”这个方法，每当调用时间超过指定时间时(默认为1000ms)，断路器将会中断对这个方法的调用。

当然你可以对这个注解的很多属性进行设置，比如设置超时时间，像这样。

```java
@HystrixCommand(
    commandProperties = {@HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "1200")}
)
public List<Xxx> getXxxx() {
    // ...省略代码逻辑
}
```

但是，我查阅了一些博客，发现他们都将 **熔断** 和 **降级** 的概念混淆了，以我的理解，**降级是为了更好的用户体验，当一个方法调用异常时，通过执行另一种代码逻辑来给用户友好的回复**。这也就对应着 `Hystrix` 的 **后备处理** 模式。你可以通过设置 `fallbackMethod` 来给一个方法设置备用的代码逻辑。比如这个时候有一个热点新闻出现了，我们会推荐给用户查看详情，然后用户会通过id去查询新闻的详情，但是因为这条新闻太火了(比如最近什么*易对吧)，大量用户同时访问可能会导致系统崩溃，那么我们就进行 **服务降级** ，一些请求会做一些降级处理比如当前人数太多请稍后查看等等。

```java
// 指定了后备方法调用
@HystrixCommand(fallbackMethod = "getHystrixNews")
@GetMapping("/get/news")
public News getNews(@PathVariable("id") int id) {
    // 调用新闻系统的获取新闻api 代码逻辑省略
}
// 
public News getHystrixNews(@PathVariable("id") int id) {
    // 做服务降级
    // 返回当前人数太多，请稍后查看
}
```

### 微服务网关——Zuul

> ZUUL 是从设备和 web 站点到 Netflix 流应用后端的所有请求的前门。作为边界服务应用，ZUUL 是为了实现动态路由、监视、弹性和安全性而构建的。它还具有根据情况将请求路由到多个 Amazon Auto Scaling Groups（亚马逊自动缩放组，亚马逊的一种云计算方式） 的能力

在上面我们学习了 `Eureka` 之后我们知道了 *服务提供者* 是 *消费者* 通过 `Eureka Server` 进行访问的，即 `Eureka Server` 是 *服务提供者* 的统一入口。那么整个应用中存在那么多 *消费者*需要用户进行调用，这个时候用户该怎样访问这些 *消费者工程* 呢？当然可以像之前那样直接访问这些工程。但这种方式没有统一的消费者工程调用入口，不便于访问与管理，而 Zuul 就是这样的一个对于 *消费者* 的统一入口。

> 如果学过前端的肯定都知道 Router 吧，比如 Flutter 中的路由，Vue，React中的路由，用了 Zuul 你会发现在路由功能方面和前端配置路由基本是一个理。 我偶尔撸撸 Flutter。

大家对网关应该很熟吧，简单来讲网关是系统唯一对外的入口，介于客户端与服务器端之间，用于对请求进行**鉴权**、**限流**、 **路由**、**监控**等功能。

![img](https://pic3.zhimg.com/80/v2-e2a6075906b3df6f670b9394eacdde1e_720w.jpg)

没错，网关有的功能，`Zuul` 基本都有。而 `Zuul` 中最关键的就是 **路由和过滤器** 了，在官方文档中 `Zuul` 的标题就是

> Router and Filter : Zuul

#### Zuul 的路由功能

##### 1.简单配置

本来想给你们复制一些代码，但是想了想，因为各个代码配置比较零散，看起来也比较零散，我决定还是给你们画个图来解释吧。

> 请不要因为我这么好就给我点赞 。 疯狂暗示。

比如这个时候我们已经向 `Eureka Server` 注册了两个 `Consumer` 、三个 `Provicer` ，这个时候我们再加个 `Zuul` 网关应该变成这样子了。

![img](https://pic2.zhimg.com/80/v2-a769d86a41446baba4e91b3c8e129d8d_720w.jpg)

emmm，信息量有点大，我来解释一下。关于前面的知识我就不解释了 。

首先，`Zuul` 需要向 `Eureka` 进行注册，注册有啥好处呢？

你傻呀，`Consumer` 都向 `Eureka Server` 进行注册了，我网关是不是只要注册就能拿到所有 `Consumer` 的信息了？

拿到信息有什么好处呢？

我拿到信息我是不是可以获取所有的 `Consumer` 的元数据(名称，ip，端口)？

拿到这些元数据有什么好处呢？拿到了我们是不是直接可以做**路由映射**？比如原来用户调用 `Consumer1` 的接口 `localhost:8001/studentInfo/update` 这个请求，我们是不是可以这样进行调用了呢？`localhost:9000/consumer1/studentInfo/update` 呢？你这样是不是恍然大悟了？

> 这里的url为了让更多人看懂所以没有使用 restful 风格。

上面的你理解了，那么就能理解关于 `Zuul` 最基本的配置了，看下面。

```java
server:
  port: 9000
eureka:
  client:
    service-url:
      # 这里只要注册 Eureka 就行了
      defaultZone: http://localhost:9997/eureka
```

然后在启动类上加入 `@EnableZuulProxy` 注解就行了。没错，就是那么简单 。

##### 2.统一前缀

这个很简单，就是我们可以在前面加一个统一的前缀，比如我们刚刚调用的是 `localhost:9000/consumer1/studentInfo/update`，这个时候我们在 `yaml` 配置文件中添加如下。

```java
zuul:
  prefix: /zuul
```

这样我们就需要通过 `localhost:9000/zuul/consumer1/studentInfo/update` 来进行访问了。

##### 3.路由策略配置

你会发现前面的访问方式(直接使用服务名)，需要将微服务名称暴露给用户，会存在安全性问题。所以，可以自定义路径来替代微服务名称，即自定义路由策略。

```java
zuul:
  routes:
    consumer1: /FrancisQ1/**
    consumer2: /FrancisQ2/**
```

这个时候你就可以使用 `localhost:9000/zuul/FrancisQ1/studentInfo/update` 进行访问了。

##### 4.服务名屏蔽

这个时候你别以为你好了，你可以试试，在你配置完路由策略之后使用微服务名称还是可以访问的，这个时候你需要将服务名屏蔽。

```java
zuul:
  ignore-services: "*"
```

##### 5.路径屏蔽

`Zuul` 还可以指定屏蔽掉的路径 URI，即只要用户请求中包含指定的 URI 路径，那么该请求将无法访问到指定的服务。通过该方式可以限制用户的权限。

```java
zuul:
  ignore-patterns: **/auto/**
```

这样关于 auto 的请求我们就可以过滤掉了。

> ** 代表匹配多级任意路径
> *代表匹配一级任意路径

##### 6.敏感请求头屏蔽

默认情况下，像 Cookie、Set-Cookie 等敏感请求头信息会被 zuul 屏蔽掉，我们可以将这些默认屏蔽去掉，当然，也可以添加要屏蔽的请求头。

#### Zuul 的过滤功能

如果说，路由功能是 `Zuul` 的基操的话，那么**过滤器**就是 `Zuul`的利器了。毕竟所有请求都经过网关(Zuul)，那么我们可以进行各种过滤，这样我们就能实现 **限流**，**灰度发布**，**权限控制**等等。

##### 1.简单实现一个请求时间日志打印

要实现自己定义的 `Filter` 我们只需要继承 `ZuulFilter` 然后将这个过滤器类以 `@Component`注解加入 Spring 容器中就行了。

在给你们看代码之前我先给你们解释一下关于过滤器的一些注意点。

![img](https://pic4.zhimg.com/80/v2-440c30bd92ef85a9370a4c4aafe5fe8f_720w.jpg)

过滤器类型：Pre、Routing、Post。前置Pre就是在请求之前进行过滤，Routing路由过滤器就是我们上面所讲的路由策略，而Post后置过滤器就是在 `Response` 之前进行过滤的过滤器。你可以观察上图结合着理解，并且下面我会给出相应的注释。

```java
// 加入Spring容器
@Component
public class PreRequestFilter extends ZuulFilter {
    // 返回过滤器类型 这里是前置过滤器
    @Override
    public String filterType() {
        return FilterConstants.PRE_TYPE;
    }
    // 指定过滤顺序 越小越先执行，这里第一个执行
    // 当然不是只真正第一个 在Zuul内置中有其他过滤器会先执行
    // 那是写死的 比如 SERVLET_DETECTION_FILTER_ORDER = -3
    @Override
    public int filterOrder() {
        return 0;
    }
    // 什么时候该进行过滤
    // 这里我们可以进行一些判断，这样我们就可以过滤掉一些不符合规定的请求等等
    @Override
    public boolean shouldFilter() {
        return true;
    }
    // 如果过滤器允许通过则怎么进行处理
    @Override
    public Object run() throws ZuulException {
        // 这里我设置了全局的RequestContext并记录了请求开始时间
        RequestContext ctx = RequestContext.getCurrentContext();
        ctx.set("startTime", System.currentTimeMillis());
        return null;
    }
}
```



```java
// lombok的日志
@Slf4j
// 加入 Spring 容器
@Component
public class AccessLogFilter extends ZuulFilter {
    // 指定该过滤器的过滤类型
    // 此时是后置过滤器
    @Override
    public String filterType() {
        return FilterConstants.POST_TYPE;
    }
    // SEND_RESPONSE_FILTER_ORDER 是最后一个过滤器
    // 我们此过滤器在它之前执行
    @Override
    public int filterOrder() {
        return FilterConstants.SEND_RESPONSE_FILTER_ORDER - 1;
    }
    @Override
    public boolean shouldFilter() {
        return true;
    }
    // 过滤时执行的策略
    @Override
    public Object run() throws ZuulException {
        RequestContext context = RequestContext.getCurrentContext();
        HttpServletRequest request = context.getRequest();
        // 从RequestContext获取原先的开始时间 并通过它计算整个时间间隔
        Long startTime = (Long) context.get("startTime");
        // 这里我可以获取HttpServletRequest来获取URI并且打印出来
        String uri = request.getRequestURI();
        long duration = System.currentTimeMillis() - startTime;
        log.info("uri: " + uri + ", duration: " + duration / 100 + "ms");
        return null;
    }
}
```

上面就简单实现了请求时间日志打印功能，你有没有感受到 `Zuul` 过滤功能的强大了呢？

没有？好的、那我们再来。

##### 2.令牌桶限流

当然不仅仅是令牌桶限流方式，`Zuul` 只要是限流的活它都能干，这里我只是简单举个 。

![img](https://pic3.zhimg.com/80/v2-98d610867e6063d7ada0c69cff061cca_720w.jpg)

我先来解释一下什么是 **令牌桶限流** 吧。

首先我们会有个桶，如果里面没有满那么就会以一定 **固定的速率** 会往里面放令牌，一个请求过来首先要从桶中获取令牌，如果没有获取到，那么这个请求就拒绝，如果获取到那么就放行。很简单吧，啊哈哈、

下面我们就通过 `Zuul` 的前置过滤器来实现一下令牌桶限流。

```java
@Component
@Slf4j
public class RouteFilter extends ZuulFilter {
    // 定义一个令牌桶，每秒产生2个令牌，即每秒最多处理2个请求
    private static final RateLimiter RATE_LIMITER = RateLimiter.create(2);
    @Override
    public String filterType() {
        return FilterConstants.PRE_TYPE;
    }
 
    @Override
    public int filterOrder() {
        return -5;
    }
 
    @Override
    public Object run() throws ZuulException {
        log.info("放行");
        return null;
    }
 
    @Override
    public boolean shouldFilter() {
        RequestContext context = RequestContext.getCurrentContext();
        if(!RATE_LIMITER.tryAcquire()) {
            log.warn("访问量超载");
            // 指定当前请求未通过过滤
            context.setSendZuulResponse(false);
            // 向客户端返回响应码429，请求数量过多
            context.setResponseStatusCode(429);
            return false;
        }
        return true;
    }
}
```

这样我们就能将请求数量控制在一秒两个，有没有觉得很酷？

##### 3.关于 Zuul 的其他

`Zuul` 的过滤器的功能肯定不止上面我所实现的两种，它还可以实现 **权限校验**，包括我上面提到的 **灰度发布** 等等。

当然，`Zuul` 作为网关肯定也存在 **单点问题** ，如果我们要保证 `Zuul` 的高可用，我们就需要进行 `Zuul` 的集群配置，这个时候可以借助额外的一些负载均衡器比如 `Nginx` 。

### Spring Cloud配置管理——Config

#### 为什么要使用进行配置管理？

当我们的微服务系统开始慢慢地庞大起来，那么多 `Consumer` 、`Provider` 、`Eureka Server`、`Zuul` 系统都会持有自己的配置，这个时候我们在项目运行的时候可能需要更改某些应用的配置，如果我们不进行配置的统一管理，我们只能**去每个应用下一个一个寻找配置文件然后修改配置文件再重启应用**。

首先对于分布式系统而言我们就不应该去每个应用下去分别修改配置文件，再者对于重启应用来说，服务无法访问所以直接抛弃了可用性，这是我们更不愿见到的。

那么有没有一种方法**既能对配置文件统一地进行管理，又能在项目运行时动态修改配置文件呢？**

那就是我今天所要介绍的 `Spring Cloud Config` 。

> 能进行配置管理的框架不止 `Spring Cloud Config` 一种，大家可以根据需求自己选择（disconf，阿波罗等等）。而且对于 `Config` 来说有些地方实现的不是那么尽人意。

#### Config 是什么

> `Spring Cloud Config` 为分布式系统中的外部化配置提供服务器和客户端支持。使用 `Config` 服务器，可以在中心位置管理所有环境中应用程序的外部属性。

简单来说，`Spring Cloud Config` 就是能将各个 应用/系统/模块 的配置文件存放到 **统一的地方然后进行管理**(Git 或者 SVN)。

你想一下，我们的应用是不是只有启动的时候才会进行配置文件的加载，那么我们的 `Spring Cloud Config` 就暴露出一个接口给启动应用来获取它所想要的配置文件，应用获取到配置文件然后再进行它的初始化工作。就如下图。

![img](https://pic4.zhimg.com/80/v2-de13cbef4173976d3de4330da8add933_720w.jpg)

当然这里你肯定还会有一个疑问，如果我在应用运行时去更改远程配置仓库(Git)中的对应配置文件，那么依赖于这个配置文件的已启动的应用会不会进行其相应配置的更改呢？

答案是不会的。

什么？那怎么进行动态修改配置文件呢？这不是出现了 **配置漂移** 吗？你个渣男 ，你又骗我！

别急嘛，你可以使用 `Webhooks` ，这是 `github` 提供的功能，它能确保远程库的配置文件更新后客户端中的配置信息也得到更新。

噢噢，这还差不多。我去查查怎么用。

慢着，听我说完，`Webhooks` 虽然能解决，但是你了解一下会发现它根本不适合用于生产环境，所以基本不会使用它的。

而一般我们会使用 `Bus` 消息总线 + `Spring Cloud Config` 进行配置的动态刷新。

### 引出 Spring Cloud Bus

> 用于将服务和服务实例与分布式消息系统链接在一起的事件总线。在集群中传播状态更改很有用（例如配置更改事件）。

你可以简单理解为 `Spring Cloud Bus` 的作用就是**管理和广播分布式系统中的消息**，也就是消息引擎系统中的广播模式。当然作为 **消息总线** 的 `Spring Cloud Bus` 可以做很多事而不仅仅是客户端的配置刷新功能。

而拥有了 `Spring Cloud Bus` 之后，我们只需要创建一个简单的请求，并且加上 `@ResfreshScope` 注解就能进行配置的动态修改了，下面我画了张图供你理解。

![img](https://pic3.zhimg.com/80/v2-a720f82bc14734b7c31f22d5a280acfa_720w.jpg)





