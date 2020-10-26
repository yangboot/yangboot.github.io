---
title: shiro+Thymeleaf授权
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
weight: 1
menu:
  spring-shiro-thymeleaf:
   parent: spring+shiro+thymeleaf用户登录授权
   weight: 5

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---


## shiro+Thymeleaf授权
> springboot-shiro-simple
> 项目git地址：https://gitee.com/MrPen/springboot-shiro.git

## 效果 

## 导入Thymeleaf-shiro依赖
### pom.xml
```xml
<dependency>
    <groupId>com.github.theborakompanioni</groupId>
    <artifactId>thymeleaf-extras-shiro</artifactId>
    <version>2.0.0</version>
</dependency>
```
### ShiroConfig.java
```java
//授权过滤器
filterMap.put("/add", "perms[user:add]");
filterMap.put("/update", "perms[user:update]");

//设置未授权URL
shiroFilterFactoryBean.setUnauthorizedUrl("/unAuth");
```
## 编写授权逻辑
### UserRealm.java
```java
protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
		//给资源进行授权
		SimpleAuthorizationInfo info=new SimpleAuthorizationInfo();
	    Subject subject = SecurityUtils.getSubject();
	    User u=(User) subject.getPrincipal();
	    
	    User user=userService.findByUsername(u.getUsername());
	    info.addStringPermission(user.getPerms());
	    return  info;
	}
```
## 编写界面

通过`shiro:hasPermission`属性进行授权

```html
<div shiro:hasPermission="user:add">　
<a href="user/add">用户添加</a>
</div>
<div shiro:hasPermission="user:update">　
<a href="user/update">用户修改</a>
</div>
```