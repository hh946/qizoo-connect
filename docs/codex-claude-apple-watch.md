# Get Codex or Claude Code questions on Apple Watch

Qizoo lets a computer AI send a question to your Apple Watch and retrieve your answer. This guide covers the watch connection; it does not replace your AI tool's own permission and approval controls.

## What you need

- An Apple Watch running watchOS 10 or later, with Qizoo installed from the [App Store](https://apps.apple.com/us/app/qizoo/id6838017470).
- A Mac and an AI tool that can run terminal commands. The documented tested path is Codex desktop or Codex CLI on an Apple silicon Mac. Claude Code can follow the same CLI path; its host must allow the commands and network access.
- Internet access on the computer and watch. No Qizoo account is required for the current watch-confirmation flow.

Qizoo connects to the AI you already use; it does not supply a model or model subscription.

## Connect and test

1. Open Qizoo on the watch, allow notifications, and choose **Connect computer**. A short-lived connection code appears.
2. In a private Codex or Claude Code session on your Mac, paste:

   > Help me connect Qizoo to my Apple Watch. Follow https://qizoo.tipuu.com/agent-setup.md to install or reuse the CLI. Ask me for my watch connection code when needed. Tell me the computer verification code so I can compare it with the watch and confirm. Then send a test request and retrieve my reply using the original request ID.

3. Give the AI the watch connection code in that private session. Compare the computer name and verification code on the watch, then tap **Confirm connection**.
4. Answer the test request on the watch. Ask the AI to verify that it received the answer for the same request. A successful pairing alone does not prove the round trip worked.

Keep connection codes and credentials out of public chats, screenshots, and issues. For the complete setup and troubleshooting instructions, see the [agent setup guide](agent-setup.md).

## Using it in a task

After the connection test succeeds, tell your AI when to send a Qizoo question and what context it should include. For example:

> When you reach a decision that requires my input, send me a concise question through Qizoo and wait for my answer. Do not include passwords, tokens, or private file contents in the question.

The AI must have permission to run the CLI and access the network. It decides what to send; the watch connection does not automatically redirect every built-in approval prompt. Continue to use your AI tool's normal authorization controls.

## Privacy, disconnecting, and help

Questions and answers sent through Qizoo pass through its service for delivery and synchronization. Avoid including secrets or sensitive content. See the [privacy policy](https://qizoo.tipuu.com/privacy/). Disconnect a computer from the watch or use `qizoo-watch revoke`.

If setup fails, consult [agent setup](agent-setup.md) and [notifications and approvals](approvals.md), or open a [GitHub issue](https://github.com/hh946/qizoo-connect/issues). Include your watch model, watchOS version, AI tool, CLI version (`qizoo-watch --version`), and the error with secrets removed.
