# Messaging with RabbitMQ

通过此教程你将建立一个RabbitMQ AMQP服务器来发布和订阅消息。

> This guide walks you through the process of setting up a RabbitMQ AMQP server that publishes and subscribes to messages.

## 将要做什么

> What you’ll build

你将构建一个应用，使用Spring AMQP的`RabbitTemplate`来发布消息，然后使用`MessageListenerAdapter`在[POJO](https://spring.io/understanding/POJO)订阅此消息。

> You’ll build an application that publishes a message using Spring AMQP’s `RabbitTemplate` and subscribes to the message on a [POJO](https://spring.io/understanding/POJO) using `MessageListenerAdapter`.

`pom.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>gs-messaging-rabbitmq</artifactId>
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
            <artifactId>spring-boot-starter-amqp</artifactId>
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

## 设置RabbitMQ broker

> Set up RabbitMQ broker

在建立你的消息应用之前，你需要先设置一个处理接收发送消息的服务器。

> Before you can build your messaging application, you need to set up the server that will handle receiving and sending messages.

RabbitMQ是一个AMQP服务器。可在[http://www.rabbitmq.com/download.html](https://www.rabbitmq.com/download.html)获取。你可以手动下载，也可以使用Mac的homebrew：

> RabbitMQ is an AMQP server. The server is freely available at [http://www.rabbitmq.com/download.html](https://www.rabbitmq.com/download.html). You can download it manually, or if you are using a Mac with homebrew:

```
brew install rabbitmq
```

解压服务器后使用默认设置启动。

> Unpack the server and launch it with default settings.

```
rabbitmq-server
```

你可以看见如下信息：

> You should see something like this:

```
            RabbitMQ 3.1.3. Copyright (C) 2007-2013 VMware, Inc.
##  ##      Licensed under the MPL.  See http://www.rabbitmq.com/
##  ##
##########  Logs: /usr/local/var/log/rabbitmq/rabbit@localhost.log
######  ##        /usr/local/var/log/rabbitmq/rabbit@localhost-sasl.log
##########
            Starting broker... completed with 6 plugins.
```

如果你本地已经运行了docker，你可以使用[Docker Compose](https://docs.docker.com/compose/)来快速启动一个RabbitMQ服务器。在Github上的工程根目录有一个`docker-compose.yml`。内容很简单：

> You can also use [Docker Compose](https://docs.docker.com/compose/) to quickly launch a RabbitMQ server if you have docker running locally. There is a `docker-compose.yml` in the root of the "complete" project in Github. It is very simple:

```yml
rabbitmq:
  image: rabbitmq:management
  ports:
    - "5672:5672"
    - "15672:15672"
```

在当前文件夹中添加此文件，运行`docker-compose up`就可以在容器中运行RabbitMQ。

> With this file in the current directory you can run `docker-compose up` to get RabbitMQ running in a container.

## 创建一个RabbitMQ消息接收器

> Create a RabbitMQ message receiver

任何基于消息的应用，都需要先创建一个接收器来响应发布的消息。

> With any messaging-based application, you need to create a receiver that will respond to published messages.

`src/main/java/hello/Receiver.java`

```java
package hello;

import java.util.concurrent.CountDownLatch;
import org.springframework.stereotype.Component;

@Component
public class Receiver {

    private CountDownLatch latch = new CountDownLatch(1);

    public void receiveMessage(String message) {
        System.out.println("Received <" + message + ">");
        latch.countDown();
    }

    public CountDownLatch getLatch() {
        return latch;
    }

}
```

`Receiver`是一个简单的POJO类，定义了一个接收消息的方法。当你用来注册接收消息时，可以命名为任何名字。

> The `Receiver` is a simple POJO that defines a method for receiving messages. When you register it to receive messages, you can name it anything you want.

为了方便，此POJO类有一个`CountDownLatch`。当收到消息时将发出信号。在生产环境的应用一般不会这么使用。

> For convenience, this POJO also has a `CountDownLatch`. This allows it to signal that the message is received. This is something you are not likely to implement in a production application.

## 注册一个监听器并发送消息

> Register the listener and send a message

Spring AMQP的`RabbitTemplate`提供了对使用RabbitMQ发送和接收消息的一切支持。你需要特别的设置一下：

> Spring AMQP’s `RabbitTemplate` provides everything you need to send and receive messages with RabbitMQ. Specifically, you need to configure:

- 一个消息监听器容器
- 声明队列和交换，以及两者的绑定
- 一个发送消息的组件，用于测试监听器

> - A message listener container
> - Declare the queue, the exchange, and the binding between them
> - A component to send some messages to test the listener

Spring Boot会自动创建一个连接工厂和一个RabbitTemplate，减少要写的代码量。

> Spring Boot automatically creates a connection factory and a RabbitTemplate, reducing the amount of code you have to write.

你将使用`RabbitTemplate`来发送消息，并在消息监听器容器中注册一个`Receiver`来接收消息。连接工厂驱动二者，并允许他们连接到RabbitMQ服务器。

> You’ll use `RabbitTemplate` to send messages, and you will register a `Receiver` with the message listener container to receive messages. The connection factory drives both, allowing them to connect to the RabbitMQ server.

`src/main/java/hello/Application.java`

```java
package hello;

import org.springframework.amqp.core.Binding;
import org.springframework.amqp.core.BindingBuilder;
import org.springframework.amqp.core.Queue;
import org.springframework.amqp.core.TopicExchange;
import org.springframework.amqp.rabbit.connection.ConnectionFactory;
import org.springframework.amqp.rabbit.listener.SimpleMessageListenerContainer;
import org.springframework.amqp.rabbit.listener.adapter.MessageListenerAdapter;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class Application {

    static final String topicExchangeName = "spring-boot-exchange";

    static final String queueName = "spring-boot";

    @Bean
    Queue queue() {
        return new Queue(queueName, false);
    }

    @Bean
    TopicExchange exchange() {
        return new TopicExchange(topicExchangeName);
    }

    @Bean
    Binding binding(Queue queue, TopicExchange exchange) {
        return BindingBuilder.bind(queue).to(exchange).with("foo.bar.#");
    }

    @Bean
    SimpleMessageListenerContainer container(ConnectionFactory connectionFactory,
            MessageListenerAdapter listenerAdapter) {
        SimpleMessageListenerContainer container = new SimpleMessageListenerContainer();
        container.setConnectionFactory(connectionFactory);
        container.setQueueNames(queueName);
        container.setMessageListener(listenerAdapter);
        return container;
    }

    @Bean
    MessageListenerAdapter listenerAdapter(Receiver receiver) {
        return new MessageListenerAdapter(receiver, "receiveMessage");
    }

    public static void main(String[] args) throws InterruptedException {
        SpringApplication.run(Application.class, args).close();
    }

}
```

在`listenerAdapter()`方法中定义的bean在容器中作为一个消息监听器被注册，容器则在`container()`方法中定义。此监听器将会监听"spring-boot"队列中的消息。因为`Receiver`类是一个POJO类，所以须要在`MessageListenerAdapter`中封装，用来指定调用`receiveMessage`方法。

> The bean defined in the `listenerAdapter()` method is registered as a message listener in the container defined in `container()`. It will listen for messages on the "spring-boot" queue. Because the `Receiver` class is a POJO, it needs to be wrapped in the `MessageListenerAdapter`, where you specify it to invoke `receiveMessage`.

JMS队列和AMQP队列在语义上有不同。例如，JMS只发送排序的消息到一个消费者上。虽然AMQP也一样，但AMQP生产者不直接发送消息到队列，而是发到一个交换处，然后可以根据JMS主题的概念，转到单个队列中或者分列到多个多列中。可以查看[了解AMQP](https://spring.io/understanding/AMQP)来获取更多。

> JMS queues and AMQP queues have different semantics. For example, JMS sends queued messages to only one consumer. While AMQP queues do the same thing, AMQP producers don’t send messages directly to queues. Instead, a message is sent to an exchange, which can go to a single queue, or fanout to multiple queues, emulating the concept of JMS topics. For more, see [Understanding AMQP](https://spring.io/understanding/AMQP).

你需要消息监听器容器和接收器bean来监听消息。发送消息则需要Rabbit模板。

> The message listener container and receiver beans are all you need to listen for messages. To send a message, you also need a Rabbit template.

在`queue()`方法中创建了一个AMQP队列。`exchange()`方法创建了一个主体交换。`binding()`方法绑定了前两者，并在RabbitTemplate发布到一个交换时定义了发生的行为。

> The `queue()` method creates an AMQP queue. The `exchange()` method creates a topic exchange. The `binding()` method binds these two together, defining the behavior that occurs when RabbitTemplate publishes to an exchange.

Spring AMQP需要将`Queue`、`TopicExchange`和`Binding`作为顶级Spring bean进行声明，以便可以正常设置。

> Spring AMQP requires that the `Queue`, the `TopicExchange`, and the `Binding` be declared as top level Spring beans in order to be set up properly.

在这个例子中，我们使用了一个主题交换，并使用路由key`foo.bar.#`绑定了队列，意味着任何路由key以`foo.bar.`开头消息都会被路由到这个队列中。

> In this case, we use a topic exchange and the queue is bound with routing key `foo.bar.#` which means any message sent with a routing key beginning with `foo.bar.` will be routed to the queue.

## 发送测试消息

> Send a Test Message

使用`CommandLineRunner`来发送测试消息，同样需要等待接收器的latch并关闭应用上下文。

> Test messages are sent by a `CommandLineRunner`, which also waits for the latch in the receiver and closes the application context:

`src/main/java/hello/Runner.java`

```java
package hello;

import java.util.concurrent.TimeUnit;

import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

@Component
public class Runner implements CommandLineRunner {

    private final RabbitTemplate rabbitTemplate;
    private final Receiver receiver;

    public Runner(Receiver receiver, RabbitTemplate rabbitTemplate) {
        this.receiver = receiver;
        this.rabbitTemplate = rabbitTemplate;
    }

    @Override
    public void run(String... args) throws Exception {
        System.out.println("Sending message...");
        rabbitTemplate.convertAndSend(Application.topicExchangeName, "foo.bar.baz", "Hello from RabbitMQ!");
        receiver.getLatch().await(10000, TimeUnit.MILLISECONDS);
    }

}
```

注意模板将消息路由到了交换上，因为路由key`foo.bar.baz`匹配上了绑定。

> Notice that the template routes the message to the exchange, with a routing key of `foo.bar.baz` which matches the binding.

因为此运行器可以在测试中模拟，所以接收器可以单独测试。

> The runner can be mocked out in tests, so that the receiver can be tested in isolation.

## 运行应用

> Run the Application

`main()`方法启动进程并创建一个Spring应用上下文。这将会调起消息监听器容器并开始监听消息。还有一个`Runner` bean将自动执行：首先从应用上下文中获取`RabbitTemplate`并在"spring-boot"队列上发布一个消息"Hello from RabbitMQ!"。最后关闭Spring应用上下文结束。

> The `main()` method starts that process by creating a Spring application context. This starts the message listener container, which will start listening for messages. There is a `Runner` bean which is then automatically executed: it retrieves the `RabbitTemplate` from the application context and sends a "Hello from RabbitMQ!" message on the "spring-boot" queue. Finally, it closes the Spring application context and the application ends.

## 创建一个可执行JAR包

> Build an executable JAR

打包并执行，你将看见如下输出：

> You should see the following output:

```
Sending message...
Received <Hello from RabbitMQ!>
```

## 总结

> Summary

恭喜！你使用Spring和RabbitMQ完成了一个简单的发布和订阅应用。可以在查看[更多关于Spring和RabbitMQ](https://docs.spring.io/spring-amqp/reference/html/_introduction.html#quick-tour)。

> Congratulations! You’ve just developed a simple publish-and-subscribe application with Spring and RabbitMQ. There’s [more you can do with Spring and RabbitMQ](https://docs.spring.io/spring-amqp/reference/html/_introduction.html#quick-tour) than what is covered here, but this should provide a good start.

*翻译部分版权归YahaCode团队所有。仅供学习研究之用，任何组织或个人不得私自以此用于任何形式的商业目的*