---
title: SpringBoot整合Knife4j
date: 2024-01-25 15:21:01
tags:
index_img: /img/knife4j.png
---

在实际的项目开发中，往往会涉及到前后端并行开发，而二者需要一份接口文档进行规范开发，这里提供的 `knife4j` 是对 `swagger` 在线接口文档的一个增强文档，界面更加完善。

## Knife4j

### 使用方法

1、引入knife4j-spring-boot-starter

```xml
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-spring-boot-starter</artifactId>
    <version>2.0.9</version>
</dependency>
```



2、添加配置类代码

```java
@Configuration
@EnableSwagger2WebMvc
public class Knife4jConfiguration {
    @Bean(value = "defaultApi2")
    public Docket defaultApi2() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(new ApiInfoBuilder()
                        .title("TITLE")
                        .description("接口文档")
                        .version("1.0")
                        .build())
                .groupName("all")
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.example.controller"))
                .paths(PathSelectors.any())
                .build();
    }
}
```

运行项目，访问地址：http://localhost:8080/doc.html



3、注解的使用

[Knife4j注解说明](https://blog.csdn.net/qq_46126559/article/details/118487809)。感谢观看～
