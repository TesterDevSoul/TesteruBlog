# SwaggerAPI生成

Swagger是一种API文档工具，可以生成易于阅读和交互的API文档。它可以帮助开发者自动生成API文档，并提供一个UI界面，可以用于测试和验证API接口。

>前后端分离开发更加方便，有利于团队协作。<br>接口文档在线自动生成，降低后端开发编写接口文档负担

除此之外，Swagger还提供了一些高级功能，如API测试、调试、部署和监视等。使用Swagger可以提高API开发效率，减少沟通成本，提高项目交付速度。

## 步骤

2.*版本的才可使用springfox；如果是3.*使用 [springdoc-openapi v2](https://springdoc.org/v2/)。 



提到Swagger，我们一般在SpringBoot中集成的都是springfox给我们提供的工具库，看了下官网，该项目已经快两年没有发布新版本了。

再看下Maven仓库中的版本，依旧停留在之前的`3.0.0`版本。springfox不出新版本的话，估计随着SpringBoot版本3.*的更新，已经开始使用另外一个[springdoc-openapi v2](https://springdoc.org/v2/) 


Swagger 生成 API 文档的一般步骤：

### 1. 添加依赖

在 `Spring Boot` 项目的 `pom.xml`文件中添加 `Swagger` 依赖：

#### 旧版本
- 如果springboot是旧版本用这种：
```xml
<dependency>  
    <groupId>io.springfox</groupId>  
    <artifactId>springfox-swagger-ui</artifactId>  
    <version>2.9.2</version>  
</dependency>  
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->  
<dependency>  
    <groupId>io.springfox</groupId>  
    <artifactId>springfox-swagger2</artifactId>  
    <version>2.9.2</version>  
</dependency>
```

#### 2.*的新版本

- 如果Springboot是新版本，使用以下一个依赖替换：

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-boot-starter</artifactId>
    <version>3.0.0</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>${swagger.version}</version>
</dependency>
```


### 2. Swagger配置类

创建一个 `Swagger` 配置类，用于配置 `Swagger`：


- 旧版本通过 **@EnableSwagger2** 扫描那些注解，生辰个相关的接口文档。

- 新版本只需要 **@Configuration** 注解即可。
  >我们使用的是新版本，所以直接在类上添加`@Configuration`注解。

```java
@Configuration
public class SwaggerConfiguration {
    @Bean
    public static BeanPostProcessor springfoxHandlerProviderBeanPostProcessor() {
        return new BeanPostProcessor() {

            @Override
            public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
                if (bean instanceof WebMvcRequestHandlerProvider || bean instanceof WebFluxRequestHandlerProvider) {
                    customizeSpringfoxHandlerMappings(getHandlerMappings(bean));
                }
                return bean;
            }

            private <T extends RequestMappingInfoHandlerMapping> void customizeSpringfoxHandlerMappings(List<T> mappings) {
                List<T> copy = mappings.stream()
                        .filter(mapping -> mapping.getPatternParser() == null)
                        .collect(Collectors.toList());
                mappings.clear();
                mappings.addAll(copy);
            }

            @SuppressWarnings("unchecked")
            private List<RequestMappingInfoHandlerMapping> getHandlerMappings(Object bean) {
                try {
                    Field field = ReflectionUtils.findField(bean.getClass(), "handlerMappings");
                    field.setAccessible(true);
                    return (List<RequestMappingInfoHandlerMapping>) field.get(bean);
                } catch (IllegalArgumentException | IllegalAccessException e) {
                    throw new IllegalStateException(e);
                }
            }
        };
    }



    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                // 页面标题
                .title("aitest-mini系统")
                // 描述
                .description("aitest-mini接口文档")
                // 创建人信息
                .contact(new Contact("gaigai", "", "gaigai@ceshiren.com"))
                // 项目API版本号
                .version("1.0.0")
                .build();
    }


    @Bean
    public Docket docket() {
        //Swagger 的配置主要围绕Docket bean 进行：
        return new Docket(DocumentationType.OAS_30)
                //配置是否启用Swagger，如果是false，在浏览器将无法访问，默认是true
                .enable(true)
                .groupName("aitestssm_interface")
                .apiInfo(apiInfo())
//                .globalRequestParameters(globalRequestParameters())
//在定义Docket bean 之后，它的select()方法返回一个ApiSelectorBuilder的实例，它提供了一种控制 Swagger 暴露的端点的方法
                .select()
                //any 任何请求都扫描  none   任何请求都不扫描
//                .apis(RequestHandlerSelectors.any())
                  .apis(RequestHandlerSelectors.basePackage("com.ceshiren.aitestmini"))
                .paths(PathSelectors.any()).build();
//我们可以在RequestHandlerSelectors和PathSelectors的帮助下配置用于选择RequestHandler的谓词。
// 对两者都使用any()将使我们的整个 API 的文档可以通过 Swagger 获得。
    }

    //生成全局通用参数
    private List<RequestParameter> globalRequestParameters() {
        List<RequestParameter> parameters = new ArrayList<>();

        //   公共请求参数生成器-token
        RequestParameter tokenParameter = new RequestParameterBuilder()
                .in(ParameterType.HEADER)//在swagger里显示header
                .name("token")//header的参数名为 token
                .description("对应的token值")

                .required(true)//对应参数是否为必传，如果不是必传参数则设置为false
                .query(param -> param.model(model -> model.scalarModel(ScalarType.STRING)))

                .build();
        //为消费者提供帮助建立模型  更新标量类型
        //   公共请求参数生成器-udid
        RequestParameter udidParameter = new RequestParameterBuilder()
                .in(ParameterType.QUERY)//在swagger里显示header
                .name("udid")//header的参数名为 token
                .description("设备的ID")
                .required(false)//对应参数是否为必传，如果不是必传参数则设置为false
                .query(param -> param.model(model -> model.scalarModel(ScalarType.STRING)))
                .build();

        parameters.add(tokenParameter);
        parameters.add(udidParameter);
        return parameters;
//        Collections.singletonList(parameterBuilder.build());
    }
}
```



### 3. 解决启动报错

如果新版本不进行配置对应的内容，直接启动会报错：

![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230327192143.png)



#### 配置文件添加

>请求路径与 Spring MVC 处理映射匹配的默认策略已从AntPathMatcher更改为PathPatternParser。你可以设置spring.mvc.pathmatch.matching-strategy为ant-path-matcher来改变它。

在配置文件添加：

```css
spring.mvc.pathmatch.matching-strategy=ANT_PATH_MATCHER
```

![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230327191923.png)

#### 直接编写代码

直接编写代码{治本}。

```java
@Configuration
public class SwaggerConfiguration {
    @Bean
    public static BeanPostProcessor springfoxHandlerProviderBeanPostProcessor() {
        return new BeanPostProcessor() {

            @Override
            public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
                if (bean instanceof WebMvcRequestHandlerProvider || bean instanceof WebFluxRequestHandlerProvider) {
                    customizeSpringfoxHandlerMappings(getHandlerMappings(bean));
                }
                return bean;
            }

            private <T extends RequestMappingInfoHandlerMapping> void customizeSpringfoxHandlerMappings(List<T> mappings) {
                List<T> copy = mappings.stream()
                        .filter(mapping -> mapping.getPatternParser() == null)
                        .collect(Collectors.toList());
                mappings.clear();
                mappings.addAll(copy);
            }

            @SuppressWarnings("unchecked")
            private List<RequestMappingInfoHandlerMapping> getHandlerMappings(Object bean) {
                try {
                    Field field = ReflectionUtils.findField(bean.getClass(), "handlerMappings");
                    field.setAccessible(true);
                    return (List<RequestMappingInfoHandlerMapping>) field.get(bean);
                } catch (IllegalArgumentException | IllegalAccessException e) {
                    throw new IllegalStateException(e);
                }
            }
        };
    }
}
```



##### 只看controller的请求

但是会发现多出来`basic-error-controller`、`operation-handler`、`web-mvc-links-handler`，
可以通过RequestHandlerSelectors的规则来消除，只保留我们对外提供的接口

```
.apis(RequestHandlerSelectors.basePackage("top.testeru.ssmturorials")) // 设置扫描路径
```

### 4. 访问

启动 `Spring Boot` 项目。

- **旧版本**启动项目可访问`http://localhost:8080/swagger-ui.html`

- **新版本**：`http://localhost:8080/swagger-ui/`




在访问上面的 `URL` 后，将看到 `Swagger UI` 界面，其中包含了 `API` 文档和测试工具。在这里，可以使用 `Swagger` 的 `UI` 来测试和查看您的 `API`。

![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/202204261659003.png)


### 5. No operations defined in spec!

如果使用代码解决启动报错问题，在浏览器访问时会发现页面无法显示对应控制请求接口及实体类。

#### 解决方案：

修改`application`文件，`MVC`默认的路径匹配策略为`PATH_PATTERN_PARSER`，需要修改为`ANT_PATH_MATCHER`；

```css
spring.mvc.pathmatch.matching-strategy=ANT_PATH_MATCHER
```


![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230327200850.png)



这时发现直接显示对应控制类名称及方法，没有相关解释说明，如果想要添加，则需要使用