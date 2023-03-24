# @GetMapping

`@GetMapping`是`Spring`框架中的注解，用于将`HTTP GET`请求映射到特定的处理程序方法上。它可以帮助开发者快速而简便地处理`GET`请求。

## 语法

```java
@GetMapping(value = "/path", 
            consumes = MediaType.APPLICATION_JSON_VALUE, 
            produces = MediaType.APPLICATION_JSON_VALUE)

```


### value属性

指定了映射到的URL路径。
  
`value`属性可以使用字符串数组来指定多个路径，例如：

```java
@GetMapping(value = {"/path1", "/path2"})
```

当value属性只有一个值时，可以省略value属性。

### consumes属性

指定**请求中的数据格式**（如JSON）。

### name属性

指定了该控制器方法的**逻辑名称**，不能作为请求路径显示。例如：

```java
@GetMapping(name = "myController", value = "/url")
public ReturnType methodName() {
    // method body
}
```

`name`属性的**作用**是**将控制器方法的逻辑名称与URL路径解耦**，使得控制器方法的逻辑名称可以在其他地方引用。

例如，可以在`Spring Security`的配置文件中使用`name`属性**指定授权规则**。

### produces属性

指定**响应的数据格式**。

表示响应的媒体类型（MIME类型），例如：
```java
produces = "application/json"

produces = MediaType.APPLICATION_JSON_VALUE
```

如果没有指定`consumes`和`produces`属性，`Spring`会默认使用 **通配符（/）**，表示支持任何数据格式。

## 无参数并且返回值类型为String

请求路径为 [/user](http://127.0.0.1:8080/user)，页面显示："用户名：张三，年龄：18"。

以下三种表达，随意选择一种即可。

#### 最简单声明

```java
@GetMapping("/user")
public String get(){
    return "用户名：张三，年龄：18";
}
```

#### 指定value值

```java
@GetMapping(value = "/user")
public String get(){
    return "用户名：张三，年龄：18";
}
```

#### 指定path、value及返回结果类型

```java
@GetMapping(path = "/user",
            value = "/user",
            produces = MediaType.TEXT_PLAIN_VALUE)
public String get(){
    HashMap<String, String> userMap = new HashMap<>(){{
        put("用户名3","张三");
        put("年龄3","18");
    }};
    return userMap.toString();
}
```

