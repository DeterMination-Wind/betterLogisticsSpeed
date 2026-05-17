# src/main/java/betterlogisticsspeed/features Detail

## 这一层在做什么

这里放项目唯一的功能类 `LongWindowFlowFeature.java`，它承担了整个模组的核心职责：接入原版悬停面板、采样物品流速、计算长窗口平均值、补充设置项和追加 UI 行。

## 直接内容及职责

- `LongWindowFlowFeature.java`：核心功能实现文件。

## 实现方式

这个类采用“单文件承载完整功能”的设计，但内部又拆出 `BuildFlowTracker`、`ItemRateWindow`、`SamplePoint` 三个内部类，把生命周期/UI/反射逻辑与数据结构逻辑分开。它还集中管理所有设置 key、反射字段、缓存集合和当前追踪状态，符合项目“反射访问集中在功能类初始化方法中”的约束。

## 与其他层级的关系

- 上游由入口类调用 `init()`。
- 横向依赖 `src/main/resources/bundles/` 提供设置标题和显示文案。
- 下游编译后会拆成多个 `.class`，进入 `build/classes/.../features/`。
