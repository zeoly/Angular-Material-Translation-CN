# 构建一个RESTful Web服务

> Building a RESTful Web Service

此教程将告诉你如何使用Spring创建一个"hello world"[RESTful web服务](https://spring.io/understanding/REST)。

> This guide walks you through the process of creating a "hello world" [RESTful web service](https://spring.io/understanding/REST) with Spring.

## 将要做什么

> What you’ll build

你将构建一个接受HTTP GET请求的服务：

> You’ll build a service that will accept HTTP GET requests at:

```
http://localhost:8080/greeting
```

并返回表示问候的一个[JSON](https://spring.io/understanding/JSON)响应。

> and respond with a [JSON](https://spring.io/understanding/JSON) representation of a greeting:

```
{"id":1,"content":"Hello, World!"}
```

也可以通过备选参数`name`来自定义问候：

> You can customize the greeting with an optional `name` parameter in the query string:

```
http://localhost:8080/greeting?name=User
```

`name`参数将覆写并替代响应中默认的"World"：

> The `name` parameter value overrides the default value of "World" and is reflected in the response:

```
{"id":1,"content":"Hello, User!"}
```

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

如果**从头开始**，可参考[使用Gradle构建](https://spring.io/guides/gs/rest-service/#scratch)。

> To **start from scratch**, move on to [Build with Gradle](https://spring.io/guides/gs/rest-service/#scratch).

如果要**跳过基本步骤**，按如下步骤：

> To **skip the basics**, do the following:

- [下载](https://github.com/spring-guides/gs-rest-service/archive/master.zip)并解压此教程的源码仓库，或者使用[Git](https://spring.io/understanding/Git)进行clone：`git clone https://github.com/spring-guides/gs-rest-service.git`
- cd into `gs-rest-service/initial`
- 跳至[创建一个资源表示类](https://spring.io/guides/gs/rest-service/#initial).

> - [Download](https://github.com/spring-guides/gs-rest-service/archive/master.zip) and unzip the source repository for this guide, or clone it using [Git](https://spring.io/understanding/Git): `git clone https://github.com/spring-guides/gs-rest-service.git`
> - cd into `gs-rest-service/initial`
> - Jump ahead to [Create a resource representation class](https://spring.io/guides/gs/rest-service/#initial).

**当你完成时**，你可以与`gs-rest-service/complete`中的代码对比检查。

> **When you’re finished**, you can check your results against the code in `gs-rest-service/complete`.

## 创建一个资源表示类

> Create a resource representation class

现在你已经配置好工程与构建系统，现在可以创建web服务了。

> Now that you’ve set up the project and build system, you can create your web service.

首先要想清楚服务交互。

> Begin the process by thinking about service interactions.

此服务将处理`/greeting`的`GET`请求，请求字符串中有个`name`可选参数。`GET`请求将返回一个`200 OK`响应的，响应体是表示问候的一个字符串。看起来应该如下：

> The service will handle `GET` requests for `/greeting`, optionally with a `name` parameter in the query string. The `GET` request should return a `200 OK` response with JSON in the body that represents a greeting. It should look something like this:

```
{
    "id": 1,
    "content": "Hello, World!"
}
```

`id`字段表示问候的唯一id，`content`表示问候的文本。

> The `id` field is a unique identifier for the greeting, and `content` is the textual representation of the greeting.

为了模型化此问候，你可以创建一个资源表示类。提供一个java对象类，包括字段、构造、`id`和`content`数据的访问等：

> To model the greeting representation, you create a resource representation class. Provide a plain old java object with fields, constructors, and accessors for the `id` and `content` data:

`src/main/java/hello/Greeting.java`

```java
package hello;

public class Greeting {

    private final long id;
    private final String content;

    public Greeting(long id, String content) {
        this.id = id;
        this.content = content;
    }

    public long getId() {
        return id;
    }

    public String getContent() {
        return content;
    }
}
```

在下面的步骤中，Spring将使用[Jackson JSON](http://wiki.fasterxml.com/JacksonHome)库把`Greeting`类型实例自动转换为JSON。

> As you see in steps below, Spring uses the [Jackson JSON](http://wiki.fasterxml.com/JacksonHome) library to automatically marshal instances of type `Greeting` into JSON.

接下来你将创建提供问候功能的资源控制器。

> Next you create the resource controller that will serve these greetings.

## 创建一个资源控制器

> Create a resource controller

使用Spring构建RESTful web服务，需要用一个控制器来处理HTTP请求。此组件可以使用`@RestController`注解来声明，下面的`GreetingController`处理`/greeting`的`GET`请求，并返回一个`Greeting`类实例：

> In Spring’s approach to building RESTful web services, HTTP requests are handled by a controller. These components are easily identified by the `@RestController` annotation, and the `GreetingController` below handles `GET` requests for `/greeting` by returning a new instance of the `Greeting` class:

`src/main/java/hello/GreetingController.java`

```java
package hello;

import java.util.concurrent.atomic.AtomicLong;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {

    private static final String template = "Hello, %s!";
    private final AtomicLong counter = new AtomicLong();

    @RequestMapping("/greeting")
    public Greeting greeting(@RequestParam(value="name", defaultValue="World") String name) {
        return new Greeting(counter.incrementAndGet(),
                            String.format(template, name));
    }
}
```

此控制器虽然比较简洁且简单，但后台也有大量的细节。让我们来一步步分解。

> This controller is concise and simple, but there’s plenty going on under the hood. Let’s break it down step by step.

`@RequestMapping`注解确保`/greeting`的HTTP请求映射到`greeting()`方法上。

> The `@RequestMapping` annotation ensures that HTTP requests to `/greeting` are mapped to the `greeting()` method.

上面的例子并没有指定是`GET`，`PUT`, `POST`等，因为`@RequestMapping`默认映射所有的HTTTP操作。使用`@RequestMapping(method=GET)`来限定其映射。

> The above example does not specify `GET` vs. `PUT`, `POST`, and so forth, because `@RequestMapping` maps all HTTP operations by default. Use `@RequestMapping(method=GET)` to narrow this mapping.

`@RequestParam`将请求中的字符串参数`name`绑定到`greeting()`方法中的`name`参数上。如果请求中没有`name`参数，则将使用指定的默认值`defaultValue`的"World"。

> `@RequestParam` binds the value of the query string parameter `name` into the `name` parameter of the `greeting()` method. If the `name` parameter is absent in the request, the `defaultValue` of "World" is used.

方法体的实现中创建并返回了一个新的`Greeting`对象，其中包含`id`和`content`属性，值分别为`counter`对象的下一个值，和使用问候`template`来格式化指定的`name`。

> The implementation of the method body creates and returns a new `Greeting` object with `id` and `content` attributes based on the next value from the `counter`, and formats the given `name` by using the greeting `template`.

传统MVC控制器和RESTful web服务控制器的一个主要区别是HTTP响应体的创建方式。不同于依赖于[视图技术](https://spring.io/understanding/view-templates)使用服务端渲染将问候数据转为HTML，RESTful web服务控制器简单地填充并返回一个`Greeting`对象。对象数据将直接通过HTTP响应输出为JSON。

> A key difference between a traditional MVC controller and the RESTful web service controller above is the way that the HTTP response body is created. Rather than relying on a [view technology](https://spring.io/understanding/view-templates) to perform server-side rendering of the greeting data to HTML, this RESTful web service controller simply populates and returns a `Greeting` object. The object data will be written directly to the HTTP response as JSON.

上面代码使用了Spring 4中新的注解[`@RestController`](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/bind/annotation/RestController.html)，标记此类为控制类并且每个方法都返回对象而不是视图。相当于`@Controller`和`@ResponseBody`的合并简化版。

> This code uses Spring 4’s new [`@RestController`](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/bind/annotation/RestController.html) annotation, which marks the class as a controller where every method returns a domain object instead of a view. It’s shorthand for `@Controller` and `@ResponseBody` rolled together.

`Greeting`对象必须转换为JSON。因为Spring的HTTP消息转换器的支持，你不需要手动转换。因为Jackson 2在classpath中，将自动选择Spring的[`MappingJackson2HttpMessageConverter`](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/http/converter/json/MappingJackson2HttpMessageConverter.html)把`Greeting`实例转换为JSON。

> The `Greeting` object must be converted to JSON. Thanks to Spring’s HTTP message converter support, you don’t need to do this conversion manually. Because Jackson 2 is on the classpath, Spring’s [`MappingJackson2HttpMessageConverter`](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/http/converter/json/MappingJackson2HttpMessageConverter.html) is automatically chosen to convert the `Greeting` instance to JSON.

## 让应用可执行

> Make the application executable

虽然可以将服务打包作为传统的[WAR](https://spring.io/understanding/WAR)包，并部署在一个外部应用服务上，但下面展示的更简单的方法是创建一个独立的应用。你可以将所有东西打包为一个独立的可执行的JAR包，由Java `main()`方法驱动。这样一来，就代替了使用外部实例进行部署，转而使用Spring支持的内嵌[Tomcat](https://spring.io/understanding/Tomcat) servlet容器作为HTTP运行时。

> Although it is possible to package this service as a traditional [WAR](https://spring.io/understanding/WAR) file for deployment to an external application server, the simpler approach demonstrated below creates a standalone application. You package everything in a single, executable JAR file, driven by a good old Java `main()` method. Along the way, you use Spring’s support for embedding the [Tomcat](https://spring.io/understanding/Tomcat) servlet container as the HTTP runtime, instead of deploying to an external instance.

`src/main/java/hello/Application.java`

```java
package hello;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

`@SpringBootApplication`是一个方便的注解，可以添加如下：

> `@SpringBootApplication` is a convenience annotation that adds all of the following:

- `@Configuration`标记此类作为应用上下文的bean定义来源。
- `@EnableAutoConfiguration`告诉Spring Boot基于classpath设置、其他bean或者各种属性设置来开始添加bean。
- 通常你会在Spring MVC应用中添加`@EnableWebMvc`，如果在classpath中发现**spring-webmvc**，则Spring Boot会自动添加。此注解会标记应用为web应用，并激活一些关键行为例如设置一个`DispatcherServlet`。
- `@ComponentScan`告诉Spring在`hello`包中寻找其他组件、配置、服务等，也允许找到controllers。

> - `@Configuration` tags the class as a source of bean definitions for the application context.
> - `@EnableAutoConfiguration` tells Spring Boot to start adding beans based on classpath settings, other beans, and various property settings.
> - Normally you would add `@EnableWebMvc` for a Spring MVC app, but Spring Boot adds it automatically when it sees **spring-webmvc** on the classpath. This flags the application as a web application and activates key behaviors such as setting up a `DispatcherServlet`.
> - `@ComponentScan` tells Spring to look for other components, configurations, and services in the `hello` package, allowing it to find the controllers.

`main()`方法中使用了Spring Boot的`SpringApplication.run()`方法来加载一个应用。你注意到了没有任何XML吗？同样也没有**web.xml**文件。此web应用是100%纯Java并且你不要处理任何基础架构方面的配置。

> The `main()` method uses Spring Boot’s `SpringApplication.run()` method to launch an application. Did you notice that there wasn’t a single line of XML? No **web.xml** file either. This web application is 100% pure Java and you didn’t have to deal with configuring any plumbing or infrastructure.

## 创建一个可执行的JAR

> Build an executable JAR

你可以使用Gradle或者Maven命令行来运行此应用。或者你可以构建一个包含所有必须依赖、类、资源的独立可执行JAR文件，并运行它。这样可以更简单地在整个部署声明周期中，包括不同的环境等，将此服务作为应用来迁移、版本管理以及部署。

> You can run the application from the command line with Gradle or Maven. Or you can build a single executable JAR file that contains all the necessary dependencies, classes, and resources, and run that. This makes it easy to ship, version, and deploy the service as an application throughout the development lifecycle, across different environments, and so forth.

如果使用Gradle，可以使用`./gradlew bootRun`来运行应用。或者可以使用`./gradlew build`构建JAR文件。然后可以运行此JAR文件：

> If you are using Gradle, you can run the application using `./gradlew bootRun`. Or you can build the JAR file using `./gradlew build`. Then you can run the JAR file:

```shell
java -jar build/libs/gs-rest-service-0.1.0.jar
```

如果使用Maven，可以使用`./mvnw spring-boot:run`来运行应用。或者可以使用`./mvnw clean package`构建JAR文件。然后可以运行此JAR文件：

> If you are using Maven, you can run the application using `./mvnw spring-boot:run`. Or you can build the JAR file with `./mvnw clean package`. Then you can run the JAR file:

```shell
java -jar target/gs-rest-service-0.1.0.jar
```

上述过程将创建一个可运行的JAR。你也可以选择构建一个传统的WAR文件。

> The procedure above will create a runnable JAR. You can also opt to build a classic WAR file instead.

然后将显示日志输出。服务将在几秒内被建立并运行。

> Logging output is displayed. The service should be up and running within a few seconds.

## 测试此服务

> Test the service

现在服务已经就绪，通过[http://localhost:8080/greeting](http://localhost:8080/greeting)访问，将看见如下输出：

> Now that the service is up, visit [http://localhost:8080/greeting](http://localhost:8080/greeting), where you see:

```
{"id":1,"content":"Hello, World!"}
```

在请求字符串参数中提供一个`name`，如[http://localhost:8080/greeting?name=User](http://localhost:8080/greeting?name=User)。注意`content`属性的值由"Hello, World!"变为了"Hello User!"：

> Provide a `name` query string parameter with [http://localhost:8080/greeting?name=User](http://localhost:8080/greeting?name=User). Notice how the value of the `content` attribute changes from "Hello, World!" to "Hello User!":

```
{"id":2,"content":"Hello, User!"}
```

此变化意味着`GreetingController`中的`@RequestParam`正如预期一样运行。`name`指定了默认值为"World"，可以通过请求字符串被明确覆写。

> This change demonstrates that the `@RequestParam` arrangement in `GreetingController` is working as expected. The `name` parameter has been given a default value of "World", but can always be explicitly overridden through the query string.

注意`id`属性由`1`变为了`2`。证明了同一个`GreetingController`实例承接了多个请求，并且其`counter`字段也正如预期一样随着每次调用而增加。

> Notice also how the `id` attribute has changed from `1` to `2`. This proves that you are working against the same `GreetingController` instance across multiple requests, and that its `counter` field is being incremented on each call as expected.

## 总结

> Summary

恭喜！你已经完成了使用Spring来进行RESTful web服务的开发。

> Congratulations! You’ve just developed a RESTful web service with Spring.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*