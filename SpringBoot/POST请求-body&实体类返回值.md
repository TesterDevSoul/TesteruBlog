# POST请求 - 有Body参数并且返回值类型为实体类

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




