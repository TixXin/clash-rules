# Tixxin Clash 规则集

这是给 Clash Party / Mihomo 使用的个人规则集仓库，用来把常用规则和脱敏版配置模板放到 GitHub + jsDelivr CDN 上，避免在每台电脑上额外复制 `ruleset` 文件夹。

这个仓库是公开仓库，只能放规则文件和脱敏模板。不要上传真实代理服务器地址、UUID、Reality public key、short-id、订阅地址或其它私密信息。

## 文件说明

| 文件 | 用途 |
| --- | --- |
| `clash.yaml` | 脱敏版 Clash Party / Mihomo 完整配置模板，节点信息使用占位符 |
| `ruleset/tixxin-ai-core.yaml` | ChatGPT、Codex、OpenAI、Claude、Anthropic 核心规则 |
| `ruleset/tixxin-ip-check.yaml` | IP / DNS 检测站规则，用于另一台电脑手动测试住宅出口 |
| `ruleset/tixxin-cn-direct.yaml` | 微信、QQ、腾讯等常见国内应用和域名直连规则 |
| `ruleset/tixxin-game-direct.yaml` | PUBG、PUBG Datadog 遥测、反作弊、游戏启动器、加速器直连规则 |
| `ruleset/tixxin-lan.yaml` | 本地、回环、局域网、私有地址直连规则 |
| `ruleset/tixxin-steam-direct.yaml` | Steam 进程和 Steam 域名直连规则 |
| `ruleset/tixxin-webrtc-block.yaml` | 常见 WebRTC / STUN UDP 端口阻断规则 |

## CDN 链接

```text
https://cdn.jsdelivr.net/gh/TixXin/clash-rules/clash.yaml
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/tixxin-ai-core.yaml
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/tixxin-ip-check.yaml
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/tixxin-cn-direct.yaml
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/tixxin-game-direct.yaml
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/tixxin-lan.yaml
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/tixxin-steam-direct.yaml
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/tixxin-webrtc-block.yaml
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
  - RULE-SET,tixxin-ai-core,Tixxin-ISP
  - RULE-SET,tixxin-ip-check,Tixxin-ISP
  - RULE-SET,tixxin-game-direct,DIRECT
  - RULE-SET,ai-openai,Tixxin-ISP
  - RULE-SET,ai-anthropic,Tixxin-ISP
```

模板默认 DNS 已改为国外 DoH 并经 `Tixxin-ISP` 转发，减少 DNS 深度测试暴露中国 DNS：

```yaml
nameserver:
  - https://1.1.1.1/dns-query#Tixxin-ISP
  - https://8.8.8.8/dns-query#Tixxin-ISP
```

## Clash Party / Mihomo 示例配置

下面示例覆盖本仓库全部 7 个 YAML 规则集。`path` 是 Clash 本地缓存路径，不需要手动提前创建文件。

```yaml
rule-providers:
  tixxin-lan:
    type: http
    behavior: classical
    format: yaml
    interval: 86400
    url: https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/tixxin-lan.yaml
    path: ./ruleset/tixxin-lan.yaml

  tixxin-game-direct:
    type: http
    behavior: classical
    format: yaml
    interval: 86400
    url: https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/tixxin-game-direct.yaml
    path: ./ruleset/tixxin-game-direct.yaml

  tixxin-steam-direct:
    type: http
    behavior: classical
    format: yaml
    interval: 86400
    url: https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/tixxin-steam-direct.yaml
    path: ./ruleset/tixxin-steam-direct.yaml

  tixxin-webrtc-block:
    type: http
    behavior: classical
    format: yaml
    interval: 86400
    url: https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/tixxin-webrtc-block.yaml
    path: ./ruleset/tixxin-webrtc-block.yaml

  tixxin-ai-core:
    type: http
    behavior: classical
    format: yaml
    interval: 86400
    url: https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/tixxin-ai-core.yaml
    path: ./ruleset/tixxin-ai-core.yaml

  tixxin-ip-check:
    type: http
    behavior: classical
    format: yaml
    interval: 86400
    url: https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/tixxin-ip-check.yaml
    path: ./ruleset/tixxin-ip-check.yaml

  tixxin-cn-direct:
    type: http
    behavior: classical
    format: yaml
    interval: 86400
    url: https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/tixxin-cn-direct.yaml
    path: ./ruleset/tixxin-cn-direct.yaml

rules:
  # ChatGPT / Codex / Claude / Anthropic 走住宅 ISP 策略组。
  - RULE-SET,tixxin-ai-core,Tixxin-ISP

  # IP / DNS 检测站走住宅 ISP，便于在另一台电脑手动验证。
  - RULE-SET,tixxin-ip-check,Tixxin-ISP

  # 本地、局域网、私有地址直连。
  - RULE-SET,tixxin-lan,DIRECT

  # 游戏 / 加速器 / 反作弊优先直连，避免 Datadog 被宽泛 AI 规则抢走。
  - RULE-SET,tixxin-game-direct,DIRECT

  # AI 扩展规则放在游戏规则之后，避免影响 PUBG 进游戏。
  - RULE-SET,ai-openai,Tixxin-ISP
  - RULE-SET,ai-anthropic,Tixxin-ISP

  # Steam 全量直连。
  - RULE-SET,tixxin-steam-direct,DIRECT

  # 常见浏览器 WebRTC / STUN 泄露防护。
  - RULE-SET,tixxin-webrtc-block,REJECT

  # 国内应用和常见国内域名直连。
  - RULE-SET,tixxin-cn-direct,DIRECT
```

## 推荐规则顺序

如果和其它公开规则集一起使用，建议按下面顺序放置：

```yaml
rules:
  - RULE-SET,tixxin-ai-core,Tixxin-ISP
  - RULE-SET,tixxin-ip-check,Tixxin-ISP
  - RULE-SET,private,DIRECT
  - RULE-SET,lancidr,DIRECT
  - RULE-SET,tixxin-lan,DIRECT
  - RULE-SET,tixxin-game-direct,DIRECT
  - RULE-SET,ai-openai,Tixxin-ISP
  - RULE-SET,ai-anthropic,Tixxin-ISP
  - RULE-SET,tixxin-steam-direct,DIRECT
  - RULE-SET,steam,DIRECT
  - RULE-SET,steam-cn,DIRECT
  - RULE-SET,tixxin-webrtc-block,REJECT
  - RULE-SET,applications,DIRECT
  - RULE-SET,tixxin-cn-direct,DIRECT
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

## 另一台电脑手动测试步骤

在另一台电脑导入或刷新下面的配置：

```text
https://cdn.jsdelivr.net/gh/TixXin/clash-rules/clash.yaml
```

Clash Party 手动确认：

```text
DNS 覆写：开启
嗅探覆写：开启
虚拟网卡：开启
Tun 模式堆栈：Mixed
严格路由：开启
自动设置全局路由：开启
自动选择流量出口接口：开启
DNS 劫持：any:53,tcp://any:53
```

重启 Clash Party 内核后测试：

```text
https://ip.net.coffee/gpt/
https://ip.net.coffee/claude/
https://ip.net.coffee/dns/
https://ippure.com/
https://www.iping.cc/
https://whoer.net/zh
https://ping0.cc/
```

预期结果：

```text
ChatGPT / Claude 检测：显示住宅 ISP。
DNS 检测：不再出现中国电信 / 联通 DNS。
IP 检测站：走 Tixxin-ISP。
国内网站、Steam、PUBG：仍应可用。
```

## 规则格式

本仓库所有规则文件都使用 Clash / Mihomo 的 `behavior: classical` YAML 格式：

```yaml
payload:
  - "DOMAIN-SUFFIX,example.com"
  - "PROCESS-NAME,example.exe"
```
