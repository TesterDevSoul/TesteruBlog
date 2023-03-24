# 查询参数：@RequestParam

用于将`HTTP`请求参数绑定到方法的参数上。

下面是`@RequestParam`注解的语法和使用方法：

## 语法

请求路径：`/请求路径?参数名1=方法参数值1&参数名2=方法参数值2`。

```java
@GetMapping("/请求路径")
public UserDTO get12(@RequestParam(value = "参数名1", required = true/false, defaultValue = "默认值") Long 方法参数1,
@RequestParam(value = "参数名2", required = true/false, defaultValue = "默认值") String 方法参数2){...}
```

### value属性

请求参数的名称，必须要指定，可以使用别名。

### required属性

是否必须提供该请求参数，默认为`true`，即必须提供。


### defaultValue属性

如果请求中未提供该参数，则使用默认值。

## 示例

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


