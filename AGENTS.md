# AGENTS.md - betterLogisticsSpeed 工作约束

本文件用于指导 AI 代理在 `betterLogisticsSpeed` 项目内工作。

## 项目边界

- 项目名：`betterLogisticsSpeed`
- 类型：Mindustry Java 客户端模组
- 目标版本：`minGameVersion: 154`
- 主入口：`betterlogisticsspeed.BetterLogisticsSpeedMod`
- 功能范围：物品物流长窗速度显示增强（悬停 UI）、OverlayUI 多节点标记列表（最多 12 节点）、世界内编号绘制、MI2-Utilities-Java `replaceTopTable` 反射桥

## 文档索引

分类文档位于 [docs/](docs/README.md)：文档导航、架构总览、开发指南、版本与发布、测试指南、术语表。从 [docs/README.md](docs/README.md) 进入；文档内容必须与源码对应。

## 代码约束

- Java 8 兼容（`sourceCompatibility/targetCompatibility=1.8`）。
- 不引入新三方依赖。
- 反射访问集中在功能类初始化方法中，统一容错（`try/catch Throwable`）。
- 以“后置追加 UI”为主，不直接重写原版核心逻辑。
- 文案统一走 bundle key，不写死用户可见字符串。

## 当前实现关键点

- 功能类：`src/main/java/betterlogisticsspeed/features/LongWindowFlowFeature.java`
- 通过 `PlacementFragment` 的反射字段定位悬停状态与目标建筑：
  - `topTable`
  - `menuHoverBlock`
  - `hover`
  - `nextFlowBuild`
- 在原版物品流速区域下方注入额外表：`bls-flow-extra`
- OverlayUI 标记列表：反射注册 `mindustryX.features.ui.OverlayUI` 窗口（`betterLogisticsSpeed`，最多 12 个标记节点），无 OverlayUI 时自动禁用标记功能
- 键位：`;`（`bls-mark-node`）标记/重置当前悬停节点，`'`（`bls-delete-hovered-node`）删除鼠标悬停的列表行
- 世界标记：`drawWorldMarkers` 在 `Layer.overlayUI` 绘制旋转方框与编号，编号与 OverlayUI 列表顺序对应
- MI2U 反射桥：`probeMi2u()` 探测 `mi2u.MI2UVars.mi2ui`、`Mindow2.settings`、`SettingHandler.getBool("replaceTopTable")`、`HoverTopTable` 的 `hoverInfo`/`build`/`buildt`；MI2U 启用 `replaceTopTable` 时改为跟随 `HoverTopTable.buildt` 注入扩展行
- 标记节点采样：以 `TrackedItemModule` 包装替换目标建筑的 flow `ItemModule`（`FlowBinding` 负责还原），独立追踪，避免不同标记节点间速率串扰

## 设置项规范

设置项标题必须提供 `setting.<name>.name`，例如：

- `setting.bls-enabled.name`
- `setting.bls-window-seconds.name`
- `setting.bls-show-total.name`
- `setting.bls-decimals.name`

## Neon 聚合形态（bekBundled）

本模组可独立安装，也可并入 Neon 聚合模组，靠主类契约切换：

- 主类提供 `public static boolean bekBundled` 与 `public static void bekBuildSettings(SettingsMenuDialog.SettingsTable)`；重构不得移除或改名这两个成员。
- 独立态（`bekBundled == false`）：`ClientLoadEvent` 中自行 `ui.settings.addCategory(...)` 注册设置分类；vanilla 客户端依赖 `softDependencies` 声明的 `overlay-compat-bridge` 提供 OverlayUI。
- 聚合态（`bekBundled == true`，由 Neon 置位）：跳过自建设置分类，设置由 Neon 总入口调用 `bekBuildSettings(...)` 接管；`mod.json` 标记 `hidden: true`，不在模组列表单独展示。
- 并入 Neon 发布时的 `classes.dex` 打包由 Neon 侧聚合管线负责，本仓库构建链不含 D8。

## 构建/发布规范

每次变更后至少执行：

1. `gradle classes`
2. `gradle deploy`
3. 确认以下产物存在：
   - `dist/betterLogisticsSpeed.jar`
   - `dist/betterLogisticsSpeed.zip`
   - `../构建/betterLogisticsSpeed/betterLogisticsSpeed-<version>.jar`
   - `../构建/betterLogisticsSpeed/betterLogisticsSpeed-<version>.zip`
4. 涉及悬停注入、标记节点或 MI2U/OverlayUI 桥的改动，按 [docs/testing.md](docs/testing.md) 的手测清单实机回归。

## 提交约束

- 不改动与当前任务无关文件。
- 不做无关格式化。
- 修改设置项时同步更新全部语言 bundle（英文 / 简体中文 / 繁体中文）。
- 提交信息使用简短前缀风格（如 `betterLogisticsSpeed: ...`、`docs:`、`release: ...`），只提交与当前任务相关的文件。

命令操作请使用 PowerShell 7（`pwsh`）。
