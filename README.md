# Qizoo Connect

Get a notification from your computer AI on Apple Watch, answer an approval request on your wrist, and let the original AI pick up your reply.

## Connect with one message

Paste this into your own computer AI session:

> Help me connect Qizoo to my Apple Watch. Follow https://qizoo.tipuu.com/agent-setup.md to install or reuse the CLI, ask me for my watch connection code when needed, and use `--campaign github`. If watch confirmation is available, give me the computer verification code so I can approve it on my watch. Ask me before using browser confirmation. Send a test request and retrieve my reply using the original request ID.

With Qizoo Watch preview build 53 and the matching relay, use the [CLI v0.2.0rc1 prerelease](https://github.com/hh946/qizoo-connect/releases/tag/v0.2.0rc1) to confirm on the watch without signing in. For Watch build 52, use [CLI v0.1.1 stable](https://github.com/hh946/qizoo-connect/releases/tag/v0.1.1) and confirm in the browser. Keep both codes out of public chats and issues.

Use the [setup guide](docs/agent-setup.md) for the CLI download and checksum verification. The initial CLI target is an Apple silicon Mac with Codex desktop or Codex CLI. The Apple Watch App Store listing has yet to be published. To run the full flow you need an installed Qizoo Watch build.

[Qizoo website](https://qizoo.tipuu.com/?utm_source=github) · [Agent setup](docs/agent-setup.md) · [Notifications and approvals](docs/approvals.md) · [HTTP example](examples/http.md) · [API definition](openapi.json)

## 中文快速接入

把下面这句话发给正在使用的电脑 AI：

> 请按 https://qizoo.tipuu.com/agent-setup.md 安装或复用启奏 CLI，需要时向我索取手表连接码，连接时使用 `--campaign github`。如果支持在手表上确认，请把电脑校验码告诉我，我会在表上核对并确认。若需网页确认，请先问我。连接好后请发送一条测试请示，按原请求编号取回我的答复。

手表预览第 53 版配合 CLI v0.2.0rc1 在表上确认，第 52 版配合 CLI v0.1.1 在账号网页确认。启奏手表 App Store 版尚未上架。
