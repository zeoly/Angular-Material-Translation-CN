[toc]

# 使用Jackson序列化/反序列化枚举类

## 定义枚举类

```java
public enum Distance {
    KILOMETER("km", 1000), 
    MILE("miles", 1609.34),
    METER("meters", 1), 
    INCH("inches", 0.0254),
    CENTIMETER("cm", 0.01), 
    MILLIMETER("mm", 0.001);

    private String unit;
    private final double meters;

    private Distance(String unit, double meters) {
        this.unit = unit;
        this.meters = meters;
    }

    // standard getters and setters
}
```

## 序列化枚举类

### 默认的枚举序列化

Jackson默认将枚举类序列化为字符串，例如：

```java
new ObjectMapper().writeValueAsString(Distance.MILE);
```

对应结果为：

```json
"MILE"
```

但一般期望的序列化结果可能如下：

```json
{"unit":"miles","meters":1609.34}
```

### 将枚举作为JSON对象

从Jackson 2.1.2开始，可以通过配置来解决这个问题，在类上使用*@JsonFormat*注解：

```java
@JsonFormat(shape = JsonFormat.Shape.OBJECT)
public enum Distance { ... }
```

这样配置后，*Distance*.MILE序列化的结果如下：

```json
{"unit":"miles","meters":1609.34}
```

### 使用*@JsonValue*

另一个方案是在getter方法上使用*@JsonValue*注解：

```java
public enum Distance { 
    ...
 
    @JsonValue
    public String getMeters() {
        return meters;
    }
}
```

这样就表示*getMeters()*代替了这个枚举，所以序列化的结果如下：

```json
1609.34
```

### 自定义枚举序列化器

如果使用Jackson版本早于2.1.2，或者需要自定义操作，可以使用自定义序列化器的方法：

```java
public class DistanceSerializer extends StdSerializer {
    
    public DistanceSerializer() {
        super(Distance.class);
    }

    public DistanceSerializer(Class t) {
        super(t);
    }

    public void serialize(
      Distance distance, JsonGenerator generator, SerializerProvider provider) 
      throws IOException, JsonProcessingException {
        generator.writeStartObject();
        generator.writeFieldName("name");
        generator.writeString(distance.name());
        generator.writeFieldName("unit");
        generator.writeString(distance.getUnit());
        generator.writeFieldName("meters");
        generator.writeNumber(distance.getMeters());
        generator.writeEndObject();
    }
}
```

并在类上指定：

```java
@JsonSerialize(using = DistanceSerializer.class)
public enum TypeEnum { ... }
```

序列化结果为：

```json
{"name":"MILE","unit":"miles","meters":1609.34}
```

## 反序列化为枚举类

首先定义一个*City*类，包含一个*Distance*属性：

```java
public class City {
    
    private Distance distance;
    ...    
}
```

### 默认的反序列化

Jackson默认使用枚举名称来反序列化JSON，例如：

```json
{"distance":"KILOMETER"}
```

会被反序列化为*Distance.KILOMETER*对象：

```java
City city = new ObjectMapper().readValue(json, City.class);
assertEquals(Distance.KILOMETER, city.getDistance());
```

### 使用*@JsonValue*

在上面的例子中，使用*@JsonValue*注解完成了枚举的序列化，同样此注解可以完成反序列化，是因为枚举本身是常量。

首先还是在getter方法上使用*@JsonValue*：

```java
public enum Distance {
    ...

    @JsonValue
    public double getMeters() {
        return meters;
    }
}
```

*getMeters()*方法的返回值表示了枚举对象，因此反序列化这个样例JSON时：

```json
{"distance":"0.0254"}
```

Jackson将查找*getMeters()*方法的返回值为0.0254的枚举对象，此例中即为*Distance.*INCH：

```java
assertEquals(Distance.INCH, city.getDistance());
```

### 使用*@JsonProperty*

可以在枚举实例上使用*@JsonProperty*注解：

```java
public enum Distance {
    @JsonProperty("distance-in-km")
    KILOMETER("km", 1000), 
    @JsonProperty("distance-in-miles")
    MILE("miles", 1609.34);
 
    ...
}
```

这表示被*@JsonProperty*标记的枚举实例对应为注解的值。例如如下JSON字符串：

```json
{"distance": "distance-in-km"}
```

对应为*Distance.KILOMETER*对象：

```java
assertEquals(Distance.KILOMETER, city.getDistance());
```

### 使用*@JsonCreator*

Jackson会调用被*@JsonCreator*注解标记的方法来实例化，例如如下JSON字符串：

```json
{
    "distance": {
        "unit":"miles", 
        "meters":1609.34
    }
}
```

定义一个*forValues()*工厂方法，并用*@JsonCreator*标记：

```json
public enum Distance {
   
    @JsonCreator
    public static Distance forValues(@JsonProperty("unit") String unit,
      @JsonProperty("meters") double meters) {
        for (Distance distance : Distance.values()) {
            if (
              distance.unit.equals(unit) && Double.compare(distance.meters, meters) == 0) {
                return distance;
            }
        }

        return null;
    }

    ...
}
```

注意在方法中使用了*@JsonProperty*注解来绑定JSON属性与方法参数。然后反序列化就可以得到结果：

```java
assertEquals(Distance.MILE, city.getDistance());
```

### 使用自定义反序列化器

例如在无法访问枚举源码的情况下，或者使用的老版本Jackson而不支持一些特性的情况下，如果上述的方法都无法满足，还可以使用自定义反序列化器。首先定义一个反序列化类：

```java
public class CustomEnumDeserializer extends StdDeserializer<Distance> {

    @Override
    public Distance deserialize(JsonParser jsonParser, DeserializationContext ctxt)
      throws IOException, JsonProcessingException {
        JsonNode node = jsonParser.getCodec().readTree(jsonParser);

        String unit = node.get("unit").asText();
        double meters = node.get("meters").asDouble();

        for (Distance distance : Distance.values()) {
           
            if (distance.getUnit().equals(unit) && Double.compare(
              distance.getMeters(), meters) == 0) {
                return distance;
            }
        }

        return null;
    }
}
```

然后在枚举类上使用*@JsonDeserialize*注解指定自定义序列化器：

```java
@JsonDeserialize(using = CustomEnumDeserializer.class)
public enum Distance {
   ...
}
```

则对应结果为：

```java
assertEquals(Distance.MILE, city.getDistance());
```

