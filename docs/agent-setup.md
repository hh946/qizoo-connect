# Connect your computer AI to Qizoo on Apple Watch

1. Install **Qizoo** on Apple Watch from the App Store: https://apps.apple.com/us/app/qizoo/id6838017470
2. Give this guide to the AI on your computer. It installs the Qizoo CLI and asks you for the connection code shown on the watch.
3. Compare the computer name and verification code on the watch, then tap **Confirm connection**.

No account or sign-up is needed. Qizoo does not provide an AI model; it connects the AI you already use on your computer. Questions or problems: [GitHub Issues](https://github.com/hh946/qizoo-connect/issues) · [Discussions](https://github.com/hh946/qizoo-connect/discussions) · support@tipuu.com

## For the AI running setup

Run `qizoo-watch --version` and `qizoo-watch status --source <your-ai>` using the same computer account that will send requests. Use CLI `0.2.0` or later. Reuse a connected CLI and send a test request. For a new connection, ask the user for the short-lived code only when you are ready to pair. Use the code only in the private setup session. Keep it out of logs and shared files.

### Pair with watch confirmation

The CLI defaults to `https://qizoo.tipuu.com`. The user opens Qizoo on the watch and picks **Connect computer**. Ask them for the connection code, start `qizoo-watch connect --code-stdin`, and give the code to the process as one line of standard input. For an isolated relay, include `--relay-url <test-origin>`. If a campaign prompt specifies a recognized channel, include `--campaign <channel>`.

Keep the command running and read its progress from standard error. Tell the user the **computer verification code** as soon as the CLI displays it. The user compares the code and computer name with the watch, then taps **Confirm connection** on the watch. If your terminal integration cannot show progress while the command runs, have the user run the command in their terminal so they can see the code. When the CLI reports `connected`, send a watch request and retrieve the user's reply with the same request ID. If the connection expires or the user cancels, ask for a fresh code. If the CLI was force-closed after watch confirmation and the computer now appears on the watch, revoke it from the watch before retrying. `qizoo-watch service` shows any current service announcement.

## Install or reuse the CLI

Reuse a compatible official installation after checking `qizoo-watch --version`. AI tools running under the same computer account can share a completed connection if their host grants access to the local credential store and network.

On an Apple silicon Mac, use the bundled package with its own Python runtime. Download the assets from the matching tag and verify the release checksum before extracting. The versioned directory lets you keep both versions installed.

[Apple silicon package for v0.2.0](https://github.com/hh946/qizoo-connect/releases/download/v0.2.0/qizoo-watch-0.2.0-macos-arm64.tar.gz) · [Python archive for v0.2.0](https://github.com/hh946/qizoo-connect/releases/download/v0.2.0/qizoo-watch-0.2.0-python3.pyz) · [SHA256SUMS for v0.2.0](https://github.com/hh946/qizoo-connect/releases/download/v0.2.0/SHA256SUMS)

```sh
version=0.2.0
release=https://github.com/hh946/qizoo-connect/releases/download/v$version
bundle=qizoo-watch-$version-macos-arm64
asset=$bundle.tar.gz
download_dir=$(mktemp -d) &&
cd "$download_dir" &&
curl -fLsS "$release/$asset" -o "$asset" &&
curl -fLsS "$release/SHA256SUMS" -o SHA256SUMS &&
awk -v asset="$asset" '$2 == asset {print; matched++} END {if (matched != 1) exit 1}' SHA256SUMS > selected.sha256 &&
shasum -a 256 -c selected.sha256 &&
mkdir -p "$HOME/.local/lib" &&
tar -xzf "$asset" -C "$HOME/.local/lib" &&
"$HOME/.local/lib/$bundle/qizoo-watch" --version
```

On a Mac with Python 3.10 or later, you can instead use the smaller Python archive.

```sh
version=0.2.0
release=https://github.com/hh946/qizoo-connect/releases/download/v$version
asset=qizoo-watch-$version-python3.pyz
download_dir=$(mktemp -d) &&
cd "$download_dir" &&
curl -fLsS "$release/$asset" -o "$asset" &&
curl -fLsS "$release/SHA256SUMS" -o SHA256SUMS &&
awk -v asset="$asset" '$2 == asset {print; matched++} END {if (matched != 1) exit 1}' SHA256SUMS > selected.sha256 &&
shasum -a 256 -c selected.sha256 &&
mkdir -p "$HOME/.local/bin" &&
install -m 755 "$asset" "$HOME/.local/bin/qizoo-watch-$version" &&
"$HOME/.local/bin/qizoo-watch-$version" --version
```

Use the installed launcher’s full path in the following commands or add its directory to your shell’s `PATH`.

## Older preview builds with browser confirmation

Only early preview builds (52 and earlier) show a browser pairing code. The App Store version uses watch confirmation above. For an older preview build, use `qizoo-watch connect --browser`.

1. On Apple Watch, open Qizoo, allow notifications, and tap **Connect your computer AI** (shown as **连接电脑上的 AI** in the current build). The watch shows a short-lived pairing code.
2. The AI runs `qizoo-watch connect --code <watch-pairing-code>`. For a campaign-specific prompt, pass the named `--campaign` value to this pairing command. With `--no-browser`, open the printed URL yourself. Use the official page at `https://qizoo.tipuu.com/watch-connect`.
3. Sign in or register on the website yourself. Enter the **computer verification code** from the CLI, check the computer and watch names, and confirm. If the CLI exits first, run `qizoo-watch connect` again to continue.
4. Check that `qizoo-watch status` returns `connected`. Send one test request and answer it from the watch. Check the reply before reporting success to the user.

```sh
qizoo-watch ask-wait --source codex --task "Connection test" --body "Tap Received if you can see this request." --option "Received"
qizoo-watch status --source codex
```

Record the request ID and the watch’s actual reply. A matching reply sets `roundtrip_verified` for that source. Each AI host still needs its own permissions to run the CLI and access the network. Run `qizoo-watch revoke` to disconnect this computer. The CLI keeps its revocable credentials in the current user’s restricted directory at `~/Library/Application Support/Qizoo Watch`.

## Supported paths

Tested with Codex desktop and Codex CLI on Apple silicon Macs and the App Store version of Qizoo for Apple Watch (watchOS 10 or later). Other AI tools that can run terminal commands, such as Claude Code, follow the same steps; pass their name with `--source`, for example `--source claude-code`.

For AI hosts that can make HTTPS requests and store credentials securely, use the [HTTP example](https://github.com/hh946/qizoo-connect/blob/main/examples/http.md). MCP-only hosts need a separate adapter. See [notifications, approvals and reconnecting](https://github.com/hh946/qizoo-connect/blob/main/docs/approvals.md) for how to keep the same request ID through retries.
