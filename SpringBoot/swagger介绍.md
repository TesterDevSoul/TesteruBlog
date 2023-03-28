
# Swagger布局

![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230327200850.png)

> **Swagger实例Bean是Docket，所以必须通过配置Docket实例来配置Swaggger。**

## API分组

第一部分--API分组：如果没有配置分组默认是default。通过Swagger实例Docket的`groupName()`方法即可配置分组 

## 基本描述

第二部分--基本描述：可以通过Swagger实例Docket的`apiInfo()`方法中的ApiInfo实例参数配置文档信息 

## 请求接口列表

第三部分--请求接口列表：在组范围内，只要被Swagger2扫描匹配到的请求都会在这里出现。 

## 实体列表

第四部分--实体列表：只要实体在请求接口的返回值上（即使是泛型），都能映射到实体项中！

注意：并不是因为`@ApiModel`注解让实体显示在`Models`列表里，而是只要出现在接口方法的返回值上的实体都会显示在这里，而`@ApiModel`和`@ApiModelProperty`这两个注解**只是为实体添加注释**的。

`@ApiModel`为**类**添加注释，`@ApiModelProperty`为类**属性**添加注释。

## Swagger常用注解

以下是一些常用的Swagger注解：

|swagger常用注解|说明|示例|
|---|---|---|
|@Api|用在请求的类上，表示对类的说明|
|@ApiOperation(value = "/2031")|用在请求类的方法上，说明方法的用途和作用|@ApiOperation(value = "Get user by ID", notes = "Returns a single user by their ID", response = User.class)|
|@ApiParam|可用在方法，参数和字段上，一般用在请求体参数上，描述请求体信息|getUsers(@ApiParam(value = "Page number", required = true, example = "1") @RequestParam int page)|
|@ApiResponses||@ApiResponses(value = {@ApiResponse(code = 201, message = "User created", response = User.class)})<br>@PostMapping("/users")<br>public ResponseEntity createUser|
|@ApiModel||@ApiModel(description = "User object")<br>public class User|
|@ApiModelProperty|描述属性，包括名称、类型、描述、是否必需等信息。|@ApiModelProperty(value = "User name", example = "John Doe", required = true)<br>private String name;


### @Api

用于描述整个API的信息，包括标题、描述和版本号等。

```java
@Api(value = "Example API", description = "This is an example API", tags = { "Example" }, produces = "application/json")
@RestController
public class ExampleController {
    // ...
}
```
### @ApiOperation

用于描述API的一个操作，包括HTTP方法、路径、参数、响应等信息。

```java
@ApiOperation(value = "Get user by ID", notes = "Returns a single user by their ID", response = User.class)
@GetMapping("/users/{id}")
public ResponseEntity<User> getUser(@PathVariable Long id) {
    // ...
}
```

### @ApiParam

用于描述一个操作的参数，包括名称、类型、是否必需、默认值等信息。

```java
@GetMapping("/users")
public ResponseEntity<List<User>> getUsers(
        @ApiParam(value = "Page number", required = true, example = "1") @RequestParam int page,
        @ApiParam(value = "Page size", required = true, example = "10") @RequestParam int size) {
    // ...
}
```

### @ApiResponses

用于描述API的响应，包括HTTP状态码、响应消息、响应类型等信息。

```java
@ApiOperation(value = "Create a new user", notes = "Creates a new user with the given details", response = User.class)
@ApiResponses(value = {
        @ApiResponse(code = 201, message = "User created", response = User.class),
        @ApiResponse(code = 400, message = "Invalid input", response = ErrorResponse.class),
        @ApiResponse(code = 500, message = "Internal server error", response = ErrorResponse.class)
})
@PostMapping("/users")
public ResponseEntity<User> createUser(@RequestBody CreateUserRequest request) {
    // ...
}
```

### @ApiModel

用于描述一个数据模型，包括名称、描述和属性等信息。

```java
@ApiModel(description = "User object")
public class User {
    @ApiModelProperty(value = "User ID", example = "1")
    private Long id;

    @ApiModelProperty(value = "User name", example = "John Doe")
    private String name;

    // ...
}
```

### @ApiModelProperty

用于描述一个数据模型的属性，包括名称、类型、描述、是否必需等信息。

```java
@ApiModel(description = "Create user request")
public class CreateUserRequest {
    @ApiModelProperty(value = "User name", example = "John Doe", required = true)
    private String name;

    @ApiModelProperty(value = "User email", example = "john.doe@example.com", required = true)
    private String email;

    // ...
}
```

### @ApiIgnore

用于忽略某个API或API的某个操作。

```java
@ApiIgnore
@GetMapping("/ignored")
public void ignoredApi() {
    // ...
}
```

### @ApiImplicitParam

用于描述隐式参数，如HTTP头部、请求体等。

```java
@ApiImplicitParams({
    @ApiImplicitParam(name = "Authorization", value = "Authorization header", required = true, dataTypeClass = String.class, paramType = "header"),
    @ApiImplicitParam(name = "request", value = "Request body", required = true, dataTypeClass = CreateUserRequest.class, paramType = "body")
})
@PostMapping("/users")
public ResponseEntity<User> createUserWithHeadersAndBody(@RequestBody CreateUserRequest
```

### @ApiImplicitParams

用于描述多个隐式参数。

以上是一些常用的`Swagger`注解，还有其他注解可用于自定义`Swagger`文档。这些注解的使用可以大大简化`API`文档的编写和维护。


## 不显示

`Springfox3.0`默认用`swagger v3`来返回信息，但自定义的@ApiResponses不显示，需要要在配置文件application.properties里加上一句：

```css
springfox.documentation.swagger.v2.use-model-v3=false
```

![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230327215602.png)



