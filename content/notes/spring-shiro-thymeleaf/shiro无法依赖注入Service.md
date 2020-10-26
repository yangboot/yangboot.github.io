---
title: shiro无法依赖注入Service
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
weight: 1
menu:
  spring-shiro-thymeleaf:
   parent: spring+shiro+thymeleaf用户登录授权
   weight: 7

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---


## shiro无法依赖注入Service

这里是使用`getBean()`的方式

### 添加工具类SpringBeanFactoryUtils

```java
@Component
public class SpringBeanFactoryUtils implements ApplicationContextAware {

    private static ApplicationContext applicationContext;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext)
            throws BeansException {
        if (SpringBeanFactoryUtils.applicationContext == null) {
            SpringBeanFactoryUtils.applicationContext = applicationContext;
        }

    }

    public static ApplicationContext getApplicationContext() {
        return applicationContext;
    }

    //根据名称（@Resource 注解）
    public static Object getBean(String name) {
        return getApplicationContext().getBean(name);
    }

    //根据类型（@Autowired）
    public static <T> T getBean(Class<T> clazz) {
        return getApplicationContext().getBean(clazz);
    }

    public static <T> T getBean(String name, Class<T> clazz) {
        return getApplicationContext().getBean(name, clazz);
    }
}

```

### 调用

```java
User u=SpringBeanFactoryUtils.getBean(IUserService.class).findByPhone(token.getUsername());
```


如果用的的远程调用可以下面这样写java

```java
User u = ApplicationContextProvider.getBean(IUserService.class)
				.findByPhone(token.getUsername());
```
