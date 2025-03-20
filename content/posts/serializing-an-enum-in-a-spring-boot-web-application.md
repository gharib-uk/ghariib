---
title: "Serializing an enum in a Spring Boot web application"
date: 2025-01-08
tags: 
  - "un"
---

Enum is a good structure to define a set of limited and well defined values inside the domain of our application. They could help to prevent impossible states in our codebase.

## The scenario

Let's use a note taking web application as an example to show the possible ways to serialize and deserialize an eum value.

We are going to implement it using Spring Boot 3.3.x and MongoDB.

We define a `Type` enum class to represent the permitted types of todos inside the application: events and activities.  

```
public enum Type {
    EVENT("event"),
    ACTIVITY("activity");

    private Type(String value) {
       this.value = value;
    }

   public String getValue() {
       return value;
    }

}
```

Our `Todo` class  

```
public class Todo {

    private String id;
    private String name;
    private boolean completed;

    private Type type;

    ...
    ...

}

```

We are going to analyze enum serialization in these scenarios:

1. Enum as query param.
2. Enum as part of a JSON body request.
3. Enum as a field of a MongoDB document.

### Enum as query param

In this scenario we need only a deserialize method because we are interested in transforming from a string value to the enum.

Below a snippet of code representing the Controller method to read all todos by types, the type is passed as a query parameter.  

```
 public Collection<Todo> read(@RequestParam(required = false) Type type) {
     ...
     ...
 }
```

The query parameter is a string so we have to define an appropriate `Converter` to transform it.  
Inside the `convert` method we call `Type.fromString`, a static method created in the enum class, that will be used in other scenarios too. The code of `fromString` method is presented in the next scenario.  

```
public class StringToType implements Converter<String, Type> {

    @Override
    public Type convert(String source) {
        return Type.fromString(source);
    }

}
```

The converter must be registered in the application.  

```
@Configuration
public class Config implements WebMvcConfigurer {

    @Override
    public void addFormatters(FormatterRegistry registry) {
        registry.addConverter(new StringToType());
        WebMvcConfigurer.super.addFormatters(registry);
    }
}
```

Now when we will use `Type` as a `RequestParam` the `StringToType` converter will be used to try converting string values in our enum.

### Enum as part of JSON body request

To manage correctly the enum as a field of the JSON body content we should add some code to the enum `Type` class.

We need to use the annotation `@JsonValue` to mark the field as the value to use for serializing the enum.  
Then we should add a static map inside the enum to map the Type with the related string representation.  

```
public enum Type {
    EVENT("event"),
    ACTIVITY("activity");

    @JsonValue
    private String value;

    private static Map<String, Type> enumMap;

    private Type(String value) {
        this.value = value;
    }

    static {
        enumMap = Stream.of(values()).collect(Collectors.toMap(t -> t.value, t -> t));
    }

    public static Type fromString(String value) {
        return enumMap.get(value);
    }

    public String getValue() {
        return value;
    }

}
```

### Enum as a field of a MongoDB document.

To manage the serialization/deserialization of a enum in a MongoDB document we have to use the `@ValueConverter` annotation that binds a field of the document with a specific `PropertyValueConverter` class.

In the example the field `Type type` is bound with the `MongoEnumConverter` that provide `read` and `write` methods to manage the conversion.  

```
@Document(collection = "todo")
@TypeAlias("todo")
public class Todo {

    @Id
    private String id;
    private String name;
    private boolean completed;

    @ValueConverter(MongoEnumConverter.class)
    private Type type;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public boolean isCompleted() {
        return completed;
    }

    public void setCompleted(boolean completed) {
        this.completed = completed;
    }

    public Type getType() {
        return type;
    }

    public void setType(Type type) {
        this.type = type;
    }

}
```

In detail in the `read` method we call `Type.fromString` from the enum class to try to convert the string in a valid Type instance.  

```
public class MongoEnumConverter
        implements PropertyValueConverter<Type, String, ValueConversionContext<?>> {

    @Override
    public Type read(String value, ValueConversionContext<?> context) {
        return Type.fromString(value);
    }

    @Override
    public String write(Type value, ValueConversionContext<?> context) {
        return value.getValue();
    }

}
```

## In conclusion

In this article I presented some ways to manage the serialization/deserializion of a enum class in a typical Web scenario. Spring and the Jackson library provide some facilities to simplify this work.

The code is publically available in this Gitlab repository.

The code presented in this article are under CC0 license.

Go to Source
