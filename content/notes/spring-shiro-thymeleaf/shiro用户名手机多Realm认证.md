---
title: shiro用户名手机多Realm认证
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
weight: 1
menu:
  spring-shiro-thymeleaf:
   parent: spring+shiro+thymeleaf用户登录授权
   weight: 6

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

> springboot-shiro-realms
> 项目git地址：https://gitee.com/MrPen/springboot-shiro.git

## shiro用户名手机多Realm认证
## 效果

![](/img/shiro/8.gif)

## 步骤

### 新建PhoneRealm
创建PhoneRealm.java修改认证方法

```java
	User u=SpringBeanFactoryUtils.getBean(IUserService.class).findByPhone(token.getUsername());
		
		if (null == u||u.equals("")) {
			return null;
		} 
				return	new SimpleAuthenticationInfo(u.getUsername(),
						u.getPassword(), ByteSource.Util.bytes(u.getSalt()), // 如果密码需要加盐验证，需要使用这个构造方法，后面会讲到。
						getName());JA
```


### 配置文件
.在`src/main/resources`下添加配置文件`shiro.properties`，如果 是多个realm用` | `隔开

```xml
[main]
shiro.realm.person=com.yzg.shiro.PhoneRealm
```
创建LoginConstants和LoginConfigurationBean用于解析配置文件

**LoginConstants**
```java
public interface LoginConstants {
	String CLASSPATH_SHIRO_PROPERTIES = "classpath:shiro.properties" ; 
}
```
**LoginConfigurationBean**
```java
public class LoginConfigurationBean {
	private static Props props = new Props(LoginConstants.CLASSPATH_SHIRO_PROPERTIES) ; 
	public static String shiroRealm() {
		return props.getProperty("shiro.realm.person", "") ; 
	}
}
```
### 修改ShiroConfig


```java
	@Bean(name="securityManager")
	public SecurityManager securityManager() {
		DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
		// 添加多认证器
		securityManager.setRealms(shiroRealmList());
		return securityManager;}


/**
	 * 添加多个shiro认证方式
	 * 
	 * @return
	 */
	private Collection<Realm> shiroRealmList() {
		List<Realm> realms = new ArrayList<Realm>();
		realms.add(shiroRealm());
		// 读取系统[shiro.ini]配置
		realms.add(new IniRealm(LoginConstants.CLASSPATH_SHIRO_PROPERTIES));
		String personRealm = LoginConfigurationBean.shiroRealm();
		try {
			String realmArr[] = personRealm.split("\\|");
			if (realmArr != null && realmArr.length > 0) {
				for (String s : realmArr) {
					System.out.println("realm:"+s);
					if (!(s==null||s.equals(""))) {
						AuthorizingRealm pRealm = (AuthorizingRealm) Class.forName(s).newInstance();
						realms.add(pRealm);
					}
				}
			}
		} catch (InstantiationException | IllegalAccessException | ClassNotFoundException e) {
		}

		return realms;
	}
```

