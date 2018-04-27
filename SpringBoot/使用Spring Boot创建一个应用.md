# 使用Spring Boot创建一个应用

> Building an Application with Spring Boot

此教程提供了一个如何使用[Spring Boot](https://github.com/spring-projects/spring-boot)来帮助你更好地进行应用开发的例子。随着你阅读Spring入门教程，你会发现Spring Boot的更多用法。所以这里只是Spring Boot的浅尝。如果想创建基于Spring Boot的项目，查看[Spring Initializr](https://start.spring.io/)，填入项目信息，选择你的选项，然后就可以下载Maven版本的文件，或者打成zip包的项目工程。

> This guide provides a sampling of how [Spring Boot](https://github.com/spring-projects/spring-boot) helps you accelerate and facilitate application development. As you read more Spring Getting Started guides, you will see more use cases for Spring Boot. It is meant to give you a quick taste of Spring Boot. If you want to create your own Spring Boot-based project, visit [Spring Initializr](https://start.spring.io/), fill in your project details, pick your options, and you can download either a Maven build file, or a bundled up project as a zip file.

## 将要做什么

> What you’ll build

你将使用Spring Boot创建一个简单的web应用，并添加一些有用的服务。

> You’ll build a simple web application with Spring Boot and add some useful services to it.

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

如果**从头开始**，可参考[使用Gradle构建](https://spring.io/guides/gs/spring-boot/#scratch)。

> To **start from scratch**, move on to [Build with Gradle](https://spring.io/guides/gs/spring-boot/#scratch).

如果要**跳过基本步骤**，按如下步骤：

> To **skip the basics**, do the following:

- [下载](https://github.com/spring-guides/gs-spring-boot/archive/master.zip)并解压此教程的源码仓库，或者使用[Git](https://spring.io/understanding/Git)进行clone：`git clone https://github.com/spring-guides/gs-spring-boot.git`
- 进入`gs-spring-boot/initial`目录
- 跳至[[initial]](https://spring.io/guides/gs/spring-boot/#initial).

> - [Download](https://github.com/spring-guides/gs-spring-boot/archive/master.zip) and unzip the source repository for this guide, or clone it using [Git](https://spring.io/understanding/Git): `git clone https://github.com/spring-guides/gs-spring-boot.git`
> - cd into `gs-spring-boot/initial`
> - Jump ahead to [[initial]](https://spring.io/guides/gs/spring-boot/#initial).

**当你完成时**，你可以与`gs-spring-boot/complete`中的代码对比检查。

> **When you’re finished**, you can check your results against the code in `gs-spring-boot/complete`.

## 使用Maven构建

> Build with Maven

首先要设置一个基础构建脚本。你可以使用你喜欢的任何构建系统来构建Spring应用，与[Maven](https://maven.apache.org/)协作的代码如下。如果不熟悉Maven，可以参考[使用Maven构建Java项目](https://spring.io/guides/gs/maven)。

> First you set up a basic build script. You can use any build system you like when building apps with Spring, but the code you need to work with [Maven](https://maven.apache.org/) is included here. If you’re not familiar with Maven, refer to [Building Java Projects with Maven](https://spring.io/guides/gs/maven).

### 建立目录结构

> Create the directory structure

在项目路径下，创建如下子目录结构；例如，在*nix系统中使用命令`mkdir -p src/main/java/hello`：

> In a project directory of your choosing, create the following subdirectory structure; for example, with `mkdir -p src/main/java/hello` on *nix systems:

```
└── src
    └── main
        └── java
            └── hello
```

`pom.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>gs-spring-boot</artifactId>
    <version>0.1.0</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.1.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <properties>
        <java.version>1.8</java.version>
    </properties>


    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

[Spring Boot Maven plugin](https://docs.spring.io/spring-boot/docs/current/maven-plugin) 提供了许多便捷的特性：

> The [Spring Boot Maven plugin](https://docs.spring.io/spring-boot/docs/current/maven-plugin) provides many convenient features:

- 在classpath中集合所有jar包并构建一个独立可运行的"über-jar"，可以更方便的执行并传输你的服务。
- 会搜索public static void main()方法来标记为一个可运行的类。
- 提供了一个内建依赖解决方案，设置版本号并匹配[Spring Boot依赖](https://github.com/spring-projects/spring-boot/blob/master/spring-boot-project/spring-boot-dependencies/pom.xml)。你也可以覆写任何版本，但默认为Boot选择的版本集合。

> - It collects all the jars on the classpath and builds a single, runnable "über-jar", which makes it more convenient to execute and transport your service.
> - It searches for the public static void main() method to flag as a runnable class.
> - It provides a built-in dependency resolver that sets the version number to match [Spring Boot dependencies](https://github.com/spring-projects/spring-boot/blob/master/spring-boot-project/spring-boot-dependencies/pom.xml). You can override any version you wish, but it will default to Boot’s chosen set of versions.

## 学习如何使用Spring Boot

> Learn what you can do with Spring Boot

Spring Boot提供了快速构建应用的方式。会搜索你的classpath与你配置的bean，且合理推断缺失的部分并自动添加。使用Spring Boot你可以更关注与业务特性而不是基础架构。

> Spring Boot offers a fast way to build applications. It looks at your classpath and at beans you have configured, makes reasonable assumptions about what you’re missing, and adds it. With Spring Boot you can focus more on business features and less on infrastructure.

例如：

> For example:

- 使用Spring MVC？你可能总是需要几个特别的bean，Spring Boot可以自动加载它们。一个Spring MVC应用还需要一个servlet容器，所以Spring Boot还会自动配置内嵌的Tomcat。
- 使用Jetty？这样的话你可能不需要Tomcat，而是要内嵌的Jetty。Spring Boot同样可以帮你处理。
- 使用Thymeleaf？在你应用的上下文中总有几个bean必须添加；Spring Boot也会帮你添加进去。

> - Got Spring MVC? There are several specific beans you almost always need, and Spring Boot adds them automatically. A Spring MVC app also needs a servlet container, so Spring Boot automatically configures embedded Tomcat.
> - Got Jetty? If so, you probably do NOT want Tomcat, but instead embedded Jetty. Spring Boot handles that for you.
> - Got Thymeleaf? There are a few beans that must always be added to your application context; Spring Boot adds them for you.

以上只是Spring Boot提供的自动配置的几个例子。同时Spring Boot并不会妨碍你。例如如果Thymeleaf在你的路径下，Spring Boot在你的应用上下文中添加了`SpringTemplateEngine`。但如果你用自己的设置定义了自己的`SpringTemplateEngine`，Spring Boot就不会添加它。所以也有部分功能是你可以控制的。

> These are just a few examples of the automatic configuration Spring Boot provides. At the same time, Spring Boot doesn’t get in your way. For example, if Thymeleaf is on your path, Spring Boot adds a `SpringTemplateEngine` to your application context automatically. But if you define your own `SpringTemplateEngine` with your own settings, then Spring Boot won’t add one. This leaves you in control with little effort on your part.

Spring Boot并不生成代码或者修改你的文件，当你建立你的应用时，Spring Boot动态的绑定bean并设置和应用到你的应用上下文中。

> Spring Boot doesn’t generate code or make edits to your files. Instead, when you start up your application, Spring Boot dynamically wires up beans and settings and applies them to your application context.

## 创建一个简单的web应用

> Create a simple web application

现在你可以创建一个简单的web应用的web controller。

> Now you can create a web controller for a simple web application.

`src/main/java/hello/HelloController.java`

```java
package hello;

import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;

@RestController
public class HelloController {

    @RequestMapping("/")
    public String index() {
        return "Greetings from Spring Boot!";
    }

}
```

此类标记为一个`@RestController`，意思是让Spring MVC处理web请求。`@RequestMapping`映射`/`到`index()`方法。当从浏览器调用或者命令行使用curl时，此方法将返回纯文本。因为`@RestController`合并了`@Controller`和`@ResponseBody`，两个注解使web请求返回数据而不是视图。

> The class is flagged as a `@RestController`, meaning it’s ready for use by Spring MVC to handle web requests. `@RequestMapping` maps `/` to the `index()` method. When invoked from a browser or using curl on the command line, the method returns pure text. That’s because `@RestController` combines `@Controller` and `@ResponseBody`, two annotations that results in web requests returning data rather than a view.

## 创建一个应用类

> Create an Application class

你需要在组件中创建一个应用类：

> Here you create an Application class with the components:

`src/main/java/hello/Application.java`

```java
package hello;

import java.util.Arrays;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Bean
    public CommandLineRunner commandLineRunner(ApplicationContext ctx) {
        return args -> {

            System.out.println("Let's inspect the beans provided by Spring Boot:");

            String[] beanNames = ctx.getBeanDefinitionNames();
            Arrays.sort(beanNames);
            for (String beanName : beanNames) {
                System.out.println(beanName);
            }

        };
    }

}
```

`@SpringBootApplication`是一个便捷的注解，添加了如下内容：

> `@SpringBootApplication` is a convenience annotation that adds all of the following:

- `@Configuration`将此类标记为应用上下文的bean定义的源文件。
- `@EnableAutoConfiguration`告诉Spring Boot基于classpath设置、其他bean或者各种属性设置来开始添加bean。
- 通常你会在Spring MVC应用中添加`@EnableWebMvc`，如果在classpath中发现**spring-webmvc**，则Spring Boot会自动添加。此注解会标记应用为web应用，并激活一些关键行为例如设置一个`DispatcherServlet`。
- `@ComponentScan`告诉Spring在`hello`包中寻找其他组件、配置、服务等，也允许找到controllers。

> - `@Configuration` tags the class as a source of bean definitions for the application context.
> - `@EnableAutoConfiguration` tells Spring Boot to start adding beans based on classpath settings, other beans, and various property settings.
> - Normally you would add `@EnableWebMvc` for a Spring MVC app, but Spring Boot adds it automatically when it sees **spring-webmvc** on the classpath. This flags the application as a web application and activates key behaviors such as setting up a `DispatcherServlet`.
> - `@ComponentScan` tells Spring to look for other components, configurations, and services in the `hello` package, allowing it to find the controllers.

`main()`方法中使用了Spring Boot的`SpringApplication.run()`方法来加载一个应用。你注意到了没有任何XML吗？同样也没有**web.xml**文件。此web应用是100%纯Java并且你不要处理任何基础架构方面的配置。

> The `main()` method uses Spring Boot’s `SpringApplication.run()` method to launch an application. Did you notice that there wasn’t a single line of XML? No **web.xml** file either. This web application is 100% pure Java and you didn’t have to deal with configuring any plumbing or infrastructure.

这里有一个`CommandLineRunner`方法被标记了`@Bean`，并且会在启动时运行。它将获取所有应用创建或者Spring Boot自动添加的bean。并在排序之后打印出来。

> There is also a `CommandLineRunner` method marked as a `@Bean` and this runs on start up. It retrieves all the beans that were created either by your app or were automatically added thanks to Spring Boot. It sorts them and prints them out.

## 运行应用

> Run the application

执行以下命令来运行应用：

> To run the application, execute:

`./gradlew build && java -jar build/libs/gs-spring-boot-0.1.0.jar`

如果使用的Maven，则执行：

> If you are using Maven, execute:

`mvn package && java -jar target/gs-spring-boot-0.1.0.jar`

你可以看见如下输出：

> You should see some output like this:

```
Let's inspect the beans provided by Spring Boot:
application
beanNameHandlerMapping
defaultServletHandlerMapping
dispatcherServlet
embeddedServletContainerCustomizerBeanPostProcessor
handlerExceptionResolver
helloController
httpRequestHandlerAdapter
messageSource
mvcContentNegotiationManager
mvcConversionService
mvcValidator
org.springframework.boot.autoconfigure.MessageSourceAutoConfiguration
org.springframework.boot.autoconfigure.PropertyPlaceholderAutoConfiguration
org.springframework.boot.autoconfigure.web.EmbeddedServletContainerAutoConfiguration
org.springframework.boot.autoconfigure.web.EmbeddedServletContainerAutoConfiguration$DispatcherServletConfiguration
org.springframework.boot.autoconfigure.web.EmbeddedServletContainerAutoConfiguration$EmbeddedTomcat
org.springframework.boot.autoconfigure.web.ServerPropertiesAutoConfiguration
org.springframework.boot.context.embedded.properties.ServerProperties
org.springframework.context.annotation.ConfigurationClassPostProcessor.enhancedConfigurationProcessor
org.springframework.context.annotation.ConfigurationClassPostProcessor.importAwareProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalRequiredAnnotationProcessor
org.springframework.web.servlet.config.annotation.DelegatingWebMvcConfiguration
propertySourcesBinder
propertySourcesPlaceholderConfigurer
requestMappingHandlerAdapter
requestMappingHandlerMapping
resourceHandlerMapping
simpleControllerHandlerAdapter
tomcatEmbeddedServletContainerFactory
viewControllerHandlerMapping
```

你可以清楚地看见**org.springframework.boot.autoconfigure** bean。同样还有一个`tomcatEmbeddedServletContainerFactory`。

> You can clearly see **org.springframework.boot.autoconfigure** beans. There is also a `tomcatEmbeddedServletContainerFactory`.

验证一下服务。

> Check out the service.

```shell
$ curl localhost:8080
Greetings from Spring Boot!
```

## 添加单元测试

> Add Unit Tests

你可能希望为你添加的端点添加一个测试，Spring Test已经为此提供了一些机制，并且很容易加入到项目中。

> You will want to add a test for the endpoint you added, and Spring Test already provides some machinery for that, and it’s easy to include in your project.

将以下添加进你的构建文件的依赖列表中：

> Add this to your build file’s list of dependencies:

`testCompile("org.springframework.boot:spring-boot-starter-test")`

如果使用的是Maven，将以下添加进依赖列表中：

> If you are using Maven, add this to your list of dependencies:

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
```

现在写一个简单的单元测试，通过端点模拟servlet请求和响应。

> Now write a simple unit test that mocks the servlet request and response through your endpoint:

`src/test/java/hello/HelloControllerTest.java`

```java
package hello;

import static org.hamcrest.Matchers.equalTo;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.MediaType;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;

@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
public class HelloControllerTest {

    @Autowired
    private MockMvc mvc;

    @Test
    public void getHello() throws Exception {
        mvc.perform(MockMvcRequestBuilders.get("/").accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andExpect(content().string(equalTo("Greetings from Spring Boot!")));
    }
}
```

Spring Test中的`MockMvc`允许你通过一系列便捷构建类，来放松HTTP请求到`DispatcherServlet`中，并为结果设置断言。注意使用`@AutoConfigureMockMvc`和`@SpringBootTest`来注入一个`MockMvc`实例。使用了`@SpringBootTest`后，我们可以获取整个应用的上下文。另一个方法是使用`@WebMvcTest`让Spring Boot来创建上下文的web层。Spring Boot会自动尝试定位应用中的主应用类，你可以覆写它，或者缩小范围，来构建一些不同的事。

> The `MockMvc` comes from Spring Test and allows you, via a set of convenient builder classes, to send HTTP requests into the `DispatcherServlet` and make assertions about the result. Note the use of the `@AutoConfigureMockMvc` together with `@SpringBootTest` to inject a `MockMvc` instance. Having used `@SpringBootTest` we are asking for the whole application context to be created. An alternative would be to ask Spring Boot to create only the web layers of the context using the `@WebMvcTest`. Spring Boot automatically tries to locate the main application class of your application in either case, but you can override it, or narrow it down, if you want to build something different.

我们也可以使用Spring Boot来写入一个简单的全栈集成测试，来模拟HTTP请求循环。例如，替换上述的模拟测试，我们可以按如下做：

> As well as mocking the HTTP request cycle we can also use Spring Boot to write a very simple full-stack integration test. For example, instead of (or as well as) the mock test above we could do this:

`src/test/java/hello/HelloControllerIT.java`

```java
package hello;

import static org.hamcrest.Matchers.*;
import static org.junit.Assert.*;

import java.net.URL;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.boot.web.server.LocalServerPort;
import org.springframework.http.ResponseEntity;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class HelloControllerIT {

    @LocalServerPort
    private int port;

    private URL base;

    @Autowired
    private TestRestTemplate template;

    @Before
    public void setUp() throws Exception {
        this.base = new URL("http://localhost:" + port + "/");
    }

    @Test
    public void getHello() throws Exception {
        ResponseEntity<String> response = template.getForEntity(base.toString(),
                String.class);
        assertThat(response.getBody(), equalTo("Greetings from Spring Boot!"));
    }
}
```

内嵌的服务器因为`webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT`，使用一个随机的端口启动。实际的端口可通过`@LocalServerPort`实时获取。

> The embedded server is started up on a random port by virtue of the `webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT` and the actual port is discovered at runtime with the `@LocalServerPort`.

## 添加生产级别服务

> Add production-grade services

如果你正在为你的业务构建网站，你可能需要添加一些管理服务。Spring Boot在[actuator module](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#production-ready)中提供了一些现成的东西。例如健康、审计、bean等等。

> If you are building a web site for your business, you probably need to add some management services. Spring Boot provides several out of the box with its [actuator module](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#production-ready), such as health, audits, beans, and more.

将以下加入构建文件中的依赖列表中：

> Add this to your build file’s list of dependencies:

```
compile("org.springframework.boot:spring-boot-starter-actuator")
```

如果使用Maven，将如下加入依赖列表中：

> If you are using Maven, add this to your list of dependencies:

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
```

然后重启应用：

> Then restart the app:

```shell
./gradlew build && java -jar build/libs/gs-spring-boot-0.1.0.jar
```

如果使用Mavan，则使用如下命令：

> If you are using Maven, execute:

```shell
mvn package && java -jar target/gs-spring-boot-0.1.0.jar
```

你将看见有新的一组RESTful端点加入到应用中。这些是Spring Boot提供的管理服务。

> You will see a new set of RESTful end points added to the application. These are management services provided by Spring Boot.

```
2018-03-17 15:42:20.088  ... : Mapped "{[/error],produces=[text/html]}" onto public org.s...
2018-03-17 15:42:20.089  ... : Mapped "{[/error]}" onto public org.springframework.http.R...
2018-03-17 15:42:20.121  ... : Mapped URL path [/webjars/**] onto handler of type [class ...
2018-03-17 15:42:20.121  ... : Mapped URL path [/**] onto handler of type [class org.spri...
2018-03-17 15:42:20.157  ... : Mapped URL path [/**/favicon.ico] onto handler of type [cl...
2018-03-17 15:42:20.488  ... : Mapped "{[/actuator/health],methods=[GET],produces=[application/vnd...
2018-03-17 15:42:20.490  ... : Mapped "{[/actuator/info],methods=[GET],produces=[application/vnd.s...
2018-03-17 15:42:20.491  ... : Mapped "{[/actuator],methods=[GET],produces=[application/vnd.spring...
```

其中包括：errors, [actuator/health](http://localhost:8080/actuator/health), [actuator/info](http://localhost:8080/actuator/info), [actuator](http://localhost:8080/actuator)。

> They include: errors, [actuator/health](http://localhost:8080/actuator/health), [actuator/info](http://localhost:8080/actuator/info), [actuator](http://localhost:8080/actuator).

还有一个`/actuator/shutdown`端点，但默认只通过JMX可见。为了[将其作为HTTP端点开启](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#production-ready-endpoints-enabling-endpoints)，需要在`application.properties`文件中添加`management.endpoints.shutdown.enabled=true`。

> There is also a `/actuator/shutdown` endpoint, but it’s only visible by default via JMX. To [enable it as an HTTP endpoint](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#production-ready-endpoints-enabling-endpoints), add `management.endpoints.shutdown.enabled=true` to your `application.properties` file.

可以较容易的检查应用的健康。

> It’s easy to check the health of the app.

```shell
$ curl localhost:8080/actuator/health
{"status":"UP"}
```

可以通过curl来尝试调用关闭。

> You can try to invoke shutdown through curl.

```shell
$ curl -X POST localhost:8080/actuator/shutdown
{"timestamp":1401820343710,"error":"Method Not Allowed","status":405,"message":"Request method 'POST' not supported"}
```

因为我们还没有开启，该请求因为不存在而被拦截。

> Because we didn’t enable it, the request is blocked by the virtue of not existing.

你可以在[端点文档](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#production-ready-endpoints)中查看每个REST端点的详细信息以及如何在`application.properties`文件中进行配置。

> For more details about each of these REST points and how you can tune their settings with an `application.properties` file, you can read detailed [docs about the endpoints](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#production-ready-endpoints).

## 查看Spring Boot的starter

> View Spring Boot’s starters

以上你已经看到了一些[Spring Boot的"starter"](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#using-boot-starter)。你可以在此查看其[所有源代码](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-project/spring-boot-starters)。

> You have seen some of [Spring Boot’s "starters"](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#using-boot-starter). You can see them all [here in source code](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-project/spring-boot-starters).

## JAR支持与Groovy支持

> JAR support and Groovy support

最后一个例子是展示Spring Boot如何在你没有意识到的时候简单的绑定bean。也展示了如何开启便捷管理服务。

> The last example showed how Spring Boot makes it easy to wire beans you may not be aware that you need. And it showed how to turn on convenient management services.

Spring Boot还可以做到更多。不仅支持传统的WAR文件部署，还可以简单的利用Spring Boot的loader模块来打包可执行的JAR。多个教程都通过`spring-boot-gradle-plugin`和`spring-boot-maven-plugin`展示了这种双重支持。

> But Spring Boot does yet more. It supports not only traditional WAR file deployments, but also makes it easy to put together executable JARs thanks to Spring Boot’s loader module. The various guides demonstrate this dual support through the `spring-boot-gradle-plugin` and `spring-boot-maven-plugin`.

Spring Boot也支持Groovy，允许你最少用一个文件构建Spring MVC web应用。

> On top of that, Spring Boot also has Groovy support, allowing you to build Spring MVC web apps with as little as a single file.

创建一个名为**app.groovy**的新文件，并加入如下代码：

> Create a new file called **app.groovy** and put the following code in it:

```groovy
@RestController
class ThisWillActuallyRun {

    @RequestMapping("/")
    String home() {
        return "Hello World!"
    }

}
```

文件放置在哪里都可以。甚至可以适应[单个tweet](https://twitter.com/rob_winch/status/364871658483351552)中的应用！

> It doesn’t matter where the file is. You can even fit an application that small inside a [single tweet](https://twitter.com/rob_winch/status/364871658483351552)!

接下来安装[Spring Boot’s CLI](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#getting-started-installing-the-cli)。

> Next, install [Spring Boot’s CLI](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#getting-started-installing-the-cli).

并按如下运行：

> Run it as follows:

```shell
$ spring run app.groovy
```

在此假定你已经关闭了前一个应用，以避免端口冲突。

> This assumes you shut down the previous application, to avoid a port collision.

在另一个终端窗口运行如下：

> From a different terminal window:

```shell
$ curl localhost:8080
Hello World!
```

Spring Boot是动态地在代码中添加关键注解，并使用[Groovy Grape](http://groovy.codehaus.org/Grape)来获取运行所需的库。

> Spring Boot does this by dynamically adding key annotations to your code and using [Groovy Grape](http://groovy.codehaus.org/Grape) to pull down libraries needed to make the app run.

## 总结

> Summary

恭喜！你已经使用Spring Boot构建了一个简单的web应用，并学习了如何使用其来加快开发效率。同样还开启了一些即用的生产服务。这只是一个Spring Boot功能的小样例。可以查看[Spring Boot在线文档](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle)进行深入学习。

> Congratulations! You built a simple web application with Spring Boot and learned how it can ramp up your development pace. You also turned on some handy production services. This is only a small sampling of what Spring Boot can do. Checkout [Spring Boot’s online docs](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle) if you want to dig deeper.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*