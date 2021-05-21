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
  @GeneratedValue(strategy=GenerationType.AUTO)
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

