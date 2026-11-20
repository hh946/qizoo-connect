# Connect your computer AI to Qizoo on Apple Watch

Choose the CLI for the Qizoo preview already installed on your watch. Qizoo Watch is available to preview testers. The App Store listing is still pending.

| Installed Watch preview | CLI and confirmation |
| --- | --- |
| Build 53 | [CLI v0.2.0rc1 prerelease](https://github.com/hh946/qizoo-connect/releases/tag/v0.2.0rc1). Compare the verification code and confirm on the watch. This requires the matching relay on `https://qizoo.tipuu.com`. |
| Build 52 | [CLI v0.1.1 stable release](https://github.com/hh946/qizoo-connect/releases/tag/v0.1.1). Sign in and confirm in the browser. |

## For the AI running setup

Run `qizoo-watch --version` and `qizoo-watch status --source <your-ai>` using the same computer account that will send requests. Reuse a connected CLI and send a test request. For a new connection, identify the available flow before asking the user for a short-lived code. Use the code only in the private setup session. Keep it out of logs and shared files.

### Watch confirmation with build 53

Install or reuse CLI `0.2.0rc1` using the steps below. The CLI defaults to `https://qizoo.tipuu.com`. The user picks **Connect computer** on the watch. Ask them for the connection code, start `qizoo-watch connect --code-stdin`, and give the code to the process as one line of standard input. For an isolated relay, include `--relay-url <test-origin>`. If a campaign prompt specifies a recognized channel, include `--campaign <channel>`.

Keep the command running and read its progress from standard error. Tell the user the **computer verification code** as soon as the CLI displays it. The user compares the code and computer name with the watch, then taps **Confirm connection** on the watch. If your terminal integration cannot show progress while the command runs, have the user run the command in their terminal so they can see the code. When the CLI reports `connected`, send a watch request and retrieve the user's reply with the same request ID. If the connection expires or the user cancels, ask for a fresh code. If the CLI was force-closed after watch confirmation and the computer now appears on the watch, revoke it from the watch before retrying. `qizoo-watch service` shows any current service announcement.

If browser setup is the supported option, explain it and let the user choose before starting it. CLI v0.1.1 has a browser flow by default. CLI v0.2.0rc1 can handle a browser pairing code from build 52 with `connect --browser`. Add `--no-browser` to print the link for the user. A build 53 code is for watch confirmation.

## Install or reuse the CLI

Reuse a compatible official installation after checking `qizoo-watch --version`. AI tools running under the same computer account can share a completed connection if their host grants access to the local credential store and network.

On an Apple silicon Mac, use the bundled package with its own Python runtime. The commands below default to `0.2.0rc1`. Change the first line to `version=0.1.1` for Watch build 52. Download the assets from the matching tag and verify the release checksum before extracting. The versioned directory lets you keep both versions installed.

[Apple silicon package for v0.2.0rc1](https://github.com/hh946/qizoo-connect/releases/download/v0.2.0rc1/qizoo-watch-0.2.0rc1-macos-arm64.tar.gz) · [Python archive for v0.2.0rc1](https://github.com/hh946/qizoo-connect/releases/download/v0.2.0rc1/qizoo-watch-0.2.0rc1-python3.pyz) · [SHA256SUMS for v0.2.0rc1](https://github.com/hh946/qizoo-connect/releases/download/v0.2.0rc1/SHA256SUMS)

```sh
version=0.2.0rc1
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
version=0.2.0rc1
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

## Pair with browser confirmation on build 52

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

Local testing covered Codex desktop and Codex CLI on Apple silicon. A watchOS simulator running build 53 completed watch confirmation and returned both an approval and a decline to their originating requests. Background delivery to a physical watch still needs verification.

For AI hosts that can make HTTPS requests and store credentials securely, use the [HTTP example](https://github.com/hh946/qizoo-connect/blob/main/examples/http.md). MCP-only hosts need a separate adapter. See [notifications, approvals and reconnecting](https://github.com/hh946/qizoo-connect/blob/main/docs/approvals.md) for how to keep the same request ID through retries.
