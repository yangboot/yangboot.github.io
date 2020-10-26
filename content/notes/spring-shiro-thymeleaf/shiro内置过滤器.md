---
title: shiro内置过滤器
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
weight: 1
menu:
  spring-shiro-thymeleaf:
   parent: spring+shiro+thymeleaf用户登录授权
   weight: 4

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---


## shiro内置过滤器

*  **anon:	无需认证**


*  **authc:	必需认证才可以访问**


*  **user:	如果使用rememberMe的功能可以直接访问**


*  **perms:	资源必需得到资源权限才可以访问**


*  **role:   该资源必需得到角色权限才能访问**