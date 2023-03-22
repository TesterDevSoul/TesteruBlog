# 初步搭建SpringBoot工程

## 需求：项目创建访问

搭建`Spring Boot`工程，实现`web`的请求响应。

浏览器访问在页面中输出：`用户名：张三，年龄：18` 。

![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230321190805.png)

### 实现步骤

1. 创建`Spring Boot`工程项目。
2. `pom.xml` 文件中配置相关依赖。
3. 编写`Controller`。「使用**2种**不同的注解实现」
4. 访问[http://localhost:8080/user](http://localhost:8080/user)页面查看。


### 实现过程

1. [创建工程](项目创建.md)。

2. [了解项目结构](项目结构.md)。
   >在写代码之前，需要对当前项目有一个结构化的了解，才能继续编写对应的Java类。

3. [编写控制器Controller](Controller控制器.md)。

4. 浏览器输入地址访问查看。

## 优化：项目端口号冲突

电脑/服务器 已经`把SpringBoot`默认启动的端口号`8080`占用，当项目启动时报错。

### 解决方案

需要更改SpringBoot项目的默认启动端口号，比如修改为8888。

### 实现过程

[修改端口](全局配置文件.md)

```python
#application.properties文件以下配置
server.port=8888
```

![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230322120039.png)

## 优化：数据返回格式为Json

前面的返回值类型为`String`类型，前端获取的时候无法通过`key`获取对应`value`值。还需要对字符串进行切割获取，麻烦。

### 解决方案

可以返回Map类型数据，但是如果key太多，需要每个返回都要写put，并且key值不能出错。

最优解决方案：创建返回的**实体类**，对应请求注解内标明返回值类型为Json类型。


### 实现过程

[修改端口](全局配置文件.md)


Map示例代码：
```java
@GetMapping(path = "/user",
                value = "/user",
                produces = MediaType.APPLICATION_JSON_VALUE)
public HashMap<String, String> get4(){
   HashMap<String, String> userMap = new HashMap<>(){{
      put("用户名3","张三");
      put("年龄3","18");
   }};
   return userMap;
}
```

