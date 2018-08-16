# 使用Spring缓存数据

> Caching Data with Spring

通过此教程你将完成在Spring管理的bean上启用缓存。

> This guide walks you through the process of enabling caching on a Spring managed bean.

## 将要做什么

> What you’ll build

你将构建一个应用，并在一个简单的book书籍库上启用缓存。

> You’ll build an application that enables caching on a simple book repository.

`pom.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>gs-caching</artifactId>
    <version>0.1.0</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.3.RELEASE</version>
    </parent>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-cache</artifactId>
        </dependency>
    </dependencies>

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

## 创建一个书籍库

> Create a book repository

首先创建一个简单的书籍模型

> First, let’s create a very simple model for your book

`src/main/java/hello/Book.java`

```java
package hello;

public class Book {

    private String isbn;
    private String title;

    public Book(String isbn, String title) {
        this.isbn = isbn;
        this.title = title;
    }

    public String getIsbn() {
        return isbn;
    }

    public void setIsbn(String isbn) {
        this.isbn = isbn;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    @Override
    public String toString() {
        return "Book{" + "isbn='" + isbn + '\'' + ", title='" + title + '\'' + '}';
    }

}
```

再为此模型创建对应的仓库：

> And a repository for that model:

`src/main/java/hello/BookRepository.java`

```java
package hello;

public interface BookRepository {

    Book getByIsbn(String isbn);

}
```

你可能已经使用过[Spring Data](https://projects.spring.io/spring-data/)来提供一个仓库实现，来提供SQL或者NoSQL等的存储，但此教程只需要你使用一个简单的实现来模拟延迟（例如网络服务，缓慢延迟等）。

> You could have used [Spring Data](https://projects.spring.io/spring-data/) to provide an implementation of your repository over a wide range of SQL or NoSQL stores, but for the purpose of this guide, you will simply use a naive implementation that simulates some latency (network service, slow delay, etc).

`src/main/java/hello/SimpleBookRepository.java`

```java
package hello;

import org.springframework.stereotype.Component;

@Component
public class SimpleBookRepository implements BookRepository {

    @Override
    public Book getByIsbn(String isbn) {
        simulateSlowService();
        return new Book(isbn, "Some book");
    }

    // Don't do this at home
    private void simulateSlowService() {
        try {
            long time = 3000L;
            Thread.sleep(time);
        } catch (InterruptedException e) {
            throw new IllegalStateException(e);
        }
    }

}
```

在`simulateSlowService`的每次`getByIsbn`调用中故意插入了一个3秒的延迟。这是一个举例，稍后你可以使用缓存加速。

> `simulateSlowService` is deliberately inserting a three second delay into each `getByIsbn` call. This is an example that later on, you’ll speed up with caching.

## 使用仓库

> Using the repository

下一步对接仓库并用来获取书籍数据。

> Next, wire up the repository and use it to access some books.

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

在`BookRepository`中注入`CommandLineRunner`，然后使用不同的参数进行多次调用。

> There is also a `CommandLineRunner` that injects the `BookRepository` and calls it several times with different arguments.

`src/main/java/hello/AppRunner.java`

```java
package hello;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

@Component
public class AppRunner implements CommandLineRunner {

    private static final Logger logger = LoggerFactory.getLogger(AppRunner.class);

    private final BookRepository bookRepository;

    public AppRunner(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }

    @Override
    public void run(String... args) throws Exception {
        logger.info(".... Fetching books");
        logger.info("isbn-1234 -->" + bookRepository.getByIsbn("isbn-1234"));
        logger.info("isbn-4567 -->" + bookRepository.getByIsbn("isbn-4567"));
        logger.info("isbn-1234 -->" + bookRepository.getByIsbn("isbn-1234"));
        logger.info("isbn-4567 -->" + bookRepository.getByIsbn("isbn-4567"));
        logger.info("isbn-1234 -->" + bookRepository.getByIsbn("isbn-1234"));
        logger.info("isbn-1234 -->" + bookRepository.getByIsbn("isbn-1234"));
    }

}
```

如果你尝试使用此端点运行应用，尽管有几次是获取同样的书籍，你会发现还是会相当慢。

> If you try to run the application at this point, you’ll notice it’s quite slow even though you are retrieving the exact same book several times.

```
2014-06-05 12:15:35.783  ... : .... Fetching books
2014-06-05 12:15:40.783  ... : isbn-1234 -->Book{isbn='isbn-1234', title='Some book'}
2014-06-05 12:15:43.784  ... : isbn-1234 -->Book{isbn='isbn-1234', title='Some book'}
2014-06-05 12:15:46.786  ... : isbn-1234 -->Book{isbn='isbn-1234', title='Some book'}
```

从时间戳可以看出，尽管获取的都是一样的，每次获取大概需要3秒。

> As can be seen by the timestamps, each book took about three seconds to retrieve, even though it’s the same title being repeatedly fetched.

## 开启缓存

> Enable caching

现在在`SimpleBookRepository`中开启缓存，则书籍信息可被缓存在`books`缓存中。

> Let’s enable caching on your `SimpleBookRepository` so that the books are cached within the `books` cache.

`src/main/java/hello/SimpleBookRepository.java`

```java
package hello;

import org.springframework.cache.annotation.Cacheable;
import org.springframework.stereotype.Component;

@Component
public class SimpleBookRepository implements BookRepository {

    @Override
    @Cacheable("books")
    public Book getByIsbn(String isbn) {
        simulateSlowService();
        return new Book(isbn, "Some book");
    }

    // Don't do this at home
    private void simulateSlowService() {
        try {
            long time = 3000L;
            Thread.sleep(time);
        } catch (InterruptedException e) {
            throw new IllegalStateException(e);
        }
    }

}
```

现在你需要使用缓存注解来开启功能。

> You now need to enable the processing of the caching annotations

`src/main/java/hello/Application.java`

```java
package hello;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cache.annotation.EnableCaching;

@SpringBootApplication
@EnableCaching
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```

`@EnableCaching`注解的作用是检测每个有缓存注解的公共方法的Spring bean的后处理器。如果发现了缓存注解，将自动创建一个代理来拦截方法的调用并使用缓存。

> The `@EnableCaching` annotation triggers a post processor that inspects every Spring bean for the presence of caching annotations on public methods. If such an annotation is found, a proxy is automatically created to intercept the method call and handle the caching behavior accordingly.

此后处理器管理的注解为`Cacheable`, `CachePut`和`CacheEvict`。你可以参考javadoc以及[文档](https://docs.spring.io/spring/docs/current/spring-framework-reference/html/cache.html)获取更多细节信息。

> The annotations that are managed by this post processor are `Cacheable`, `CachePut` and `CacheEvict`. You can refer to the javadocs and [the documentation](https://docs.spring.io/spring/docs/current/spring-framework-reference/html/cache.html) for more details.

Spring Boot会自动配置一个合适的`CacheManager`来作为相关缓存的提供服务。参考[Spring Boot文档](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-caching.html)来获取更多细节信息。

> Spring Boot automatically configures a suitable `CacheManager` to serve as a provider for the relevant cache. See [the Spring Boot documentation](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-caching.html) for more details.

在本例中并没有使用一个具体的缓存库，所以缓存将使用`ConcurrentHashMap`进行存储。此存储抽象支持许多存储库并且完全符合JSR-107 (JCache)。

> Our sample does not use a specific caching library so our cache store is the simple fallback that uses `ConcurrentHashMap`. The caching abstraction supports a wide range of cache library and is fully compliant with JSR-107 (JCache).

## 测试应用

> Test the application

现在已经开启了缓存，再次执行后可以看见各类isbn的调用。与之前有较大区别

> Now that caching is enabled, you can execute it again and see the difference by adding additional calls with or without the same isbn. It should make a huge difference.

```
2016-09-01 11:12:47.033  .. : .... Fetching books
2016-09-01 11:12:50.039  .. : isbn-1234 -->Book{isbn='isbn-1234', title='Some book'}
2016-09-01 11:12:53.044  .. : isbn-4567 -->Book{isbn='isbn-4567', title='Some book'}
2016-09-01 11:12:53.045  .. : isbn-1234 -->Book{isbn='isbn-1234', title='Some book'}
2016-09-01 11:12:53.045  .. : isbn-4567 -->Book{isbn='isbn-4567', title='Some book'}
2016-09-01 11:12:53.045  .. : isbn-1234 -->Book{isbn='isbn-1234', title='Some book'}
2016-09-01 11:12:53.045  .. : isbn-1234 -->Book{isbn='isbn-1234', title='Some book'}
```

在控制台的输出可以看到第一次获取书籍时需要3秒，而后面的调用几乎是瞬间完成。

> This excerpt from the console shows that the first time to fetch each title took three seconds, but each subsequent call was near instantaneous.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*