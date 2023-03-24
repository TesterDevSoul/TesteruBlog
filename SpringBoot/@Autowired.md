# @Autowired

一个`Service`接口的实现类，如何在`controller`层调用？？？


`@Autowired` 是 `Spring` 框架中的注解，它用于自动装配 `Bean` 对象。

具体来说，`@Autowired` 注解可以通过**类型**、**名称**、**限定符**等方式来确定要**自动装配**的 `Bean` 对象。明确地告诉 `Spring` 我们要注入哪个`Bean`对象。


## 语法结构

```java
@Autowired
private SomeBean someBean;
```

在这个语法结构中，`@Autowired` 注解用于将 `someBean` 对象自动装配到 `SomeBean` 类型的属性中。

当 `Spring` 容器启动时，它会自动扫描应用中所有带有 `@Autowired` 注解的 `Bean`，找到其类型所匹配的 `Bean` 对象，并将其自动装配到对应的属性中。

除了上述语法结构外，`@Autowired` 注解还支持一些属性，它们的含义如下：

### required 属性

指定该属性是否必须被装配，默认为 `true`。如果该属性的类型无法在 `Spring` 容器中找到对应的 `Bean` 对象且 `required` 属性为 `true`，则会抛出 `NoSuchBeanDefinitionException` 异常。


#### 案例

以下示例代码演示了如何使用 `@Autowired` 注解装配 `SomeBean` 类型的 `Bean` 对象，并通过 `required` 属性来指定该属性是否必须被装配：

```java
@Autowired(required = false)
private SomeBean someBean;
```

在这个示例代码中，`@Autowired` 注解的 `required` 属性被设置为 `false`，表示该属性不是必须被装配的。如果 `Spring` 容器中找不到 `SomeBean` 类型的 `Bean` 对象，那么 `someBean` 属性将会被设置为 `null`。


### value/name 属性

指定要装配的 `Bean` 对象的名称。当 `Spring` 容器中存在多个同一类型的 `Bean` 对象时，可以通过这个属性来指定要装配的 `Bean` 对象的名称。

### qualifier 属性

指定 `Bean` 对象的限定符，用于进一步区分多个同类型的 `Bean` 对象。如果存在多个同类型的 `Bean` 对象，且它们都具有相同的名称，可以通过 `qualifier` 属性来区分它们。

## 装配Bean对象逻辑

当使用 `@Autowired` 注解时，`Spring` 容器会在应用程序上下文中查找与该属性类型相同的 `Bean` 对象，并自动注入到该属性中。

### 多个Bean对象

如果应用程序上下文中**有多个与该属性类型相同的Bean对象**，Spring 容器会尝试根据一定的规则选择一个 Bean 对象进行注入。

**默认**情况下，Spring 容器会选择与**该属性类型相同的首个 Bean 对象进行注入**。

#### 容器查询Bean对象

1. 若查询结果刚好为一个，就将该`bean`装配给`@Autowired`指定的数据。

2. 若查询的结果不止一个，那`@Autowired`根据名称来查找。

    ![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230324171453.png)

3. 若查询的结果为空，则会抛出异常`NoUniqueBeanDefinitionException`。


### 指定Bean对象名称

如果要**选择指定的Bean对象**进行注入，可以使用 `@Qualifier` 注解指定 `Bean` 对象的名称。

### 手动查询Bean对象指定

还可以使用 `ApplicationContext` 接口提供的 `getBean()` 方法来手动查询`Bean`对象。使用该方法时，需要指定`Bean`对象的名称和类型。以下是一个查询`Bean`对象的示例代码：

```java
@Autowired
private ApplicationContext context;

public void doSomething() {
    MyBean myBean = context.getBean("myBeanName", MyBean.class);
    // ...
}
```

在这个示例代码中，我们使用 `@Autowired` 注解将 `ApplicationContext` 对象自动装配到 `context` 属性中。

然后，我们调用 `getBean()` 方法，传入 `myBeanName` 和 `MyBean` 类型，从而获取名为 `myBeanName` 的 `MyBean` 类型的 `Bean` 对象。




## 需求

如何在 REST 接口中使用 UserService 类。

### 实现

```java
@RestController
@RequestMapping("/users")
public class UserController {
    @Autowired
    private UserService userService;
    
    @GetMapping("/{userId}")
    public UserDTO getUser(@PathVariable Long userId) {
        User user = userService.getUserById(userId);
        return convertToDTO(user);
    }
    
    // ...
}
```
在这个示例代码中，我们在 `UserController` 类中定义了一个 `REST` 接口，它通过 `@Autowired` 注解将 `userService` 属性自动装配到 `UserService` 类型的 `Bean` 对象上。

在`getUser()`方法中，我们调用 `userService`对象中的`getUserById()`方法，从而获取指定`ID`的用户信息，并将其转换为`DTO`对象返回给客户端。