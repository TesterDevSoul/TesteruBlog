# @Service

`@Service`主要作用是将一个类标记为**服务层组件**，从而允许 `Spring` 自动管理和装配该组件。

通常情况下，服务层组件是应用程序中的**核心组件**，它们负责处理业务逻辑、操作数据、管理事务等任务。

`@Service` 注解就是一个`@Component`，告诉 Spring 这个类将成为一个 @Component 并用一个特殊的构造型标记它，
Spring 将从类路径中自动检测这些类，并自动将它们定义为托管 bean。


## 好处

使用 @Service 注解可以带来以下好处：

1. 将服务层组件与其他层进行解耦，从而提高代码的可维护性和可测试性；

2. 允许 Spring 自动扫描和管理服务层组件，简化配置和部署流程；

3. 通过依赖注入（DI）和控制反转（IOC）机制，提供更好的灵活性和可扩展性。


除了 @Service 注解外，Spring 还提供了一些其他的注解，例如 @Repository、@Controller 等，用于标记其他类型的组件，并帮助 Spring 自动管理和装配它们。

## 语法结构

```java
@Service(value="serviceBeanName")
public class MyService {
    // service implementation
}
``` 
将`@Service`注解放在接口上没有任何效果，
并且只有具体类在使用`@Service`注解时才会被`Spring`组件扫描拾取。

### value 属性

该属性为可选择的，用于指定 `Spring` 容器中服务层组件的 `bean` 名称。

如果**没有显式指定**，则使用默认的 `bean` 名称，即**类名的第一个字母小写**。

如果**指定了`bean`名称**，则可以通过该名称在`Spring`**容器中查找和引用该组件**。

## 使用步骤

1. 创建实体类。

   >Order。

2. 创建Service接口。

    >OrderServic接口。

3. 创建接口实现类ServiceImpl，并添加@Service。
   >创建一个 OrderServiceImpl 类，实现 OrderService 接口，并使用 @Service 注解标记该类作为服务层组件。

## 需求

假设我们有一个订单服务，需要实现以下业务逻辑：

- 创建新订单；
- 查询订单详情；
- 更新订单状态；
- 删除订单。

### 实现

我们可以通过以下步骤使用 `@Service` 注解来实现这个服务：

#### 1. 实体类创建

创建一个 `Order` 实体类，用于表示订单对象：
   
```java
public class Order {
    private Long orderId;
    private Long customerId;
    private Date orderDate;
    private Long productId;
    private int quantity;
    private double price;

    // getters and setters
}
```

#### 2. 订单服务接口创建

创建一个 `OrderService` 接口，用于定义订单服务的接口方法：

```java
public interface OrderService {
    Order createOrder(Order order);
    Order getOrder(Long orderId);
    void updateOrder(Long orderId, String status);
    void deleteOrder(Long orderId);
}
```

#### 3. 订单服务接口实现类创建

创建一个 `OrderServiceImpl` 类，实现 `OrderService` 接口，并使用 `@Service` 注解标记该类作为服务层组件：

```java
@Service
public class OrderServiceImpl implements OrderService {
    private Map<Long, Order> orderStore = new HashMap<>();
    private AtomicLong orderIdCounter = new AtomicLong();

    public Order createOrder(Order order) {
        order.setOrderId(orderIdCounter.incrementAndGet());
        orderStore.put(order.getOrderId(), order);
        return order;
    }

    public Order getOrder(Long orderId) {
        return orderStore.get(orderId);
    }

    public void updateOrder(Long orderId, String status) {
        Order order = orderStore.get(orderId);
        order.setStatus(status);
    }

    public void deleteOrder(Long orderId) {
        orderStore.remove(orderId);
    }
}
```

在 `OrderServiceImpl` 类中，我们使用了 `@Service` 注解标记该类作为服务层组件，并实现了 `OrderService` 接口中定义的四个方法。

其中，`createOrder()` 方法用于**创建**新订单，`getOrder()` 方法用于**查询**订单详情，`updateOrder()` 方法用于**更新**订单状态，`deleteOrder()` 方法用于**删除**订单。

#### 4. 控制层注入

在控制层（`Controller`）中使用 `@Autowired` 注解注入 `OrderService` 组件，并调用其接口方法：

```java
@RestController
public class OrderController {
    @Autowired
    private OrderService orderService;

    @PostMapping("/orders")
    public Order createOrder(@RequestBody Order order) {
        return orderService.createOrder(order);
    }

    @GetMapping("/orders/{orderId}")
    public Order getOrder(@PathVariable Long orderId) {
        return orderService.getOrder(orderId);
    }

    @PutMapping("/orders/{orderId}")
    public void updateOrder(@PathVariable Long orderId, @RequestParam String status) {
        orderService.updateOrder(orderId, status);
    }

    @DeleteMapping("/orders/{orderId}")
    public void deleteOrder(@PathVariable Long orderId) {
        orderService.deleteOrder(orderId);
    }
}
```

在 `OrderController` 类中，我们使用了 `@Autowired` 注解注入 `OrderService` 组件，并定义了四个 `RESTful API` 接口方法，分别用于创建新订单、查询订单详情、更新订单状态和删除订单。

在每个方法中，我们都调用了 `OrderService` 接口中相应的方法来实现业务逻辑。

## 注意

**@Service 注解不可以放在接口上**。如果将 `@Service` 注解放在接口上，则实际上并没有将服务层组件实现标记为 `Spring` 容器的 `Bean`，这会导致无法通过**依赖注入**来使用该组件。

如果`@Service` 注解放在接口上并依赖注入该组件，项目启动报错`NoSuchBeanDefinitionException`，如下：
```
2023-03-24T15:42:51.122+08:00  WARN 7613 --- [  restartedMain] ConfigServletWebServerApplicationContext : Exception encountered during context initialization - cancelling refresh attempt: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'userController': Unsatisfied dependency expressed through field 'userService': No qualifying bean of type 'top.testeru.mini.service.UserService' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
```


![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230324154913.png)



`@Service` 注解应该放在实现类上，而不是接口上。

因为 `@Service` 注解是用来标记服务层组件的，而服务层组件的实现是在实现类中完成的，因此将 `@Service` 注解放在实现类上更符合实际情况，也可以在启动时被Spring当作组件扫描到容器中。

