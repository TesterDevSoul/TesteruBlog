# SpringBoot基本应用

## 需求V1.0：项目创建访问

搭建`Spring Boot`工程，实现`web`的请求响应。

浏览器访问在页面中输出：`用户名：张三，年龄：18` 。

![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230321190805.png)

### 分析

1. 浏览器发送请求路径，页面显示内容   --->   请求为`GET`请求。

2. 对应请求只有路径，没有参数。

3. 请求返回值是`String`的字段。

4. `springboot` 服务启动端口为默认端口`8080`，不需要改配置文件。


### 实现步骤

1. 创建`Spring Boot`工程项目。

2. `pom.xml` 文件中配置相关依赖。

3. 创建与前端请求交互的控制器包 `controller`。

   - 使用**2种**不同的注解实现。

4. 在 `controller` 包下创建与`/user`请求交互的类`UserController`。

5. 在 `UserController` 类内编写`get()`请求方法。

6. 在 `get()` 方法上添加`GET`请求的注解及其请求路径参数。
   
7. 启动服务，浏览器访问 [http://localhost:8080/user](http://localhost:8080/user) 页面验证。


### 实现过程

1. [创建Spring Boot工程项目及配置相关依赖](项目创建.md)。

2. [了解项目结构](项目结构.md)。
   >在写代码之前，需要对当前项目有一个结构化的了解，才能继续编写对应的Java类。

3. [编写控制器Controller](Controller控制器.md)。

4. [编写GET的请求方法](GET请求-无参&String返回值.md)。
   
5. 浏览器输入地址访问查看。


## 优化：项目端口号冲突

电脑/服务器 已经`把SpringBoot`默认启动的端口号`8080`占用，当项目启动时报错。

### 解决方案

需要更改SpringBoot项目的默认启动端口号，比如修改为8888。

### 实现过程

[在全局配置文件中修改项目的端口号。](全局配置文件.md)

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

[没有参数传入的GET请求，设置返回值类型为Json类型。](GET请求-无参&Json返回值.md)


## 需求V1.1：根据不同姓名显示对应用户信息

有多个用户时，根据传入的`name`参数确定展示对应用户信息。

### 实现步骤

可以把`name`作为参数进行传入，不同`name`展示不同的用户信息。

1. 创建带参数的请求`/user`。「至少两种方式」
   - http://localhost:8080/user/{name}
   - http://localhost:8080/user?name={uname}
  
2. 访问请求页面查看。



### 实现过程

[有一个类型为基本数据类型或String的参数传入的GET请求。](GET请求-有参单个&实体类返回值.md)




## 需求V1.2：增加用户信息

增加用户信息，如果成功返回OK。

### 实现步骤


1. 在 `UserController`类内编写`add()`请求方法。

2. 在 `add()` 方法上添加`POST`请求的注解及其请求路径参数`/user/add`。

3. 添加成功对应返回ok。

### 实现过程

[编写POST请求](POST请求-有参多个&实体类返回值.md)。


## 需求V1.3：修改用户信息

根据用户姓名修改对应的用户姓名、年龄。

### 实现步骤

可以把`name`作为参数进行传入，根据用户姓名修改用户的**姓名**、**年龄**。

1.  在 `UserController` 类内编写`update()`请求方法。

2.   在 `update()` 方法上添加`POST`请求的注解及其请求路径参数`/user/update`。「至少两种方式」
   - http://localhost:8080/user/update/{name}
   - http://localhost:8080/user/update?name={uname}

3. 访问请求页面查看。



### 实现过程

[编写POST请求](POST请求-有参单个&实体类返回值.md)。


## 优化：业务逻辑拆分

所有的请求相关的业务逻辑不能都写在`controller`的类内，如果业务太复杂会导致代码太多。

### 解决方案

把请求相关业务单独拿出来创建一个业务层。

### 实现过程

[业务层新建拆分。](Service层.md)

