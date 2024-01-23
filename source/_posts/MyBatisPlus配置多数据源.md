---
title: MyBatisPlus配置多数据源
date: 2024-01-24 14:28:31
tags:
index_img: /img/mybatisplus.png
---

在实际的企业开发中，有时单个数据源、单一数据源已经无法满足项目的需求，由此引入[MyBatisPlus的多数据源方案](https://baomidou.com/pages/a61e1b/)。

个人推荐使用MyBatisPlus去解决多数据源的问题这样只需要修改配置文件，然后加上注解即可，当然实际情况实际而论。

## Dynamic-DataSource

### 使用方法

1、引入dynamic-datasource-spring-boot-starter。

```xml
<dependency>
  <groupId>com.baomidou</groupId>
  <artifactId>dynamic-datasource-spring-boot-starter</artifactId>
  <version>3.5.1</version>
</dependency>
```



2、配置数据源

```yaml
spring:
  datasource:
    # mybatis-plus 多数据源方案
    # 不同数据源的标识可以在mapper层、impl层、方法上 ---> @DS("postgres")
    # 优先级：方法上使用该注解 > 类上使用该注解
    dynamic:
      primary: mysql
      # 若将该值设置为true，则所有的mapper层都需要进行数据源的标识，若为false，则没有标明数据源时，默认使用primary的数据源。该值默认为false
      strict: false 
      datasource:
        mysql:
          driver-class-name: com.mysql.cj.jdbc.Driver
          url: jdbc:mysql://127.0.0.1:3306/test
          username: root
          password: 123456
        postgres:
          driver-class-name: org.postgresql.Driver
          url: jdbc:postgresql://localhost:5432/postgres
          username: root
          password: 123456

# 日志输出
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

这里我配置了两种不同类型的数据库：mysql + postgres。



3、使用 **@DS** 切换数据源

```java
@Mapper
@DS("mysql")
public interface UserMapper1 extends BaseMapper<User1> {

}
```



```java
@Mapper
@DS("postgres")
public interface UserMapper2 extends BaseMapper<User2> {

}
```



参考文章：[Springboot实现多数据源整合的两种方式](https://juejin.cn/post/7025511138364227621#heading-0)。感谢观看～
