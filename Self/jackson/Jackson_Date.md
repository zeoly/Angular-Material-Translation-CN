[toc]

# Jackson与日期

将日期类型序列化是常见场景，以下将介绍java.util.*Date*，Joda-Time，以及Java 8的*DateTime。*

## 将*Date*序列化为时间戳

如下例，*Event*实例中包含一个*Date*类型的属性*eventDate*

```java
@Test
public void whenSerializingDateWithJackson_thenSerializedToTimestamp()
  throws JsonProcessingException, ParseException {
 
    SimpleDateFormat df = new SimpleDateFormat("dd-MM-yyyy hh:mm");
    df.setTimeZone(TimeZone.getTimeZone("UTC"));

    Date date = df.parse("01-01-1970 01:00");
    Event event = new Event("party", date);

    ObjectMapper mapper = new ObjectMapper();
    mapper.writeValueAsString(event);
}
```

默认Jackson会将Date类型序列化为时间戳格式（从1970年1月1日至今的毫秒数）。

*event*对象序列化的数据为：

```json
{
   "name":"party",
   "eventDate":3600000
}
```

## 将*Date*序列化为ISO-8601

序列化为时间戳的方式并不常用，可以将*Date*序列化为*ISO-8601*格式：

```java
@Test
public void whenSerializingDateToISO8601_thenSerializedToText()
  throws JsonProcessingException, ParseException {
 
    SimpleDateFormat df = new SimpleDateFormat("dd-MM-yyyy hh:mm");
    df.setTimeZone(TimeZone.getTimeZone("UTC"));

    String toParse = "01-01-1970 02:30";
    Date date = df.parse(toParse);
    Event event = new Event("party", date);

    ObjectMapper mapper = new ObjectMapper();
    mapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);
    // StdDateFormat is ISO8601 since jackson 2.9
    mapper.setDateFormat(new StdDateFormat().withColonInTimeZone(true));
    String result = mapper.writeValueAsString(event);
    assertThat(result, containsString("1970-01-01T02:30:00.000+00:00"));
}
```

这种格式的可读性更强。

## 配置*ObjectMapper*的*DateFormat*

上面方法的时间格式不太灵活，也可以配置*DateFormat*自定义日期格式：

```java
@Test
public void whenSettingObjectMapperDateFormat_thenCorrect()
  throws JsonProcessingException, ParseException {
 
    SimpleDateFormat df = new SimpleDateFormat("dd-MM-yyyy hh:mm");

    String toParse = "20-12-2014 02:30";
    Date date = df.parse(toParse);
    Event event = new Event("party", date);

    ObjectMapper mapper = new ObjectMapper();
    mapper.setDateFormat(df);

    String result = mapper.writeValueAsString(event);
    assertThat(result, containsString(toParse));
}
```

这种方法虽然看起来灵活，但要注意是一个全局配置。

## 使用*@JsonFormat*来格式化日期

使用*@JsonFormat*注解可以在单个类中控制日期类型格式化：

```java
public class Event {
    public String name;

    @JsonFormat
      (shape = JsonFormat.Shape.STRING, pattern = "dd-MM-yyyy hh:mm:ss")
    public Date eventDate;
}
```

测试如下：

```java
@Test
public void whenUsingJsonFormatAnnotationToFormatDate_thenCorrect()
  throws JsonProcessingException, ParseException {
 
    SimpleDateFormat df = new SimpleDateFormat("dd-MM-yyyy hh:mm:ss");
    df.setTimeZone(TimeZone.getTimeZone("UTC"));

    String toParse = "20-12-2014 02:30:00";
    Date date = df.parse(toParse);
    Event event = new Event("party", date);

    ObjectMapper mapper = new ObjectMapper();
    String result = mapper.writeValueAsString(event);
    assertThat(result, containsString(toParse));
}
```

## 自定义日期序列化器

想要全方位的控制输出，可以自定义Date类型的序列化器：

```java
public class CustomDateSerializer extends StdSerializer<Date> {
 
    private SimpleDateFormat formatter 
      = new SimpleDateFormat("dd-MM-yyyy hh:mm:ss");

    public CustomDateSerializer() {
        this(null);
    }

    public CustomDateSerializer(Class t) {
        super(t);
    }
    
    @Override
    public void serialize (Date value, JsonGenerator gen, SerializerProvider arg2)
      throws IOException, JsonProcessingException {
        gen.writeString(formatter.format(value));
    }
}
```

然后在*eventDate*属性上使用此序列化器：

```java
public class Event {
    public String name;

    @JsonSerialize(using = CustomDateSerializer.class)
    public Date eventDate;
}
```

最后，使用如下：

```java
@Test
public void whenUsingCustomDateSerializer_thenCorrect()
  throws JsonProcessingException, ParseException {
 
    SimpleDateFormat df = new SimpleDateFormat("dd-MM-yyyy hh:mm:ss");

    String toParse = "20-12-2014 02:30:00";
    Date date = df.parse(toParse);
    Event event = new Event("party", date);

    ObjectMapper mapper = new ObjectMapper();
    String result = mapper.writeValueAsString(event);
    assertThat(result, containsString(toParse));
}
```

## 使用Jackson序列化Joda-Time

日期类型并不只是*java.util.Date*，常见的还有Joda-Time库实现的*DateTime*。

首先需要引入*jackson-datatype-joda*模块：

```xml
<dependency>
  <groupId>com.fasterxml.jackson.datatype</groupId>
  <artifactId>jackson-datatype-joda</artifactId>
  <version>2.9.7</version>
</dependency>
```

然后注册*JodaModule*即可：

```java
@Test
public void whenSerializingJodaTime_thenCorrect() 
  throws JsonProcessingException {
    DateTime date = new DateTime(2014, 12, 20, 2, 30, 
      DateTimeZone.forID("Europe/London"));

    ObjectMapper mapper = new ObjectMapper();
    mapper.registerModule(new JodaModule());
    mapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);

    String result = mapper.writeValueAsString(date);
    assertThat(result, containsString("2014-12-20T02:30:00.000Z"));
}
```

## 使用自定义序列化器来序列化Joda *DateTime*

如果不想依赖额外的Joda-Time Jackson，可以使用自定义的序列化器来序列化*DateTime*：

```java
public class CustomDateTimeSerializer extends StdSerializer<DateTime> {

    private static DateTimeFormatter formatter = 
      DateTimeFormat.forPattern("yyyy-MM-dd HH:mm");

    public CustomDateTimeSerializer() {
        this(null);
    }

     public CustomDateTimeSerializer(Class<DateTime> t) {
         super(t);
     }
    
    @Override
    public void serialize
      (DateTime value, JsonGenerator gen, SerializerProvider arg2)
      throws IOException, JsonProcessingException {
        gen.writeString(formatter.print(value));
    }
}
```

并在*eventDate*属性上使用此序列化器：

```java
public class Event {
    public String name;

    @JsonSerialize(using = CustomDateTimeSerializer.class)
    public DateTime eventDate;
}
```

测试如下：

```java
@Test
public void whenSerializingJodaTimeWithJackson_thenCorrect() 
  throws JsonProcessingException {
 
    DateTime date = new DateTime(2014, 12, 20, 2, 30);
    Event event = new Event("party", date);

    ObjectMapper mapper = new ObjectMapper();
    String result = mapper.writeValueAsString(event);
    assertThat(result, containsString("2014-12-20 02:30"));
}
```

## 使用Jackson序列化Java 8日期

接下来是使用Jackson来序列化Java 8 *DateTime*，例如*LocalDateTime*，需要使用*jackson-datatype-jsr310*模块：

```xml
<dependency>
    <groupId>com.fasterxml.jackson.datatype</groupId>
    <artifactId>jackson-datatype-jsr310</artifactId>
    <version>2.9.7</version>
</dependency>
```

然后注册*JavaTimeModule*（*JSR310Module*已过期）即可：

```java
@Test
public void whenSerializingJava8Date_thenCorrect()
  throws JsonProcessingException {
    LocalDateTime date = LocalDateTime.of(2014, 12, 20, 2, 30);

    ObjectMapper mapper = new ObjectMapper();
    mapper.registerModule(new JavaTimeModule());
    mapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);

    String result = mapper.writeValueAsString(date);
    assertThat(result, containsString("2014-12-20T02:30"));
}
```

## 不使用外部依赖序列化Java 8日期

同样，可以使用自定义序列化器来实现Java 8 *DateTime*的序列化：

```java
public class CustomLocalDateTimeSerializer 
  extends StdSerializer<LocalDateTime> {

    private static DateTimeFormatter formatter = 
      DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm");

    public CustomLocalDateTimeSerializer() {
        this(null);
    }
 
    public CustomLocalDateTimeSerializer(Class<LocalDateTime> t) {
        super(t);
    }
    
    @Override
    public void serialize(
      LocalDateTime value,
      JsonGenerator gen,
      SerializerProvider arg2)
      throws IOException, JsonProcessingException {
 
        gen.writeString(formatter.format(value));
    }
}
```

并在*eventDate*属性上使用此序列化器：

```java
public class Event {
    public String name;

    @JsonSerialize(using = CustomLocalDateTimeSerializer.class)
    public LocalDateTime eventDate;
}
```

使用与测试如下：

```java
@Test
public void whenSerializingJava8DateWithCustomSerializer_thenCorrect()
  throws JsonProcessingException {
 
    LocalDateTime date = LocalDateTime.of(2014, 12, 20, 2, 30);
    Event event = new Event("party", date);

    ObjectMapper mapper = new ObjectMapper();
    String result = mapper.writeValueAsString(event);
    assertThat(result, containsString("2014-12-20 02:30"));
}
```

## 反序列化日期

以下是使用**Jackson**反序列化日期的方法：

```java
@Test
public void whenDeserializingDateWithJackson_thenCorrect()
  throws JsonProcessingException, IOException {
 
    String json = "{"name":"party","eventDate":"20-12-2014 02:30:00"}";

    SimpleDateFormat df = new SimpleDateFormat("dd-MM-yyyy hh:mm:ss");
    ObjectMapper mapper = new ObjectMapper();
    mapper.setDateFormat(df);

    Event event = mapper.readerFor(Event.class).readValue(json);
    assertEquals("20-12-2014 02:30:00", df.format(event.eventDate));
}
```

## 保留时区并反序列化Joda *ZonedDateTime*

在默认配置下，Jackson会将Joda *ZonedDateTime*的时区设置为本地时区。例如下例中，手动配置了时区，但Jackson反序列化时，会将时区调整为GMT：

```java
@Test
public void whenDeserialisingZonedDateTimeWithDefaults_thenNotCorrect()
  throws IOException {
    ObjectMapper objectMapper = new ObjectMapper();
    objectMapper.findAndRegisterModules();
    objectMapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);
    ZonedDateTime now = ZonedDateTime.now(ZoneId.of("Europe/Berlin"));
    String converted = objectMapper.writeValueAsString(now);

    ZonedDateTime restored = objectMapper.readValue(converted, ZonedDateTime.class);
    System.out.println("serialized: " + now);
    System.out.println("restored: " + restored);
    assertThat(now, is(restored));
}
```

此单元测试会失败，因为时区不同：

```
serialized: 2017-08-14T13:52:22.071+02:00[Europe/Berlin]
restored: 2017-08-14T11:52:22.071Z[UTC]
```

但有一个简单方法，可以使Jackson保留原有时区：

```java
objectMapper.disable(DeserializationFeature.ADJUST_DATES_TO_CONTEXT_TIME_ZONE);
```

## 自定义日期反序列化器

与序列化类似，同样可以自定义日期的反序列化器，如下：

```java
public class CustomDateDeserializer extends StdDeserializer<Date> {

    private SimpleDateFormat formatter = 
      new SimpleDateFormat("dd-MM-yyyy hh:mm:ss");

    public CustomDateDeserializer() {
        this(null);
    }

    public CustomDateDeserializer(Class<?> vc) {
        super(vc);
    }

    @Override
    public Date deserialize(JsonParser jsonparser, DeserializationContext context)
      throws IOException, JsonProcessingException {
        String date = jsonparser.getText();
        try {
            return formatter.parse(date);
        } catch (ParseException e) {
            throw new RuntimeException(e);
        }
    }
}
```

并在*eventDate*属性上使用：

```java
public class Event {
    public String name;

    @JsonDeserialize(using = CustomDateDeserializer.class)
    public Date eventDate;
}
```

测试如下：

```java
@Test
public void whenDeserializingDateUsingCustomDeserializer_thenCorrect()
  throws JsonProcessingException, IOException {
 
    String json = "{"name":"party","eventDate":"20-12-2014 02:30:00"}";

    SimpleDateFormat df = new SimpleDateFormat("dd-MM-yyyy hh:mm:ss");
    ObjectMapper mapper = new ObjectMapper();

    Event event = mapper.readerFor(Event.class).readValue(json);
    assertEquals("20-12-2014 02:30:00", df.format(event.eventDate));
}
```

## 修复*InvalidDefinitionException*

当创建*LocalDate*实例时，可能遇到如下异常：

```
com.fasterxml.jackson.databind.exc.InvalidDefinitionException: Cannot construct instance
of `java.time.LocalDate`(no Creators, like default construct, exist): no String-argument
constructor/factory method to deserialize from String value ('2014-12-20') at [Source:
(String)"2014-12-20"; line: 1, column: 1]
```

是因为JSON本身没有日期类型，所以是一个*String*类型。

而*String*类型与*LocalDate*类型并不一致，所以需要一个外部的反序列化器来读取*String*数据，对等也需要一个序列化器来将日期类型转换为*String*类型。

### Jackson依赖

解决这个问题有多种方法，首先要确认在*pom.xml*中有*jsr310*依赖：

```xml
<dependency>
    <groupId>com.fasterxml.jackson.datatype</groupId>
    <artifactId>jackson-datatype-jsr310</artifactId>
    <version>2.11.0</version>
</dependency>
```

### 序列化为单个日期对象

要处理*LocalDate*，需要先注册*JavaTimeModule*。

同样也需要先关闭*WRITE_DATES_AS_TIMESTAMPS*，防止输出时间戳格式：

```java
@Test
public void whenSerializingJava8DateAndReadingValue_thenCorrect() throws IOException {
    String stringDate = "\"2014-12-20\"";

    ObjectMapper mapper = new ObjectMapper();
    mapper.registerModule(new JavaTimeModule());
    mapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);

    LocalDate result = mapper.readValue(stringDate, LocalDate.class);
    assertThat(result.toString(), containsString("2014-12-20"));
}
```

### 在对象中使用注解

另一个方法是在实体中使用*LocalDateDeserializer*和*JsonFormat*注解：

```java
public class EventWithLocalDate {

    @JsonDeserialize(using = LocalDateDeserializer.class)
    @JsonSerialize(using = LocalDateSerializer.class)
    @JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "dd-MM-yyyy")
    public LocalDate eventDate;
}
```

*@JsonDeserialize*与*@JsonSerialize*注解分别指定自定义的反序列化与序列化器，*@JsonFormat*注解指定格式：

```java
@Test
public void whenSerializingJava8DateAndReadingFromEntity_thenCorrect() throws IOException {
    String json = "{\"name\":\"party\",\"eventDate\":\"20-12-2014\"}";

    ObjectMapper mapper = new ObjectMapper();

    EventWithLocalDate result = mapper.readValue(json, EventWithLocalDate.class);
    assertThat(result.getEventDate().toString(), containsString("2014-12-20"));
}
```

此方法相比使用*JavaTimeModule*更灵活。

