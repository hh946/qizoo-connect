# Connect your computer AI to Qizoo on Apple Watch

## Install or reuse the CLI

Run `qizoo-watch --version` in the current computer user’s terminal. Reuse an existing official installation at version 0.1.1 or later. Update an older official installation to 0.1.1 before using `--campaign`. AI tools running under the same computer account can share a completed connection if their host grants access to the local credential store and network.

On an Apple silicon Mac, the bundled package includes its own Python runtime. Download the package and checksum manifest from the [Qizoo Connect v0.1.1 release](https://github.com/hh946/qizoo-connect/releases/tag/v0.1.1). Verify the archive before extracting it. Keep the versioned install directory intact.

```sh
release=https://github.com/hh946/qizoo-connect/releases/download/v0.1.1
asset=qizoo-watch-0.1.1-macos-arm64.tar.gz
download_dir=$(mktemp -d) &&
cd "$download_dir" &&
curl -fLsS "$release/$asset" -o "$asset" &&
curl -fLsS "$release/SHA256SUMS" -o SHA256SUMS &&
awk -v asset="$asset" '$2 == asset {print; matched++} END {if (matched != 1) exit 1}' SHA256SUMS > selected.sha256 &&
shasum -a 256 -c selected.sha256 &&
mkdir -p "$HOME/.local/lib" &&
tar -xzf "$asset" -C "$HOME/.local/lib" &&
"$HOME/.local/lib/qizoo-watch-0.1.1-macos-arm64/qizoo-watch" --version
```

On a Mac with Python 3.10 or later, you can instead use the smaller Python archive.

```sh
release=https://github.com/hh946/qizoo-connect/releases/download/v0.1.1
asset=qizoo-watch-0.1.1-python3.pyz
download_dir=$(mktemp -d) &&
cd "$download_dir" &&
curl -fLsS "$release/$asset" -o "$asset" &&
curl -fLsS "$release/SHA256SUMS" -o SHA256SUMS &&
awk -v asset="$asset" '$2 == asset {print; matched++} END {if (matched != 1) exit 1}' SHA256SUMS > selected.sha256 &&
shasum -a 256 -c selected.sha256 &&
mkdir -p "$HOME/.local/bin" &&
install -m 755 "$asset" "$HOME/.local/bin/qizoo-watch" &&
"$HOME/.local/bin/qizoo-watch" --version
```

Use the installed launcher’s full path in the following commands or add its directory to your shell’s `PATH`.

## Pair for the first time

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

The initial local verification covered Codex desktop and Codex CLI running on an Apple silicon Mac with a watch simulator. Delivery to a physical watch while the app is in the background still needs separate verification.

For AI hosts that can make HTTPS requests and store credentials securely, use the [HTTP example](https://github.com/hh946/qizoo-connect/blob/main/examples/http.md). MCP-only hosts need a separate adapter. See [notifications, approvals and reconnecting](https://github.com/hh946/qizoo-connect/blob/main/docs/approvals.md) for how to keep the same request ID through retries.
