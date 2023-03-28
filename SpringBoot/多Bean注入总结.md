# 多Bean注入总结

在 `Spring` 中，多个实现类都实现了同一个接口时，我们可以使用 **`@Autowired`**、**`@Qualifier`**、**`@Primary`** 等注解来注入实现类，从而达到多**个实现类注入**的目的。

### @Autowired

自动注入一个实现类的实例，但是如果有多个实现类的实例时，会抛出 `NoUniqueBeanDefinitionException` 异常，需要结合 **`@Qualifier`** 或 **`@Primary`** 来使用。

### @Qualifier

配合`@Autowired` 注解使用，**指定要注入的实现类的名称或别名**。

### @Primary

为多个实现类中的某个实现类标记为默认实现，当使用 `@Autowired` 注解注入时，**如果没有指定名称或别名，则默认注入 `@Primary` 标记的实现类**。


注意⚠️：如果既有 **`@Qualifier`** 又有 **`@Primary`**，优先使用的是`@Qualifier`，因为`@Qualifier`在注入的声明时使用。


## 多个实现类接口

### 接口

以下是多个实现类的接口定义：

```java
public interface SomeService {
    void doSomething();
}
```

### 实现类1

以下是两个实现类：

```java
@Service
public class SomeServiceImpl1 implements SomeService {
    @Override
    public void doSomething() {
        System.out.println("Doing something in SomeServiceImpl1");
    }
}
```

### 实现类2

```java

@Service
public class SomeServiceImpl2 implements SomeService {
    @Override
    public void doSomething() {
        System.out.println("Doing something in SomeServiceImpl2");
    }
}
```

### 指定别名注入实现 - @Qualifier

我们可以使用 `@Autowired`和 `@Qualifier`来注入某个实现类：

```java
@Service
public class AnotherService {

    private final SomeService someService;

    @Autowired
    public AnotherService(@Qualifier("someServiceImpl1") SomeService someService) {
        this.someService = someService;
    }

    public void doSomethingElse() {
        someService.doSomething();
    }
}
```

在上面的例子中，我们使用 `@Qualifier` 指定要注入的实现类的名称为 "`someServiceImpl1`"，表示注入 `SomeServiceImpl1`实现类的实例。当我们调用`doSomethingElse()`方法时，将会调用`SomeServiceImpl1`中的 `doSomething()`方法。

### 优先级注入实现 - @Primary

如果我们希望将 `SomeServiceImpl1` 标记为默认实现类，可以使用 `@Primary` 注解：

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

现在，当我们使用 `@Autowired` 注解注入 `SomeService` 时，如果没有指定名称或别名，则默认注入`SomeServiceImpl1` 实现类的实例。

```java
@Service
public class AnotherService {

    private final SomeService someService;

    @Autowired
    public AnotherService(SomeService someService) {
        this.someService = someService;
    }

    public void doSomethingElse() {
        someService.doSomething();
    }
}
```

在上面的例子中，当我们调用 `doSomethingElse()` 方法时，将会调用 `SomeServiceImpl1` 中的 `doSomething()` 方法，因为它被标记为默认的实现。



