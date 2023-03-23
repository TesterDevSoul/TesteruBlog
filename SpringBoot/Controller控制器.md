# Controller控制器

`SpringBoot`项目已经都整合了`SpringMVC`，我们可以直接创建一个用于`Web`访问`Controller`控制器。

`SpringBoot`中的`Controller`控制器是一个用来处理`HTTP`请求的`Java`类。控制器通过处理请求并返回响应，来实现对`Web`应用程序的控制和管理。

在`SpringBoot`中，控制器通常使用**注解**的方式进行配置。


## @RestController

`@RestController` 是 `Spring Framework` 中用于构建 `RESTful web` 服务的注解。它是 `@Controller` 注解的一种特殊版本，用于指示一个特定的类是 `RESTful web` 服务控制器。

`@RestController`注解为组合注解，等同于`Spring`中 **`@Controller`+`@ResponseBody`** 注解。通常被用于构建 `Web API`，提供可靠和可扩展的服务。

### 语法

```java
@RestController(value="/路径")
```

#### value属性

指定映射的 **`URL`路径** ，可以是**一个字符串**或者**一个字符串数组**。

注意⚠️：一旦类上添加该注解，在当前类下的所有方法的请求都需要先拼接该路径。

## 需求

浏览器获取当前用户信息，请求路径为`/user`，用户信息为：`用户名：张三，年龄：18`。

### 分析

1. 浏览器发送请求路径，页面显示内容。
   >请求为`GET`请求。

2. 对应请求只有路径，没有参数。

3. 请求返回值是`String`的字段。

### 步骤

#### 1. 创建controller包

启动类`Application`所在的`top.testeru.mini`包下创建名称为
**`controller`** 的包。

![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230321160629.png)

#### 2. 创建User控制类

在`controller`包下创建一个用户请求控制类`UserController`。

![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230321160939.png)


#### 3. 编写无参的GET请求

[无参数传入的GET请求并且返回值类型为String。](GET请求-无参&String返回值.md)

```java
@RequestMapping("/user")
public String get(){
    return "用户名：张三，年龄：18";
}
```

#### 4. 运行项目 

运行主程序启动类`MiniApplication`，项目启动成功后，在控制台上会发现 `Spring Boot` 项目默认启动的端口号为 `8080`。

![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230321162216.png)


#### 5. 浏览器验证

浏览器上访问 [http://127.0.0.1:8080/user](http://127.0.0.1:8080/user)。

![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230321162306.png)

页面输出的内容是：**用户名：张三，年龄：18**。


## 问题

#### 为何controller要是启动类的子包？

启动类在启动时会做**注解扫描**(`@Controller`、`@Service`、`@Repository`...)。

启动类扫描**位置**为**同包或者子包下的注解**，所以要在启动类`Application`同级或同级包下编写代码。

#### 每次更改代码后都要重启项目？

在开发测试过程中发现，如果每次更改了业务代码后对应的项目都要重新启动运行，否则更改的代码不生效。

解决方案：使用[热部署](热部署.md)来解决每次更改完代码后都要重启项目的问题。


# 学习反馈

1. 在`SpringBoot`项目中，`Java`业务代码要写在( )。

   - [ ] A. 启动类同包下
   - [ ] B. 启动类同级包下
   - [x] C. 启动类同包下或同级包下
   - [ ] D. 以上都不正确

2. 控制类上需要添加的注解是( )。

   - [x] A. @RestController
   - [ ] B. @RequestMapping
   - [ ] C. @Service
   - [ ] D. @Configuration