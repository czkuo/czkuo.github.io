---
layout:     post   				    # 使用的布局（不需要改）
title:      Spring MVC —— 参数校验 				# 标题 
subtitle:  做权限项目校验参数                         #副标题
date:      2019-3-20 				# 时间
author:    czkuo 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - spring
    - mvc
    - 参数
    - 校验
---

# Spring MVC —— 参数校验 

> 参数校验

##### 简述
Spring MVC 的校验可以通过 hibernate-validator 框架来进行校验的，所以在了解 Spring MVC 是怎么进行参数校验之前，我们先了解一下 JSR303/JSR-349、hibernate-validator、Spring MVC 之间的关系。

* JSR303 是一个校验规范，JSR-349 是其的升级版本，添加了一些新特性，他们规定一些校验规范即校验注解，如@Null，@NotNull，@Pattern，他们位于**javax.validation.constraints**包下，只提供规范不提供实现。
*  hibernate-validator 是对 JSR303/JSR-349 这两个规范的实现，并增加了一些其他校验注解，如@Email，@Length，@Range等等，他们位于**org.hibernate.validator.constraints**包下

##### 引入依赖
```
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>5.4.0.Final</version>
</dependency>
```

> 例如这样
```
<bean class="org.springframework.validation.beanvalidation.LocalValidatorFactoryBean" id="validator" />
<bean class="org.springframework.validation.beanvalidation.MethodValidationPostProcessor">
    <property name="validator" ref="validator" />
</bean> 
<mvc:annotation-driven validator="validator"/>

```

![**校验规则注解**](https://github.com/czkuo/czkuo.github.io/blob/master/postimages/clipboard.png)

###    有两种校验方式 1.实体类校验 2.rest中单个参数校验 

1. <font color="red">实体类校验</font>               


![**校验实体参数**](E:\gitblog\czkuo.github.io\postimages\20190304021.png)
