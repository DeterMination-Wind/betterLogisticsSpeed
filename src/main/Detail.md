# src/main Detail

## 这一层在做什么

这是 Gradle `main` 源集的根目录。模组运行时真正需要的所有源码和资源都从这里出发。

## 直接内容及职责

- `java/`：业务逻辑与入口类源码。
- `resources/`：本地化文本等资源。

## 实现方式

Gradle 把 `java/` 和 `resources/` 分开处理：前者编译为 `.class`，后者原样复制进资源输出目录，再一同打包。

## 与其他层级的关系

- 它对应 `build/classes/java/main/` 和 `build/resources/main/` 两条输出分支。
- 对当前模组来说，代码和 bundle 需要配合使用，缺一不可。
