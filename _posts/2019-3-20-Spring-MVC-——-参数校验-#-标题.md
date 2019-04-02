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
*校验规则注解*

![**校验规则注解**](https://czkuo.github.io/postimages/clipboard.png)

###    有两种校验方式 1.实体类校验 2.rest中单个参数校验 

1. # <font color="red">实体类校验</font>                

*校验实体参数*
![**校验实体参数**](https://czkuo.github.io/postimages/20190304021.png)

 在处理方法的实体参数前标注<font color="red">@Validated</font> ，并且后面紧跟一个Errors类型的参数

*rest例子*

![rest例子](https://czkuo.github.io/postimages/201904022.png)

####  &nbsp;&nbsp;&nbsp;&nbsp; @Validated和@Valid的区别

1. @Valid：使用Hibernate的校验机制，不支持分组校验
2. @Validated：使用Spring的校验机制，支持分组校验

####  &nbsp;&nbsp;&nbsp;&nbsp; Errors和BindingResult的区别

1. Errors：存储数据绑定错误和验证错误相关信息
2. BindingResult： 继承Errors，增加了可以添加错误字段和信息的方法


2. # <font color="red">单个参数校验</font>                

***校验单个参数***
![单个参数校验](https://czkuo.github.io/postimages/201904023.png)

=====================================================================================

#### 分组校验

在实际开发过程中，同一个实体类在不同的场景下有可能有不同的校验规则，例如添加 User 时，id 必须为空，但是修改 User 时，id又必须不能为空，这个时候，我们就可以使用分组校验

1. 创建两个 interface 类型

```
public class ValidateGrop{
    //这个接口类型表示保存
    public interface save{}
    //这个接口类型表示修改
    public interface update{}
    //这个接口类型表示删除
    public interface delete{}
}
```

2. 在实体中定义多个分组的校验规则

```
public class User {
    //当分组是save的情况下
    @Null(message = "id必须为空", groups = ValidateGroup.save.class)
    //当分组是update和delete的情况下
    @NotNull(message = "id不能为空", groups = {ValidateGroup.update.class,ValidateGroup.delete.class})
    private Integer id;

    //...其他字段同理也可分组
}
@RestController
public class UserController {
    //新增
    @PostMapping
    public Map<String,Object> save(@Validated(ValidateGroup.save.class) User user){
        userService.save(user);
        //...
    }
    //修改
    @PostMapping
    public Map<String,Object> update(@Validated(ValidateGroup.update.class) User user){
        userService.update(user);
        //...
    }
    //删除
    @GetMapping
    public Map<String,Object> delete(@Validated(ValidateGroup.delete.class) User user){
        userService.delete(user.getId());
        //...
    }
}
```

#### 自定义校验规则

1. 创建注解

```
@Target({METHOD, FIELD, ANNOTATION_TYPE, CONSTRUCTOR, PARAMETER})
@Retention(RUNTIME)
@Documented
@Constraint(validatedBy = {PhoneValidator.class})
public @interface Phone {

    //默认错误消息
    String message() default "手机格式不正确";
    //分组
    Class<?>[] groups() default {};
    //负载
    Class<? extends Payload>[] payload() default {};
    //指定多个时使用
    @Target({FIELD, METHOD, PARAMETER, ANNOTATION_TYPE})
    @Retention(RUNTIME)
    @Documented
    @interface List {
        Phone[] value();
    }
}
```

2. 创建验证该规则的验证器
```
public class PhoneValidator implements ConstraintValidator<Phone, String> {
    @Override
    public void initialize(Phone phone) {

    }

    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {
        Pattern pattern = Pattern.compile("");
        //为null时不校验
        if(value == null){
            return true;
        }
        //手机号码
        return value.matches("^((1[3,5,8][0-9])|(14[5,7])|(17[0,6,7,8])|(19[7]))\\\\\\\\d{8}$");
    }
}
```

3. 使用校验规则
```
public class User{
    //必须是手机格式
    @Phone
    private String phone;
    //...
}
```