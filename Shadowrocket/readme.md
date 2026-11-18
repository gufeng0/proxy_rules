# Shadowrocket

收集 Shadowrocket 配置与去广告规则。沿用原来的 `module/blockAds.srmodule` 合集形式，去广告规则在同一个模块内按应用分组，不单独拆分应用模块。

## 去广告合集

文件：[blockAds.srmodule](./module/blockAds.srmodule)。当前包含彩云天气、中国移动规则，后续其他应用的去广告规则也添加到这个合集。

## 已收集条目

| 应用 / 用途 | 文件 | 说明 |
| --- | --- | --- |
| 彩云天气安全去广告 | [blockAds.srmodule](./module/blockAds.srmodule) 的彩云天气分组 | 仅拦截明确的广告域名 `ad.cyapi.cn` |
| 中国移动去广告 | [blockAds.srmodule](./module/blockAds.srmodule) 的中国移动分组 | 开屏接口空响应、广告列表及悬浮窗接口拦截，需要 MITM，当前版本未实测 |

## 使用方法

将 `blockAds.srmodule` 导入 Shadowrocket 并启用，启动连接后重新打开对应应用。

远程模块地址：

```text
https://raw.githubusercontent.com/gufeng0/proxy_rules/main/Shadowrocket/module/blockAds.srmodule
```

本地修改需提交并推送后，远程更新才会加载新内容；也可直接导入本地模块文件。不要使用旧项目的模块地址。

中国移动规则需要在 Shadowrocket 中开启 HTTPS 解密（MITM），安装并信任其证书。模块只追加 `client.app.coc.10086.cn`、`app.10086.cn` 两个域名，不提供证书、私钥或认证凭据；不应分享自己的解密证书或私钥。具体操作以客户端当前版本为准。

### 当前彩云规则的功能范围

- 只有一条精确域名拦截规则，不拦截整个 `cyapi.cn` 后缀。
- 不拦截 `api.caiyunapp.com` 或 `wrapper.cyapi.cn` 的活动接口，避免影响天气助手、彩云 AI 等入口。
- 不修改天气数据、账户信息、会员状态或认证凭据。
- 彩云分组只有域名拦截，不使用响应重写、脚本或外部响应文件，无需 HTTPS 解密；合集中的 MITM 配置仅用于中国移动。

本模块不保证去除所有彩云广告。已有其他模块时，请检查是否仍启用了彩云活动接口拦截或账户重写。

### 中国移动规则的功能范围

| 接口 | 处理方式 | 上游标注用途 |
| --- | --- | --- |
| `client.app.coc.10086.cn/biz-orange/DN/init/startInit` | 返回空白文本 | 开屏广告 |
| `client.app.coc.10086.cn/biz-orange/DN/explorePage/getAdverList` | 拒绝请求 | 广告列表 |
| `client.app.coc.10086.cn`、`app.10086.cn` 下的 `/biz-orange/DN/emotionMarket` | 拒绝请求 | 多个悬浮窗 |

- 三条规则都针对中国移动 App，不是“中国移动云盘”规则；不封锁整个 `10086.cn` 或其业务域名，也不加入上游的 IP 拦截。
- 广告列表及悬浮窗可能包含运营活动或功能入口；`startInit` 属于初始化接口，不能保证当前版本没有其他功能受影响。
- 沿用上游的路径前缀匹配方式，同前缀的后续路径也可能命中，不代表只匹配表中完整 URL。
- 不修改账户、话费、套餐或支付响应，但仍需实测这些功能是否受接口拦截或 HTTPS 解密影响。
- 如中国移动出现异常，可注释合集内的三条中国移动接口规则进行对照；若仍异常，再从解密域名列表中移除这两个中国移动域名，保留其他模块需要的域名。

### 验证与排查

1. 在请求记录中确认 `ad.cyapi.cn` 被拒绝。
2. 确认天气查询、定位、账户和常用入口仍可正常使用。
3. 中国移动：确认上述三个接口命中对应规则，检查开屏、广告列表和悬浮窗的变化，并验证登录、话费和套餐查询、充值、支付及常用入口正常。
4. 如开屏广告仍存在，先检查连接、MITM 及证书信任，再排查广告缓存；上游提示可能需要清理缓存或重新安装，重新安装前确认可以再次登录。
5. 如出现异常，禁用本模块后重新打开应用进行对照，检查是否有其他规则同时生效。

应用版本和接口可能变化，实际效果需在设备上验证。

彩云规则来自 [fmz200/wool_scripts](https://github.com/fmz200/wool_scripts) 的 `Shadowrocket/module/blockAds.srmodule`，原作者奶思，只保留广告域名拦截，不包含原独立彩云模块的活动接口规则。

中国移动规则来自同项目的 [ChinaMobile.srmodule](https://github.com/fmz200/wool_scripts/blob/main/Shadowrocket/module/split/partZ/ChinaMobile.srmodule)，原作者奶思，沿用三个接口的处理方式，不包含 IP 拦截。接口用途依据上游规则及注释，未在当前中国移动 App 上实测。

## 目录约定

- `config/`：完整配置，导入前备份原有设置。
- `module/blockAds.srmodule`：统一去广告合集，按应用分组追加规则，不拆分单应用模块。
- `rule/`：独立规则集。

新增条目需注明作者、来源、适用范围、依赖及验证情况，并更新[项目总索引](../README.md)。
