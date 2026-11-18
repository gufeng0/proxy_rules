# Shadowrocket

收集 Shadowrocket 配置与去广告规则。沿用原来的 `module/blockAds.srmodule` 合集形式，去广告规则在同一个模块内按应用分组，不单独拆分应用模块。

## 去广告合集

文件：[blockAds.srmodule](./module/blockAds.srmodule)。当前仅保留彩云天气规则，后续其他应用的去广告规则也添加到这个合集。

## 已收集条目

| 应用 / 用途 | 文件 | 说明 |
| --- | --- | --- |
| 彩云天气安全去广告 | [blockAds.srmodule](./module/blockAds.srmodule) 的彩云天气分组 | 仅拦截明确的广告域名 `ad.cyapi.cn` |

## 使用方法

将 `blockAds.srmodule` 导入 Shadowrocket 并启用，启动连接后重新打开彩云天气。远程安装时使用你自己仓库中该文件的 Raw 链接，不要使用旧项目的模块地址。

### 当前彩云规则的功能范围

- 只有一条精确域名拦截规则，不拦截整个 `cyapi.cn` 后缀。
- 不拦截 `api.caiyunapp.com` 或 `wrapper.cyapi.cn` 的活动接口，避免影响天气助手、彩云 AI 等入口。
- 不修改天气数据、账户信息、会员状态或认证凭据。
- 不包含响应重写、脚本、外部响应文件或 MITM 配置，无需 HTTPS 解密。

本模块不保证去除所有彩云广告。已有其他模块时，请检查是否仍启用了彩云活动接口拦截或账户重写。

### 验证与排查

1. 在请求记录中确认 `ad.cyapi.cn` 被拒绝。
2. 确认天气查询、定位、账户和常用入口仍可正常使用。
3. 如出现异常，禁用本模块后重新打开应用进行对照，检查是否有其他规则同时生效。

应用版本和接口可能变化，实际效果需在设备上验证。

规则来自 [fmz200/wool_scripts](https://github.com/fmz200/wool_scripts) 的 `Shadowrocket/module/blockAds.srmodule`，原作者奶思。本项目仅保留广告域名拦截，不包含原独立彩云模块的活动接口规则。

## 目录约定

- `config/`：完整配置，导入前备份原有设置。
- `module/blockAds.srmodule`：统一去广告合集，按应用分组追加规则，不拆分单应用模块。
- `rule/`：独立规则集。

新增条目需注明作者、来源、适用范围、依赖及验证情况，并更新[项目总索引](../README.md)。
