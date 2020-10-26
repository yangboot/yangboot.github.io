---
title: shiro登录注册MD5加密
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

> springboot-shiro-md5
> 项目git地址：https://gitee.com/MrPen/springboot-shiro.git

## 效果

![](/img/shiro/7.gif)

## 什么是MD5

MD5信息摘要算法（英语：MD5 Message-Digest Algorithm），一种被广泛使用的密码散列函数，可以产生出一个128位（16字节）的散列值（hash value），用于确保信息传输完整一致。MD5由美国密码学家罗纳德·李维斯特（Ronald Linn Rivest）设计，于1992年公开，用以取代MD4算法。这套算法的程序在 RFC 1321 标准中被加以规范。1996年后该算法被证实存在弱点，可以被加以破解，对于需要高度安全性的数据，专家一般建议改用其他算法，如SHA-2。2004年，证实MD5算法无法防止碰撞（collision），因此不适用于安全性认证，如SSL公开密钥认证或是数字签名等用途。

## 什么是盐值
就好比做菜，盐饭得多少菜的味道都不一样，同样的加密算法盐值不一样，得到的密文也不一样

## 编写加密工具类

```java
	public static String shiroEncryption(String password,String salt) {
		// shiro 自带的工具类生成salt
		//String salt = new SecureRandomNumberGenerator().nextBytes().toString();
		// 加密次数
		int times = 1024;
		// 算法名称
		String algorithmName = "md5";
		//shiro自帶MD5加密
		String encodedPassword = new SimpleHash(algorithmName,password,salt,times).toString();
		// 返回加密后的密码
		return encodedPassword;		
	}
```

## 用戶注冊
```java
@RequestMapping("/save")
	public String save(Model model,User user) {
		String password=user.getPassword();
		//密码加密
		String salt = new SecureRandomNumberGenerator().nextBytes().toString();
		String md5password = ShiroEncryption.shiroEncryption(password, salt);
		user.setSalt(salt);
		user.setPassword(md5password);
		
		userService.save(user);
		return "register";
	}
```
## 修改ShiroConfig

**思路：注册的密码是进行加密存入数据库的，所登录的时候也需要将用户输入的密码进行加密后比对**

shiro有自帶的密碼比對的方法`SimpleCredentialsMatcher()`需要修改比对方法，使用的是`hashedCredentialsMatcher()`所以在ShiroConfig写`hashedCredentialsMatcher()`方法时加密次数.算法名称都需要跟注册的一致。
```java
	@Bean
	public HashedCredentialsMatcher hashedCredentialsMatcher() {
		HashedCredentialsMatcher hashedCredentialsMatcher = new HashedCredentialsMatcher();
		hashedCredentialsMatcher.setHashAlgorithmName("md5");
		hashedCredentialsMatcher.setHashIterations(1024);
		return hashedCredentialsMatcher;
	}
```
## 修改Realm

修改返回的构造方法
```java
return	new SimpleAuthenticationInfo("数据库存的用户名",
				"数据库存的密码", ByteSource.Util.bytes("数据库存的盐值"), 
				getName());
```
