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


## 需求：路径参数

使用注解[@PathVariable](@PathVariable.md)实现。

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

使用注解[@RequestParam](@RequestParam.md)实现。

发送请求[http://localhost:8080/user?name=张三](http://localhost:8080/user?name=张三)，对应返回内容为：`{"name":"张三","age":18}`。

发送请求[http://localhost:8080/user?name=莉丝](http://localhost:8080/user?name=莉丝)，对应返回内容为：`{"name":"莉丝","age":20}`。



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