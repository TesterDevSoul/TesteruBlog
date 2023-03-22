# GET请求 - 无参&String返回值


## 需求

发送请求`/user`，对应返回内容为：`用户名：张三，年龄：18`。

>无需提交参数，并且为获取数据请求，则为GET请求。



### 无参且返回值为String

编写`/user`的请求处理方法`get()`。
```java
@RestController
public class UserController {
    @RequestMapping("/user")
    public String get(){
        return "用户名：张三，年龄：18";
    }
}
```


## @RequestMapping

`@RequestMapping`注解用于将`HTTP`请求映射到特定的处理方法上。

### 语法

```java
@RequestMapping(value="/路径", path="/路径",  method=RequestMethod.HTTP方法)
```
- `value`属性：指定映射的 **`URL`路径** ，可以是一个字符串或者一个字符串数组。

- `path`属性：等同于`value`属性。

- `method`属性：指定 **`HTTP`请求方法**。 
  - `RequestMethod`枚举类型中的`GET`、`POST`、`PUT`、`DELETE`等方法。
  - 默认指定为`GET`请求。

### 无参&String返回值
以下三种代码，随意选择一种使用即可。

```java
//value元素是默认的注释
@RequestMapping("/user")
public String get(){
    return "用户名：张三，年龄：18";
}
```

```java
@RequestMapping(path = "/user", method = RequestMethod.GET)
public String get(){
    return "用户名：张三，年龄：18";
}
```

```java
@RequestMapping(value = "/user", method = RequestMethod.GET)
public String get(){
    return "用户名：张三，年龄：18";
}
```

## @GetMapping

`@GetMapping`是`Spring`框架中的注解，用于将`HTTP GET`请求映射到特定的处理程序方法上。它可以帮助开发者快速而简便地处理`GET`请求。

### 语法

```java
@GetMapping(value = "/path", 
            consumes = MediaType.APPLICATION_JSON_VALUE, 
            produces = MediaType.APPLICATION_JSON_VALUE)

```

- `@GetMapping`：注解名称。

- `value`属性：指定请求路径。
  
  - `value`属性可以使用字符串数组来指定多个路径，例如：`@GetMapping(value = {"/path1", "/path2"})`。

- `consumes`属性：指定请求中的数据格式（如JSON）。

- `produces`属性：指定响应的数据格式。

如果没有指定`consumes`和`produces`属性，`Spring`会默认使用 **通配符（/）**，表示支持任何数据格式。

### 无参&String返回值

以下三种表达，随意选择一种即可。

```java
@GetMapping("/user")
public String get(){
    return "用户名：张三，年龄：18";
}
```

```java
@GetMapping(value = "/user")
public String get(){
    return "用户名：张三，年龄：18";
}
```

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



# 学习反馈

1. 以下是GET请求的表达的是( )。

   - [x] A. `@RequestMapping("/user")`
   - [x] B. `@RequestMapping(path = "/user",method = RequestMethod.GET)`
   - [x] C. `@GetMapping("/user")`
   - [ ] D. `@GetMapping(name = "/user")`