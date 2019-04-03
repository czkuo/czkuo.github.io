---
title:     PowerDesigner中name和comment转换 				# 标题 
subtitle:   PowerDesigner中name和comment转换                     #副标题
date:       2019-03-26 				# 时间
author:     czkuo						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - PowerDesigner
    - 转换
    - code
    - name

---

## 在使用PowerDesigner从oracle生成表时name会是字段应为 comment是数据库字段备注汉字，但是我们习惯于name是汉字备注 怎么办呢?

![](E:\gitblog\czkuo.github.io\postimages\2019040316.png)

###### 1.点击Tools->Execute Commands->Edit/Run Scripts

![](E:\gitblog\czkuo.github.io\postimages\2019040317.png)

###### 2.将Comment中的字符COPY至Name中



```
Option   Explicit   
ValidationMode   =   True   
InteractiveMode   =   im_Batch  
  
Dim   mdl   '   the   current   model  
  
'   get   the   current   active   model   
Set   mdl   =   ActiveModel   
If   (mdl   Is   Nothing)   Then   
      MsgBox   "There   is   no   current   Model "   
ElseIf   Not   mdl.IsKindOf(PdPDM.cls_Model)   Then   
      MsgBox   "The   current   model   is   not   an   Physical   Data   model. "   
Else   
      ProcessFolder   mdl   
End   If  
  
Private   sub   ProcessFolder(folder)   
On Error Resume Next  
      Dim   Tab   'running     table   
      for   each   Tab   in   folder.tables   
            if   not   tab.isShortcut   then   
                  tab.name   =   tab.comment  
                  Dim   col   '   running   column   
                  for   each   col   in   tab.columns   
                  if col.comment="" then  
                  else  
                        col.name=   col.comment   
                  end if  
                  next   
            end   if   
      next  
  
      Dim   view   'running   view   
      for   each   view   in   folder.Views   
            if   not   view.isShortcut   then   
                  view.name   =   view.comment   
            end   if   
      next  
  
      '   go   into   the   sub-packages   
      Dim   f   '   running   folder   
      For   Each   f   In   folder.Packages   
            if   not   f.IsShortcut   then   
                  ProcessFolder   f   
            end   if   
      Next   
end   sub  
```

###### 3.将Name中的字符COPY至Comment中  
```
Option   Explicit   
ValidationMode   =   True   
InteractiveMode   =   im_Batch  

Dim   mdl   '   the   current   model  

'   get   the   current   active   model   
Set   mdl   =   ActiveModel   
If   (mdl   Is   Nothing)   Then   
      MsgBox   "There   is   no   current   Model "   
ElseIf   Not   mdl.IsKindOf(PdPDM.cls_Model)   Then   
      MsgBox   "The   current   model   is   not   an   Physical   Data   model. "   
Else   
      ProcessFolder   mdl   
End   If  

'   This   routine   copy   name   into   comment   for   each   table,   each   column   and   each   view   
'   of   the   current   folder   
Private   sub   ProcessFolder(folder)   
      Dim   Tab   'running     table   
      for   each   Tab   in   folder.tables   
            if   not   tab.isShortcut   then   
                  tab.comment   =   tab.name   
                  Dim   col   '   running   column   
                  for   each   col   in   tab.columns   
                        col.comment=   col.name   
                  next   
            end   if   
      next  

      Dim   view   'running   view   
      for   each   view   in   folder.Views   
            if   not   view.isShortcut   then   
                  view.comment   =   view.name   
            end   if   
      next  
      
      '   go   into   the   sub-packages   
      Dim   f   '   running   folder   
      For   Each   f   In   folder.Packages   
            if   not   f.IsShortcut   then   
                  ProcessFolder   f   
            end   if   
      Next   
end   sub
```



###### 4.点击run即可