# AGENTS.md - betterLogisticsSpeed 工作约束

本文件用于指导 AI 代理在 `betterLogisticsSpeed` 项目内工作。

## 项目边界

- 项目名：`betterLogisticsSpeed`
- 类型：Mindustry Java 客户端模组
- 目标版本：`minGameVersion: 154`
- 主入口：`betterlogisticsspeed.BetterLogisticsSpeedMod`
- 功能范围：物品物流长窗速度显示增强（悬停 UI）

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

## 设置项规范

设置项标题必须提供 `setting.<name>.name`，例如：

- `setting.bls-enabled.name`
- `setting.bls-window-seconds.name`
- `setting.bls-show-total.name`
- `setting.bls-decimals.name`

## 构建/发布规范

每次变更后至少执行：

1. `gradle classes`
2. `gradle deploy`
3. 确认以下产物存在：
   - `dist/betterLogisticsSpeed.jar`
   - `dist/betterLogisticsSpeed.zip`
   - `../构建/betterLogisticsSpeed/betterLogisticsSpeed-<version>.jar`
   - `../构建/betterLogisticsSpeed/betterLogisticsSpeed-<version>.zip`

## 提交约束

- 不改动与当前任务无关文件。
- 不做无关格式化。
- 修改设置项时同步更新中英文 bundle。

命令操作请使用 PowerShell 7（`pwsh`）。
