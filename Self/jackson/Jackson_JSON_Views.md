[toc]

# Jackson JSON视图

## 使用JSON视图序列化

首先是最简单的例子，使用*@JsonView*做序列化。

先定义一个视图：

```java
public class Views {
    public static class Public {
    }
}
```

然后在*User*实体中使用：

```java
public class User {
    public int id;

    @JsonView(Views.Public.class)
    public String name;
}
```

然后使用视图来序列化：

```java
@Test
public void whenUseJsonViewToSerialize_thenCorrect() 
  throws JsonProcessingException {
 
    User user = new User(1, "John");

    ObjectMapper mapper = new ObjectMapper();
    mapper.disable(MapperFeature.DEFAULT_VIEW_INCLUSION);

    String result = mapper
      .writerWithView(Views.Public.class)
      .writeValueAsString(user);

    assertThat(result, containsString("John"));
    assertThat(result, not(containsString("1")));
}
```

由于指定了一个视图，则只有视图对应的字段被序列化。

需要特别注意的是，默认情况下，没有被视图标记的字段，都会被序列化，所以需要关闭*DEFAULT_VIEW_INCLUSION*

特性。

## 使用多个JSON视图

然后是使用多个视图的例子，每个视图对应不同的字段。视图*Internal*继承了*Public*：

```java
public class Views {
    public static class Public {
    }

    public static class Internal extends Public {
    }
}
```

实体*Item*中，只有*id*和*name*是*Public*视图：

```java
public class Item {
 
    @JsonView(Views.Public.class)
    public int id;

    @JsonView(Views.Public.class)
    public String itemName;

    @JsonView(Views.Internal.class)
    public String ownerName;
}
```

如果使用*Public*视图来序列化，只有*id*和*name*被序列化为JSON：

```java
@Test
public void whenUsePublicView_thenOnlyPublicSerialized() 
  throws JsonProcessingException {
 
    Item item = new Item(2, "book", "John");

    ObjectMapper mapper = new ObjectMapper();
    String result = mapper
      .writerWithView(Views.Public.class)
      .writeValueAsString(item);

    assertThat(result, containsString("book"));
    assertThat(result, containsString("2"));

    assertThat(result, not(containsString("John")));
}
```

如果使用*Internal*视图来序列化，则所有字段都会被序列化：

```java
@Test
public void whenUseInternalView_thenAllSerialized() 
  throws JsonProcessingException {
 
    Item item = new Item(2, "book", "John");

    ObjectMapper mapper = new ObjectMapper();
    String result = mapper
      .writerWithView(Views.Internal.class)
      .writeValueAsString(item);

    assertThat(result, containsString("book"));
    assertThat(result, containsString("2"));

    assertThat(result, containsString("John"));
}
```

## 使用JSON视图反序列化

同样可以使用JSON视图来反序列化对象：

```java
@Test
public void whenUseJsonViewToDeserialize_thenCorrect() 
  throws IOException {
    String json = "{"id":1,"name":"John"}";

    ObjectMapper mapper = new ObjectMapper();
    User user = mapper
      .readerWithView(Views.Public.class)
      .forType(User.class)
      .readValue(json);

    assertEquals(1, user.getId());
    assertEquals("John", user.getName());
}
```

## 自定义JSON视图

JSON视图也可以自定义，例如*User*实体的*name*字段在序列化时转大写。

可以使用*BeanPropertyWriter*和*BeanSerializerModifier*来自定义JSON视图，如下：

```java
public class UpperCasingWriter extends BeanPropertyWriter {
    BeanPropertyWriter _writer;

    public UpperCasingWriter(BeanPropertyWriter w) {
        super(w);
        _writer = w;
    }

    @Override
    public void serializeAsField(Object bean, JsonGenerator gen, 
      SerializerProvider prov) throws Exception {
        String value = ((User) bean).name;
        value = (value == null) ? "" : value.toUpperCase();
        gen.writeStringField("name", value);
    }
}
```

然后将name字段的*BeanPropertyWriter*设置为自定义的*UpperCasingWriter*：

```java
public class MyBeanSerializerModifier extends BeanSerializerModifier{

    @Override
    public List<BeanPropertyWriter> changeProperties(
      SerializationConfig config, BeanDescription beanDesc, 
      List<BeanPropertyWriter> beanProperties) {
        for (int i = 0; i < beanProperties.size(); i++) {
            BeanPropertyWriter writer = beanProperties.get(i);
            if (writer.getName() == "name") {
                beanProperties.set(i, new UpperCasingWriter(writer));
            }
        }
        return beanProperties;
    }
}
```

最后使用修改后的序列化器来序列化*User*实例：

```java
@Test
public void whenUseCustomJsonViewToSerialize_thenCorrect() 
  throws JsonProcessingException {
    User user = new User(1, "John");
    SerializerFactory serializerFactory = BeanSerializerFactory.instance
      .withSerializerModifier(new MyBeanSerializerModifier());

    ObjectMapper mapper = new ObjectMapper();
    mapper.setSerializerFactory(serializerFactory);

    String result = mapper
      .writerWithView(Views.Public.class)
      .writeValueAsString(user);

    assertThat(result, containsString("JOHN"));
    assertThat(result, containsString("1"));
}
```

## 在Spring中使用JSON视图

在**Spring框架**中也可以使用JSON视图，同样使用*@JsonView*注解来在API接口级别自定义JSON响应：

```java
@JsonView(Views.Public.class)
@RequestMapping("/items/{id}")
public Item getItemPublic(@PathVariable int id) {
    return ItemManager.getById(id);
}
```

响应为：

```json
{"id":2,"itemName":"book"}
```

如果使用*Internal*视图：

```java
@JsonView(Views.Internal.class)
@RequestMapping("/items/internal/{id}")
public Item getItemInternal(@PathVariable int id) {
    return ItemManager.getById(id);
}
```

则响应为：

```json
{"id":2,"itemName":"book","ownerName":"John"}
```

