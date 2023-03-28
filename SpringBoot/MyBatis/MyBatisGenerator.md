# MyBatisGenerator


`MyBatisGenerator`是MyBatis框架提供的一个代码生成工具，它可以自动生成符合MyBatis规范的Mapper接口、Mapper映射文件以及POJO实体类等基础代码，减少开发人员手动编写这些代码的工作量，提高开发效率。`MyBatisGenerator`支持从数据库中读取表结构和列信息，生成基础代码时也会根据表结构和列信息自动生成对应的注释，这些注释可以提高代码可读性和可维护性。同时，`MyBatisGenerator`也支持自定义插件，开发人员可以编写自定义插件扩展代码生成功能。


## 插件导入

```xml
<dependency>
    <groupId>tk.mybatis</groupId>
    <artifactId>mapper-spring-boot-starter</artifactId>
    <version>${tk.version}</version>
</dependency>
<plugin>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-maven-plugin</artifactId>
    <version>1.4.2</version>
    <!--插件设置-->
    <configuration>
        <!--允许移动生成的文件-->
        <verbose>true</verbose>
        <!--启用覆盖-->
        <overwrite>true</overwrite>
        <!--自动生成配置 如果名字是generatorConfig.xml可以省略配置-->
        <configurationFile>src/main/resources/generator/generatorConfig.xml</configurationFile>
    </configuration>
    <dependencies>
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <version>${mysql.version}</version>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>tk.mybatis</groupId>
            <artifactId>mapper</artifactId>
            <version>${tk.version}</version>
        </dependency>
    </dependencies>
</plugin>
```

导入成功后，在maven的插件内发现该插件，如图：

![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230327223848.png)

导入成功后对应maven可以看到有该插件。



## 实体类继承BaseEntity

- 进行序列化
```java
import javax.persistence.Column;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import java.io.Serializable;
import java.util.Date;

public class BaseEntity implements Serializable {
    private static final long serialVersionUID = 498820763181265L;

}

```

## 自定义Mapper接口


`MySqlExtensionMapper`是一个自定义的Mapper接口，它继承了MyBatis的Mapper接口，用于在MyBatis中操作MySQL数据库的扩展功能，包括但不限于增删改查等基本操作，以及复杂的SQL查询语句的执行等。

通过在该接口中定义不同的方法，并在对应的`Mapper.xml`文件中实现这些方法，可以方便地对`MySQL`数据库进行操作。

>常见的`ORM`框架如`MyBatis`、`Hibernate`等都提供了自定义`Mapper`接口的支持，使得开发人员可以灵活地根据业务需求自定义数据访问接口，进而提高代码的可读性和可维护性。

- 通用mapper，基本方法都有
- 新建⼀个通⽤Mapper继承Mapper、MySqlMapper，点击进去看
- 被Mapper继承的BaseMapper⾥⾯有还有很多增删改查的⽅法


```java
import tk.mybatis.mapper.common.IdsMapper;
import tk.mybatis.mapper.common.Mapper;
import tk.mybatis.mapper.common.MySqlMapper;
import top.testeru.sbm.entity.BaseEntity;

public interface MySqlExtensionMapper<T extends BaseEntity> extends Mapper<T>, MySqlMapper<T>, IdsMapper<T> {
}
```


#### Mapper<T> 接口

通用 Mapper 接口，提供了基本的 CRUD 操作。

#### MySqlMapper<T> 接口

MyBatis 操作 MySQL 数据库的通用接口，提供了一些 MySQL 特有的操作方法。

#### IdsMapper<T> 接口

提供根据 ID 集合批量删除实体的方法。

通过继承这三个接口，`MySqlExtensionMapper` 接口集成了它们的方法，并且还可以自定义一些方法，以满足特定的业务需求。它主要的作用是提供了一些常用的数据访问操作，可以避免重复编写常用的数据访问方法。


## generator配置
### config.properties
```properties
# 数据库配置 A
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://127.0.0.1:3306/mini?characterEncoding=UTF-8&useUnicode=true&nullCatalogMeansCurrent=true
jdbc.username=root
jdbc.password=hogwarts
# 通用Mapper配置
mapper.plugin=tk.mybatis.mapper.generator.MapperPlugin
mapper.Mapper=top.testeru.mini.common.MySqlExtensionMapper
#dao类和实体类的位置
java.targetProject=src/main/java
#实体类的包名
java.targetPackage=top.testeru.mini.entity
#实体类的根包名
java.rootClass=top.testeru.mini.entity.BaseEntityNew
#mapper.xml位置
mapper.targetProject=src/main/resources
#mapperXML文件路径
mapper.targetPackage=mapper

#dao类的包名
java.targetMapperPackage=top.testeru.mini.dao
```



### generatorConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">


<!-- 配置生成器 -->
<generatorConfiguration>


    <!--执行generator插件生成文件的命令： call mvn mybatis-generator:generate -e -->
    <!-- 引入配置文件 -->
    <properties resource="generator/config.properties"/>
    <!--classPathEntry:数据库的JDBC驱动,换成你自己的驱动位置 可选 -->
    <!--    <classPathEntry location="${jdbc.jar.path}"/>-->

    <!-- 一个数据库一个context 配置对象环境-->
    <!--    id="MysqlTable" ：此上下文的唯一标识符。此值将用于一些错误消息。
    targetRuntime="MyBatis3Simple"：为了避免生成Example相关的代码和方法。如果需要则改为Mybatis3
    defaultModelType="flat" ：每个表只生成一个实体类    -->
    <context id="MysqlTable" targetRuntime="MyBatis3Simple" defaultModelType="flat">

        <!-- 配置起始与结束标识符 指明数据库的用于标记数据库对象名的符号-->
        <!-- ORACLE就是双引号，MYSQL默认是`反引号  数据库使用mysql,所以前后的分隔符都设为”`”-->
        <property name="beginningDelimiter" value="`"/>
        <property name="endingDelimiter" value="`"/>


        <!--用来定义一个插件，用于扩展或者修改MBG生成的代码，不常用，可以配置0个或者多个，个数不受限制。
        只有一个Type标签，其中填插件的全限定名。
        常用的有缓存插件，序列化插件，RowBounds插件，ToString插件等。-->
        <plugin type="${mapper.plugin}">
            <property name="mappers" value="${mapper.Mapper}"/>
        </plugin>



        <!--数据库连接配置-->
        <jdbcConnection driverClass="${jdbc.driver}"
                        connectionURL="${jdbc.url}"
                        userId="${jdbc.username}"
                        password="${jdbc.password}">
        </jdbcConnection>

        <!-- 配置生成的实体类位置 type使用XMLMAPPER，会使接口和XML完全分离。
            targetPackage：放置生成的类的包。 MyBatis Generator 将根据需要为生成的包创建文件夹
            targetProject：包所在的project下的位置，指定了将保存对象的项目和源文件夹。该目录不存在，MyBatis Generator 将不会创建该目录        -->
    <javaModelGenerator targetPackage="${java.targetPackage}" targetProject="${java.targetProject}">
        <!-- 设置一个根对象，
        如果设置了这个根对象，那么生成的keyClass或者recordClass会继承这个类；在Table的rootClass属性中可以覆盖该选项            注意：如果在key class或者record class中有root class相同的属性，MBG就不会重新生成这些属性了，包括：                1，属性名相同，类型相同，有相同的getter/setter方法；
        rootClass：所有实体类的父类，如果父类定义了一些字段以及对应的getter、setter方法，那么实体类中就不会再生成。必须要类的安全限定名，如com.momo.test.BasePo         -->
        <property name="rootClass"
                  value="${java.rootClass}"/>
    </javaModelGenerator>


        <!-- sqlMapGenerator：配置SQL映射生成器（Mapper.xml文件）的属性，可选且最多配置1个  配置映射位置
                只有两个必选属性(和实体类的差不多)：
                targetPackage：生成映射文件存放的包名，可能会受到其他配置的影响。
                 targetProject：指定目标targetPackage的项目路径，可以用相对路径或者绝对路径        -->
        <sqlMapGenerator targetPackage="${mapper.targetPackage}" targetProject="${mapper.targetProject}"/>

        <!-- 配置接口位置
            typetype="XMLMAPPER"：接口和XML完全分离；所有方法都在XML中，接口用依赖Xml文件
            targetPackage：生成Mapper文件存放的包名，可能会受到其他配置的影响。
            targetProject：指定目标targetPackage的项目路径，可以用相对路径
             -->
        <javaClientGenerator targetPackage="${java.targetMapperPackage}" targetProject="${java.targetProject}"
                             type="XMLMAPPER"/>

        <!-- table可以有多个,每个数据库中的表都可以写一个table，
        tableName表示要匹配的数据库表,也可以在tableName属性中通过使用%通配符来匹配所有数据库表,只有匹配的表才会自动生成文件 -->        <!-- 配置数据库表 -->
        <table schema="mini" tableName="%">

        <!-- generatedKey：用来指定自动生成的主键的属性。针对MySql,SQL Server等自增类型主键
               indetity：设置为true时会被标记为indentity列，
               并且selectKey标签会被插入在Insert标签（order=AFTER）。
               设置为false时selectKey会插入到Insert之前（oracal序列），默认为false.            -->
        <generatedKey column="id" sqlStatement="Mysql" identity="true"/>
        </table>

        <!-- 生成用户的相关类 -->
        <!--        <table schema="mini" tableName="user" domainObjectName="User" enableCountByExample="false" enableDeleteByExample="false"-->
        <!--               enableSelectByExample="false" enableUpdateByExample="false" >-->
        <!--            <generatedKey column="id" sqlStatement="Mysql" identity="true"/>-->
        <!--        </table>-->
    </context>
</generatorConfiguration>
```

## 执行

点击右侧`maven`对应的`generator`插件

![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230328105139.png)

命令行运行：

```
mvn mybatis-generator:generate
```

### 命令行显示

![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230328105449.png)

最终生成：

![](https://cdn.jsdelivr.net/gh/TesterDevSoul/blog_pic/springboot/20230328105043.png)


## 报错
### ERROR

```
[ERROR] Failed to execute goal org.mybatis.generator:mybatis-generator-maven-plugin:1.3.7:generate (default-cli) on project 项目名: configfile /Users/.../src/main/resources/generatorConfig.xml does not exist -> [Help 1]
```


原因：由于没有该插件的相关配置文件导致。

解决方案：需要编写配置文件`generatorConfig.xml`。




## generatorConfig.xml详解
### properties标签

- 引入配置文件
	- resource为对应配置文件的全路径
### context标签



- 生成一组对象
	- defaultModelType：flat，每个表只生成一个实体类
	- targetRuntime：MyBatis3Simple，避免生成Example相关的代码和方法

#### property标签


- 配置起始 beginningDelimiter 与结束 endingDelimiter 标识符 指明数据库的用于标记数据库对象名的符号
	- ORACLE：双引号
	- MYSQL：`反引号

#### plugin标签




- 定义一个插件
	



#### jdbcConnection标签

- 数据库连接配置
	

#### javaModelGenerator标签

- 配置生成的实体类
	- targetPackage：放置生成的类的包
	- targetProject：包所在的project下的位置 src/main/java


#### sqlMapGenerator标签

- 配置SQL映射生成器mapper.xml
	- targetPackage：生成映射文件存放的包名
	- targetProject：项目路径 mapper

#### javaClientGenerator标签

- 配置接口位置
	- targetPackage：生成Mapper文件存放的包名
	- targetProject：指定目标targetPackage的项目路径
	- typetype="XMLMAPPER"：接口和XML完全分离；所有方法都在XML中，接口用依赖Xml文件




