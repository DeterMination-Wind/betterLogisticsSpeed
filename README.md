# betterLogisticsSpeed

一个 Mindustry Java 客户端模组（`minGameVersion: 154`），用于在原版建筑悬停信息中追加更稳定的物流速度读数。

## 功能

- 保留原版物品流速显示（`items/s`）。
- 在原版流速行下方追加“长窗口滑动平均”流速。
- 可选显示“总吞吐”行（所有当前显示物品的长窗速度求和）。
- 不改原版 block 逻辑，仅在 UI 悬停面板后置追加信息。

## 设置项

- `Enable betterLogisticsSpeed`：总开关（默认开启）。
- `Long-window duration`：滑动窗口秒数（2~60，默认 10）。
- `Show total throughput row`：是否显示总吞吐行（默认开启）。
- `Decimal places`：显示小数位（1~2，默认 1）。

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
- 若目标客户端字段结构变化导致反射失败，功能会静默降级为不显示扩展行（不影响游戏运行）。
