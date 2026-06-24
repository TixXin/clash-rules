# Clash Rules for Tixxin

Personal Clash Party / Mihomo rule sets for routing AI traffic, Steam, games,
WebRTC/STUN blocking, LAN traffic, and common China-direct domains.

This repository is designed to be public. It should contain rule-set files only.
Do not upload your full Clash profile, proxy server address, UUID, Reality
public key, short ID, or any private subscription URL.

## Files

| File | Purpose |
| --- | --- |
| `ruleset/local-ai-core.yaml` | ChatGPT, Codex, OpenAI, Claude, Anthropic core rules |
| `ruleset/local-cn-direct.yaml` | WeChat, QQ, Tencent and other China-direct app/domain rules |
| `ruleset/local-game-direct.yaml` | PUBG, anti-cheat, game launcher and accelerator direct rules |
| `ruleset/local-lan.yaml` | LAN, loopback and private address direct rules |
| `ruleset/local-steam-direct.yaml` | Steam process and Steam domain direct rules |
| `ruleset/local-webrtc-block.yaml` | Common WebRTC/STUN UDP port blocking rules |

## jsDelivr URLs

```text
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-ai-core.yaml
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-cn-direct.yaml
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-game-direct.yaml
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-lan.yaml
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-steam-direct.yaml
https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-webrtc-block.yaml
```

## Clash Party / Mihomo provider example

```yaml
rule-providers:
  local-ai-core:
    type: http
    behavior: classical
    format: yaml
    interval: 86400
    url: https://cdn.jsdelivr.net/gh/TixXin/clash-rules@main/ruleset/local-ai-core.yaml
    path: ./ruleset/local-ai-core.yaml

rules:
  - RULE-SET,local-ai-core,Tixxin-ISP
```

## Cache refresh

If a rule file was updated but Clash Party still receives the old version,
purge the corresponding jsDelivr URL:

```text
https://www.jsdelivr.com/tools/purge
```

## Validation

Each rule file uses Clash/Mihomo `behavior: classical` YAML format:

```yaml
payload:
  - "DOMAIN-SUFFIX,example.com"
```
