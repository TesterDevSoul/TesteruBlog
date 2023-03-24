# POST请求-有Body参数并且返回值类型为实体类

## POST请求应用场景

POST请求通常用于向服务器提交数据，例如在Web应用程序中向服务器提交表单数据。以下是一些POST请求的应用场景：

### 注册或登录

当用户填写注册或登录表单时，他们的用户名和密码等敏感信息需要通过POST请求发送到服务器。

### 发布博客或评论

当用户想要发布博客或评论时，他们需要通过POST请求将文本和其他相关信息发送到服务器。

### 购买商品

当用户在在线商店中购买商品时，他们需要通过POST请求向服务器发送订单和付款信息。

### 上传文件

当用户需要上传文件时，他们可以使用POST请求将文件发送到服务器。

### 执行操作

当用户需要执行某些操作时，例如更改密码、删除帐户等，他们可以使用POST请求向服务器发送请求。

总之，POST请求通常用于**向服务器提交数据并进行某些操作**。

## 需求：只有body参数

添加用户信息，请求路径为`/add`，用户信息为：`用户名：新增名，年龄：66`。

>需要提交具体的用户信息参数，则为POST请求。

### 分析

1. 添加用户为了安全性使用`POST`请求。

2. 请求路径`/add`，对应方法为`add()`。

3. 请求方法返回值类型为`String`。


### 步骤

添加用户，直接实体类作为对象。

```java
@PostMapping("/add")
public String add(@RequestBody UserDTO user) {
    userList.add(user);
    System.out.println(userList);
    return "ok";
}
```

postman验证：

![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230323183228.png)




## 需求：body参数&路径参数


## 需求：body参数&路径参数&查询参数
