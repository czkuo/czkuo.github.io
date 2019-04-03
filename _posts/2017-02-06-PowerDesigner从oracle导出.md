---
layout:     post   				    # 使用的布局（不需要改）
title:      PowerDesigner从oracle导出			# 标题 
subtitle:  PowerDesigner从oracle导出
date:      2019-03-25			# 时间
author:    czkuo						# 作者
header-img: img/post-bg-map.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - PowerDesigner
    - oracle
---

##  PowerDesigner从oracle导出

>> PowerDesigner版本为64 16.5

### 在PowerDesigner中把数据库表导出来

​      1).配置odbc数据源    <font color="red" >（*看你的PowerDesigner是64还是32来选择数据源是64还是32）</font>

![](https://czkuo.github.io/postimages/201904036.png)



2). 点击配置

![](https://czkuo.github.io/postimages/201904037.png)

3).配置数据源



![](https://czkuo.github.io/postimages/201904038.png)

4). 输入密码 测试连接 显示 connecttion successful 连接成功 点击ok保存

![](https://czkuo.github.io/postimages/201904039.png)



5).点击File -->ReverseEngineer --> Database

![](https://czkuo.github.io/postimages/201904035.png)

6).选择数据库版本 我的是11g 

![](https://czkuo.github.io/postimages/2019040310.png)

7).选择 useing a datasource  点击右侧按钮

![](https://czkuo.github.io/postimages/2019040311.png)



8).选择刚才配置好的数据源



![](https://czkuo.github.io/postimages/2019040312.png)

出现问题

![](https://czkuo.github.io/postimages/2019040313.png)

###### 解决办法

网上百度是说需要配置oracle环境变量  G:\Oracle\product\11.2.0\dbhome_1\BIN

但是我管理员权限打开就解决了

9).选择表  导出

![](https://czkuo.github.io/postimages/2019040314.png)

10).导出成功

![](https://czkuo.github.io/postimages/2019040315.png)