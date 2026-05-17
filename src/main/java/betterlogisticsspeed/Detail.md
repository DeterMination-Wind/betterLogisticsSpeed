# src/main/java/betterlogisticsspeed Detail

## 这一层在做什么

这是模组的主包源码层，负责定义入口类并组织功能子包。目录结构直接映射到 `mod.json` 中声明的主类命名空间。

## 直接内容及职责

- `BetterLogisticsSpeedMod.java`：模组入口类。
- `features/`：功能实现子包。

## 实现方式

入口类继承 `mindustry.mod.Mod`，在 `init()` 中先调用 `LongWindowFlowFeature.init()` 建立功能，再通过 `Events.on(ClientLoadEvent.class, ...)` 延后添加设置页分类。这里不直接堆大量业务逻辑，而是把真正实现放到 `features/`，入口层保持很薄。

## 与其他层级的关系

- 这一层通过 `mod.json` 的 `main = "betterlogisticsspeed.BetterLogisticsSpeedMod"` 暴露给 Mindustry。
- `features/` 是它的实际能力来源；没有下层功能类，入口类只剩初始化框架。
