# src/main/resources/bundles Detail

## 这一层在做什么

这里是本地化键值资源层，负责把设置项标题、描述、扩展流速行标题和缺失值占位符提供给 UI。

## 直接内容及职责

- `bundle.properties`：默认语言文本，定义英文键值。
- `bundle_zh_CN.properties`：简体中文文本。

## 实现方式

Java 代码通过 `Core.bundle.get(...)` 和 `Core.bundle.format(...)` 读取这里的键。当前定义的关键键包括：

- `settings.betterlogisticsspeed`
- `setting.bls-enabled.name`
- `setting.bls-window-seconds.name`
- `setting.bls-window-seconds.description`
- `setting.bls-show-total.name`
- `setting.bls-decimals.name`
- `bls.flow.avg.label`
- `bls.flow.total.label`
- `bls.flow.na`

## 与其他层级的关系

- 这些键直接支撑 `BetterLogisticsSpeedMod` 添加的设置分类和 `LongWindowFlowFeature` 生成的附加 UI 行。
- 复制后会出现在 `build/resources/main/bundles/` 和最终 jar 的 `bundles/` 目录中。
