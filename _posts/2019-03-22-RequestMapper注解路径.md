---
layout:     post   				    # 使用的布局（不需要改）
title:      RequestMapper注解 				# 标题 
subtitle:   注解访问后台不常用记录一下                  #副标题
date:        2019-03-21 				# 时间
author:    czkuo 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - RequestMapping
    - spring
    - 注解
---


## @RequestMapping注解

>做用户项目后台请求路径之前不是很常用记录一下

```
    /**
     * @函数名称:delete
     * @函数描述:单个对象删除，公共API接口
     * @参数与返回说明:
     * @算法描述:
     */
    @PostMapping("/delete/{id}")
    protected ResultDto<Integer> delete(@PathVariable String id) {
        int result = msStripeDictService.deleteByPrimaryKey(id);
        return new ResultDto<Integer>(result);
    }
```

路径中带有{id},前台不需要指定键值对直接传值即可


```
 http://localhost:50020/api/msdictitem/delete/100
 
```

![例子](https://czkuo.github.io/postimages/201904025.png)
