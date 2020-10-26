---
# Course title, summary, and position.
linktitle: spring+shiro+thymeleaf用户登录授权
summary: spring+shiro+thymeleaf用户登录授权
weight: 1

# Page metadata.
title: spring+shiro+thymeleaf用户登录授权
date: "2020-05-02500:00:00Z"
lastmod: "2020-05-25T00:00:00Z"
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.

# Add menu entry to sidebar.
# - name: Declare this menu item as a parent with ID `name`.
# - weight: Position of link in menu.
menu:
  spring-shiro-thymeleaf:
    name: spring+shiro+thymeleaf用户登录授权
    weight: 1
---





## shiro安全框架 

官方开发文档：http://shiro.apache.org/architecture.html
## Apache Shiro Architecture
Apache Shiro’s design goals are to simplify application security by being intuitive and easy to use. Shiro’s core design models how most people think about application security - in the context of someone (or something) interacting with an application.

Software applications are usually designed based on user stories. That is, you’ll often design user interfaces or service APIs based on how a user would (or should) interact with the software. For example, you might say, “If the user interacting with my application is logged in, I will show them a button they can click to view their account information. If they are not logged in, I will show a sign-up button.”

This example statement indicates that applications are largely written to satisfy user requirements and needs. Even if the ‘user’ is another software system and not a human being, you still write code to reflect behavior based on who (or what) is currently interacting with your software.

Shiro reflects these concepts in its own design. By matching what is already intuitive for software developers, Apache Shiro remains intuitive and easy to use in practically any application.
### High-Level Overview

At the highest conceptual level, Shiro’s architecture has 3 primary concepts: the `Subject`, `SecurityManager` and `Realms`. The following diagram is a high-level overview of how these components interact, and we’ll cover each concept below:
![](/img/shiro/1.jpg)

* ** Subject: As we’ve mentioned in our Tutorial, the Subject is essentially a security specific ‘view’ of the the currently executing user. Whereas the word ‘User’ often implies a human being, a Subject can be a person, but it could also represent a 3rd-party service, daemon account, cron job, or anything similar - basically anything that is currently interacting with the software.**
     Subject instances are all bound to (and require) a SecurityManager. When you interact with a Subject, those interactions translate to subject-specific interactions with the SecurityManager.
    
 * ** SecurityManager: The SecurityManager is the heart of Shiro’s architecture and acts as a sort of ’umbrella’ object that coordinates its internal security components that together form an object graph. However, once the SecurityManager and its internal object graph is configured for an application, it is usually left alone and application developers spend almost all of their time with the Subject API.**

	We will talk about the SecurityManager in detail later on, but it is important to realize that when you interact with a Subject, it is really the SecurityManager behind the scenes that does all the heavy lifting for any Subject security operation. This is reflected in the basic flow diagram above.

 * ** Realms: Realms act as the ‘bridge’ or ‘connector’ between Shiro and your application’s security data. When it comes time to actually interact with security-related data like user accounts to perform authentication (login) and authorization (access control), Shiro looks up many of these things from one or more Realms configured for an application.:**


	In this sense a Realm is essentially a security-specific DAO: it encapsulates connection details for data sources and makes the associated data available to Shiro as needed. When configuring Shiro, you must specify at least one Realm to use for authentication and/or authorization. The SecurityManager may be configured with multiple Realms, but at least one is required.
	
	Shiro provides out-of-the-box Realms to connect to a number of security data sources (aka directories) such as LDAP, relational databases (JDBC), text configuration sources like INI and properties files, and more. You can plug-in your own Realm implementations to represent custom data sources if the default Realms do not meet your needs.
	
	Like other internal components, the Shiro SecurityManager manages how Realms are used to acquire security and identity data to be represented as Subject instances.
	
	Detailed Architecture
The following diagram shows Shiro’s core architectural concepts followed by short summaries of each

## Detailed Architecture
The following diagram shows Shiro’s core architectural concepts followed by short summaries of each:
![](/img/shiro/2.jpg)



![](/img/shiro/6.jpg)

![](/img/shiro/7.jpg)











