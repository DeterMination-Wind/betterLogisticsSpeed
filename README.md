# betterLogisticsSpeed
<h1 align="center">
  <a href="https://github.com/DeterMination-Wind/betterLogisticsSpeed/releases/latest"><img src="https://img.shields.io/github/v/release/DeterMination-Wind/betterLogisticsSpeed?display_name=release&label=Latest%20Release&color=green"></a>
  <a href="https://github.com/DeterMination-Wind/betterLogisticsSpeed/releases"><img src="https://img.shields.io/github/downloads/DeterMination-Wind/betterLogisticsSpeed/total?label=Downloads&color=blue"></a>
  <a href="LICENSE"><img src="https://img.shields.io/github/license/DeterMination-Wind/betterLogisticsSpeed?label=License"></a>
  <a href="https://github.com/DeterMination-Wind/betterLogisticsSpeed"><img src="https://img.shields.io/github/stars/DeterMination-Wind/betterLogisticsSpeed?style=flat&label=Star%20this%20mod!&color=yellow"></a>
</h1>

一个 Mindustry Java 客户端模组（`minGameVersion: 154`），用于增强物流建筑的速率显示：既保留悬停面板中的长窗平均速率，也支持多节点标记列表与 OverlayUI 显示。

## 功能

- 保留原版物品流速显示（`items/s`）。
- 在原版流速行下方追加“长窗口滑动平均”流速。
- 可选显示“总吞吐”行（所有当前显示物品的长窗速度求和）。
- 支持用 `;` 标记当前悬停的物流节点，并在 OverlayUI 中持续查看最多 12 个节点的速率。
- 支持用 `'` 删除当前鼠标悬停的 OverlayUI 行，并可一键“全部清除”。
- 已标记节点会在世界中绘制编号，与 OverlayUI 列表顺序对应。
- 支持 MindustryX OverlayUI，以及 vanilla + OverlayCompatBridge 的兼容窗口注册。
- 兼容 MI2-Utilities-Java：启用其 `replaceTopTable` 设置后，扩展行会跟随 MI2U 的悬停信息窗口继续显示。
- 不改原版 block 逻辑，仅在 UI 悬停面板后置追加信息。

## 设置项

- `Enable betterLogisticsSpeed`：总开关（默认开启）。
- `Long-window duration`：滑动窗口秒数（2~60，默认 10）。
- `Show total throughput row`：是否显示总吞吐行（默认开启）。
- `Decimal places`：显示小数位（1~2，默认 1）。
- `;`：标记/重置当前悬停物流节点。
- `'`：删除当前悬停的 OverlayUI 行。

## 构建

```bash
gradle classes
gradle deploy
```

`deploy` 产物：

- `dist/betterLogisticsSpeed.jar`
- `dist/betterLogisticsSpeed.zip`
- `../构建/betterLogisticsSpeed/betterLogisticsSpeed-<version>.jar`
- `../构建/betterLogisticsSpeed/betterLogisticsSpeed-<version>.zip`

## 说明

- 当前统计范围为“物品物流”。
- 仅在建筑悬停信息区域生效。
- 标记列表需要 MindustryX OverlayUI 或 OverlayCompatBridge；未安装时会自动禁用列表功能，但悬停面板增强仍然可用。
- 安装 MI2-Utilities-Java 且启用 `replaceTopTable` 时，悬停面板增强在 MI2U 的悬停信息窗口中生效。
- 若目标客户端字段结构变化导致反射失败，功能会静默降级为不显示扩展行（不影响游戏运行）。
