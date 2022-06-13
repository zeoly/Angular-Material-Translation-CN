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

