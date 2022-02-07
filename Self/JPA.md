# JPA

[TOC]

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

### 主键定义

1. 主键需要使用`@Id`注解声明，并且需要使用`@GeneratedValue`声明主键生成方式，生成方式有4中策略：
  
   1. **AUTO**策略，为自动选择主键生成策略，是默认的生成策略。因为可控性较差，一般不建议采用：
     
      ```java
      @Id
      @GeneratedValue(strategy = GenerationType.AUTO)
      private Long id;
      ```
   
   2. **IDENTITY**策略，使用数据库自增序列，注意oracle并不支持此种主键。
     
      ```java
      @Id
      @GeneratedValue(strategy = GenerationType.IDENTITY)
      private Long id;
      ```
   
   3. **SEQUENCE**策略，需要显式声明序列。通常序列自增为1，需要指定`allocationSize=1`（默认为50，与数据库序列可能不对应）。
     
      ```java
      @Id
      @GeneratedValue(strategy = GenerationType.SEQUENCE, generator="my_seq")  
      @SequenceGenerator(name="my_seq", sequenceName="seq_customer", allocationSize = 1)
      private Long id;
      ```
   
   4. **TABLE**策略，通过数据库表的方式存储并维护主键，一般不常使用。
     
      ```java
      
      ```

2. 分布式系统常用主键也有选择UUID的，需要使用自定义主键生成策略。可以声明hibernate的uuid生成器，并在主键上使用此生成器：

```java
@GenericGenerator(name = "my_uuid", strategy = "uuid")
public  class Customer {

  @Id
  @GeneratedValue(generator = "my_uuid")
  private String id;
}
```

> hibernate自带的主键生成器如下：

```java
public  DefaultIdentifierGeneratorFactory() {
  register( "uuid2", UUIDGenerator.class);
  register( "guid", GUIDGenerator.class);
  register( "uuid", UUIDHexGenerator.class);
  register( "uuid.hex", UUIDHexGenerator.class);
  register( "hilo", TableHiLoGenerator.class);
  register( "assigned", Assigned.class);
  register( "identity", IdentityGenerator.class);
  register( "select", SelectGenerator.class);
  register( "sequence", SequenceGenerator.class);
  register( "seqhilo", SequenceHiLoGenerator.class);
  register( "increment", IncrementGenerator.class);
  register( "foreign", ForeignGenerator.class);
  register( "sequence-identity", SequenceIdentityGenerator.class);
  register( "enhanced-sequence", SequenceStyleGenerator.class);
  register( "enhanced-table", TableGenerator.class);
}
```

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

命名操作是jpa非常大的一个优势，仅需按照规范定义接口，就可以直接操作数据库，例如：

```java
public class UserRepository extends JpaRepository<User, Long> {

  User findByName(String name);
}
```

上述接口即定义了一个根据姓名查询用户的方法，即等于：

```sql
select * from user where name = ?
```

具体命名规范如下：

#### 查询主体

| 关键词                                                                  | 说明                        |
| -------------------------------------------------------------------- | ------------------------- |
| find...By, read...By, get...By, query...By, search...By, stream...By | 返回集合                      |
| exists...By                                                          | 返回布尔类型`boolean`           |
| count...By                                                           | 返回数字类型                    |
| delete..By, remove...By                                              | 删除操作可以返回空`void`，也可以返回删除数量 |
| ...First<number>..., ...Top<number>...                               | 限制结果集数量                   |
| ...Distinct...                                                       | 返回唯一数据                    |

#### 查询条件

| 关键词                | 例子                                                      | 等效sql                                          |
| ------------------ | ------------------------------------------------------- | ---------------------------------------------- |
| And                | findByLastnameAndFirstname                              | ... where x.lastname = ?1 and x.firstname = ?2 |
| Or                 | findByLastnameOrFirstname                               | ... where x.lastname = ?1 or x.firstname = ?2  |
| Is, Equals         | findByFirstname,findByFirstnameIs,findByFirstnameEquals | ... where x.firstname = ?1                     |
| Between            | findByStartDateBetween                                  | ... where x.startDate between ?1 and ?2        |
| LessThan           | findByAgeLessThan                                       | ... where x.age < ?1                           |
| LessThanEqual      | findByAgeLessThanEqual                                  | ... where x.age <= ?1                          |
| GreaterThan        | findByAgeGreaterThan                                    | ... where x.age > ?1                           |
| GreaterThanEqual   | findByAgeGreaterThanEqual                               | ... where x.age >= ?1                          |
| After              | findByStartDateAfter                                    | ... where x.startDate > ?1                     |
| Before             | findByStartDateBefore                                   | ... where x.startDate < ?1                     |
| IsNull, Null       | findByAge(Is)Null                                       | ... where x.age is null                        |
| IsNotNull, NotNull | findByAge(Is)NotNull                                    | ... where x.age not null                       |
| Like               | findByFirstnameLike                                     | ... where x.firstname like ?1                  |
| NotLike            | findByFirstnameNotLike                                  | ... where x.firstname not like ?1              |
| StartingWith       | findByFirstnameStartingWith                             | ... where x.firstname like ?1 (参数结尾绑定%)        |
| EndingWith         | findByFirstnameEndingWith                               | ... where x.firstname like ?1 (参数开头绑定%)        |
| Containing         | findByFirstnameContaining                               | ... where x.firstname like ?1 (参数两端绑定%)        |
| OrderBy            | findByAgeOrderByLastnameDesc                            | ... where x.age = ?1 order by x.lastname desc  |
| Not                | findByLastnameNot                                       | ... where x.lastname <> ?1                     |
| In                 | findByAgeIn(Collection<Age> ages)                       | ... where x.age in ?1                          |
| NotIn              | findByAgeNotIn(Collection<Age> ages)                    | ... where x.age not in ?1                      |
| True               | findByActiveTrue()                                      | ... where x.active = true                      |
| False              | findByActiveFalse()                                     | ... where x.active = false                     |
| IgnoreCase         | findByFirstnameIgnoreCase                               | ... where UPPER(x.firstname) = UPPER(?1)       |

#### 查询修饰

| 关键词                            | 说明      |
| ------------------------------ | ------- |
| IgnoreCase, IgnoringCase       | 忽略大小写   |
| AllIgnoreCase, AllIgnoringCase | 全局忽略大小写 |
| OrderBy...                     | 排序      |

### Query

第二种操作数据的方法为写`@Query`注解，注解中可以使用hql，也可以使用原生的sql：

```java
@Query("SELECT n FROM Note n WHERE n.featured = true")
List<Note> findByActiveNotes();
```

原生方式需要指定`nativeQuery`为`true`：

```java
@Query(value = "SELECT * FROM Notes n WHERE n.featured = 1", nativeQuery = true)
List<Note> findByFeaturedNotesNative();
```

### Specifications

这种方式适合复杂查询（例如and/or混合，命名不方便逻辑分层）、动态sql（根据入参可能有不同的查询逻辑，命名可能会写成多个，并通过逻辑分支调用不同的接口），手动去拼接查询条件。使用时，多继承一个接口即可：

```java
public interface CustomerRepository extends JpaRepository<Customer, Long>, JpaSpecificationExecutor<Customer>
```

`JpaSpecificationExecutor`这个接口中有提供一些方法，比如：

- Optional<T> findOne(Secification<T> spec);
- List<T> findAll(Secification<T> spec);
- Page<T> findAll(Specification<T> spec, Pageable pageable);
- List<T> findAll(Secification<T> spec, Sort sort);
- long count(Secification<T> spec);

可以看到关键参数均为`Specification<T>`，所以查询的重点就是实现接口`Specification<T>`中的方法`Predicate toPredicate(Root<T> root, CriteriaQuery<?> query, CriteriaBuilder criteriaBuilder)`。三个参数的意思是：

- `Root`：表示from语句中的实体
- `CriteriaQuery`：表示顶级查询逻辑，例如groupBy/having/orderBy/distinc等等
- `CriteriaBuilder`：用于构建查询，例如and/or/sum/avg/like/abs等等，包含逻辑与函数

```java

```

### 修改操作

修改操作数据jpa的一个“劣势”，通常来说，一般是通过某些条件，将需要修改的对象查出，并修改对象后，再将对象进行存储，例如：

```java
User user = userRepository.findById(id);
user.setName(newName);
userRepository.save(user);
```

其实这是种很好的思路，脱离开sql的禁锢想法，聚焦在对象上。但在某些场景下，可能也会有局限性，所以也有其他修改数据的方式：

```java
@Modifying
@Query("update User u set u.firstname = ?1 where u.lastname = ?2")
int setFixedFirstnameFor(String firstname, String lastname);
```

通过使用`@Query`注解，手写jpql的方式来实现更新操作，类似还可以使用原生sql来实现：

```java
@Modifying
@Query("update user_table u set u.first_name = ?1 where u.last_name = ?2", nativeQuery = true)
int setFixedFirstnameFor(String firstname, String lastname);
```

### 分页查询

分页查询也是常见业务场景，在命名查询中可以使用`Pageable`对象来封装：

```java
Page<User> findByLastname(String lastname, Pageable pageable);
```

如果只关注列表数据本身，也可以按如下定义接口：

```java
List<User> findByLastname(String lastname, Pageable pageable);
```

使用也比较简单，直接声明分页关键参数即可：

```java
Page<User> users = userRepository.findByLastname(lastName, PageRequest.of(1, 20));
```

### 批量操作

使用`JpaRepository`的`saveAll`方法可批量插入，但默认为单条提交，如果想实现真正批量提交，需要如下操作：

1. 在`application.properties`文件中增加配置项：
  
   ```properties
   spring.jpa.properties.hibernate.jdbc.batch_size=500
   spring.jpa.properties.hibernate.jdbc.batch_versioned_data=true
   spring.jpa.properties.hibernate.order_inserts=true
   spring.jpa.properties.hibernate.order_updates=true
   ```
   
   但通过生成的sql日志无法看出是否批量，依旧会分为多个单条insert显示日志，需要增加以下配置：
   
   ```properties
   spring.jpa.properties.hibernate.generate_statistics=true
   ```
   
   然后可以通过日志看到单批次提交量。

2. 需确定模型定义中主键的生成类型，如果为`identity`，则批量插入不会生效，原文为：[Hibernate disables insert batching at the JDBC level transparently if you use an identity identifier generator.](https://docs.jboss.org/hibernate/orm/4.3/manual/en-US/html/ch15.html)。所以需要显式配置id生成策略，例如：
  
   ```java
   public class Customer {
   
       @Id
       @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "my_gen")
       @SequenceGenerator(name = "my_gen", sequenceName = "table_customer_seq")
       private Long id;
   }
   ```

## 关系映射

关系映射是Jpa，或者说是Hibernate的一个特性。在模型定义时，有的模型不仅仅是平的简单模型，可能是复杂的嵌套模型，则在表与模型的对应处理时，可使用此项特性来构建模型。

### 四个主要的关系映射注解

关系映射中的M对N，是指**模型**对**属性**，

#### @OneToOne, @ManyToOne

一对一，一般用于扩展关系，例如**Person模型**与其**Address属性**的对应，一个人有一个地址，地址是个复杂对象，且不具备复用的特点；

多对一，多用于包含关系，例如**Person模型**与其**Class属性**的对应，多个人属于一个班级，且此班级为同一个可复用对象；

#### @OneToMany

一对多，多用于上述多对一的反向映射；

#### @ManyToMany

多对多，常见于两个对象的匹配关联关系，例如**Person模型**和**Role模型**，一个人可以有多个角色，且不同的人也可能对应同一个角色；

### 使用1

在需要映射属性上使用上述注解即可：

```java
@Entity
public class Person {
    
    @OneToOne
    private Address address;
}
```

但按目前的表设计来说，有可能不直接使用主键外联或外键的方式，来建立表与表之间的关系。所以通常还需要使用`@JoinColumn`注解，再配置具体的表关联字段：

```java
@Entity
public class Person {
    
    @OneToOne
    @JoinColumn(name = "address_code", referencedColumnName = "code")
    private Address address;
}
```

> `@JoinColumn`注解中有两个重要参数：
> `name`表示在此对象中，对应的关联字段；
> `referencedColumnName`表示在属性对应的对象中，与`name`对应的字段；

### 使用2

另一个是**多对多**的场景，即有一个中间表管理两个对象的关联关系，需要使用`@JoinTable`来指定中间表：

```java
@Entity
public class Person {
    
    @ManyToMany
    @JoinTable(name = "t_relation", joinColumns = @JoinColumn(name = "person_code", referencedColumnName = "code"),
            inverseJoinColumns = @JoinColumn(name = "role_id", referencedColumnName = "id"))
    private List<Role> roleList;
}
```

> 此处的`joinColumns`和`inverseJoinColumns`的主体对象都是指中间表，所以两个`name`属性对应的字段均为中间表的字段，而`referencedColumnName`对应的则是两个对象表中对应的字段；

**反向映射**

对象与属性的映射同样存在反向的场景，以上述**使用2**为例，在角色中可以带出对应的人，例如：

```java
@Entity
public class Role {
    
    @ManyToMany(mappedBy = "roleList")
    private List<Person> personList;
}
```

> 反向映射中的被控方，只需要配置主控方对应的字段即可

### 加载策略

在四种对应关系中，参数`fetch`对应的默认值有不同，`@ManyToOne`和`@OneToOne`的默认值是`FetchType.EAGER`，而`@OneToMany`和`@ManyToMany`的默认值是`FetchType.LAZY`

## 常用项

### sql打印

因为jpa为自动生成sql，在分析问题时可能需要看具体的sql。

```properties
# 开启jpa sql日志，为单行形式
spring.jpa.show-sql=true

# 格式化日志输出，便于阅读
spring.jpa.properties.hibernate.format_sql=true
```

如上配置后，只有sql，如果还需要查看每次sql执行时的参数，需要将对应日志级别调低，将trace日志输出。

```properties
# 修改日志等级
logging.level.org.hibernate.type.descriptor.sql.BasicBinder=trace
```

## 其他常用注解

### @DynamicInsert

当对象属性非空，才会生成在sql中，一般用于使用数据库default值

### @DynamicUpdate

当对象属性值有改变时，才会生成在sql中
