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


## 注意

使用`@Autowired`注入成员变量的好处是可以**省略构造方法**或者**`setter`方法**，使代码更加简洁易懂，提高了开发效率。此外，注入的成员变量可以直接访问，不需要再通过方法来获取。

然而，过多的直接注入成员变量可能会导致代码可读性降低，因为它使依赖关系变得不明显。

此外，使用`@Autowired`注入成员变量可能会导致代码的可测试性下降，因为测试过程中无法控制注入的对象。因此，最好将`@Autowired`注入的对象声明为`private`，同时提供**构造方法**或 **`setter`方法**，以便进行**依赖注入**和**测试**。



### 构造方法
```java
private final SomeService someService;

@Autowired
public AnotherService(@Qualifier("someServiceImpl1") SomeService someService) {
    this.someService = someService;
}
```
### setter方法

@Autowired注解可以通过setter方法来进行依赖注入。这种方式的好处是可以更加灵活地进行注入，因为可以根据不同的情况来调用不同的setter方法进行注入。此外，这种方式也更容易进行测试，因为可以通过setter方法来注入不同的mock对象进行测试。


```java
@Service
public class UserServiceImpl implements UserService {

    private UserRepository userRepository;

    @Autowired
    public void setUserRepository(@Qualifier("userService1Impl") UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    // 其他方法省略...
}
```