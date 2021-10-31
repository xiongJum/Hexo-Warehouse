---
title: PlantUML流程图
date: 2020-06-30 09:00:00
cover: https://cdn.jsdelivr.net/gh/xiongJum/Picture/img/78101956_p0.jpg
tag: 
- vscode
- 工具
- 流程图
---



PlantUML(流程图)

--基于vscIDE编辑器的PLantUML模块

##### Ⅰ、说明

+   本流程图是通过使用代码行(伪代码)进行实现
+   需要Java、需要IDE：[Visual Studio Code](https://code.visualstudio.com/)
+   如果需要画时序图和活动图以外的流程图，需要安装grephviz-dot
+   本教程或则记录仅为活动图
+   本次不是完整活动图记录，若需要别的条件或关键词，可以通过附录链接，跳转到官方文档

##### Ⅱ、开始

+   @startuml 和 @enduml 为开始和结束流程
+   start 和 stop为开始和结束，end表现为另种结束icon
+   if、elseif、和else为条件语句。else只能连续出现一次
+   endif 设置多个分支

```python
@startuml [Test测试]
if(线路判断规则) then(按照机具上传方向) 
    if(上下车方向是否相同) then(Y)
        if(上车站点是否大于下车站点) then(N)
            :定为一条行程，线路方向和站点序号为机具上传的方向;
            stop
        else(Y)
            if(上车站点是否等于下车站点) then(Y)
                :定为一条线路，线路方向和序号为机具上传的;
                stop
            else(N)
                if(上车站点大于下车站点的扣费规则) then(扣除最低票价(有优惠))
                    :定为一条行程，线路方向和站点序号为机具上传;
                    stop
                else(扣除最低票价(无优惠))
                    :定为一条行程，线路方向和站点序号为机具上传;
                    stop
                endif
            endif
        endif
    else(Y)
        if(上车站点是否小于下车站点) then(Y)
            :定为一条行程，上下车为机具上传的方向和站点，扣除正常的区间票价;
            stop
        else(N)
            if(上车站点是否等于下车站点) then(Y)
                :定为一条行程上下车为机具上传的方向和站点，扣除线路最低票价;
                stop
            else(N)
                :定为一条行程，扣除方向的最低区间票价;
                stop
            endif
        endif
    endif
else(根据上传站点序号进行判断)
    :此处省略一万字.....;
    stop
@enduml
```

+   缩进不会影响实际的表现，缩进只是本人习惯和方便查看分支
+   查看实时预览图片使用 atl+d组合键
+   文件后缀使用 wad文件或别的后缀名

Ⅲ、附录

[plantuml](https://plantuml.com/zh/)

