## mapstruct
>MapSturct 是一个生成类型安全， 高性能且无依赖的 JavaBean 映射代码的注解处理器（annotation processor）。
- [b] [mapstruct官网](https://mapstruct.org/documentation/dev/reference/html/#setup)
  - 注解处理器
  - 可以生成 JavaBean 之间那的映射代码
  - 类型安全， 高性能， 无依赖性

- [i] 通过注解的方式帮我们实现 _JavaBean_ 之间的转换

#### pom导入
```xml
 <properties>
    <mapstruct.version>1.5.3.Final</mapstruct.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.mapstruct</groupId>
        <artifactId>mapstruct</artifactId>
        <version>${mapstruct.version}</version>
    </dependency>
</dependencies>

<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>${maven.compiler.version}</version>
    <configuration>
        <!-- 设置编码为 UTF-8 -->
        <encoding>${maven.compiler.encoding}</encoding>
        <source>${java.version}</source>
        <target>${java.version}</target>
        <annotationProcessorPaths>
            <!-- Lombok 在编译时会通过这个插件生成代码 -->
            <path>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>${lombok.version}</version>
            </path>
            <!-- MapStruct 在编译时会通过这个插件生成代码 -->
            <path>
                <groupId>org.mapstruct</groupId>
                <artifactId>mapstruct-processor</artifactId>
                <version>${mapstruct.version}</version>
            </path>
        </annotationProcessorPaths>
    </configuration>
</plugin>

```

#### 转换类编写
- 使用mapper注解，导入的包为mapstruct的包
```java
package top.testeru.sbm.converter;

import org.mapstruct.Mapper;
import org.mapstruct.Mapping;
import org.mapstruct.Mappings;
import top.testeru.sbm.dto.UserDTO;
import top.testeru.sbm.entity.User;

import java.util.List;


//生成的映射器是一个单例范围的 Spring bean，可以通过以下方式检索@Autowired
@Mapper(componentModel = "spring")
public interface UserConverter {
    @Mappings({
            @Mapping(target = "id",source = "id"),
            @Mapping(target = "userName",source = "userName"),
            @Mapping(target = "password",source = "password"),
            @Mapping(target = "email",source = "email")
    })
    User userDtoForUser(UserDTO userDto);
    @Mappings({
            @Mapping(target = "id",source = "id"),
            @Mapping(target = "userName",source = "userName"),
            @Mapping(target = "password",source = "password"),
            @Mapping(target = "email",source = "email")
    })
    UserDTO userForUserDto(User user);
    @Mappings({
            @Mapping(target = "id",source = "id"),
            @Mapping(target = "userName",source = "userName"),
            @Mapping(target = "password",source = "password"),
            @Mapping(target = "email",source = "email")
    })
    List<User> UserDTOListForUserList(List<UserDTO> userDtoList);
    @Mappings({
            @Mapping(target = "id",source = "id"),
            @Mapping(target = "userName",source = "userName"),
            @Mapping(target = "password",source = "password"),
            @Mapping(target = "email",source = "email")
    })
    List<UserDTO> UserListForUserDTOList(List<User> userList);
}
```

#### 验证
- controller层
```java
@RestController
public class UserController {
    @Autowired
    UserService userService;

//    用户注册
    @PostMapping(value = "/reg",produces = "application/json")
    String registerUser(@RequestBody UserDTO user){

        return userService.register(user);
    }
    //    用户更改信息
    @PostMapping(value = "/update",produces = "application/json")
    String updateUser(@RequestBody UserDTO user){
        return userService.update(user);
    }

    //    用户注销
    @PostMapping(value = "/dele",produces = "application/json")
    String deleteUser(@RequestBody UserDTO user){
        return userService.delete(user);
    }
    //    用户查询
    @PostMapping(value = "/find",produces = "application/json")
    List<UserDTO> findUser(@RequestBody UserDTO user){
        return userService.find(user);

    }
}
```

- service层
```java
package top.testeru.sbm.service.impl;

import cn.hutool.core.bean.BeanUtil;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import tk.mybatis.mapper.entity.Example;
import top.testeru.sbm.converter.UserConverter;
import top.testeru.sbm.dao.UserMapper;
import top.testeru.sbm.dto.UserDTO;
import top.testeru.sbm.entity.User;
import top.testeru.sbm.service.UserService;


import java.util.Date;
import java.util.List;

/**
 * @program: springboot-turorials
 * @author: testeru.top
 * @description:
 * @Version 1.0
 * @create: 2022/5/17 2:38 PM
 */

@Service
public class UserServiceImpl implements UserService {

    @Autowired
    UserMapper userMapper;

    @Autowired
    UserConverter converter;

    @Override
    public String register(UserDTO userDto) {
        User user = converter.userDtoForUser(userDto);
        List<User> byNameUser = findByName(user);
        if(byNameUser.size()==0){
            System.out.println(user);
            user.setCreateTime(new Date());
            user.setUpdateTime(new Date());
            user.setFlag(0);
            //保存一个实体，null的属性不会保存，会使用数据库默认值
            int insertNum = userMapper.insertSelective(user);
            System.out.println("插入 " + insertNum + "条数据");
            if(insertNum >0){
                return "注册成功";
            }
            return "注册失败";
        }else {
            return "该用户名已经注册";
        }

    }

    @Override
    public List<UserDTO> find(UserDTO userDto) {
        User user = converter.userDtoForUser(userDto);
        List<User> userList = findByName(user);
        return converter.UserListForUserDTOList(userList);

    }


    /**
     * 根据用户名查找用户
     * @param
     * @return
     */
    private List<User> findByName(User user) {
        Example example = new Example(User.class);
        Example.Criteria criteria = example.createCriteria();
        criteria.andEqualTo("userName", user.getUserName());
        criteria.andEqualTo("flag", 0);
        List<User> userList = userMapper.selectByExample(example);
        System.out.println("userlist:");
        userList.forEach(System.out::println);
        return userList;
    }

    @Override
    public String delete(UserDTO userDTO) {

        return upAndDele(userDTO,"del");
    }



    @Override
    public String update(UserDTO userDto) {

        return upAndDele(userDto,"up");
    }


    private String upAndDele(UserDTO userDto, String str) {
        User user = converter.userDtoForUser(userDto);
        List<User> byNameUser = findByName(user);

        if("up".equals(str)){
            str = "更新用户";
        }else if("del".equals(str)){
            str = "删除用户";
            user.setFlag(1);
            user.setEmail(null);
            user.setPassword(null);
        }


        if(byNameUser.size()!=0){
            //放入主键
            user.setId(byNameUser.get(0).getId());
            //更新的时间
            user.setUpdateTime(new Date());
            System.out.println(user);
            int updateNum = userMapper.updateByPrimaryKeySelective(user);
            System.out.println("更改 " + updateNum + "条数据");
            if(updateNum >0){
                return str + "成功";
            }
            return str + "失败";
        }else {
            return "该用户不存在";
        }
    }
}

```