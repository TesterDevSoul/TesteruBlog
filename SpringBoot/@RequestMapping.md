## @RequestMapping

`@RequestMapping`注解用于将`HTTP`请求映射到特定的处理方法上。

该注解为类和方法上的注解。

### 语法

```java
@RequestMapping(value="/路径", path="/路径",  method=RequestMethod.HTTP方法,headers="content-type=text/*")
```


### value属性

指定映射的 **`URL`路径** ，可以是一个字符串或者一个字符串数组。

### path属性

等同于`value`属性。

`path`或`value`写其中一个就可以。

### headers属性

`headers`属性可以用来指定 `HTTP` 请求头中必须存在的值，从而进一步限制映射到该方法的请求。

`headers` 属性接受一个字符串数组作为参数，其中每个字符串都是一个 `HTTP` 请求头的键值对，键和值之间使用 **=** 符号分隔。示例代码：

- 限制 `User-Agent` 请求头必须包含 `Mozilla` 字符串：

```java
@RequestMapping(value = "/example", headers = "User-Agent=Mozilla")
```

- 限制 `Content-Type` 请求头必须是 `application/json`：

```java
@RequestMapping(value = "/example", headers = "Content-Type=application/json")
```

- 限制 `Accept` 请求头必须包含 `application/json` 和 `text/plain`：

```java
@RequestMapping(value = "/example", headers = {"Accept=application/json", "Accept=text/plain"})
```

通过使用 `headers` 属性，我们可以在 `Spring MVC` 中更细粒度地控制哪些请求可以映射到特定的处理方法中。这可以增强我们应用程序的安全性，并且可以更好地控制与客户端之间的交互。

### method属性

指定 **`HTTP`请求方法**。 

`RequestMethod` 枚举类型中的`GET`、`POST`、`PUT`、`DELETE`等方法，默认指定为`GET`请求。

### 无参数并且返回值类型为String

请求路径为 [/user](http://127.0.0.1:8080/user)，页面显示："用户名：张三，年龄：18"。

以下三种代码，随意选择一种使用即可。

##### 最简单声明

```java
//value元素是默认的注释
@RequestMapping("/user")
public String get(){
    return "用户名：张三，年龄：18";
}
```

##### 指定value值及请求方法

```java
@RequestMapping(value = "/user", method = RequestMethod.GET)
public String get(){
    return "用户名：张三，年龄：18";
}
```

##### 指定path路径及请求方法

```java
@RequestMapping(path = "/user", method = RequestMethod.GET)
public String get(){
    return "用户名：张三，年龄：18";
}
```