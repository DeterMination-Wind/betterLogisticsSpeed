# betterLogisticsSpeed

> 用更稳定的长期数据判断物流是否真的够用。

betterLogisticsSpeed 是一个 Mindustry 客户端信息增强模组。原版物流速率容易受短时间波动影响，它补充更稳定的观察方式，帮助你判断一条运输线的真实吞吐，而不是被某一瞬间的数字误导。

它适合检查工厂瓶颈、比较多条运输线和长期观察生产效率。模组只在信息界面追加本地显示，不修改建筑和物流逻辑；OverlayUI 或兼容桥接可提供额外的节点列表，但不是悬停面板增强的必要条件。

## 安装

将 Release 中的 betterLogisticsSpeed 模组包放入 Mindustry 的 mods 目录并启用。

## 构建

~~~powershell
gradle classes
gradle deploy
~~~

## 说明

当前主要服务于物品物流。若使用节点列表功能，需要 MindustryX OverlayUI 或 OverlayCompatBridge。

## 文档

分类文档见 [docs/README.md](docs/README.md)：架构总览、开发指南、版本与发布、测试指南与术语表。
