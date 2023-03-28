# 基本请求

|名称|备注|
|---|---|
|1. [SpringBoot基本应用](SpringBoot基本应用.md)|主线流程|
|1.1 [项目创建](项目创建.md)|Spring Initializr|
|1.2 [项目结构](项目结构.md)|pom.xml;<br>@SpringBootApplication;<br>命令行启动|
|1.3 [热部署](热部署.md)|Spring Boot DevTools|
|1.4 [全局配置文件](全局配置文件.md)|properties;<br>yaml/yml;|
|1.5 [Controller](Controller控制器.md)|controller包;<br>@RestController|
|1.6 [GET请求](GET请求应用场景.md)|GET请求应用场景|
|1.6.1 [GET请求-无参&String返回值](GET请求-无参&String返回值.md)|@RequestMapping;<br>@GetMapping;|
|1.6.1.1 [@RequestMapping](@RequestMapping.md)|@RequestMapping属性介绍|
|1.6.1.2 [@GetMapping](@GetMapping.md)|@GetMapping属性介绍|
|1.6.2 [GET请求-无参&Json返回值](GET请求-无参&Json返回值.md)|返回类型为Json的优点|
|1.6.2.1 [实体类对应包](实体类对应包.md)|dto、dao、entity|
|1.6.2.2 [业务实体类](业务实体类.md)|dto包|
|1.6.2.3 [Lomobok](Lombok插件.md)|@Getter;<br>@Setter;<br>@ToString;|
|1.6.3 [GET请求-有参&实体类返回值](GET请求-有参&实体类返回值.md)|路径参数@PathVariable;<br>查询参数@RequestParam;|
|1.6.3.1 [@PathVariable](@PathVariable.md)|路径参数@PathVariable属性介绍|
|1.6.3.2 [@RequestParam](GET请求-@RequestParam.md)|查询参数@RequestParam属性介绍;|
|1.7 [POST请求](POST请求应用场景.md)|POST请求应用场景|
|1.7.1 [POST请求-body&实体类返回值](POST请求-body&实体类返回值.md)||
|1.7.2 [POST请求-body有参&实体类返回值](POST请求-body有参&实体类返回值.md)||
|1.8 [请求注解总结](请求注解总结.md)||

# 进阶

|名称|备注|
|---|---|
|2.1 [Service层](Service层.md)|优点|
|2.1.1 [@Service注解](@Service.md)|必须放在实现类上|
|2.2 [@Autowired注解](@Autowired.md)|注入接口对象|
|2.2.1 [@Qualifier](@Qualifier.md)|指定要注入的具体 Bean|
|2.2.2 [@Primary](@Primary.md)||
|2.3 [多Bean注入总结](多Bean注入总结.md)||
|2.4 [统一响应模版](统一响应模版.md)||
|2.5 [统一异常处理](统一异常处理.md)||
|2.6 [SwaggerAPI生成](SwaggerAPI生成.md)||
|2.6.1 [swagger介绍](swagger介绍.md)||

swagger介绍

# SpringBoot与MyBatis集成

|名称|备注|
|---|---|
|2.1 [MyBatisGenerator](MyBatisGenerator.md)|插件生成数据库Mapper代码|



# 用例实战
[](用例.md)