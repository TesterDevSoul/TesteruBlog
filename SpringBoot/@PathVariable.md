
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
