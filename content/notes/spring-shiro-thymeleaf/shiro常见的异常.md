---
title: shiro常见的异常
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
weight: 1
menu:
  spring-shiro-thymeleaf:
   parent: spring+shiro+thymeleaf用户登录授权
   weight: 3

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---


## shiro 常见的异常
(验证)
**authc:**
**AuthencationException:**
**AuthenticationException 异常是Shiro在登录认证过程中，认证失败需要抛出的异常。**

**AuthenticationException包含以下子类：**

    **CredentitalsException 凭证异常**

        **IncorrectCredentialsException 不正确的凭证**

        **ExpiredCredentialsException 凭证过期**

    **AccountException 账号异常**

        **ConcurrentAccessException 并发访问异常（多个用户同时登录时抛出）**

        **UnknownAccountException 未知的账号**

        **ExcessiveAttemptsException 认证次数超过限制**

        **DisabledAccountException 禁用的账号**
         
    **LockedAccountException 账号被锁定**

  **UnsupportedTokenException 使用了不支持的Token**

**authz:**
**AuthorizationException:**
**子类:**
    **UnauthorizedException:抛出以指示请求的操作或对请求的资源的访问是不允许的。**
    **UnanthenticatedException:当尚未完成成功认证时，尝试执行授权操作时引发异常。**
    (授权只能在成功的认证之后执行，因为授权数据（角色、权限等）必须总是与已知的标识相关联。这样的已知身份只能在成功登录时获得)
原文链接：https://blog.csdn.net/ssjq123/java/article/details/81086882