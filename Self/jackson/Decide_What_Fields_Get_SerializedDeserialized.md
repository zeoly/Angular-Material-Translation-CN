[toc]

# 哪些字段会被序列化/反序列化

## public字段

想让一个字段被序列化/反序列化的最简单的方式，是以public声明：

```java
public class MyDtoAccessLevel {
    private String stringValue;
    int intValue;
    protected float floatValue;
    public boolean booleanValue;
    // NO setters or getters
}
```

在这些字段中，只有*booleanValue*默认参与了序列化：

```java
@Test
public void givenDifferentAccessLevels_whenPublic_thenSerializable() 
  throws JsonProcessingException {
    ObjectMapper mapper = new ObjectMapper();

    MyDtoAccessLevel dtoObject = new MyDtoAccessLevel();

    String dtoAsString = mapper.writeValueAsString(dtoObject);
    assertThat(dtoAsString, not(containsString("stringValue")));
    assertThat(dtoAsString, not(containsString("intValue")));
    assertThat(dtoAsString, not(containsString("floatValue")));

    assertThat(dtoAsString, containsString("booleanValue"));
}
```

## Getter方法可以让非public字段被序列化/反序列化

另一个简单的方法是对应字段增加一个getter方法：

```java
public class MyDtoWithGetter {
    private String stringValue;
    private int intValue;

    public String getStringValue() {
        return stringValue;
    }
}
```

则*stringValue*字段会被序列化，而其他private字段因为没有getter方法，不会被序列化：

```java
@Test
public void givenDifferentAccessLevels_whenGetterAdded_thenSerializable() 
  throws JsonProcessingException {
    ObjectMapper mapper = new ObjectMapper();

    MyDtoGetter dtoObject = new MyDtoGetter();

    String dtoAsString = mapper.writeValueAsString(dtoObject);
    assertThat(dtoAsString, containsString("stringValue"));
    assertThat(dtoAsString, not(containsString("intValue")));
```

getter方法同样会使字段被反序列化，因为字段一旦有getter方法，即表示对应上一个属性：

```java
@Test
public void givenDifferentAccessLevels_whenGetterAdded_thenDeserializable() 
  throws JsonProcessingException, JsonMappingException, IOException {
    String jsonAsString = "{\"stringValue\":\"dtoString\"}";
    ObjectMapper mapper = new ObjectMapper();
    MyDtoWithGetter dtoObject = mapper.readValue(jsonAsString, MyDtoWithGetter.class);

    assertThat(dtoObject.getStringValue(), equalTo("dtoString"));
}
```

## Setter方法只会使非public字段被反序列化

相对于getter方法可以使private字段序列化和反序列化都生效，setter方法只会使非public字段的反序列化生效：

```java
public class MyDtoWithSetter {
    private int intValue;

    public void setIntValue(int intValue) {
        this.intValue = intValue;
    }

    public int accessIntValue() {
        return intValue;
    }
}
```

private字段*intValue*只有一个setter方法，虽然另外有一个方法可以获取到对应的值，但不是一个标准的getter方法。如下，反序列化是正常的：

```java
@Test
public void givenDifferentAccessLevels_whenSetterAdded_thenDeserializable() 
  throws JsonProcessingException, JsonMappingException, IOException {
    String jsonAsString = "{\"intValue\":1}";
    ObjectMapper mapper = new ObjectMapper();

    MyDtoSetter dtoObject = mapper.readValue(jsonAsString, MyDtoSetter.class);

    assertThat(dtoObject.anotherGetIntValue(), equalTo(1));
}
```

但序列化是不起效的：

```java
@Test
public void givenDifferentAccessLevels_whenSetterAdded_thenStillNotSerializable() 
  throws JsonProcessingException {
    ObjectMapper mapper = new ObjectMapper();

    MyDtoSetter dtoObject = new MyDtoSetter();

    String dtoAsString = mapper.writeValueAsString(dtoObject);
    assertThat(dtoAsString, not(containsString("intValue")));
}
```

## 所有字段全局可序列化

某些情况下，上述方法无法实现，例如无法修改一些源码对象，则需要通过配置Jackson对非public字段的处理方式来解决问题。

具体是在ObjectMapper中配置全局变量，打开*public*字段、或者getter/setter方法的序列化*AutoDetect*功能，同样也可以打开所有字段的序列化：

```java
ObjectMapper mapper = new ObjectMapper();
mapper.setVisibility(PropertyAccessor.ALL, Visibility.NONE);
mapper.setVisibility(PropertyAccessor.FIELD, Visibility.ANY);
```

则*MyDtoAccessLevel*对象的所有字段，包括非public字段，都被序列化：

```java
@Test
public void givenDifferentAccessLevels_whenSetVisibility_thenSerializable() 
  throws JsonProcessingException {
    ObjectMapper mapper = new ObjectMapper();
    mapper.setVisibility(PropertyAccessor.FIELD, Visibility.ANY);

    MyDtoAccessLevel dtoObject = new MyDtoAccessLevel();

    String dtoAsString = mapper.writeValueAsString(dtoObject);
    assertThat(dtoAsString, containsString("stringValue"));
    assertThat(dtoAsString, containsString("intValue"));
    assertThat(dtoAsString, containsString("booleanValue"));
}
```

