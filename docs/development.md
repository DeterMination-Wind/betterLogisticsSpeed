# 开发指南

环境、日常构建命令与代码约束。仓库级硬性约束在根目录 [AGENTS.md](../AGENTS.md)，两者冲突时以 AGENTS.md 为准。

## 环境

- 编译目标为 Java 8 字节码（`sourceCompatibility/targetCompatibility = 1.8`，且 `options.release.set(8)`）；运行 Gradle 9.3 wrapper 需 JDK 17+，编译产物仍兼容 Java 8。
- Gradle：仓库自带 wrapper（Gradle 9.3.0），Windows 下用 `.\gradlew.bat`。
- Mindustry API：编译期 `compileOnly com.github.Anuken.MindustryJitpack:core:v154.2`（JitPack），可用 `-PmindustryVersion=<ver>` 覆盖；`mod.json` 声明 `minGameVersion: 154`。
- 命令操作使用 PowerShell 7（`pwsh`）。

## 常用命令

```powershell
# 只编译检查（最快验证）
.\gradlew.bat classes

# 完整产物：jar + zip，并自动复制到 dist/ 与 ../构建/betterLogisticsSpeed/
.\gradlew.bat deploy

# 单跑 jar / zip（jar 的 finalizer 也会复制产物）
.\gradlew.bat jar
.\gradlew.bat zipMod

# 清理
.\gradlew.bat clean
```

`deploy` = `jar` + `zipMod`，复制由 finalizer 自动完成：

| 任务 | 产物 | 说明 |
| --- | --- | --- |
| `jar` | `build/libs/betterLogisticsSpeed.jar` → 复制到 `dist/betterLogisticsSpeed.jar`、`../构建/betterLogisticsSpeed/betterLogisticsSpeed-<version>.jar` | class 文件 + `mod.json`，无 dex 步骤 |
| `zipMod` | 同上，名为 `betterLogisticsSpeed.zip` | 与 jar 内容一致 |

仓库没有自定义测试任务，`test` 只是 Java 插件的默认空任务（无测试源码）。验证以 `classes` 编译 + 实机手测为主，见[测试指南](testing.md)。

## 代码风格要点

- Java 8 语法与 API，不引入新三方依赖。
- 反射访问集中在功能类初始化方法（`tryInitReflection` / `probeMi2u`），统一 `try/catch Throwable` 容错；新增反射点照此办理。
- 以"后置追加 UI"为主：定位原版表后追加自己的行/窗口，不重写原版核心逻辑。
- 用户可见文案一律走 bundle key；修改设置项或提示时同步更新 `bundle.properties`、`bundle_zh_CN.properties`、`bundle_zh_TW.properties` 三份。
- 环境缺失必须降级而不是崩溃：MI2U 缺失走原版路径，OverlayUI 缺失禁用标记列表。
- 保持变更聚焦，不做无关格式化与重构。

## 调试建议

- 反射点对 Mindustry 版本敏感：`topTable/menuHoverBlock/hover/nextFlowBuild` 等字段若在新版改名，失败是静默的（`catch Throwable ignored`），表现为悬停增强不出现；先怀疑字段名，再怀疑逻辑。
- OverlayUI 注册失败会在首次失败时输出 `[betterLogisticsSpeed] OverlayUI compatibility registration failed.` 日志；之后每 1s 重试一次。
- MI2U 探测一次性：类不存在即置 `mi2uClassMissing` 不再尝试；调试 MI2U 路径需重开游戏。
- 功能路径双确认：原版 `PlacementFragment.topTable` 路径与 MI2U `replaceTopTable` 路径都要实机看一遍，确认不会同时出现两份扩展行。
- 高吞吐压力下关注采样稳定性与 GC（标记节点记账路径每 250ms 结算一次）。
