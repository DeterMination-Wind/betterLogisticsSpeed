# 术语表

按主题归类的 betterLogisticsSpeed 术语。每条说明它在本模组语境下的准确含义。

## 物流与统计

### 物流节点（logistics node）
可被追踪的物流建筑：`block.displayFlow` 为真、分类为 分发（distribution）或液体（liquid）、且 `flowItems()` 非空的建筑。悬停增强与标记列表都以它为对象。

### 吞吐量（throughput）
单位时间通过物流建筑的物品量，以"物品/秒"展示。"总吞吐"是当前窗口内有数据的各物品速率之和，由 `bls-show-total` 控制是否显示。

### 滑动平均 / 移动平均（moving average）
在最近一个时间窗口上取样本算术平均，以抑制短期波动。本模组用它计算物流速率：窗口越长数值越稳但反应慢，越短越灵敏但噪声大。Neon 术语表同样收录该词条。

### 长窗口（long window）
`bls-window-seconds` 设置的统计窗口，默认 10s，可调 2–60s。实现是 `ItemRateWindow`：`ArrayDeque<SamplePoint>` 滑动队列，剔除过期样本；样本不足 2 个时显示 `...`（`bls.flow.na`）。

### 采样间隔（sample interval）
两次入窗的最小间隔，250ms（`sampleIntervalMs`）。悬停路径每间隔读取一次原版瞬时速率；标记路径把间隔内记账的物品量换算为速率。

### 独立采样（isolated tracking）
标记节点用 `TrackedItemModule` 包装替换建筑的原版 `ItemModule`，自行记账，不复用原版共享流速缓存，避免多个标记节点之间速率串扰。`FlowBinding` 在取消标记时还原原模块。

## UI 与注入

### bls-flow-extra
本模组注入到原版物品流速表下方的扩展行名字。原版流速表按"物品图标 + 每秒流速标签"特征定位，注入行随悬停对象切换显示/隐藏。

### PlacementFragment
原版建造/悬停信息 UI 片段。模组反射读取其 `topTable`、`menuHoverBlock`、`hover`、`nextFlowBuild` 字段定位悬停状态与目标建筑；字段名随游戏版本变化时这里最容易失效。

### MI2U / replaceTopTable
MI2-Utilities-Java 模组。其 `replaceTopTable` 设置开启时，悬停信息栏被 `HoverTopTable` 重绘；本模组经 `probeMi2u()` 反射探测后改为跟随 `HoverTopTable.buildt` 注入，并隐藏原版路径的注入行。

### OverlayUI
MindustryX 的游戏内悬浮窗管理器。本模组以反射注册名为 `betterLogisticsSpeed` 的窗口（设置名"物流速率"），按 250ms 节流重建，列出至多 12 个标记节点。

### OverlayCompatBridge
vanilla 客户端提供 OverlayUI 能力的桥接模组，`mod.json` 中的 `softDependencies`。没有 MindustryX 也没有它时，悬停增强仍可用，标记列表禁用并在设置页提示。

## 项目形态

### bekBundled
主类上的静态布尔标记，表示"当前运行在 Neon 聚合环境中"。为 `true` 时不自建设置分类，由 Neon 调用 `bekBuildSettings(...)` 把设置并入总页；独立安装时为 `false`，模组自行 `addCategory`。

### 聚合形态（Neon）
本模组作为 Neon 聚合模组子模组并入时的形态：设置由聚合入口接管，功能代码与独立形态完全相同；`mod.json` 额外标记 `hidden: true`，不在模组列表中单独展示。

### minGameVersion 154
`mod.json` 声明的最低游戏版本，编译依赖对应 `MindustryJitpack core v154.2`。

### bundle
Mindustry 的 i18n 文案文件。本模组维护 `bundle.properties`（英文）、`bundle_zh_CN.properties`、`bundle_zh_TW.properties` 三份，位于 `src/main/resources/bundles/`；用户可见文案不允许硬编码。
