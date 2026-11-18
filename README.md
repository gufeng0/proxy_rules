# 配置与去广告规则

收集和整理 **Loon、Surge、Quantumult X、Shadowrocket、Egern** 的配置与去广告规则。

本项目从 `wool_scripts` 精简整理而来，不限定单一应用，按客户端分别维护。Shadowrocket 沿用原来的 `module/blockAds.srmodule` 去广告合集形式，当前包含彩云天气规则，后续按需补充，不自动同步上游全集。

## 当前客户端

| 客户端 | 目录与说明 | 当前内容 |
| --- | --- | --- |
| Shadowrocket | [Shadowrocket](./Shadowrocket/readme.md) | 去广告合集（当前包含彩云天气） |

目前只维护 `Shadowrocket/` 独立目录。Loon、Surge、Quantumult X、Egern 属于后续收集范围，有实际内容时再创建对应目录。不同客户端的配置语法与能力不同，不应直接混用。

## 已收集条目

| 应用 / 用途 | 客户端 | 文件 | 功能与限制 |
| --- | --- | --- | --- |
| 彩云天气去广告 | Shadowrocket | [blockAds.srmodule](./Shadowrocket/module/blockAds.srmodule) | 合集中的彩云天气分组：仅拦截 `ad.cyapi.cn`，不拦截活动接口，不修改天气、账户或会员信息 |

其他客户端暂未提供这些规则，后续适配后再更新列表。

## 使用说明

1. 在当前客户端列表中查看对应说明和条目。
2. 使用对应客户端的模块、插件或规则导入方式；完整配置可能覆盖原有设置，导入前先备份。
3. 远程引用使用本项目的 [Raw 模块链接](https://raw.githubusercontent.com/gufeng0/proxy_rules/main/Shadowrocket/module/blockAds.srmodule)。不要直接使用上游链接，否则加载的不是本项目整理后的内容；本地修改需推送后才会出现在远程链接中。
4. 导入后检查规则冲突，通过客户端请求记录确认命中，并验证应用主要功能正常。

当前模块仅包含彩云广告域名拦截，不需要 HTTPS 解密（MITM）、证书或外部脚本。

具体导入操作以客户端当前版本为准。去广告效果可能随应用和接口更新变化，不保证所有条目长期有效或覆盖全部广告。

## 收集与维护约定

- 按客户端分类；Shadowrocket 去广告规则统一维护在 `module/blockAds.srmodule`，在文件内按应用分组，不为每个应用单独拆分模块。新增内容时同步更新本页及客户端索引。
- 有实际条目时再创建客户端目录；配置、模块 / 插件、规则集分别放在其子目录，不创建占位目录或空白可导入配置。
- 每个条目注明原作者、来源、适用范围、外部依赖和已知限制，不将收集内容标为原创。
- 跨客户端适配需要单独检查语法和行为，不仅修改文件扩展名。
- 优先采用影响范围明确的规则；需要 HTTPS 解密或响应重写时，注明域名及可能受影响的功能。
- 不提交节点凭据、Token、Cookie、私钥、证书或个人配置中的敏感信息。
- 不恢复旧的全量同步任务；只收集需要维护的条目。

## 项目结构

```text
.
├── Shadowrocket/          # Shadowrocket 配置、模块与规则
│   ├── readme.md
│   └── module/
│       └── blockAds.srmodule # 去广告合集，按应用分组
├── README.md              # 总索引与维护约定
├── LICENSE                # 沿用上游 GPL-3.0 许可证
└── .gitignore             # 忽略本地配置及敏感文件
```

当前内容无需 Node.js、npm 依赖或构建。已移除旧项目的无关合集、图标库、工具和自动同步工作流；本地编辑器配置保留，不提交到仓库。

## 来源与许可

- 原项目：[fmz200/wool_scripts](https://github.com/fmz200/wool_scripts)。
- 当前彩云广告域名规则来源：原项目 `Shadowrocket/module/blockAds.srmodule`；原作者奶思。
- 本项目为精简修改版；保留作者归属，沿用 [GNU GPL v3](./LICENSE)。后续收集内容需确认其许可及署名要求。

本项目使用全新 Git 历史，仅提交当前整理后的内容，不包含旧项目的提交历史。
