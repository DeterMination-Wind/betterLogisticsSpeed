# betterLogisticsSpeed 根目录 Detail

## 这一层在做什么

这是整个模组工程的总控层。这里同时放了三类东西：可编辑输入、构建脚本、以及构建输出目录入口。`src/` 是真正决定功能行为的源码树；`build.gradle`、`settings.gradle`、`mod.json` 共同决定 Gradle 如何把源码打成 Mindustry 可加载的 Java 模组；`build/`、`dist/`、`.gradle/` 则是构建过程中逐步产生的中间结果、缓存和最终分发物。

## 直接内容及职责

- `AGENTS.md`：项目内 AI 工作约束，定义 Java 8、反射容错、bundle 文案、构建发布流程等规则。
- `build.gradle`：最关键的构建脚本，声明 Java 插件、Mindustry `core` 的 `compileOnly` 依赖、`jar`/`zipMod`/`deploy` 等任务。
- `settings.gradle`：给 Gradle 工程命名为 `betterLogisticsSpeed`。
- `mod.json`：Mindustry 识别模组的元数据入口，告诉游戏主类、最低版本、显示名等。
- `README.md`：面向开发者或使用者的概览说明。
- `progress.md`：项目阶段性进度和产物状态记录。
- `src/`：人工维护的源代码和本地化资源。
- `build/`：Gradle 编译、复制、打包、报告输出的主工作目录。
- `dist/`：方便取用的发布目录，里面是从 `build/libs` 复制出的最终 jar/zip。
- `.gradle/`：本地 Gradle 状态缓存，不属于发布内容。

## 实现方式

这一层没有业务逻辑本身，但决定了业务逻辑怎样被组织和变成产物。项目通过 `build.gradle` 指定 `compileOnly "com.github.Anuken.MindustryJitpack:core:v154.2"`，因此源码可以引用 Mindustry API，却不会把游戏本体打进模组包。`jar` 任务把 `src/main` 编译后的 `.class`、资源文件和根目录 `mod.json` 合并为模组 jar；`zipMod` 再生成一个 zip 版本；`deploy` 串联两者并复制到 `dist/` 和上级 `构建/` 目录。

## 与其他层级的关系

- 向下看，`src/main/java` 决定功能实现，`src/main/resources/bundles` 决定界面文案。
- 横向看，`.gradle/` 和 `build/` 都是由这里的 Gradle 配置驱动出来的，只是前者偏缓存，后者偏产物。
- 向外看，`mod.json` 和打包出的 `jar/zip` 是 Mindustry 客户端真正消费的接口。
- 当前仓库是一个很典型的“源码层 + 构建层 + 分发层”三段式结构，文档中的每一层都可以追溯到这里的总控定义。
