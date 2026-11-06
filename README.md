# Quantumult X 分流配置

完整配置文件：[`QuantumultX.conf`](./QuantumultX.conf)

QX 配置订阅直链：

`https://raw.githubusercontent.com/zrw29/quantumultx-routing-rules/main/QuantumultX.conf`

## 导入与使用

在 Quantumult X 的配置文件管理中通过上方 Raw 链接下载或关联此配置。节点订阅不包含在公开文件中，请在 QX 中单独添加。

- Apple：策略组默认直连，可切换到通用代理
- ChatGPT：日本或新加坡节点
- Netflix：香港节点
- YouTube、Telegram、Google、Microsoft、X、Spotify：各有独立策略组，可分别选择代理节点
- 美国、台湾：独立地区节点组，供手动选择
- 中国大陆规则库命中的流量：直连
- 其它未命中流量：通用代理

日本、新加坡、美国、台湾和香港节点根据节点名称中的地区关键词匹配。如订阅中的命名不同，需调整配置中的正则。美国和台湾节点组不会自动改变应用分流；各服务策略组可以单独选择出口节点。

中国大陆规则源包含少量 IP、IPv6 和 User-Agent 规则，不使用 `GEOIP,CN`。Netflix 使用域名规则，不包含大量共享 AWS IP 段。

## 策略组图标

策略组通过 img-url 参数引用 [Oasisic-Icons](https://github.com/Hawaiine/Oasisic-Icons) 的远程 PNG 图标（MIT）；ChatGPT 组使用 OpenAI 图标。图标由上游托管，本配置仅引用图标链接。
