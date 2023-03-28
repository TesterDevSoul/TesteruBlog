# @Qualifier

`@Qualifier` 注解用于在 `Spring` 容器中存在多个相同类型的 `Bean` 时，指定要注入的具体 `Bean`。

## 语法结构

语法结构如下：

```java
@Qualifier(value = "beanName")
```

### value 属性

`beanName` 是要注入的 `Bean` 的名称。

## 使用

**`@Qualifier` 注解通常与 `@Autowired` 注解一起使用**，如下所示：

```java
@Autowired
@Qualifier("beanName")
private MyBean myBean;
```

在这个示例代码中，我们使用 `@Autowired`注解将`MyBean`类型的 `Bean` 对象自动装配到`myBean`属性中，并使用`@Qualifier`注解指定要注入的具体`Bean`的名称为`beanName`。

`beanName`默认为注入类的名称首字母小写。


当有多个相同类型的 bean 时，最好使用 @Qualifier 来避免歧义。

注意，`@Qualifier` 注释的值与我们的 MyBean 实现类上的 @Service注释中声明的名称匹配。


## 需求

假设我们有一个接口`UserService` 和它的两个实现类 `UserServiceImpl1` 和 `UserServiceImpl2`，它们都被注入到了一个`Controller`类中。

### 实现

由于这两个实现类都是`UserService`的实现类，因此在`Controller`中无法区分要使用哪一个实现类，因此需要使用`@Qualifier`注解来区分。

#### 1. 接口创建

```java
@Service
public interface UserService {
    void addUser(String username);
}
```

#### 2. 实现类创建

我们分别为两个实现类添加 `@Service`和`@Qualifier` 注解，如下所示：

- 第一个实现类：

```java
@Service("userService1")
public class UserServiceImpl1 implements UserService {
    @Override
    public void addUser(String username) {
        // ...
    }
}
```

- 第二个实现类：

```java
@Service("userService2")
public class UserServiceImpl2 implements UserService {
    @Override
    public void addUser(String username) {
        // ...
    }
}
```

#### 3. 注入Bean

在 `Controller` 中使用 `@Autowired` 和 `@Qualifier` 注解注入 `UserService`类型的`Bean` 对象，并指定要注入的具体`Bean`的名称。

例如，如果我们要注入名为 `userService1` 的 `UserServiceImpl1`类型的`Bean` 对象，可以使用以下代码：

```java
@Autowired
@Qualifier("userService1")
private UserService userService;
```

这样，在应用程序启动时，`Spring`容器会自动扫描所有被`@Service`注解标记的类，并将它们作为`Bean`注册到应用程序上下文中。

在使用 `@Qualifier`注解时，我们可以指定要注入的`Bean`的名称。这样，在使用 `@Autowired`注解时，`Spring`容器就能够根据指定的`Bean`名称找到并注入正确的 `Bean`对象。