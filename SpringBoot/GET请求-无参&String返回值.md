# GET请求 - 没有参数并且返回值类型为String

## GET请求应用场景

GET请求是HTTP协议中的一种请求方法，用于请求指定资源的表示形式。

GET请求通常用于**获取数据**，而不会修改服务器上的数据。

以下是一些常见的GET请求应用场景：

### 获取网页内容

当用户在浏览器中输入URL时，浏览器会向服务器发送一个GET请求以获取网页内容。

### 获取数据

例如，获取API返回的数据，例如获取天气预报、股票数据等等。

### 获取静态文件

例如，获取图片、音频、视频等静态文件。

### 获取搜索结果

当用户在搜索引擎中输入关键字时，搜索引擎会向服务器发送一个GET请求以获取与关键字相关的搜索结果。

### 获取页面元数据

例如，获取网页标题、描述、关键字等元数据，用于搜索引擎优化（SEO）。

需要注意的是，由于GET请求在URL中传递参数，因此不应该用于传递敏感数据，因为URL中的参数可以被浏览器、服务器、代理等各个环节记录和截取。对于传递敏感数据，应该使用POST请求或其他更安全的方式。


## @RequestMapping

`@RequestMapping`注解用于将`HTTP`请求映射到特定的处理方法上。

该注解为类和方法上的注解。

### 语法

```java
@RequestMapping(value="/路径", path="/路径",  method=RequestMethod.HTTP方法)
```

### value属性

指定映射的 **`URL`路径** ，可以是一个字符串或者一个字符串数组。

### path属性

等同于`value`属性。

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



## @GetMapping

`@GetMapping`是`Spring`框架中的注解，用于将`HTTP GET`请求映射到特定的处理程序方法上。它可以帮助开发者快速而简便地处理`GET`请求。

### 语法

```java
@GetMapping(value = "/path", 
            consumes = MediaType.APPLICATION_JSON_VALUE, 
            produces = MediaType.APPLICATION_JSON_VALUE)

```


#### value属性

指定了映射到的URL路径。
  
`value`属性可以使用字符串数组来指定多个路径，例如：

```java
@GetMapping(value = {"/path1", "/path2"})
```

当value属性只有一个值时，可以省略value属性。

#### consumes属性

指定**请求中的数据格式**（如JSON）。

#### name属性

指定了该控制器方法的**逻辑名称**。例如：

```java
@GetMapping(name = "myController", value = "/url")
public ReturnType methodName() {
    // method body
}
```

`name`属性的**作用**是**将控制器方法的逻辑名称与URL路径解耦**，使得控制器方法的逻辑名称可以在其他地方引用。

例如，可以在`Spring Security`的配置文件中使用`name`属性**指定授权规则**。

#### produces属性

指定**响应的数据格式**。

表示响应的媒体类型（MIME类型），例如：
```java
produces = "application/json"

produces = MediaType.APPLICATION_JSON_VALUE
```

如果没有指定`consumes`和`produces`属性，`Spring`会默认使用 **通配符（/）**，表示支持任何数据格式。

### 无参数并且返回值类型为String

请求路径为 [/user](http://127.0.0.1:8080/user)，页面显示："用户名：张三，年龄：18"。

以下三种表达，随意选择一种即可。

##### 最简单声明

```java
@GetMapping("/user")
public String get(){
    return "用户名：张三，年龄：18";
}
```

##### 指定value值

```java
@GetMapping(value = "/user")
public String get(){
    return "用户名：张三，年龄：18";
}
```

##### 指定path、value及返回结果类型

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

## 需求

浏览器获取当前用户信息，请求路径为`/user`，用户信息为：`用户名：张三，年龄：18`。

>无需提交参数，并且为获取数据请求，则为GET请求。

### 分析

1. 浏览器发送请求路径，页面显示内容。
   >请求为`GET`请求，请求方法添加注解。

2. 请求路径`/user`，对应方法为`get()`。

3. 请求方法返回值类型为`String`。


### 步骤

无参且返回值为String类型。

```java
@RestController
public class UserController {
    @RequestMapping("/user")
    public String get(){
        return "用户名：张三，年龄：18";
    }
}
```

# 学习反馈

1. 以下是GET请求的表达的是( )。

   - [x] A. `@RequestMapping("/user")`
   - [x] B. `@RequestMapping(path = "/user",method = RequestMethod.GET)`
   - [x] C. `@GetMapping("/user")`
   - [ ] D. `@GetMapping(name = "/user")`