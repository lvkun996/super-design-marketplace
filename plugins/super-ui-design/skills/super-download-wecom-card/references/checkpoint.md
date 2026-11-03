# 接续快照：2026-10-10

## 当前需求与关键结论

- 小程序 AppID：`wxf0d970007da185b4`（公开标识，不是 Secret）。
- 客户发的是视频号视频消息；Reqable 捕获已经打开的视频媒体 URL。
- 期望自动回复原生小程序“豆腐块”，固定进入 `pages/sph/index`，携带当前视频信息。
- 目前使用短期 ticket 关联 URL：`pages/sph/index?ticket=<本任务凭证>`，点击后自动获取元数据、
  填入只读链接框、预览；用户手动点击下载。不是自动扣次卡或自动保存相册。
- UI 输入框显示捕获地址，但实际下载走固定 Spring ticket 代理，不信任编辑输入的任意地址。
- 拼接页面参数不是动态卡片发送能力。Android 的“应用 → 用过的小程序”选择器只发送静态首页，
  无法通过现有代码注入 ticket。下一步必须验证并适配真正的带参数页面分享流程。
- 用户不要求切换会话产品；普通企微客户会话与“微信客服”不是同一种入口。
  即便某官方 API 支持卡片，也不能推断可直接给当前企微会话发送，需核对权限与身份映射。

## 已完成的代码（需核对新电脑源码是否包含）

### 小程序页面

- `minigram/src/manifest.json`：mp-weixin AppID 已补齐；顶部 `__UNI__...` 是 uni-app 标识，不替换。
- `minigram/project.config.json`：同一微信 AppID。
- `minigram/src/pages/sph/index.vue`：有合法 ticket 时，原固定页面内挂载 `RpaTicketDownload`；
  没有 ticket 时保留普通分享链接解析。ticket 不再被送到 Go 分享解析，也不再强制跳去另一页面。
- `minigram/src/components/RpaTicketDownload.vue`：登录后自动读取 ticket 元数据、回填捕获 URL、预览；
  手动下载才校验会员/次卡、调用 Spring 文件代理、检查 MP4、保存相册。
- `minigram/src/pages/rpa-download/index.vue`：兼容旧独立入口；分享路径统一指向 sph 页携带 ticket。
- `minigram/src/api/rpa-download.ts`：元数据、预览和文件下载通过 Spring；不是直接调用 Go。
- `minigram/src/api/config.ts`：存储的自定义服务地址优先于 VITE_BUSINESS_API_BASE_URL。

### Spring RPA

目录前缀 `spring-service/src/main/java/com/superdownload/business/`：

- `service/RpaDownloadService.java`：捕获窗口、候选收敛、命令领取、结果幂等、ticket 元数据、下载授权。
  元数据在凭证有效且任务可下载时返回解密的 sourceUrl；过期不会返回源地址和预览。
- `service/RpaReplyBuilder.java`：三种现有模式：DIRECT_URL、MINIPROGRAM_URL_LINK、MINIPROGRAM_HOME。
  首页模式只保存随机 ticket 的哈希，不下发 ticket/动态页面路径，**不能用于动态卡片**。
  URL Link 模式下发 HTTPS URL，Android 发送文字；不能当作原生卡片验收。
- `vo/RpaAgentCommandVO.java`、`service/RpaTaskState.java`：现有命令契约与状态。
- `controller/RpaDownloadController.java`：`GET /api/mini/rpa/downloads/{ticket}`、`/file`、`/preview`。
  保留文件下载权益校验，不把捕获地址重新解析成分享链接。
- `service/RpaDiagnosticsService.java`、`controller/RpaAgentController.java`：只读 diagnostics；
  使用 `rpa/diagnose.py`，不要以 `/commands/poll` 诊断（它会领取命令）。

### Android RPA

目录前缀 `android-agent/app/src/main/`：

- `java/com/superdownload/rpa/automation/WeComAccessibilityService.java`：
  `MiniProgramHomeSession` 只选择固定小程序首页。现有个人会话标题核对和 PendingIntent 绑定必须保留。
- `automation/AutomationBridge.java`：openLatestVideo、replyUrl、replyMiniProgramHome。
  这些都位于 `java/com/superdownload/rpa/` 下；没有已完成的 replyDynamicCard。
- `service/RpaCoordinatorService.java`：队列与执行、结果上报，SEND_UNKNOWN 不自动重发。
- `service/IdlePollPolicy.java`：空闲 2 秒逐步退避至 30 秒；捕获活动窗口保持快速轮询，使用单调时钟。
- `res/raw/wecom_selector_5_0_7.json`：只支持企业微信 5.0.7；配置的小程序名称是“视频号下载”，
  分享 UI 的真正小程序名称、节点和版本需要真机重新核对。

### Reqable

- `rpa/reqable/video_capture_bridge.py`：自包含脚本；设备映射与 Spring Token 一致。
  成功流量短时去重，失败可重试；映射失败缓存 5 秒，成功缓存可配置，不使用过期映射；日志轮转。
- `rpa/reqable/README.md`：配置、设备登记和网络方式。
- 手机 Wi-Fi 代理指向 PC Reqable 时，不同时开启手机 Reqable VPN。
- Windows Go 系统代理与 Reqable 是不同代理，避免未经核对的代理串联。

## 未完成 / 未验收

1. 动态原生卡片自动发送：尚无可靠命令与无障碍分享实现，不要宣称完成。
2. 真实企微/小程序带参数分享入口、卡片确认和发送后恢复：尚未采样或验证。
3. 最新一次 `adb devices` 返回空列表；换机后重新检查。
4. 未自动安装新 APK、部署新 Spring/H5/小程序、未向客户发送真实消息。
5. 小程序发布/体验版、商户绑定、真实微信支付回调均未因源码构建而被证明可用。

## 旁支工作与配置（不要丢失，不属于动态卡片主任务）

- 根目录 `README.md` 已新增从零启动说明；`docs/local-startup.md` 和 `docs/wecom-debugging.md` 为现有操作参考。
- 企业微信自动回复不依赖 Windows 微信 SSH 隧道；后者供服务器手动视频号链接解析使用。
  Cloudflare 方案已从当前项目移除，不恢复旧架构。
- 支付 `POST /api/public/wx-pay/notify/membership` 已完善真实 APIv2 验签及业务一致性检查；
  MembershipService 在事务内先锁用户再锁订单，发放会员、记录 PAID/交易号/会员记录，重复回调不重复发放。
  admin `/api/admin/orders` 读取同一数据；相关修改未自动提交/部署。
- `docs/wechat-payment.md` 为支付说明。商户号/APIv2 密钥复制自 jy-health 后端配置默认值至
  **本机忽略文件 start.env**；Skill 不包含这些值，也不证明线上运行使用了相同凭据。
  WX_PAY_APP_ID 单独配置为当前 AppID；支付未开启，仍需回调域名、商户绑定与真机验收。
- 其他未提交文件包括只读诊断脚本、配置测试及支付测试，先看 git status，不丢弃或重写。

## 最近验证证据（历史，不是换机后的保证）

- `npm.cmd run build:mp-weixin` 成功；产物 `minigram/dist/build/mp-weixin`，编译 AppID 正确，
  sph 页注册了 RpaTicketDownload 组件。无真机点击/视觉验收。
- RpaDownloadServiceTest：16 项通过，包含捕获链接元数据和过期行为。
- 支付修改后 Spring 完整测试：76 项通过，package 成功。
- 更早 Android：24 项测试及 debug APK 构建通过，不代表动态卡片能力（本次未实现）。
- 本地日志 `.runtime/minigram-card-build.log`、`rpa-card-page-tests.log`、`pay-callback-package.log`；
  它们被 Git 忽略，不一定随换机迁移，不把日志缺失当作代码丢失。

复验命令（仓库根目录，各命令执行前核对工具路径）：

```powershell
git status --short
adb devices
cd minigram
npm.cmd install
npm.cmd run build:mp-weixin
cd ../spring-service
mvn.cmd -B test
cd ../android-agent
.\gradlew.bat --no-daemon --max-workers=1 testDebugUnitTest assembleDebug
```

基础环境：Node 22.12+、Python 3.10+、Spring JDK 11+（建议 17）、Maven；Android JDK 17/SDK 36。
原电脑低内存时构建分别限 JVM 堆、单 worker 并顺序执行，不把原电脑 D:/environment 路径硬编码到新机。

## 换机必须携带与重新准备

- **完整源码及未提交变更**：本次改动不只 Skill。仅拉取远端未必包含上面的页面/回调/文档改动。
  让用户提交后迁移，或安全复制当前工作区；本 Skill 请求不自动授权 Git 提交/推送。
- Skill：完整复制 `.agents/skills/super-download-wecom-card/`；支持显式阅读或安装到新机 Codex skills。
- 本地 `start.env`、`spring-service/.env`、Go config 和 SSH 私钥不进入此 Skill，需由用户按需安全迁移或重配。
  不把 .runtime 整体上传，也不将密钥提交到 Git。Windows SSH 私钥仅手动解析场景需要。
- 真机与 USB 调试授权、企业微信版本、Agent Token/Device ID、Reqable 网关映射、Spring 可达地址。
  手机 127.0.0.1 是手机本身；真机使用可达的 Spring 地址，不是 Go 2022。
- 小程序真实版本与服务合法域名；打开带参数页面需要可用的小程序环境和对应后端。

下一步不是再次修改 AppID、拼字符串或升级 Go；是 **真机验证可携带本任务参数的原生卡片分享入口**，
再决定动态发送适配方式。不能保证“连接 ADB 就一定能完成”，先拿可观察证据。
