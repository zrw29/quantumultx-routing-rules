# Quantumult X 分流配置

此仓库维护一份适用于 Quantumult X（iOS / macOS）的完整配置，提供策略组、分流规则引用和策略组图标。配置不包含代理服务器节点，也不是 VPN 服务；使用者需要在 Quantumult X 中自行添加可信的节点订阅。

English summary: A Quantumult X routing configuration for iOS and macOS. It provides policy groups, rule references, and icons. It does not include proxy nodes or subscription credentials.

## 项目性质与免责声明

本项目主要供作者个人自用，并分享配置思路，不构成适用于所有人的通用方案。规则和策略组按作者的个人需求维护，可能调整、停止更新或失效。

如需使用，请自行了解并按自己的设备、Quantumult X 版本、网络环境和节点订阅进行配置、检查与排错。作者不保证规则的准确性、完整性、稳定性或持续可用性，也不保证适配所有环境；本项目不承诺答疑或技术支持。使用前请自行评估并承担相应风险。

## 快速开始

配置订阅直链：

[QuantumultX.conf](https://raw.githubusercontent.com/zrw29/quantumultx-routing-rules/main/QuantumultX.conf)

1. 在 Quantumult X 的配置文件管理中，通过上方直链下载或订阅配置。
2. 在 Quantumult X 中单独添加自己的代理节点订阅。
3. 在策略页选择服务或地区策略组需要使用的节点。
4. 配置更新后，使用 QX 的配置更新功能获取最新规则和图标链接。

请不要把包含账号、密码、订阅 token 或私有服务器地址的节点订阅写入公开仓库。

## 当前策略组

| 策略组 | 用途与选择方式 |
| --- | --- |
| 通用代理 | 可选择全部代理节点；作为未命中流量的默认出口。 |
| 日本节点、新加坡节点、美国节点、台湾节点、香港节点 | 根据节点名称中的地区关键词和旗帜筛出相应节点，供手动选择。 |
| ChatGPT | 可选择日本、新加坡、通用代理或 direct。ChatGPT/OpenAI 域名规则指向此组。 |
| Netflix-HK | 可选择香港节点、通用代理或 direct；Netflix 域名规则指向此组。 |
| YouTube、Telegram、Google、Microsoft、X、Spotify | 每组都可选择 direct、通用代理或任一代理节点。选择通用代理后，可在通用代理组集中切换实际节点。 |
| Apple | 默认选择 direct，也可手动切换到通用代理。Apple 域名规则和 Apple 远程规则指向此组。 |

节点地区组依赖订阅中的节点标签。若服务商使用的名称未包含配置正则中的地区词，需调整 QuantumultX.conf 中相应的 server-tag-regex。

未命中分流规则的流量使用通用代理。未匹配到其他 UDP 策略的 UDP 流量按当前配置拒绝。

## 分流逻辑

- localhost、局域网和本机地址按本地规则直连。
- 中国大陆常用服务通过 Blackmatrix7 China.list 远程规则直连；截至 2026-11-06 的覆盖核对，本地额外补充该列表未明确覆盖的 `lixiang.com`（理想汽车）、`xhslink.com`（小红书分享短链）、`ke.com`（贝壳找房）、`honor.com`（荣耀新官网）和 `zto.com`（中通快递）。
- Apple、ChatGPT/OpenAI、Netflix 和 X 的关键域名由 QuantumultX.conf 中的本地规则分配到对应策略组。
- Apple、Google、Microsoft、YouTube、Twitter/X、Telegram、Spotify 和中国大陆规则通过远程规则列表加载，并在配置中指定目标策略组。
- 中国大陆规则列表命中的流量走 direct。该规则库包含域名、IP、IPv6 和 User-Agent 规则；配置没有使用 GEOIP,CN。
- `12306.cn`、`feishu.cn`、`vivo.com.cn`、`kuaidi100.com` 等高频服务已由上游 China.list 覆盖，因此没有在本地重复添加。
- Netflix 使用域名规则，没有导入大范围共享云服务 IP 段。
- 其余未命中规则的流量按 final 规则走通用代理。

远程规则来自 [Blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script)，链接和 force-policy 映射以 QuantumultX.conf 为准。远程列表由上游维护，规则内容可能随上游更新。

本地中国大陆补充域名是对照上游列表时确认的缺项；上游会变化，维护时可重新核对以避免重复和遗漏。理想汽车仅将 `lixiang.com` 指定为直连。

ChatGPT/OpenAI 的本地域名规则是按需维护的选定条目，不代表 OpenAI 的完整或永久域名清单。维护时请查阅 [OpenAI 官方网络建议](https://help.openai.com/en/articles/9247338-network-recommendations-for-chatgpt-errors-on-web-and-apps)，并确认新域名确实需要纳入分流。

## 仓库文件

| 路径 | 说明 |
| --- | --- |
| QuantumultX.conf | 唯一的配置源文件，包含完整 QX 配置、策略组、本地规则和远程规则引用。 |
| icons/qx/144/ | 策略组使用的 144×144 PNG 图标。 |
| icons/qx/README.md | 图标来源和处理说明。 |
| icons/qx/Oasisic-LICENSE.txt | Oasisic-Icons 上游 MIT 许可文本副本。 |
| README.md | 导入说明、当前设计和后续维护约定。 |

## 配置结构与注意事项

QuantumultX.conf 保留 QX 完整配置需要的模块标题，包括 [general]、[dns]、[policy]、[server_remote]、[server_local]、[filter_local]、[filter_remote]、[rewrite_remote]、[rewrite_local]、[task_local]、[http_backend] 和 [mitm]。其中多个模块目前为空。此前 QX 导入时曾报告缺少 [dns]、[server_local] 和 [rewrite_remote]，因此即使模块没有规则，也不要随意删除标题；简化前先在目标 QX 版本中验证完整配置能否导入。

当前配置不自定义 DNS；不包含本地或远程代理节点；不启用重写、脚本任务、HTTP 后端或 MITM。除非维护目标明确要求，不要为此配置加入证书、MITM、脚本或 DNS 覆盖。

## 图标来源

策略组图标源自 [Oasisic-Icons](https://github.com/Hawaiine/Oasisic-Icons)，调整为 144×144 PNG 后托管在本仓库的 icons/qx/144/。ChatGPT 组使用 OpenAI 图标，因为图标源没有单独的 ChatGPT 图案。上游 MIT 许可副本保存在 icons/qx/Oasisic-LICENSE.txt。

若 QX 更新配置后仍显示默认图标，请在 QX 中清除图片缓存并完全退出、重新打开应用。图标直链使用 raw.githubusercontent.com。

## 后续维护者与 AI 接手说明

- 先阅读当前 QuantumultX.conf 和本 README；不要只根据旧聊天记录推断配置状态。
- QuantumultX.conf 是配置事实来源。修改策略组名称时，必须一并检查 filter_local、filter_remote、force-policy、final 以及 README 中的引用。
- 新增服务策略组时，需同时完成策略组定义、对应分流规则的策略映射、图标（如有）和本文档说明。
- 保留节点订阅凭据在使用者自己的 QX 配置中，不要提交到公共配置。
- 新增远程规则时优先引用上游文件，记录来源，并检查上游规则的目标语法是否适用于 Quantumult X。
- ChatGPT 域名、地区节点名称和远程规则会变化。修改前查阅对应上游资料，说明维护日期或来源，不要把选定规则描述为完整覆盖。
- 变更后核对策略组名称与规则目标是否一致，确认所有图标和规则 URL 可访问，并在目标 Quantumult X 版本中验证完整配置导入。
- 修改本地文件后同步更新 GitHub 上的 QuantumultX.conf；如果功能或行为改变，也同步更新 README。

## 许可与转载

当前仓库没有声明适用于整个项目的根目录许可证。图标文件保留 Oasisic-Icons 的 MIT 许可说明；Blackmatrix7 规则列表和其他外部材料继续适用各自上游的许可证与使用条件。不要仅凭图标的 MIT 许可推断整个仓库或所有远程规则都采用 MIT。

转发配置时请保留本 README 和上游来源链接，并提醒使用者自行添加节点订阅、按自己的节点命名调整地区正则。
