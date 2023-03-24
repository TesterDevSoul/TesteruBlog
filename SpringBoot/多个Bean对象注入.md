
## 装配Bean对象逻辑

当使用 `@Autowired` 注解时，`Spring` 容器会在应用程序上下文中查找与该属性类型相同的 `Bean` 对象，并自动注入到该属性中。

### 多个Bean对象装配

如果应用程序上下文中**有多个与该属性类型相同的Bean对象**，`Spring` 容器会尝试根据一定的规则选择一个 `Bean` 对象进行注入。

**默认**情况下，Spring 容器会选择与**该属性类型相同的首个 Bean 对象进行注入**。

### 指定Bean对象名称

如果要**选择指定的Bean对象**进行注入，可以使用 `@Qualifier` 注解指定 `Bean` 对象的名称。

### Bean对象指定优先级

如果要**指定的Bean对象**进行注入，可以使用 `@Primary` 注解声明在具体 `Bean`类上。

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






#### 容器查询Bean对象

1. 若查询结果刚好为一个，就将该`bean`装配给`@Autowired`指定的数据。

2. 若查询的结果不止一个，那`@Autowired`根据名称来查找。

    ![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230324171453.png)

3. 若查询的结果为空，则会抛出异常`NoUniqueBeanDefinitionException`。

