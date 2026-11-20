# Qizoo Connect

Get a notification from your computer AI on Apple Watch, answer an approval request on your wrist, and let the original AI pick up your reply.

## Connect with one message

On Apple Watch, open Qizoo and tap **Connect your computer AI** (shown as **连接电脑上的 AI** in the current build) to get a short-lived pairing code. Replace `[CODE]` and paste this into your own computer AI session:

> Follow the Qizoo setup guide at https://qizoo.tipuu.com/agent-setup.md to connect my Apple Watch. My watch pairing code is [CODE]. After I confirm on the official website, send a test request with one choice, “Received,” and retrieve my reply using the original request ID. Use `--campaign github` when running the pairing command.

You’ll confirm the link yourself on the official Qizoo website. Open it, sign in and enter the separate computer verification code shown by the CLI. Keep both codes out of public chats and issues.

Use the [setup guide](docs/agent-setup.md) for the CLI download and checksum verification. The initial CLI target is an Apple silicon Mac with Codex desktop or Codex CLI. The Apple Watch App Store listing has yet to be published. To run the full flow you need an installed Qizoo Watch build.

[Qizoo website](https://qizoo.tipuu.com/?utm_source=github) · [Agent setup](docs/agent-setup.md) · [Notifications and approvals](docs/approvals.md) · [HTTP example](examples/http.md) · [API definition](openapi.json)

## 中文快速接入

在启奏手表点「连接电脑上的 AI」，将下方的 `[配对码]` 替换为手表显示的临时码，再发给你正在使用的电脑 AI：

> 按 https://qizoo.tipuu.com/agent-setup.md 连接我的启奏手表。手表配对码是 [配对码]。我在启奏官网登录并核对电脑验证码后，请发送只有「收到」一个选项的测试请示，并按原请求编号取回我的答复。配对命令请使用 `--campaign github`。
