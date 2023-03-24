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

## 需求

### 实现
