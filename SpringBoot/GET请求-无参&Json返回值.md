# GET请求 - 无参&Json返回值

## Json返回值优点

使用 JSON（JavaScript Object Notation）格式返回值的好处有以下几点：

### 轻量级

相对于其他数据格式{`XML`}，JSON 是一种轻量级的数据格式，可以节省传输和存储的带宽和空间。

### 易于解析

JSON 数据格式非常易于解析和处理。

几乎所有编程语言都支持 JSON 解析，因此它成为了 Web 应用程序中最受欢迎的数据格式之一。

### 易于阅读和调试

JSON 格式采用了键值对的方式，易于理解和阅读。

同时，由于 JSON 的结构很清晰，调试时也非常方便。

### 易于扩展

JSON 格式可以轻松扩展，以满足应用程序中不断变化的需求。

### 浏览器支持

JSON 格式在浏览器端具有天然的支持，可以使用 JavaScript 对 JSON 进行处理，无需额外的插件或库。

总之，JSON 返回值的好处在于它是一种轻量级、易于解析和阅读、易于扩展、浏览器支持的数据格式。这使得 JSON 成为了 Web 应用程序中最受欢迎的数据格式之一。



## 需求

请求的返回内容由String格式「`用户名：张三，年龄：18`」修改为Json格式
「`{"name":"张三","age":18}`」。


### Map返回类型

Map示例代码：


```java
@GetMapping(path = "/user",
                value = "/user",
                produces = MediaType.APPLICATION_JSON_VALUE)
public HashMap<String, Object> get5(){
    HashMap<String, Object> userMap = new HashMap<>(){{
        put("name","张三");
        put("age",18);
    }};
    return userMap;
}
```

#### 缺点

HashMap 作为 JSON 返回值的缺点主要有以下几个方面：

##### 可读性差

HashMap 返回值的键值对格式并不像 JSON 那样易于理解和阅读。

这对于其他开发人员或客户端来说可能会造成困惑和难以维护。

##### 容错性差

HashMap 返回值的结构不像 JSON 那样严格，不支持嵌套和数组等复杂数据类型。

这使得 HashMap 返回值在处理复杂数据时的容错性较差，可能会出现错误。

##### 安全性问题

将 HashMap 直接作为 JSON 返回值可能会泄露一些敏感信息。

比如 `HashMap` 中可能会包含一些数据库连接信息或密码等敏感信息，这些信息如果直接返回可能会存在安全风险。

### 实体类返回类型

#### 步骤

1. 创建放置实体类的包。

2. 创建实体类对象。

3. 实体类对象声明成员变量及其 `Getter` / `Setter` 方法。

4. 请求方法编写。

##### 创建实体类对象

[实体类对象](业务实体类.md)


```java
package top.testeru.mini.entity;

@Getter//getter方法
@Setter//setter方法
@ToString//tostring方法
public class User {
    private String name;
    private Integer age;
}
```

##### 请求编写验证

```java
@GetMapping(path = "/user6",
        value = "/user6",
        produces = MediaType.APPLICATION_JSON_VALUE)
public User get6(){
    User user = new User();
    user.setName("张三");
    user.setAge(18);
    return user;
}
```


![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230322154245.png)