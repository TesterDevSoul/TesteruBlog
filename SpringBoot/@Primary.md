# @Primary

作用是指定一个默认的 `Bean` 实例。

当一个接口有多个实现类时，使用 `@Primary` 注解可以**指定一个默认的实现类**。

当容器中存在多个同类型的 `Bean` 实例时，使用 `@Autowired` 注解时会优先选择 `@Primary` 注解指定的`Bean`实例。

使用 `@Primary` 注解时，我们需要确保只有一个`Bean`被标记为`@Primary`。如果存在多个 `@Primary`标记的`Bean`，将会导致应用程序启动失败。

## 语法结构

```java
@Service
@Primary
public class UserServiceImpl implements UserService {...}
```

## 使用
通过 `Java` 配置文件配置 `@Primary`
可以通过在 `Bean` 的 `@Bean` 注解上添加 `@Primary` 注解来配置默认 `Bean`。例如：

```java
@Configuration
public class AppConfig {

    @Bean
    @Primary
    public SomeBean someBeanImpl() {
        return new SomeBeanImpl();
    }

    @Bean
    public SomeBean primaryBean() {
        return new PrimaryBean();
    }

    // ...
}
```

在上面的示例中，我们将`someBeanImpl()`方法标记为默认`Bean`，并将`primaryBean()`方法标记为另一个`Bean`。

需要注意的是，当使用`@Primary`注解时，应该考虑到 `Bean`的作用范围。

如果`Bean`的作用范围是`Singleton`，则`@Primary` 注解将在整个应用程序中生效。如果`Bean`的作用范围是 `Prototype`，则`@Primary`注解将只在`Bean`的作用域内生效。

因此，在使用`@Primary` 注解时，我们应该仔细考虑 `Bean`的**作用范围**以及其在应用程序中的作用。


## 需求

假设我们有一个接口`SomeService`和两个实现类 `SomeServiceImpl1`和 `SomeServiceImpl2`，我们想要在注入`SomeService`的时候优先选择 `SomeServiceImpl1`。

### 实现

可以使用`@Primary`注解进行优先选择：


#### 1. 接口创建

```java
public interface SomeService {
    void doSomething();
}
```

#### 2. 实现类创建

- 第一个实现类：

```java
@Service
@Primary
public class SomeServiceImpl1 implements SomeService {
    @Override
    public void doSomething() {
        System.out.println("Doing something in SomeServiceImpl1");
    }
}
```

- 第二个实现类：

```java
@Service
public class SomeServiceImpl2 implements SomeService {
    @Override
    public void doSomething() {
        System.out.println("Doing something in SomeServiceImpl2");
    }
}
```

在上面的例子中，我们使用`@Primary`注解标记了`SomeServiceImpl1`类，表示当注入`SomeService`时优先选择`SomeServiceImpl1`。

#### 3. 注入Bean

接下来，我们可以在另一个类中使用 `@Autowired` 注解注入`SomeService`，并调用`doSomething()`方法：

```java
private final SomeService someService;

@Autowired
public AnotherService(SomeService someService) {
    this.someService = someService;
}

public void todo() {
    someService.doSomething();
}
```

当我们调用`todo()`方法时，将会调用`SomeServiceImpl1`中的`doSomething()`方法，因为它被标记为默认的实现。