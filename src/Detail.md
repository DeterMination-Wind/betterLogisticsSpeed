# src Detail

## 这一层在做什么

`src/` 是整个工程最重要的可编辑输入层。和 `build/`、`dist/` 不同，这里不是构建结果，而是功能定义本身。

## 直接内容及职责

- `main/`：主源集，包含模组运行所需的 Java 代码和资源。

## 实现方式

Gradle Java 插件按标准约定读取 `src/main/java` 与 `src/main/resources`。当前项目没有测试源集，因此所有有效输入都集中在 `main/` 下。

## 与其他层级的关系

- 上游没有再细分的工程内输入层。
- 下游直接驱动 `build/classes/` 和 `build/resources/`。
- 理解项目功能时，优先看这里，而不是看生成物目录。
