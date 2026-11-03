---
name: super-download-wecom-card
description: 接续 Super Download 的企业微信视频号自动回复任务：Reqable 捕获视频后发送携带独立视频凭证的原生小程序卡片。用于换机接续、动态卡片发送适配及真机验收，不用于普通支付或部署任务。
---

# 企业微信动态视频卡片接续

先完整阅读 [references/checkpoint.md](references/checkpoint.md)。这是 2026-10-10 的交接快照，
不是当前运行状态证明。换电脑、换分支或继续修改后，先核对仓库文件和工作区，不重新做已完成的功能。

## 用户确认的目标

客户向企业微信转发 **视频号视频消息**，不是发送 sph 分享链接。
Android 打开原视频，PC Reqable 捕获视频链接，Spring 将捕获结果绑定独立任务。
随后自动回复原会话一张 **原生小程序卡片**（标题、封面、小程序标识），不是文字 URL，
也不是固定首页卡片。用户点击进入 `pages/sph/index`，自动填入本条视频的捕获链接，
用户点击下载后再进行会员/次卡校验、下载和保存相册。

## 必须保留的边界

- 卡片使用本任务的短期 ticket；Spring 映射到捕获 URL。不用全局“最新视频”、昵称匹配或固定首页推测任务。
- Reqable 捕获的是媒体直链，不将它再次送入 Go 的分享链接解析接口。
- 固定首页卡片已经有自动发送实现，但 **动态带参数卡片发送尚未实现/验收**。
  不能把这两者混为一谈，也不能把 URL Link 的文本发送当作原生卡片交付。
- 保留原通知绑定、原会话核对、单设备串行执行和发送结果确认；SEND_UNKNOWN 不自动重发。
- 不能假定手机“分享指定页面”的入口存在。先用真机验证，再依据实际节点树适配；不要猜资源 ID 或写死点击坐标。
- 不擅自切换为微信客服/第三方平台，不新增 Cloudflare，不部署、不推送 Git、不向真实客户发送测试消息。
  若实现需要新的企业账号权限、API 凭据、会话类型变更或真实发送授权，报告并询问用户。
- 安全规则不等于产品已验收。构建、单测、页面打开、静态首页发送都不能证明动态卡片准确绑定视频。

## 接续工作流

1. 定位 Super Download 仓库根目录（原路径 `D:/code/super/super-download`，新电脑可能不同）。
   查看 `git status --short`、当前分支/提交和关键代码，保留现有未提交的支付等工作。
2. 验证换机必备项：源码和未提交变更、对应小程序版本、Spring 地址、独立配置与必要密钥。
   不把旧电脑绝对路径、未同步构建产物或旧“已就绪”日志当作事实。
3. 检查 `adb devices` 与设备授权。最后一次检查没有连接的设备，不能推定新电脑也没有。
4. 在明确的授权测试会话中，先验证手动流程是否可行：
   打开带本条 ticket 的固定页面 → 分享为原生卡片 → 选择原会话 → 核对卡片路径/参数。
   未授权发送前只做读取和准备，使用自有测试账号，不触碰真实客户。
5. 若可行，采样实际企业微信/小程序的无障碍节点树，再设计动态卡片命令、凭证传递、自动分享和确认。
   保持原通知与任务关联，控制跨应用跳转后的恢复、命令到期、异常退出和重启幂等。
6. 若手动入口无法携带动态参数，不伪装完成，不靠固定首页或“最新任务”绕过。
   向用户说明具体观察和受限环节，再决定是否需要其他合法发送入口。
7. 按风险验证：页面参数与 ticket 到视频一一对应、原生卡片展示、原会话准确、只发送一次、
   连续/并发视频不串单、过期凭证、未知发送结果、手动下载的权益校验。
8. 更新交接参考中的已完成/未完成和证据。仅在用户要求时提交代码、部署、安装 APK或更新全局记忆。

## 换机使用

项目内此目录为可携带副本，完整复制目录（含 references 和 agents），不是只复制 SKILL.md。
可安装到新电脑的 `<CODEX_HOME>/skills/super-download-wecom-card`；默认通常为
`<用户目录>/.codex/skills/super-download-wecom-card`。已有同名目录先核对，不盲目覆盖。

示例（项目根目录，默认 Codex 目录；自定义 CODEX_HOME 时改目标路径）：

```powershell
$skillSource = Join-Path (Get-Location) '.agents/skills/super-download-wecom-card'
$skillTarget = Join-Path $env:USERPROFILE '.codex/skills/super-download-wecom-card'
if (Test-Path $skillTarget) { throw '同名 Skill 已存在，请先核对版本' }
New-Item -ItemType Directory -Force -Path (Split-Path $skillTarget) | Out-Null
Copy-Item -LiteralPath $skillSource -Destination $skillTarget -Recurse
```

接续提示词：

> 使用 $super-download-wecom-card，先读取交接快照并核对当前源码，继续实现 Reqable 捕获后自动回复带本条视频凭证的原生小程序卡片。不要把首页卡片或 URL 文本当作完成，不部署或向真实客户发送。

若 Skill 未被发现，直接要求新会话完整阅读项目内此 SKILL.md 及其 checkpoint.md，仍可接续。
