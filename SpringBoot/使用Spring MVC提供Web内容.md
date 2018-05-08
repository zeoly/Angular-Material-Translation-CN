# 使用Spring MVC提供Web内容

> Serving Web Content with Spring MVC

你将通过此教程使用Spring创建一个"hello world"网站。

> This guide walks you through the process of creating a "hello world" web site with Spring.

## 将要做什么

> What you’ll build

你将构建一个拥有静态主页的应用，并且接收如下HTTP GET请求：

> You’ll build an application that has a static home page, and also will accept HTTP GET requests at:

```
http://localhost:8080/greeting
```

然后会响应一个HTML网页。HTML的body包含一个问候：

> and respond with a web page displaying HTML. The body of the HTML contains a greeting:

```
"Hello, World!"
```

也可以通过请求字符串的一个可选参数`name`来自定义问候：

> You can customize the greeting with an optional `name` parameter in the query string:

```
http://localhost:8080/greeting?name=User
```

`name`参数将覆写默认值"World"，并替换对应的响应：

> The `name` parameter value overrides the default value of "World" and is reflected in the response:

```
"Hello, User!"
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

如果**从头开始**，可参考[使用Gradle构建](https://spring.io/guides/gs/serving-web-content/#scratch)。

> To **start from scratch**, move on to [Build with Gradle](https://spring.io/guides/gs/serving-web-content/#scratch).

如果要**跳过基本步骤**，按如下步骤：

> To **skip the basics**, do the following:

- [下载](https://github.com/spring-guides/gs-serving-web-content/archive/master.zip)并解压此教程的源码仓库，或者使用[Git](https://spring.io/understanding/Git)进行clone：`git clone https://github.com/spring-guides/gs-serving-web-content.git`
- 进入`gs-serving-web-content/initial`目录
- 跳至[创建一个web控制器](https://spring.io/guides/gs/serving-web-content/#initial).

> - [Download](https://github.com/spring-guides/gs-serving-web-content/archive/master.zip) and unzip the source repository for this guide, or clone it using [Git](https://spring.io/understanding/Git): `git clone https://github.com/spring-guides/gs-serving-web-content.git`
> - cd into `gs-serving-web-content/initial`
> - Jump ahead to [Create a web controller](https://spring.io/guides/gs/serving-web-content/#initial).

**当你完成时**，你可以与`gs-serving-web-content/complete`中的代码对比检查。

> **When you’re finished**, you can check your results against the code in `gs-serving-web-content/complete`.

## 创建一个web控制器

> Create a web controller

使用Spring构建网站的话，需要用一个控制器来处理HTTP请求。你可以简单的使用[`@Controller`](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/stereotype/Controller.html)注解来识别请求，GreetingController处理/greeting的GET请求并返回一个[`视图`](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/servlet/View.html)名称，在此例中为"greeting"。一个`视图`将响应并渲染HTML内容：

> In Spring’s approach to building web sites, HTTP requests are handled by a controller. You can easily identify these requests by the [`@Controller`](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/stereotype/Controller.html) annotation. In the following example, the GreetingController handles GET requests for /greeting by returning the name of a [`View`](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/servlet/View.html), in this case, "greeting". A `View` is responsible for rendering the HTML content:

`src/main/java/hello/GreetingController.java`

```java
package hello;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class GreetingController {

    @GetMapping("/greeting")
    public String greeting(@RequestParam(name="name", required=false, defaultValue="World") String name, Model model) {
        model.addAttribute("name", name);
        return "greeting";
    }

}
```

此控制器简单明了，但也有很多细节。让我们逐步分解来说。

> This controller is concise and simple, but there’s plenty going on. Let’s break it down step by step.

`@GetMapping`注解确保`/greeting`的HTTP GET请求映射到`greeting()`方法上。

> The `@GetMapping` annotation ensures that HTTP GET requests to `/greeting` are mapped to the `greeting()` method.

使用[`@RequestParam`](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestParam.html)将请求字符串参数`name`绑定到`greeting()`方法的`name`参数上。此请求参数非必须`required`；如果请求中无此参数，将使用默认`defaultValue`的值"World"。`name`参数的值将添加至`Model`对象，使得视图模板可以访问它。

> [`@RequestParam`](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestParam.html) binds the value of the query String parameter `name` into the `name` parameter of the `greeting()` method. This query String parameter is not `required`; if it is absent in the request, the `defaultValue` of "World" is used. The value of the `name` parameter is added to a `Model` object, ultimately making it accessible to the view template.

方法体的实现依赖于一种[视图技术](https://spring.io/understanding/view-templates)，此例中是[Thymeleaf](http://www.thymeleaf.org/doc/tutorials/2.1/thymeleafspring.html)，来执行服务端渲染HTML。Thymeleaf解析下面的`greeting.html`模板，并执行`th:text`表达式来渲染之前执行器设置的`${name}`参数值。

> The implementation of the method body relies on a [view technology](https://spring.io/understanding/view-templates), in this case [Thymeleaf](http://www.thymeleaf.org/doc/tutorials/2.1/thymeleafspring.html), to perform server-side rendering of the HTML. Thymeleaf parses the `greeting.html` template below and evaluates the `th:text` expression to render the value of the `${name}` parameter that was set in the controller.

`src/main/resources/templates/greeting.html`

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Getting Started: Serving Web Content</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
    <p th:text="'Hello, ' + ${name} + '!'" />
</body>
</html>
```

## 开发网页应用

> Developing web apps

开发网页应用的一个常见场景是，编码实现，重启应用，刷新浏览器查看变化。整个过程消耗了大量时间。为了加速这种场景，Spring Boot中带了一个开箱即用的模块[spring-boot-devtools](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using-boot-devtools)。

> A common feature of developing web apps is coding a change, restarting your app, and refreshing the browser to view the change. This entire process can eat up a lot of time. To speed up the cycle of things, Spring Boot comes with a handy module known as [spring-boot-devtools](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using-boot-devtools).

- 启用[热部署](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto-hotswapping)
- 是模板引擎关闭缓存
- 启用LiveReload自动刷新浏览器
- 其他基于开发替换生产的默认功能

> - Enable [hot swapping](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto-hotswapping)
> - Switches template engines to disable caching
> - Enables LiveReload to refresh browser automatically
> - Other reasonable defaults based on development instead of production

## 使应用可执行

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
java -jar build/libs/gs-serving-web-content-0.1.0.jar
```

如果使用Maven，可以使用`./mvnw spring-boot:run`来运行应用。或者可以使用`./mvnw clean package`构建JAR文件。然后可以运行此JAR文件：

> If you are using Maven, you can run the application using `./mvnw spring-boot:run`. Or you can build the JAR file with `./mvnw clean package`. Then you can run the JAR file:

```shell
java -jar target/gs-serving-web-content-0.1.0.jar
```

上述过程将创建一个可运行的JAR。你也可以选择构建一个传统的WAR文件。

> The procedure above will create a runnable JAR. You can also opt to build a classic WAR file instead.

然后将显示日志输出。服务将在几秒内被建立并运行。

> Logging output is displayed. The service should be up and running within a few seconds.

## 测试此应用

> Test the App

现在网站已经就绪，通过[http://localhost:8080/greeting](http://localhost:8080/greeting)访问，将看见如下输出：

> Now that the web site is up, visit [http://localhost:8080/greeting](http://localhost:8080/greeting), where you see:

```
"Hello, World!"
```

在请求字符串参数中提供一个`name`，如[http://localhost:8080/greeting?name=User](http://localhost:8080/greeting?name=User)。注意消息由"Hello, World!"变为了"Hello User!"：

> Provide a `name` query string parameter with [http://localhost:8080/greeting?name=User](http://localhost:8080/greeting?name=User). Notice how the message changes from "Hello, World!" to "Hello User!":

```
"Hello, User!"
```

此变化意味着`GreetingController`中的`@RequestParam`正如预期一样运行。`name`指定了默认值为"World"，可以通过请求字符串被明确覆写。

> This change demonstrates that the `@RequestParam` arrangement in `GreetingController` is working as expected. The `name` parameter has been given a default value of "World", but can always be explicitly overridden through the query string.

## 添加一个主页

> Add a Home Page

例如HTML或JavaScript或CSS等静态资源，可以直接放入源码的合适位置，就可以在Spring Boot应用中起效。默认Spring Boot在classpath的"/static" (或者"/public")目录放置静态资源。`index.html`比较特殊，因为如果有这个文件的话，是被当作“欢迎页”，意味着是作为根资源，例如，在本例中的`http://localhost:8080/`，所以先创建此文件：

> Static resources, like HTML or JavaScript or CSS, can easily be served from your Spring Boot application just be dropping them into the right place in the source code. By default Spring Boot serves static content from resources in the classpath at "/static" (or "/public"). The `index.html` resource is special because it is used as a "welcome page" if it exists, which means it will be served up as the root resource, i.e. at `http://localhost:8080/` in our example. So create this file:

`src/main/resources/static/index.html`

```html
<!DOCTYPE HTML>
<html>
<head>
    <title>Getting Started: Serving Web Content</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
    <p>Get your greeting <a href="/greeting">here</a></p>
</body>
</html>
```

然后重启应用，你就可以在http://localhost:8080/看见这个HTML。

> and when you restart the app you will see the HTML at http://localhost:8080/.

## 总结

> Summary

恭喜！你已经完成使用Spring开发一个网页。

> Congratulations! You have just developed a web page using Spring.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*