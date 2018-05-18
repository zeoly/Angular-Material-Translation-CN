# 使用Ribbon和Spring Cloud实现客户端负载均衡

> Client Side Load Balancing with Ribbon and Spring Cloud

通过此教程，你将使用Netflix Ribbon为一个微服务应用提供客户端负载均衡。

> This guide walks you through the process of providing client-side load balancing for a microservice application using Netflix Ribbon.

## 将要做什么

> What you’ll build

你将构建一个微服务应用，并使用Netflix Ribbon与Spring Cloud Netflix，在调用另一个微服务中提供客户端负载均衡。

> You’ll build a microservice application that uses Netflix Ribbon and Spring Cloud Netflix to provide client-side load balancing in calls to another microservice.

## 需要些什么

> What you’ll need

- 大约15分钟
- 你喜欢的文本编辑器或者IDE
- [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/index.html)或更高版本
- [Gradle 4+](http://www.gradle.org/downloads)或者[Maven 3.2+](https://maven.apache.org/download.cgi)
- 你也可以直接导入代码到IDE中：
  - [Spring Tool Suite (STS)](https://spring.io/guides/gs/sts)
  - [IntelliJ IDEA](https://spring.io/guides/gs/intellij-idea/)

> - About 15 minutes
> - A favorite text editor or IDE
> - [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) or later
> - [Gradle 4+](http://www.gradle.org/downloads) or [Maven 3.2+](https://maven.apache.org/download.cgi)
> - You can also import the code straight into your IDE:
>   - [Spring Tool Suite (STS)](https://spring.io/guides/gs/sts)
>   - [IntelliJ IDEA](https://spring.io/guides/gs/intellij-idea/)

## 如何完成此教程

> How to complete this guide

与大多数Spring的[入门教程](https://spring.io/guides)相似，你可以从头开始并完成每一步，或者你也可以跳过你熟悉的设置步骤。无论怎样，你最终应当以有效代码结束。

> Like most Spring [Getting Started guides](https://spring.io/guides), you can start from scratch and complete each step, or you can bypass basic setup steps that are already familiar to you. Either way, you end up with working code.

如果**从头开始**，可参考[使用Gradle构建](https://spring.io/guides/gs/client-side-load-balancing/#scratch)。

> To **start from scratch**, move on to [Build with Gradle](https://spring.io/guides/gs/client-side-load-balancing/#scratch).

如果要**跳过基本步骤**，按如下步骤：

> To **skip the basics**, do the following:

- [下载](https://github.com/spring-guides/gs-client-side-load-balancing/archive/master.zip)并解压此教程的源码仓库，或者使用[Git](https://spring.io/understanding/Git)进行clone：`git clone https://github.com/spring-guides/gs-client-side-load-balancing.git`
- 进入`gs-client-side-load-balancing/initial`目录
- 跳至[写一个服务端服务](https://spring.io/guides/gs/client-side-load-balancing/#initial).

> - [Download](https://github.com/spring-guides/gs-client-side-load-balancing/archive/master.zip) and unzip the source repository for this guide, or clone it using [Git](https://spring.io/understanding/Git): `git clone https://github.com/spring-guides/gs-client-side-load-balancing.git`
> - cd into `gs-client-side-load-balancing/initial`
> - Jump ahead to [Write a server service](https://spring.io/guides/gs/client-side-load-balancing/#initial).

**当你完成时**，你可以与`gs-client-side-load-balancing/complete`中的代码对比检查。

> **When you’re finished**, you can check your results against the code in `gs-client-side-load-balancing/complete`.

## 写一个服务端服务

> Write a server service

我们的“服务端”服务叫做Say Hello。访问`/greeting`端点，将返回一个随机的问候（从包含三个问候的列表中选出）

> Our “server” service is called Say Hello. It will return a random greeting (picked out of a static list of three) from an endpoint accessible at `/greeting`.

在`src/main/java/hello`目录下，创建一个`SayHelloApplication.java`文件，如下：

> In `src/main/java/hello`, create the file `SayHelloApplication.java`. It should look like this:

`say-hello/src/main/java/hello/SayHelloApplication.java`

```java
package hello;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.util.*;

@RestController
@SpringBootApplication
public class SayHelloApplication {

  private static Logger log = LoggerFactory.getLogger(SayHelloApplication.class);

  @RequestMapping(value = "/greeting")
  public String greet() {
    log.info("Access /greeting");

    List<String> greetings = Arrays.asList("Hi there", "Greetings", "Salutations");
    Random rand = new Random();

    int randomNum = rand.nextInt(greetings.size());
    return greetings.get(randomNum);
  }

  @RequestMapping(value = "/")
  public String home() {
    log.info("Access /");
    return "Hi!";
  }

  public static void main(String[] args) {
    SpringApplication.run(SayHelloApplication.class, args);
  }
}
```

`@RestController`注解相当于同时使用`@Controller`和`@ResponseBody`的效果。将`SayHelloApplication`标记为控制器类（即`@Controller`功能），并确保此类中的`@RequestMapping`方法的返回值将正确的自动转换并直接写入响应体（即`@ResponseBody`功能）。我们有一个`@RequestMapping`方法对应`/greeting`，还有一个对应根路径`/`。（稍后会用到第二个方法与Ribbon协作。）

> The `@RestController` annotation gives the same effect as if we were using `@Controller` and `@ResponseBody` together. It marks `SayHelloApplication` as a controller class (which is what `@Controller` does) and ensures that return values from the class’s `@RequestMapping` methods will be automatically converted appropriately from their original types and written directly to the response body (which is what `@ResponseBody` does). We have one `@RequestMapping` method for `/greeting` and then another for the root path `/`. (We’ll want that second method when we get to working with Ribbon in just a bit.)

我们将同时运行一个客户端服务应用和多个此应用实例，所以创建目录`src/main/resources`，并在其中创建文件`application.yml`，然后在文件中设置`server.port`的默认值。（我们将指定其他的应用实例运行在其他的端口上，所以每个Say Hello实例间都不会与客户端冲突。）同时还要设置服务的`spring.application.name`。

> We’re going to run multiple instances of this application locally alongside a client service application, so create the directory `src/main/resources`, create the file `application.yml` within it, and then in that file, set a default value for `server.port`. (We’ll instruct the other instances of the application to run on other ports, as well, so that none of the Say Hello instances will conflict with the client when we get that running.) While we’re in this file, we’ll set the `spring.application.name` for our service too.

`say-hello/src/main/resources/application.yml`

```yml
spring:
  application:
    name: say-hello

server:
  port: 8090
```

## 从一个客户端服务访问

> Access from a client service

User应用是对用户可见的。当用户通过`/hi`端点访问时，将会调用Say Hello应用，获取一个问候并发送给用户。

> The User application will be what our user sees. It will make a call to the Say Hello application to get a greeting and then send that to our user when the user visits the endpoint at `/hi`.

在User应用的`src/main/java/hello`目录下，增加文件`UserApplication.java`：

> In the User application directory, under `src/main/java/hello`, add the file `UserApplication.java`:

`user/src/main/java/hello/UserApplication.java`

```java
package hello;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

@SpringBootApplication
@RestController
public class UserApplication {

  @Bean
  RestTemplate restTemplate(){
    return new RestTemplate();
  }

  @Autowired
  RestTemplate restTemplate;

  @RequestMapping("/hi")
  public String hi(@RequestParam(value="name", defaultValue="Artaban") String name) {
    String greeting = this.restTemplate.getForObject("http://localhost:8090/greeting", String.class);
    return String.format("%s, %s!", greeting, name);
  }

  public static void main(String[] args) {
    SpringApplication.run(UserApplication.class, args);
  }
}
```

我们使用Spring的`RestTemplate`模板类，从Say Hello中获取一个问候。`RestTemplate`通过发送一个HTTP GET请求到我们提供的Say Hello服务的URL，并返回一个字符串`String`结果。（可以查看[消费一个RESTful Web服务](https://spring.io/guides/gs/consuming-rest/)，获取更多关于使用Spring消费RESTful服务的信息。）

> To get a greeting from Say Hello, we’re using Spring’s `RestTemplate` template class. `RestTemplate` makes an HTTP GET request to the Say Hello service’s URL as we provide it and gives us the result as a `String`. (For more information on using Spring to consume a RESTful service, see the [Consuming a RESTful Web Service](https://spring.io/guides/gs/consuming-rest/) guide.)

在`src/main/resources/application.properties`或`src/main/resources/application.yml`中，添加`spring.application.name`和`server.port`属性：

> Add the `spring.application.name` and `server.port` properties to `src/main/resources/application.properties` or `src/main/resources/application.yml`:

`user/src/main/resources/application.yml`

```yml
spring:
  application:
    name: user

server:
  port: 8888
```

## 负载均衡访问服务器实例

> Load balance across server instances

现在我们可以访问User服务的`/hi`，可以看见一个友好的问候：

> Now we can access `/hi` on the User service and see a friendly greeting:

```shell
$ curl http://localhost:8888/hi
Greetings, Artaban!

$ curl http://localhost:8888/hi?name=Orontes
Salutations, Orontes!
```

我们需要设置Ribbon，将硬编码的URL转换为均衡负载方案。在`user/src/main/resources/`目录下的`application.yml`文件中，添加如下属性：

> To move beyond a single hard-coded server URL to a load-balanced solution, let’s set up Ribbon. In the `application.yml` file under `user/src/main/resources/`, add the following properties:

```yml
say-hello:
  ribbon:
    eureka:
      enabled: false
    listOfServers: localhost:8090,localhost:9092,localhost:9999
    ServerListRefreshInterval: 15000
```

这些属性配置是在Ribbon客户端上。Spring Cloud Netflix为每个在应用中的Ribbon客户端名称创建一个`ApplicationContext`。用于给客户端一组Ribbon组件实例的bean，包含：

> This configures properties on a Ribbon client. Spring Cloud Netflix creates an `ApplicationContext` for each Ribbon client name in our application. This is used to give the client a set of beans for instances of Ribbon components, including:

- `IClientConfig`，为客户端或负载均衡器存储客户端配置，
- `ILoadBalancer`，表示一个软负载均衡器，
- `ServerList`，定义如何获取服务器列表，
- `IRule`，表述一个负载均衡的策略，
- `IPing`，服务器ping的周期。

> - an `IClientConfig`, which stores client configuration for a client or load balancer,
> - an `ILoadBalancer`, which represents a software load balancer,
> - a `ServerList`, which defines how to get a list of servers to choose from,
> - an `IRule`, which describes a load balancing strategy, and
> - an `IPing`, which says how periodic pings of a server are performed.

在上述的例子中，客户端名称为`say-hello`。我们设置了`eureka.enabled` (设置值为`false`)，`listOfServers`和`ServerListRefreshInterval`等属性。Ribbon中的负载均衡器通常是从Netflix Eureka服务注册中心中获取服务器列表。（查看[服务注册与发现](https://spring.io/guides/gs/service-registration-and-discovery/)教程来获取使用Spring Cloud和Eureka服务注册中心的相关信息。）对于较简单的此例，我们跳过了Eureka，所以设置`ribbon.eureka.enabled`属性为`false`，作为替代给Ribbon一个静态的`listOfServers`。`ServerListRefreshInterval`是毫秒单位的刷新Ribbon服务列表的间隔。

> In our case above, the client is named `say-hello`. The properties we set are `eureka.enabled` (which we set to `false`), `listOfServers`, and `ServerListRefreshInterval`. Load balancers in Ribbon normally get their server lists from a Netflix Eureka service registry. (See the [Service Registration and Discovery](https://spring.io/guides/gs/service-registration-and-discovery/) guide for information on using a Eureka service registry with Spring Cloud.) For our simple purposes here, we’re skipping Eureka, so we set the `ribbon.eureka.enabled` property to `false` and instead give Ribbon a static `listOfServers`. `ServerListRefreshInterval` is the interval, in milliseconds, between refreshes of Ribbon’s service list.

在`UserApplication`类中，使用Ribbon客户端切换`RestTemplate`来获取Say Hello的服务器地址：

> In our `UserApplication` class, switch the `RestTemplate` to use the Ribbon client to get the server address for Say Hello:

> `user/src/main/java/hello/UserApplication.java`

```java
package hello;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;
import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.cloud.netflix.ribbon.RibbonClient;

@SpringBootApplication
@RestController
@RibbonClient(name = "say-hello", configuration = SayHelloConfiguration.class)
public class UserApplication {

  @LoadBalanced
  @Bean
  RestTemplate restTemplate(){
    return new RestTemplate();
  }

  @Autowired
  RestTemplate restTemplate;

  @RequestMapping("/hi")
  public String hi(@RequestParam(value="name", defaultValue="Artaban") String name) {
    String greeting = this.restTemplate.getForObject("http://say-hello/greeting", String.class);
    return String.format("%s, %s!", greeting, name);
  }

  public static void main(String[] args) {
    SpringApplication.run(UserApplication.class, args);
  }
}
```

在`UserApplication`类中我们做了一些关联的修改。`RestTemplate`现在使用`LoadBalanced`标记；这样做可以告诉Spring Cloud我们想使用其负载均衡的功能（在此例中由Ribbon提供）。此类被注解为`@RibbonClient`，我们指定了客户端的名称`name`为`say-hello`，并且另一个类包含了此客户端的额外的`configuration`配置。

We’ve made a couple of other related changes to the `UserApplication` class. Our `RestTemplate` is now also marked as `LoadBalanced`; this tells Spring Cloud that we want to take advantage of its load balancing support (provided, in this case, by Ribbon). The class is annotated with `@RibbonClient`, which we give the `name` of our client (`say-hello`) and then another class, which contains extra `configuration` for that client.

我们需要创建那个类。在`user/src/main/java/hello`目录中新增文件`SayHelloConfiguration.java`：

> We’ll need to create that class. Add a new file, `SayHelloConfiguration.java`, in the `user/src/main/java/hello` directory:

`user/src/main/java/hello/SayHelloConfiguration.java`

```java
package hello;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;

import com.netflix.client.config.IClientConfig;
import com.netflix.loadbalancer.IPing;
import com.netflix.loadbalancer.IRule;
import com.netflix.loadbalancer.PingUrl;
import com.netflix.loadbalancer.AvailabilityFilteringRule;

public class SayHelloConfiguration {

  @Autowired
  IClientConfig ribbonClientConfig;

  @Bean
  public IPing ribbonPing(IClientConfig config) {
    return new PingUrl();
  }

  @Bean
  public IRule ribbonRule(IClientConfig config) {
    return new AvailabilityFilteringRule();
  }

}
```

我们可以通过创建相同名字bean，来覆写Spring Cloud Netflix提供的Ribbon相关的bean。这里我们覆写了供默认负载均衡器使用的`IPing`和`IRule`。默认的`IPing`是一个`NoOpPing`（并不是真的ping服务器实例，而是一致报告自身是稳定的），默认的`IRule`是一个`ZoneAvoidanceRule`（是为了避开有多数故障服务器的Amazon EC2区，因此可能本地环境较难重现）。

> We can override any Ribbon-related bean that Spring Cloud Netflix gives us by creating our own bean with the same name. Here, we override the `IPing` and `IRule` used by the default load balancer. The default `IPing` is a `NoOpPing` (which doesn’t actually ping server instances, instead always reporting that they’re stable), and the default `IRule` is a `ZoneAvoidanceRule` (which avoids the Amazon EC2 zone that has the most malfunctioning servers, and might thus be a bit difficult to try out in our local environment).

我们的`IPing`是一个`PingUrl`，将会ping一个URL来检查每个服务器的状态。Say Hello有一个方法映射了`/`路径；意味着Ribbon在ping一个运行Say Hello服务器时，将会获得一个HTTP 200响应。我们设置的`IRule`为`AvailabilityFilteringRule`，将使用Ribbon内嵌的断路器功能来过滤“断路”状态的服务器：如果在ping一个指定的服务器时失败，或者从一个服务器读取失败，Ribbon将认为此服务器“挂掉”，直到服务器重新正常响应。

> Our `IPing` is a `PingUrl`, which will ping a URL to check the status of each server. Say Hello has, as you’ll recall, a method mapped to the `/` path; that means that Ribbon will get an HTTP 200 response when it pings a running Say Hello server. The `IRule` we set up, the `AvailabilityFilteringRule`, will use Ribbon’s built-in circuit breaker functionality to filter out any servers in an “open-circuit” state: if a ping fails to connect to a given server, or if it gets a read failure for the server, Ribbon will consider that server “dead” until it begins to respond normally.

## 试一试

> Trying it out

使用Gradle运行Say Hello服务：

> Run the Say Hello service, using either Gradle:

```shell
$ ./gradlew bootRun
```

或者使用Maven：

> or Maven:

```shell
$ mvn spring-boot:run
```

使用Gradle在端口9092和9999上运行其他两个实例：

> Run other instances on ports 9092 and 9999, again using either Gradle:

```shell
$ SERVER_PORT=9092 ./gradlew bootRun
```

或者使用Maven：

> or Maven:

```shell
$ SERVER_PORT=9999 mvn spring-boot:run
```

然后启动User服务。访问`localhost:8888/hi`，然后查看Say Hello服务实例。你可以看见Ribbon每15秒ping一次：

> Then start up the User service. Access `localhost:8888/hi` and then watch the Say Hello service instances. You can see Ribbon’s pings arriving every 15 seconds:

```
2016-03-09 21:13:22.115  INFO 90046 --- [nio-8090-exec-1] hello.SayHelloApplication                : Access /
2016-03-09 21:13:22.629  INFO 90046 --- [nio-8090-exec-3] hello.SayHelloApplication                : Access /
```

到达User服务的请求将响应通过轮询调用的方式去请求运行中的Say Hello实例：

> And your requests to the User service should result in calls to Say Hello being spread across the running instances in round-robin form:

```
2016-03-09 21:15:28.915  INFO 90046 --- [nio-8090-exec-7] hello.SayHelloApplication                : Access /greeting
```

现在关掉一个Say Hello服务实例。一旦Ribbon无法ping通实例，认为其挂掉，你可以看见请求开始被均衡负载到剩下的实例上。

> Now shut down a Say Hello server instance. Once Ribbon has pinged the down instance and considers it down, you should see requests begin to be balanced across the remaining instances.

## 总结

> Summary

恭喜！你完成了开发一个可以执行客户端负载均衡的Spring应用。

> Congratulations! You’ve just developed a Spring application that performs client-side load balancing for calls to another application.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*