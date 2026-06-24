# Tixxin Clash 规则集

这是给 Clash Party / Mihomo 使用的个人规则集仓库，用来把常用规则和脱敏版配置模板放到 GitHub + jsDelivr CDN 上，避免在每台电脑上额外复制 `ruleset` 文件夹。

这个仓库是公开仓库，只能放规则文件和脱敏模板。不要上传真实代理服务器地址、UUID、Reality public key、short-id、订阅地址或其它私密信息。

## 文件说明

| 文件 | 用途 |
| --- | --- |
| `clash.yaml` | 脱敏版 Clash Party / Mihomo 完整配置模板，节点信息使用占位符 |
| `ruleset/local-ai-core.yaml` | ChatGPT、Codex、OpenAI、Claude、Anthropic 核心规则 |
| `ruleset/local-cn-direct.yaml` | 微信、QQ、腾讯等常见国内应用和域名直连规则 |
| `ruleset/local-game-direct.yaml` | PUBG、反作弊、游戏启动器、加速器直连规则 |
| `ruleset/local-lan.yaml` | 本地、回环、局域网、私有地址直连规则 |
| `ruleset/local-steam-direct.yaml` | Steam 进程和 Steam 域名直连规则 |
| `ruleset/local-webrtc-block.yaml` | 常见 WebRTC / STUN UDP 端口阻断规则 |

## CDN 链接

```text
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/clash.yaml
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-ai-core.yaml
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-cn-direct.yaml
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-game-direct.yaml
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-lan.yaml
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-steam-direct.yaml
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-webrtc-block.yaml
```

## clash.yaml 使用说明

`clash.yaml` 是公开模板，不包含真实节点信息。使用前需要把下面占位符替换成你自己 3x-ui / Reality 节点的真实值：

```yaml
server: <IP>
uuid: <UUID>
reality-opts:
  public-key: <PUBLIC_KEY>
  short-id: <SHORT_ID>
```

当前模板里的 `rules:` 已按你的目标把 AI 放在第一位：

```yaml
rules:
  - RULE-SET,local-ai-core,Tixxin-ISP
  - RULE-SET,ai-openai,Tixxin-ISP
  - RULE-SET,ai-anthropic,Tixxin-ISP
```

## Clash Party / Mihomo 示例配置

下面示例覆盖本仓库全部 6 个 YAML 规则集。`path` 是 Clash 本地缓存路径，不需要手动提前创建文件。

```yaml
rule-providers:
  local-lan:
    type: http
    behavior: classical
    format: yaml
    interval: 86400
    url: https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-lan.yaml
    path: ./ruleset/local-lan.yaml

  local-game-direct:
    type: http
    behavior: classical
    format: yaml
    interval: 86400
    url: https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-game-direct.yaml
    path: ./ruleset/local-game-direct.yaml

  local-steam-direct:
    type: http
    behavior: classical
    format: yaml
    interval: 86400
    url: https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-steam-direct.yaml
    path: ./ruleset/local-steam-direct.yaml

  local-webrtc-block:
    type: http
    behavior: classical
    format: yaml
    interval: 86400
    url: https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-webrtc-block.yaml
    path: ./ruleset/local-webrtc-block.yaml

  local-ai-core:
    type: http
    behavior: classical
    format: yaml
    interval: 86400
    url: https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-ai-core.yaml
    path: ./ruleset/local-ai-core.yaml

  local-cn-direct:
    type: http
    behavior: classical
    format: yaml
    interval: 86400
    url: https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-cn-direct.yaml
    path: ./ruleset/local-cn-direct.yaml

rules:
  # ChatGPT / Codex / Claude / Anthropic 走住宅 ISP 策略组。
  - RULE-SET,local-ai-core,Tixxin-ISP

  # 本地、局域网、私有地址直连。
  - RULE-SET,local-lan,DIRECT

  # 游戏 / 加速器 / 反作弊优先直连，避免 WebRTC 阻断误伤游戏 UDP。
  - RULE-SET,local-game-direct,DIRECT

  # Steam 全量直连。
  - RULE-SET,local-steam-direct,DIRECT

  # 常见浏览器 WebRTC / STUN 泄露防护。
  - RULE-SET,local-webrtc-block,REJECT

  # 国内应用和常见国内域名直连。
  - RULE-SET,local-cn-direct,DIRECT
```

## 推荐规则顺序

如果和其它公开规则集一起使用，建议按下面顺序放置：

```yaml
rules:
  - RULE-SET,local-ai-core,Tixxin-ISP
  - RULE-SET,ai-openai,Tixxin-ISP
  - RULE-SET,ai-anthropic,Tixxin-ISP
  - RULE-SET,private,DIRECT
  - RULE-SET,lancidr,DIRECT
  - RULE-SET,local-lan,DIRECT
  - RULE-SET,local-game-direct,DIRECT
  - RULE-SET,local-steam-direct,DIRECT
  - RULE-SET,steam,DIRECT
  - RULE-SET,steam-cn,DIRECT
  - RULE-SET,local-webrtc-block,REJECT
  - RULE-SET,applications,DIRECT
  - RULE-SET,local-cn-direct,DIRECT
  - RULE-SET,reject,REJECT
  - RULE-SET,direct,DIRECT
  - RULE-SET,cncidr,DIRECT
  - GEOIP,CN,DIRECT
  - RULE-SET,Adobe,Tixxin
  - RULE-SET,proxy,Tixxin
  - MATCH,漏网之鱼
```

## 更新缓存

如果已经更新了规则文件，但 Clash Party 仍然拉到旧内容，可以到 jsDelivr 的清缓存页面刷新对应 URL：

```text
https://www.jsdelivr.com/tools/purge
```

## 规则格式

本仓库所有规则文件都使用 Clash / Mihomo 的 `behavior: classical` YAML 格式：

```yaml
payload:
  - "DOMAIN-SUFFIX,example.com"
  - "PROCESS-NAME,example.exe"
```
