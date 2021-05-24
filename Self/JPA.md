# JPA

[toc]

## 快速入门

引入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

模型类增加注解

1. 增加`@Entity注解`，可通过`@Table`声明表名；
2. 在主键增加`@Id`以及`@GeneratedValue`声明主键与生成方式；
3. 可通过`@Column`声明列名；

```java
@Entity
@Table(name = "customer")
public class Customer {

  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  
  @Column(name = "first_name")
  private String firstName;
  
  private String lastName;
```

增加操作接口

1. 继承`JpaRepository`接口，可进行基本操作；
2. 可通过命名方式，实现其他个性化操作；

```java
public interface CustomerRepository extends JpaRepository<Customer, Long> {

  List<Customer> findByLastName(String lastName);

  Customer findById(long id);
}
```

## 模型定义

### 基类定义

通常业务模型有基类，对应表中有公共的字段，需要通过继承方式定义自己的业务类。使用`@MappedSuperclass`注解：

```java
@MappedSuperclass
public class BaseModel {

  private String createBy;
  
  private Date createDate;
}
```

### 字段排除

并不一定所有类定义都与表字段完全对应，存在可能忽略某些字段的场景。使用`@Transient`注解声明忽略某些在表中并不存在的字段：

```java
public class Customer {

  @Transient
  private Integer page;
  
  @Transient
  private Integer size;
}
```

### 字段统一转换

某些场景可能需要在数据库层统一处理入参，使用`@ColumnTrasformer`注解统一处理：

```java
public class Customer {

  @ColumnTransformer(read = "UPPER(user_name)", write = "UPPER(?)")
  private String userName;
}
```

## 操作接口

### 命名操作

### 批量操作

使用`JpaRepository`的`saveAll`方法可批量插入，但默认为单条提交，如果想实现真正批量提交，需要如下操作：

1. 在`application.properties`文件中增加配置项：

   ```properties
   spring.jpa.properties.hibernate.jdbc.batch_size=500
   spring.jpa.properties.hibernate.jdbc.batch_versioned_data=true
   spring.jpa.properties.hibernate.order_inserts=true
   spring.jpa.properties.hibernate.order_updates=true
   ```

2. 需确定模型定义中主键的生成类型，如果为`identity`，则批量插入不会生效，原文为：[Hibernate disables insert batching at the JDBC level transparently if you use an identity identifier generator.](https://docs.jboss.org/hibernate/orm/4.3/manual/en-US/html/ch15.html)。所以需要显式配置id生成策略，例如：

   ```java
   public class Customer {
       
       @Id
       @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "my_gen")
       @SequenceGenerator(name = "my_gen", sequenceName = "table_customer_seq")
       private Long id;
   }
   ```

   

