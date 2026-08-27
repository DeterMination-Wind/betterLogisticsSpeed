# 版本与发布

betterLogisticsSpeed 的版本规则、本地构建步骤与 Release 资产安全规则。原则：**版本号与构建都在本地完成**，仓库没有 CI 发布流水线。

## 版本号体系

- 语义版本 `X.Y.Z`（当前 `2.0.1`），没有独立的数字版本码与 tag 体系。
- 版本号写在两处，必须一致：
  - `mod.json` 的 `version`
  - `build.gradle` 的 `version`（决定产物文件名 `betterLogisticsSpeed-<version>.jar|zip`）
- 每个版本的更新说明写入 [RELEASE_NOTES.md](../RELEASE_NOTES.md)（中英双语）；功能进度记录在 [progress.md](../progress.md)。
- 历史脉络：1.0.0 悬停长窗滑动平均行 → 1.1.0 MI2U `replaceTopTable` 兼容 → 2.0.0 多节点标记 + OverlayUI 列表 + 独立采样 → 2.0.1 扩展行结构复用（性能）。

## 发布前本地必做步骤

1. 同步修改 `mod.json` 与 `build.gradle` 两处版本号。
2. 编译与打包：
   ```powershell
   .\gradlew.bat classes
   .\gradlew.bat deploy
   ```
3. 确认四件产物存在且大小合理（见下）。
4. 更新 `RELEASE_NOTES.md`（中英文）与 `progress.md` 的"当前产物"。

## 构建产物链

```text
classes ──► jar ──► copyJarToDist     ──► dist/betterLogisticsSpeed.jar
              │   copyJarToBuildDir  ──► ../构建/betterLogisticsSpeed/betterLogisticsSpeed-<version>.jar
              └─► zipMod ──► copyZipToDist    ──► dist/betterLogisticsSpeed.zip
                          copyZipToBuildDir ──► ../构建/betterLogisticsSpeed/betterLogisticsSpeed-<version>.zip
```

- `deploy` = `jar` + `zipMod`；四个 copy 任务是 finalizer，单跑 `jar` 也会触发对应复制。
- jar 内容 = class 文件 + `mod.json`。构建链不含 D8/`classes.dex` 步骤；并入 Neon 发布时，dex 由 Neon 聚合打包管线负责。
- `dist/` 与 `../构建/betterLogisticsSpeed/` 是两个落点：前者是仓库内固定文件名的便捷产物，后者按版本号命名，用于工作区归档与发版上传。

## Release 资产安全规则

Mindustry 游戏内安装器会取 Release API 返回的**第一个 `.jar`**，且不按操作系统挑资产。因此：

- 一个 Release 必须有且只有一个 `.jar` 资产，即最终可安装包（含 `mod.json` 与主类）；不要把任何中间产物当资产上传。
- `.zip` 资产不计入该限制（安装器只挑 `.jar`）。
- 发布后用 API 复查：
  ```bash
  gh release view <tag> --json assets
  ```
  发现多余 `.jar` 立即删除。
