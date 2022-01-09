---
title: PlantUML
date: 2020-06-30 09:00:00
# photos: https://cdn.jsdelivr.net/gh/xiongJum/Picture/img/78101956_p0.jpg
categories: 
    - [奇技淫巧]
tags: 
- 工具
- 流程图
- PlantUML
- vscode
---

--基于vscIDE编辑器的PLantUML模块。

1, 本流程图是通过使用代码行(伪代码)进行实现
2, 需要Java、需要IDE：[Visual Studio Code](https://code.visualstudio.com/)
3, 如果需要画时序图和活动图以外的流程图，需要安装grephviz-dot
4, 本教程或则记录仅为活动图
5, 本次不是完整活动图记录，若需要别的条件或关键词，可以通过附录链接，跳转到官方文档

<!--more-->
# 1. 开始

1, @startuml 和 @enduml 为开始和结束流程
2, start 和 stop为开始和结束，end表现为另种结束icon
3, if、elseif、和else为条件语句。else只能连续出现一次
4, endif 设置多个分支

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

# 2. 説明

1, 缩进不会影响实际的表现，缩进只是本人习惯和方便查看分支
2, 查看实时预览图片使用 atl+d组合键
3, 文件后缀使用 wad文件或别的后缀名

# 3. 參考文檔

[plantuml](https://plantuml.com/zh/)

