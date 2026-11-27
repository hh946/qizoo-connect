# Qizoo Connect

Your computer AI stops to ask "should I continue?" — Qizoo puts the question on your Apple Watch. Tap to answer, and the same task picks up your reply.

[**Download Qizoo on the App Store**](https://apps.apple.com/us/app/qizoo/id6838017470) · [Website](https://qizoo.tipuu.com/?utm_source=github) · [Report a problem](https://github.com/hh946/qizoo-connect/issues) · [Questions and ideas](https://github.com/hh946/qizoo-connect/discussions)

This repository holds the public setup guide, the CLI downloads and the API definition. Qizoo itself is not open source. Qizoo does not provide an AI model; it connects the AI you already use, such as Codex or Claude Code.

## Connect with one message

1. Install Qizoo on Apple Watch (watchOS 10 or later) from the App Store.
2. Paste this into your computer AI session:

> Help me connect Qizoo to my Apple Watch. Follow https://qizoo.tipuu.com/agent-setup.md to install or reuse the CLI, ask me for my watch connection code when needed, and use `--campaign github`. Give me the computer verification code so I can confirm it on my watch. Then send a test request and retrieve my reply using the original request ID.

3. Compare the code on the watch and tap **Confirm connection**. No account or sign-up.

Keep connection codes out of public chats and issues.

[Codex and Claude Code guide](docs/codex-claude-apple-watch.md) · [Agent setup](docs/agent-setup.md) · [Notifications and approvals](docs/approvals.md) · [HTTP example](examples/http.md) · [API definition](openapi.json) · [Changelog](CHANGELOG.md)

## Privacy and security

**Does Qizoo see my AI account, keys or code?** No. Your AI tool keeps running on your computer with your own account, as it does today. Qizoo never asks for, stores or uses your API keys or tokens, and it uses none of your AI quota.

Your AI decides when to ask and what to write. When it needs you, it calls the Qizoo CLI with a short question and a few choices. Only that text, and your answer, go through Qizoo's server, which keeps them so your devices stay in sync. Qizoo does not read your code, files or chat history. It only sees what your AI puts into the question.

You can disconnect a computer from the watch at any time, or run `qizoo-watch revoke`. Tip: tell your AI not to put passwords or secrets into a question. [Privacy policy](https://qizoo.tipuu.com/privacy/)

## Feedback

- Something broke: open an [issue](https://github.com/hh946/qizoo-connect/issues) with your watch model, watchOS version, CLI version (`qizoo-watch --version`) and which AI you use.
- Questions, ideas, or how you use Qizoo: start a [discussion](https://github.com/hh946/qizoo-connect/discussions).
- Private matters: support@tipuu.com

## 中文快速接入

1. 在 App Store 下载启奏（Qizoo）手表 App。
2. 把下面这句话发给电脑上的 AI：

> 请按 https://qizoo.tipuu.com/agent-setup.md 安装或复用启奏 CLI，需要时向我索取手表连接码，连接时使用 `--campaign github`。把电脑校验码告诉我，我会在表上核对并确认。连接好后发送一条测试请示，按原请求编号取回我的答复。

3. 在手表上核对校验码，点「确认连接」。不用注册账号。

启奏不碰你的 AI 账号、API 密钥和 Token，也不消耗你的 AI 额度。经过启奏服务器的只有 AI 写的那句问题和你的答复，用于在你的设备之间同步；启奏不读取你的代码、文件和对话记录，只看得到 AI 写进问题里的内容。随时可以在手表上断开电脑。

问题和建议请发 [Issues](https://github.com/hh946/qizoo-connect/issues) 或 [Discussions](https://github.com/hh946/qizoo-connect/discussions)，也可以发邮件到 support@tipuu.com。
