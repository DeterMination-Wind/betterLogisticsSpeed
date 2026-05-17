# src/main/resources Detail

## 这一层在做什么

这是资源文件总目录。当前项目资源很精简，但非常关键，因为所有用户可见文字都要求走 bundle key，而不是写死在 Java 代码里。

## 直接内容及职责

- `bundles/`：国际化文本。

## 实现方式

Gradle 会把这里的文件复制到 `build/resources/main/`，再打进 jar。运行时 Mindustry 的 `Core.bundle` 根据当前语言环境读取对应文本。

## 与其他层级的关系

- 它与 `src/main/java/` 形成“逻辑 + 文案”配对关系。
- Java 代码中的 `settings.betterlogisticsspeed`、`bls.flow.avg.label` 等键都必须在这里有定义。
