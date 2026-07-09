# betterLogisticsSpeed v2.0.0

## 中文

### 主要更新

- 新增多节点物流速率标记：
  按 `;` 可标记当前悬停的物流建筑，重复标记同一建筑会重置它的采样窗口。
- 新增 OverlayUI 标记列表：
  按添加顺序显示最多 12 个节点，支持总吞吐与各物品速率的持续查看。
- 新增节点管理操作：
  按 `'` 可删除当前鼠标悬停的列表行，窗口底部提供“全部清除”。
- 新增世界内编号标记：
  已标记建筑会绘制轻量编号，与列表顺序一一对应。
- 新增跨环境 OverlayUI 兼容：
  在 MindustryX 中直接注册 OverlayUI；在 vanilla + OverlayCompatBridge 中通过反射注册同一窗口；没有 OverlayUI 时自动禁用标记列表。

### 改进

- 保留原版悬停速率显示的同时，继续在其下方追加长窗口滑动平均速率。
- 多节点采样改为独立追踪，不再复用原版 `ItemModule` 的共享流速缓存，避免不同标记节点之间的速率串扰。
- OverlayUI 内容宽度改为跟随父窗口宽度扩展，避免靠固定宽度撑开。
- 支持在建筑失效、拆除、换图、Reset 后自动清理无效标记。
- 保持 MI2-Utilities-Java `replaceTopTable` 路径兼容。

### 兼容性说明

- 需要 Mindustry `minGameVersion: 154`。
- MindustryX 可直接使用 OverlayUI 窗口。
- vanilla 客户端安装 OverlayCompatBridge 后可使用同一标记列表窗口。
- vanilla 无 OverlayCompatBridge 时，悬停面板增强仍可使用，但标记列表功能会禁用并在设置页提示。

## English

### Highlights

- Added multi-node logistics rate marking:
  press `;` to mark the currently hovered logistics building, and re-marking the same building resets its sampling window.
- Added an OverlayUI tracked-node list:
  shows up to 12 marked nodes in insertion order with live total and per-item rates.
- Added node management actions:
  press `'` to remove the currently hovered OverlayUI row, and use the built-in `Clear All` action at the bottom.
- Added in-world numbered markers:
  marked buildings are labeled in the world and match the list order.
- Added cross-environment OverlayUI compatibility:
  the same window now works on MindustryX and on vanilla with OverlayCompatBridge via reflection, while degrading cleanly when no OverlayUI is available.

### Improvements

- Kept the original hover flow display and continued appending a long-window moving-average readout underneath it.
- Reworked marked-node sampling to use isolated tracking instead of vanilla `ItemModule` shared flow caches, preventing cross-node contamination.
- Changed OverlayUI content sizing to follow the parent window width instead of relying on a fixed-width spacer.
- Automatically clears invalid marks when buildings disappear, are removed, or the world resets/changes.
- Preserved compatibility with the MI2-Utilities-Java `replaceTopTable` hover path.

### Compatibility

- Requires Mindustry `minGameVersion: 154`.
- MindustryX supports the OverlayUI window directly.
- Vanilla clients can use the same tracked-node window with OverlayCompatBridge installed.
- Without OverlayCompatBridge, the hover-panel enhancement still works, but the tracked-node list is disabled and the settings page shows a compatibility hint.
