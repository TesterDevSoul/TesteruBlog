# GET请求 - 有单个参数并且返回值类型为Json

## 接口请求参数传递的场景

当我们向一个API发送请求时，往往需要在请求中传递一些参数，以便API可以正确地执行我们的请求。例如，我们向一个天气预报API发送请求时，需要传递城市名、日期等参数。

## 接口请求参数传递优点

#### 实现功能的拓展

通过参数传递，API可以接受不同的请求参数，从而实现不同的功能。这样可以让API具有更广泛的适用性，满足不同用户的需求。

#### 提高代码的复用性

通过参数传递，API的代码可以被多个应用程序或模块共享和复用。这样可以避免重复开发，提高代码的可维护性和可重用性。

#### 减少数据冗余

通过参数传递，API只需要传递必要的数据，避免了数据的冗余传递，提高了数据传输的效率和性能。

## 接口请求参数类型

接口请求参数主要可以分为以下几种：

### 查询参数

`Query Parameters`：查询参数通常作为`URL`的一部分，以问号（**?**）开头作为分隔符，多个参数之间用 **&** 符号连接的键值对的方式传递参数。

查询参数通常用于向API请求特定的资源或数据。

例如，以下URL中的查询参数是 name 和 age ：
```css
https://example.com/profile?name=John&age=30
```

### 路径参数

`Path Parameters`：路径参数通常作为URL路径的一部分，用于指定要访问的资源的ID或其他标识符。

路径参数通常用于**唯一标识资源**，例如用户、文章、商品等。

>与查询参数不同，路径参数是必需的，因为它们用于指定请求的资源。

例如：`https://api.example.com/products/12345`

### 请求体参数

`Request Body Parameters`：请求体参数通常作为请求的主体部分，用于向API发送包含更多数据的请求，例如创建、更新或删除资源。请求体参数通常使用POST、PUT、PATCH等HTTP方法进行发送。

### 标头参数

`Header Parameters`：标头参数通常包含在HTTP请求头中，用于向API提供额外的信息，如身份验证令牌、内容类型、缓存控制等。

不同的接口请求参数类型都有各自的应用场景，根据具体需求进行选择和使用。

## 路径参数：@PathVariable

`@PathVariable`是`Spring`框架中的一个注解，用于指示**一个方法参数应该绑定到一个URI模板变量上**。

在`Spring MVC`中，`@PathVariable`通常用于**从URL中提取参数值**。

##### 语法

请求路径：`/请求路径/{方法参数1}/请求路径2/{方法参数2}`。

```java
@GetMapping("/请求路径/{请求参数1}/请求路径2/{请求参数2}")
public 返回实体类 get11(@PathVariable(value = "请求参数1", required =false)String 方法参数1, @PathVariable(value = "请求参数2", required =false)int 方法参数2){...}
```

例如，假设我们有一个处理`HTTP GET`请求的控制器方法，`URL`为`/users/{id}`，其中`{id}`是一个**动态变量**，表示用户的唯一标识符。

我们可以使用`@PathVariable`注解将方法参数绑定到`{id}`变量，如下所示：

```java
@GetMapping("/users/{id}")
public UserDTO getUserById(@PathVariable(value = "id", required =false) Long uid) {...}
```

>`@PathVariable`注解将`URL`中的路径变量`id`与`getUserById()`方法的参数`Long uid`绑定起来，以便可以在方法中使用该值。

##### name/value属性

要绑定的请求参数的名称，跟`URL`上填写的路径名一样。

##### required属性

用于指定**路径变量是否是必需的**。

默认情况下，`@PathVariable`注解的`required`属性为 **`true`**，这意味着如果`URL`中没有提供该路径变量，或者提供的值为空字符串，则将引发 **异常**。

**请求缺少参数「参数不存在或参数为`null`」会引发`MissingPathVariableException`异常**。

解决方案：**`required` 声明为 `false`**。

例如，以下控制器方法中的`@PathVariable`注解有可能抛出异常，因为它指定了必需的路径变量，但在浏览器访问的`URL`中未提供该变量：

```java
@GetMapping("/users/{userId}")
public String getUser(@PathVariable("userId") Long userId) {
    // do something with userId
}
```

如果想让路径变量成为可选的，可以将`@PathVariable`注解的`required`属性设置为`false`。例如：

```java
@GetMapping("/users/{userId}")
public String getUser(@PathVariable(name = "userId", required = false) Long userId) {
    if (userId == null) {
        // handle case where userId is not provided
    } else {
        // do something with userId
    }
}
```

上面的示例中，将`@PathVariable`注解的`required`属性设置为`false`，表示在`URL`中未提供该路径变量时，参数`userId`将为`null`，而不会引发异常。

这使得你可以更好地控制方法的行为，例如在缺少路径变量时提供默认值或执行其他逻辑。

需要注意的是，`@PathVariable`注解默认情况下是**必须匹配**的。

如果请求的URL中缺少`{id}`变量，或者`{id}`变量的值无法转换为方法参数的类型，那么会抛出异常。

若参数为非必需可进行选择「参数为可选」，可以使用`@PathVariable(required = false)`。

### 多个路径参数

可以在方法的参数列表中使用多个`@PathVariable`注解来接收多个路径参数。例如：

```java
@GetMapping("/users/{userId}/orders/{orderId}")
public String getOrderDetails(@PathVariable Long userId, @PathVariable Long orderId) {
    // 根据userId和orderId获取订单详情
    // 返回订单详情的视图
}

```

通过`@GetMapping`注解的参数可知，方法`getOrderDetails()`有两个路径参数：`userId`和`orderId`。使用`@PathVariable`注解将它们绑定到方法的两个参数中。

当访问`URL` **`/users/123/orders/456`** 时，`Spring`会将 **123** 和 **456** 分别赋值给`userId`和`orderId`参数，并调用`getOrderDetails()`方法。

## 查询参数：@RequestParam

用于将`HTTP`请求参数绑定到方法的参数上。

下面是`@RequestParam`注解的语法和使用方法：

##### 语法

请求路径：`/请求路径?参数名1=方法参数值1&参数名2=方法参数值2`。

```java
@GetMapping("/请求路径")
public UserDTO get12(@RequestParam(value = "参数名1", required = true/false, defaultValue = "默认值") Long 方法参数1,
@RequestParam(value = "参数名2", required = true/false, defaultValue = "默认值") String 方法参数2){...}
```

##### value属性

请求参数的名称，必须要指定，可以使用别名。

##### required属性

是否必须提供该请求参数，默认为`true`，即必须提供。


##### defaultValue属性

如果请求中未提供该参数，则使用默认值。

##### 示例

```java
@RestController
public class MyController {
    
    @GetMapping("/hello")
    public String hello(@RequestParam(value = "name", required = false, defaultValue = "world") String name) {
        return "Hello " + name + "!";
    }
}
```

在上面的示例中，我们在方法的参数上使用了`@RequestParam`注解，指定了请求参数的名称为`“name”`，设置了`required`为`false`表示该参数是可选的，设置了`defaultValue`为`“world”`，表示如果请求中未提供该参数，则默认使用`“world”`作为参数值。

当我们访问`/hello`路径时，如果提供了`name`参数，则返回“Hello [name]!”的字符串，否则返回`“Hello world!”`的字符串。



## 需求：路径参数

发送请求 [http://127.0.0.1:8080/user10/张三](http://127.0.0.1:8080/user10/1)，对应返回内容为：`{"name":"张三","age":18}`。

发送请求 [http://127.0.0.1:8080/user11/莉丝](http://127.0.0.1:8080/user10/1)，对应返回内容为：`{"name":"莉丝","age":20}`。

### 请求参数与方法参数不一致

1. 方法声明方法参数`userId`。

2. 方法参数要和前端请求参数名`id`绑定。

```java
@GetMapping(path = "/user10/{name}", value = "/user10/{name}",
        produces = MediaType.APPLICATION_JSON_VALUE)
public UserDTO get10(@PathVariable("name") String userName){
    System.out.println(userName);
    List<UserDTO> userList = getUsers();
    UserDTO user3 = userList
                    .stream()
                    .filter(user -> userName.equals(user.getName()))
                    .toList()
                    .get(0);
    return user3;
}
```

### 请求参数与方法参数一致

```java
@GetMapping(path = "/user11/{name}", value = "/user11/{name}",
            produces = MediaType.APPLICATION_JSON_VALUE)
public UserDTO get11(@PathVariable String name){
    List<UserDTO> userList = getUsers();
    UserDTO user3 = userList
            .stream()
            .filter(user -> name.equals(user.getName()))
            .toList()
            .get(0);
    return user3;
}
```

## 需求：查询参数

发送请求[http://127.0.0.1:8080/user12?id=1](http://127.0.0.1:8080/user12?id=1)，对应返回内容为：`{"name":"张三","age":18}`。

发送请求[http://127.0.0.1:8080/user13?id=2](http://127.0.0.1:8080/user12?id=2)，对应返回内容为：`{"name":"莉丝","age":20}`。

### 请求参数与方法参数不一致

1. 方法声明`GET`请求注解及其请求路径。

2. 方法参数前使用注解声明拼接参数`name`。 
   
注意⚠️：参数作为拼接参数，请求路径中没有该参数。

```java
@GetMapping(path = "/user12")
public UserDTO get12(@RequestParam("name") String uname){
    List<UserDTO> userList = getUsers();
    UserDTO user3 = userList
                    .stream()
                    .filter(user -> uname.equals(user.getName()))
                    .toList()
                    .get(0);
    return user3;
}
```

### 请求参数与方法参数一致

```java
@GetMapping(path = "/user13")
public UserDTO get13(@RequestParam String name){
    List<UserDTO> userList = getUsers();
    UserDTO user3 = userList
            .stream()
            .filter(user -> name.equals(user.getName()))
            .toList()
            .get(0);
    return user3;
}
```

## 传入参数总结

- **查询参数**适用于**可选参数**和**筛选数据**。

- **路径参数**适用于**标识资源**和**必需参数**。