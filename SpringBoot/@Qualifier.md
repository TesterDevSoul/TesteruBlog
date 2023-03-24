# @Qualifier

`@Qualifier` 注解用于在 `Spring` 容器中存在多个相同类型的 `Bean` 时，指定要注入的具体 `Bean`。

## 语法结构

语法结构如下：

```java
@Qualifier(value = "beanName")
```

### value 属性

`beanName` 是要注入的 `Bean` 的名称。

### 

**`@Qualifier` 注解通常与 `@Autowired` 注解一起使用**，如下所示：

```java
@Autowired
@Qualifier("beanName")
private MyBean myBean;
```