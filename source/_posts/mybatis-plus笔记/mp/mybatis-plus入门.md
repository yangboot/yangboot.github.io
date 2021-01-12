---
title: 【MyBatis-Plus笔记】MyBatis-Plus入门
categories: MyBatis笔记
tags:
  - MyBatis-Plus
excerpt: MyBatis-Plus
date: 2020-12-21 20:33:36
img:  /images/MyBatis-Plus/MyBatis-Plus.jpg
---
## 快速开始

我们将通过一个简单的 Demo 来阐述 MyBatis-Plus 的强大功能，在此之前，我们假设您已经：

- 拥有 Java 开发环境以及相应 IDE
- 熟悉 Spring Boot
- 熟悉 Maven

------

现有一张 `User` 表，其表结构如下：

| id   | name   | age  | email              |
| ---- | ------ | ---- | ------------------ |
| 1    | Jone   | 18   | test1@baomidou.com |
| 2    | Jack   | 20   | test2@baomidou.com |
| 3    | Tom    | 28   | test3@baomidou.com |
| 4    | Sandy  | 21   | test4@baomidou.com |
| 5    | Billie | 24   | test5@baomidou.com |

其对应的数据库 Schema 脚本如下：

```sql
DROP TABLE IF EXISTS user;

CREATE TABLE user
(
	id BIGINT(20) NOT NULL COMMENT '主键ID',
	name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
	age INT(11) NULL DEFAULT NULL COMMENT '年龄',
	email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY (id)
);
```

其对应的数据库 Data 脚本如下：

```sql
DELETE FROM user;

INSERT INTO user (id, name, age, email) VALUES
(1, 'Jone', 18, 'test1@baomidou.com'),
(2, 'Jack', 20, 'test2@baomidou.com'),
(3, 'Tom', 28, 'test3@baomidou.com'),
(4, 'Sandy', 21, 'test4@baomidou.com'),
(5, 'Billie', 24, 'test5@baomidou.com');
```

------

Question

如果从零开始用 MyBatis-Plus 来实现该表的增删改查我们需要做什么呢？

### 初始化工程

创建一个空的 Spring Boot 工程（工程将以 mysql作为默认数据库进行演示）

TIP

可以使用 [Spring Initializer (opens new window)](https://start.spring.io/)快速初始化一个 Spring Boot 工程

### 添加依赖
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.1.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <packaging>pom</packaging>
    <groupId>com.yangboot</groupId>
    <artifactId>mybatis-plus-samples</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <modules>
        <module>mybatis-plus-sample-quickstart</module>
    </modules>
    <properties>
        <java.version>11</java.version>
        <mybatisplus.version>3.4.1</mybatisplus.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <!--  mybatis-plus starter-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>${mybatisplus.version}</version>
        </dependency>
        <!-- mybatis-plus end -->
        <!--  lombok 用来简化实体类starter-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <scope>provided</scope>
        </dependency>
        <!--  lombok end-->
        <!--  mysqlstarter-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!-- mysql starter-->
    </dependencies>
</project>
```

### 配置

在 `application.yml` 配置文件中添加 H2 数据库的相关配置：

```yaml
# DataSource Config
spring:
  application:
    name: mybatis-plus-sample-quickstart
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    data-username: root
    password: root
    url: jdbc:mysql://127.0.1:3306/mpdemo?serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf-8
```

在 Spring Boot 启动类中添加 `@MapperScan` 注解，扫描 Mapper 文件夹：

```java
@SpringBootApplication
@MapperScan("com.baomidou.mybatisplus.samples.quickstart.mapper")
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(QuickStartApplication.class, args);
    }

}
```

### 编码

编写实体类 `User.java`（此处使用了 [Lombok (opens new window)](https://www.projectlombok.org/)简化代码）

```java
@Data
public class User {
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```

编写Mapper类 `UserMapper.java`

```java
public interface UserMapper extends BaseMapper<User> {

}
```

### 开始使用

添加测试类，进行功能测试：

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class SampleTest {

    @Autowired
    private UserMapper userMapper;

    @Test
    public void testSelect() {
        System.out.println(("----- selectAll method test ------"));
        List<User> userList = userMapper.selectList(null);
        Assert.assertEquals(5, userList.size());
        userList.forEach(System.out::println);
    }

}
```

TIP

UserMapper 中的 `selectList()` 方法的参数为 MP 内置的条件封装器 `Wrapper`，所以不填写就是无任何条件

控制台输出：

```text
User(id=1, name=Jone, age=18, email=test1@baomidou.com)
User(id=2, name=Jack, age=20, email=test2@baomidou.com)
User(id=3, name=Tom, age=28, email=test3@baomidou.com)
User(id=4, name=Sandy, age=21, email=test4@baomidou.com)
User(id=5, name=Billie, age=24, email=test5@baomidou.com)
```

TIP

完整的代码示例请移步：[Spring Boot 快速启动示例 (opens new window)](https://github.com/yangboot/mybatis-plus-samples/tree/master/mybatis-plus-sample-quickstart)
