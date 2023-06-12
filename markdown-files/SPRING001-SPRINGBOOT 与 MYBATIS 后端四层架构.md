# **SPRINGBOOT 与 MYBATIS 后端四层架构**

------

## 0.配置数据库

```
spring.datasource.url=jdbc:mysql://localhost:3306/sric-blog?characterEncoding=UTF-8
spring.datasource.username=root
spring.datasource.password=admin
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.jpa.hibernate.ddl-auto = none
```

其中

```
localhost:3306 为数据库访问url
sric-blog 为数据库名称
spring.datasource.username = 数据库访问时的用户名
spring.datasource.password = 数据库访问时的密码
```

------

## 1.实体类

一个简单的例子

```java
import lombok.Data;

@Data
public class indexTime
{
    private int id;
    private String content;
    private String timestmap;
    private String type;
    private String color;
}
```

@Data 表示这是用来装数据的实体类 使用了lombok简化了代码 省略了setter和getter方法

------

## 2.Mapper

Mapper就相当于DAO层 主要实现对数据库的操作部分

这里使用纯注解的方式 直接将SQL语句封装在了Mapper中

```java
import com.spring.back_springboot.pojo.indexTime;
import org.apache.ibatis.annotations.*;

import java.util.List;

@Mapper
public interface indexTimeMapper
{
    @Select(value = "SELECT * FROM indexTime")
    List<indexTime> GetAllIndexTime();
}
```

@Mapper 注解时mybatis注解 表示这是一个Mapper类

这里只举了一个查询所有内容的例子

------

## 3.1.Service

先创建Service的接口

```java
import com.spring.back_springboot.pojo.indexTime;
import java.util.List;

public interface indexTimeService
{
    List<indexTime> GetAllIndexTime();
}
```

------

## 3.2.ServiceImpl

这是Service层的实现类 它继承Service接口

```java
import com.spring.back_springboot.mapper.indexTimeMapper;
import com.spring.back_springboot.pojo.indexTime;
import com.spring.back_springboot.services.service.indexTimeService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class indexTimeServiceImpl implements indexTimeService
{
    @Autowired
    indexTimeMapper mapper;

    @Override
    public List<indexTime> GetAllIndexTime()
    {
        return mapper.GetAllIndexTime();
    }
}
```

@Service需要写在实现类的前面

@Autowired可以自动注入需要的内容

------

## 4.Controller

```java
import com.spring.back_springboot.pojo.indexTime;
import com.spring.back_springboot.services.service.indexTimeService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
@RequestMapping("/api")//此Controller中所有api前缀路径为/api
public class indexTimeController
{
    @Autowired
    indexTimeService Service;

    @CrossOrigin
    @GetMapping("/indexTimeList")
    public List<indexTime> GetAllIndexTime() throws Exception
    {
        return Service.GetAllIndexTime();
    }
}
```

将方法映射在接口上

使用@GetMapping进行设置

需要使用@CrossOrigin来设置允许跨域访问

------

# 没了，你居然看完了 Σ(っ °Д °;)っ

​	**copyright © 2023 06 13 soraincloud all rights reserved**