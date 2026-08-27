# 架构总览

改 `src/` 下的任何代码前先读本文。它解释 betterLogisticsSpeed 的组织方式：功能如何被装载、吞吐量如何被统计、原版 UI 如何被追加、以及独立运行与 Neon 聚合两种形态的差异。

## 定位与原则

betterLogisticsSpeed 是纯客户端信息增强模组：在原版物品流速显示下方追加长窗口滑动平均行，并提供 OverlayUI 多节点标记列表。三条不变式：

1. **只追加，不重写**：以"后置追加 UI"为主，不替换原版核心逻辑；建筑与物流行为不被修改，所有显示都是本地信息。
2. **反射集中、统一容错**：对 Mindustry 内部字段与外部模组（MI2U、OverlayUI）的访问集中在功能类初始化方法中，统一 `try/catch Throwable`，失败时静默降级而不是崩溃。
3. **无三方依赖**：编译期只依赖 Mindustry core（`compileOnly`），运行时零额外库；构建链不含 D8/dex 步骤。

## 入口与生命周期

入口只有一个类 `betterlogisticsspeed.BetterLogisticsSpeedMod`（`mod.json` 的 `main` 与 `mainX` 同指，无需 X 变体）。真正的逻辑全部在 `features/LongWindowFlowFeature`（静态类）中，经事件驱动：

1. **`init()`**：写入四个设置默认值（`Core.settings.defaults`）并 `refreshSettings()`，注册两条键位（`;` 标记、`'` 删除），挂接以下事件。
2. **`ClientLoadEvent`**：重读设置、初始化 `PlacementFragment` 反射字段、探测 MI2U、重建物品图标缓存、清空追踪状态，并延迟 2 tick 首次尝试注册 OverlayUI 窗口；独立形态下在此注册设置分类（见下文"设置系统"）。
3. **`WorldLoadEvent` / `ResetEvent`**：清空悬停追踪与全部标记节点，隐藏注入行。
4. **`Trigger.update`**（每帧）：重读设置、确保 OverlayUI 注册、处理键位输入、采样悬停建筑与标记节点、节流重建 Overlay 表。
5. **`Trigger.draw`**：在 `Layer.overlayUI` 绘制已标记建筑的世界内编号标记（旋转方框 + 编号圆点）。
6. **`Trigger.uiDrawEnd`**：`updateDisplay()` 定位原版流速表并注入/更新 `bls-flow-extra` 扩展行。

## 核心类与数据流

功能集中在一个文件里，内部类各司其职：

| 类（均为 `LongWindowFlowFeature` 的内部类/辅助类） | 职责 |
| --- | --- |
| `BuildFlowTracker` | 每个被追踪建筑一个；按物品 id 维护 `ItemRateWindow`，产出滑动平均 |
| `ItemRateWindow` / `SamplePoint` | `ArrayDeque` 滑动窗口：保留最近 `windowSeconds` 的速率样本，均值 = sum/size；样本不足 2 个显示 `...` |
| `TrackedItemModule` | `ItemModule` 包装器，替换建筑的 flow 模块；`add/handleFlow/undoFlow` 记账，`updateFlow` 时按间隔结算速率 |
| `FlowSlot`（`DirectItemsSlot` / `FieldFlowSlot`）+ `FlowBinding` | 定位建筑上承载 flow 的字段（`build.items` 直取或反射遍历），装入包装器；`restore()` 还原 |
| `MarkedNode` | 一个标记节点 = 建筑 + tracker + binding；最多 `maxMarkedNodes = 12` 个 |
| `OverlayCompat` | 反射探测 `mindustryX.features.ui.OverlayUI` 并 `registerWindow` 注册 "betterLogisticsSpeed" 窗口 |

两条采样路径：

- **悬停路径**：从 `PlacementFragment.nextFlowBuild` 取当前悬停建筑，`BuildFlowTracker.sample(...)` 每 250ms 读取一次原版 `ItemModule.getFlowRate` 丢进滑窗；`Trigger.uiDrawEnd` 时在 `topTable`（或 MI2U 的 `buildt`）中按"物品图标 + 每秒流速标签"特征定位原版流速表，把名为 `bls-flow-extra` 的行追加到其下方。2.0.1 起行结构只在物品集合或"总吞吐"开关变化时重建，平时逐帧只改 Label 文本。
- **标记路径**：`;` 键标记时，`installMarkedFlow` 用 `TrackedItemModule` 替换建筑的 flow 模块，物品进出在包装器内独立记账（不复用原版共享流速缓存，避免多节点速率串扰），由 `FlowBinding` 负责取消标记时还原。OverlayUI 窗口按 250ms 节流（或 dirty 标记）重建，展示总吞吐与各物品速率；注册失败每 1s 重试。

MI2U 分支：`probeMi2u()` 反射探测 `mi2u.MI2UVars.mi2ui`、`Mindow2.settings`、`SettingHandler.getBool`、`HoverTopTable.hoverInfo/build/buildt`。当 MI2U 启用 `replaceTopTable` 时，悬停信息被重绘进 `HoverTopTable.buildt`，模组改为跟随该表注入，并隐藏原版 `topTable` 里的注入行，保证两条路径互不重复显示。

## 吞吐量统计：移动平均

- 窗口时长 `bls-window-seconds`，默认 10s，可调 2–60s；改动即时生效并重置所有窗口。
- 采样间隔 250ms（`sampleIntervalMs`）：悬停路径每个间隔取一次原版瞬时速率入窗；标记路径把间隔内记账的物品量换算为速率（`amount * 1000 / elapsedMs`）入窗。
- 展示值为窗口内样本的算术平均，窗口滑动时剔除过期样本；样本不足 2 个时显示 `...`（bundle key `bls.flow.na`）。
- "总吞吐"为当前窗口内有数据的各物品速率之和。

## 设置系统

设置项由 `buildSettings` 挂到 `SettingsMenuDialog.SettingsTable`：

| key | 含义 | 默认 | 范围 |
| --- | --- | --- | --- |
| `bls-enabled` | 总开关 | 开 | - |
| `bls-window-seconds` | 长窗口时长 | 10 | 2–60s |
| `bls-show-total` | 显示总吞吐行 | 开 | - |
| `bls-decimals` | 小数位数 | 1 | 1–2 |

- `refreshSettings()` 在 `Trigger.update` 每帧重读，设置改动即时生效；追踪中的 tracker 会同步新的窗口时长（并重置采样）。
- 无 OverlayUI 时设置页追加红色提示行（`bls.overlay.missing`）。
- 键位 `bls-mark-node`（默认 `;`）与 `bls-delete-hovered-node`（默认 `'`）归在 `betterlogisticsspeed` 键位分类，可在 `设置 → 控制` 修改。

## 独立运行 / 并入 Neon 双形态

主类持有 Neon 聚合契约的两个成员：

```java
public static boolean bekBundled = false;
public static void bekBuildSettings(SettingsMenuDialog.SettingsTable table)
```

- **独立运行**（`bekBundled == false`）：`ClientLoadEvent` 中自行 `ui.settings.addCategory("@settings.betterlogisticsspeed", ...)` 注册自己的设置分类；`mod.json` 声明 `softDependencies: ["overlay-compat-bridge"]`，vanilla 客户端安装该桥接模组后即可使用标记列表。
- **并入 Neon 聚合**（`bekBundled == true`，由聚合入口置位）：跳过自建设置分类，由 Neon 的设置系统调用 `bekBuildSettings(...)` 把设置并入总页；`mod.json` 同时标记 `hidden: true`，不在模组列表中单独展示。MindustryX 环境下 OverlayUI 由 MDtX 直接提供。

无论哪种形态，功能代码完全相同；差异只在设置入口的归属与 OverlayUI 的提供方。

## 目录速查

```text
src/main/java/betterlogisticsspeed/
|-- BetterLogisticsSpeedMod.java      入口：bekBundled 契约、事件接线、独立态设置注册
`-- features/
    `-- LongWindowFlowFeature.java    全部功能逻辑：悬停注入、吞吐统计、标记节点、MI2U/OverlayUI 桥
src/main/resources/bundles/           bundle.properties / bundle_zh_CN / bundle_zh_TW
```

## 设计约束（务必保持）

- 纯客户端、纯本地显示；不要求服务器安装，不影响其他玩家。
- 不引入三方依赖；Java 8 字节码（`options.release.set(8)`）。
- 反射访问集中且统一 `try/catch Throwable`；任何环境缺失（字段改名、MI2U 不存在、无 OverlayUI）都自动降级。
- 用户可见文案一律走 bundle key，三份语言文件同步维护。
- 悬停增强不依赖 OverlayUI；OverlayUI 只服务标记列表，缺失时禁用该功能并在设置页提示。
