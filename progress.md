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

## 2.0.0 新增

- [x] 多节点物流速率标记：支持按 `;` 标记当前悬停物流节点，重复标记时重置采样。
- [x] OverlayUI 标记列表：按添加顺序显示最多 12 个节点，并持续展示总速率与各物品速率。
- [x] 行级删除与全部清除：支持按 `'` 删除当前悬停列表行，并提供“全部清除”按钮。
- [x] 世界内编号标记：已标记建筑显示轻量编号，并与列表顺序对应。
- [x] MindustryX / OverlayCompatBridge 兼容：通过反射注册同一 OverlayUI 窗口，缺失时自动降级。
- [x] 独立采样修正：已标记节点不再复用原版共享流速缓存，避免多节点之间互相污染。
- [x] OverlayUI 宽度行为对齐 PGMM：内容宽度跟随父窗口宽度扩展，不再依赖固定宽度占位。

## 当前产物

- `dist/betterLogisticsSpeed.jar`
- `dist/betterLogisticsSpeed.zip`
- `../构建/betterLogisticsSpeed/betterLogisticsSpeed-2.0.0.jar`
- `../构建/betterLogisticsSpeed/betterLogisticsSpeed-2.0.0.zip`

## 后续待办

- [ ] 实机 UI 回归（原版路径 + MI2U replaceTopTable 路径双确认）
- [ ] 压力测试：高吞吐网络下的采样稳定性和 GC 观察
- [ ] 评估是否扩展液体长窗统计（可选）
