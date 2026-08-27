# betterLogisticsSpeed 文档

betterLogisticsSpeed 的分类文档。文档以中文为主，功能名保留英文本名，便于与代码和设置界面对照。

## 从哪里开始

| 你是 | 从这里开始 |
| --- | --- |
| 玩家 | [README](../README.md)：安装与功能简介 |
| 想了解模组怎么实现的人 | [架构总览](architecture.md) |
| 想改代码 / 排查问题 | [开发指南](development.md) 与[架构总览](architecture.md) |
| 准备发版 | [版本与发布](release.md) |
| 改完想验证 | [测试指南](testing.md) |
| 遇到不认识的词 | [术语表](glossary.md) |

## 文档地图

```text
docs/
|-- README.md          本页：文档导航
|-- architecture.md    架构总览：入口、生命周期、数据流、设置系统、聚合形态
|-- development.md     开发指南：环境、构建命令、代码风格、调试
|-- release.md         版本与发布：版本号、构建产物链、Release 资产安全
|-- testing.md         测试指南：手测清单（按功能）
`-- glossary.md        术语表
```

## 相关文件

- [README.md](../README.md)：项目主页，面向玩家的安装与功能简介。
- [AGENTS.md](../AGENTS.md)：仓库维护约束（代码约束、设置项规范、构建/发布与提交规范）。
- [RELEASE_NOTES.md](../RELEASE_NOTES.md)：逐版本中英文更新说明。
- [progress.md](../progress.md)：功能进度与后续待办。

## 维护约定

- README 描述"用"，architecture.md 描述"怎么实现"。同一主题两边都出现时，README 从操作视角写，架构文档从代码结构写。
- 按键可能被玩家自定义；文档只写默认绑定（`;` / `'`），并注明可在 `设置 → 控制` 中修改。
- 用户可见文案一律走 bundle key；新增或修改文案需同步 `bundle.properties`、`bundle_zh_CN.properties`、`bundle_zh_TW.properties` 三份。
- 文档内容必须与源码对应，不写尚未实现的功能。
