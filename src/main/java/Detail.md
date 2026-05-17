# src/main/java Detail

## 这一层在做什么

这里是 Java 源码总根目录。所有运行时逻辑，包括模组入口、事件注册、反射接入、UI 追加、采样统计，都从这一层开始定义。

## 直接内容及职责

- `betterlogisticsspeed/`：项目主包。

## 实现方式

项目遵循 Java 包名到目录名的标准映射。编译时，目录结构会被镜像到 `build/classes/java/main/`。

## 与其他层级的关系

- 上游是 `src/main/`。
- 下游是 `build/classes/java/main/`。
- 当前工程 Java 源码量很小，但逻辑集中，实际核心只有入口类和一个功能类。
