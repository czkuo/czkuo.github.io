---
layout:     post   				    # 使用的布局（不需要改）
title:      ssm调用oracle存储过程 				# 标题 
subtitle:  ssm调用oracle存储过程          #副标题
date:       2019-03-24 				# 时间
author:    czkuo						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - ssm
    - 存储过程
    - oracle
---

## ssm调用oracle存储过程

>> 1.创建简单存储过程：
```
CREATE OR REPLACE PROCEDURE czk_demo(cid in number,cc out sys_refcursor) IS

BEGIN
open cc for

select * from P_INDICATOR_TYPES where id !=cid;

END czk_demo;
```

###### 返回结果集->sys_refcursor

>> 2.xml中：
```
<select id="selelname" statementType="CALLABLE"
            parameterType="java.util.Map" >
			{call czk_demo(#{id,mode=IN,  jdbcType=NUMERIC},		#{cc ,mode=OUT,jdbcType=CURSOR,javaType=java.sql.ResultSet,resultMap=com.yqjr.fin.risk.stat.mapper.IndicatorTypesMapper.BaseResultMap})}
			</select>
```

###### 通过call{存储过程名字(参数)}的,参数有传入参数和传出参数 

1. 传入 mode=IN ,传出mode=OUT
2. 传出的jdbcType如果是结果集为CURSOR ->游标
3. 返回的resulMap中一定要加上命名空间+关系映射名称





命名空间

![命名空间](https://czkuo.github.io/postimages/201904031.png)

xml
![xml](https://czkuo.github.io/postimages/201904032.png)

###### 返回的是一个参数

![](https://czkuo.github.io/postimages/201904033.png)


>> 3.dao中：
```
public String selelname(Map<String,Object> m);
```

>> 4.rest中：

```
  public ResultMsg<String> demo(IndicatorTypes it) {
        Map<String,Object> m = new HashMap<String,Object>();
        m.put("id",it.getId());
        baseService.selelname(m);
        System.out.println(baseService.selelname(m));
        List<IndicatorTypes> list = (List)m.get("cc");
        for(IndicatorTypes a:list){
            System.out.println(a.getCode()+"==="+a.getName());
        }

        System.out.println(list.size());
        return new ResultMsg<String>(Const.CODE_SUCCESS, "修改成功", Const.SUCCESS, "");
    }
```
1. New一个map集合
2. 在里面定义xml中#{}里面的字段
3. 返回结果自动存到定义的map中
4. Map.get（字段）就能取出来结果
5. 强转成list 就OK了

![](https://czkuo.github.io/postimages/201904034.png)