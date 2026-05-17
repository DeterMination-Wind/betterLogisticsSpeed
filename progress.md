# Progress

## 已完成

- [x] 新建独立 mod 工程：`betterLogisticsSpeed`
- [x] 接入入口：`betterlogisticsspeed.BetterLogisticsSpeedMod`
- [x] 实现长窗统计功能：`LongWindowFlowFeature`
- [x] 反射接入 `PlacementFragment`（`topTable/menuHoverBlock/hover/nextFlowBuild`）
- [x] 在原版物品流速区域下方追加“滑动平均”行
- [x] 支持“总吞吐”行显示开关
- [x] 补充中英文 bundle 文案
- [x] 配置构建与发布流程（`gradle deploy` + `dist` + `构建/` 复制）
- [x] 验证 `gradle classes` 与 `gradle deploy` 通过

## 1.1.0 新增

- [x] MI2-Utilities-Java 兼容：当 `MI2UI.replaceTopTable` 启用时，反射定位
      `mi2u.ui.HoverTopTable.hoverInfo`（`build` + `buildt`），将滑动平均行
      注入 MI2U 重制的悬浮信息栏内的物品流速表下方。
- [x] 兼容回退：未安装/未启用 MI2U 时走原版 `PlacementFragment.topTable` 路径，
      行为与 1.0.0 一致。

## 当前产物

- `dist/betterLogisticsSpeed.jar`
- `dist/betterLogisticsSpeed.zip`
- `../构建/betterLogisticsSpeed/betterLogisticsSpeed-1.1.0.jar`
- `../构建/betterLogisticsSpeed/betterLogisticsSpeed-1.1.0.zip`

## 后续待办

- [ ] 实机 UI 回归（原版路径 + MI2U replaceTopTable 路径双确认）
- [ ] 压力测试：高吞吐网络下的采样稳定性和 GC 观察
- [ ] 评估是否扩展液体长窗统计（可选）
