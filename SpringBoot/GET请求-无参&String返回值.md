# GET请求 - 没有参数并且返回值类型为String


## 实现方式
可以使用两种注解进行实现：
[@RequestMapping](@RequestMapping.md)、
[@GetMapping](@GetMapping.md)。

## 需求

浏览器获取当前用户信息，请求路径为`/user`，用户信息为：`用户名：张三，年龄：18`。

>无需提交参数，并且为获取数据请求，则为GET请求。

### 分析

1. 浏览器发送请求路径，页面显示内容。
   >请求为`GET`请求，请求方法添加注解。

2. 请求路径`/user`，对应方法为`getUser()`。

3. 请求方法返回值类型为`String`。


### 步骤

无参且返回值为String类型。

```java
@RestController
public class UserController {
    @RequestMapping("/user")
    public String getUser(){
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