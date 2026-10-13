# drumai

Command line tool for [Drum AI](https://apps.apple.com/app/id6782609749) — build drum
patterns as PRESETs, render them into share links, and drive the App from a terminal or
from an AI agent.

This repository hosts the prebuilt macOS binaries. There is nothing to build here: pick a
release, or install with Homebrew.

## Install

### Homebrew

```sh
brew tap govo/tap
brew trust govo/tap
brew install drumai
```

Homebrew 6 and later refuse to load formulae from a third-party tap until it is trusted
explicitly, so the `brew trust` line is required. This tap holds nothing but `drumai`.
Older Homebrew has no `trust` command — drop that line and the rest works as is.

### Manual download

Download the tarball for your machine from
[Releases](https://github.com/govo/drumai-cli/releases/latest):

| Machine | File |
|---|---|
| Apple silicon | `drumai-<version>-darwin-arm64.tar.gz` |
| Intel | `drumai-<version>-darwin-x64.tar.gz` |

```sh
tar -xzf drumai-<version>-darwin-arm64.tar.gz
sudo mv drumai /usr/local/bin/
drumai version
```

Both binaries are signed with a Developer ID certificate and notarized by Apple, so
Gatekeeper accepts them without a warning. If your download tool stripped the notarization
ticket, clear the quarantine flag once:

```sh
xattr -d com.apple.quarantine /usr/local/bin/drumai
```

## Requirements

- macOS 13 or later
- No runtime needed: the binary embeds its own JavaScript engine, so Node.js is not required

The App itself is not required for pattern building. It is required for the commands that
control playback (`play`, `stop`, `preset`, `apply`, `live`), which talk to the App running
in the simulator or on the Mac.

## Quick start

```sh
drumai help                          # all commands
drumai kits                          # the built-in kits
drumai kit kit-10                    # one kit's voices and their indices
drumai presets                       # the built-in presets

# build a pattern and get a share link
draft=$(drumai draft create --kit kit-10 --name demo --bpm 120 --bars 2 | jq -r .draftId)
drumai draft voice "$draft" --voices '[{"index":0,"triggers":"x---x---x---x---x---x---x---x---"}]'
drumai draft validate "$draft"
drumai render "$draft"               # prints a landing-page link and a drumai:// deep link
```

Every command takes `--help`. Output is JSON when piped, so `jq` composes well with it.

## Shell completion

```sh
drumai completion zsh > "${fpath[1]}/_drumai"
drumai completion bash > /usr/local/etc/bash_completion.d/drumai
```

## Language

The CLI follows the system language and supports Simplified Chinese and English.

```sh
drumai lang            # show the language currently in effect
drumai lang en         # switch to English
drumai --lang en help  # override for a single call
```

## Uninstall

```sh
brew uninstall drumai     # Homebrew install
rm /usr/local/bin/drumai  # manual install
rm -rf ~/.drumai          # local drafts and settings
```

---

# drumai（简体中文）

Drum AI 的命令行工具：用命令行造鼓点 PRESET、生成分享链接，也可以直接操控 App，
或让 AI 工具通过它来生成鼓点。

本仓库只放编译好的 macOS 二进制，不需要在这里做任何构建。

## 安装

### Homebrew

```sh
brew tap govo/tap
brew trust govo/tap
brew install drumai
```

Homebrew 6 起，第三方 tap 必须先显式信任，否则 formula 根本不会被加载，所以
`brew trust` 这一行是必需的。这个 tap 里只有 `drumai` 一个 formula。更早的 Homebrew
没有 `trust` 命令，去掉那一行即可。

### 手动下载

到 [Releases](https://github.com/govo/drumai-cli/releases/latest) 下载对应机器的压缩包：
Apple 芯片选 `drumai-<版本>-darwin-arm64.tar.gz`，Intel 选 `drumai-<版本>-darwin-x64.tar.gz`。

```sh
tar -xzf drumai-<版本>-darwin-arm64.tar.gz
sudo mv drumai /usr/local/bin/
drumai version
```

两个架构的二进制都做了 Developer ID 签名并通过 Apple 公证，Gatekeeper 不会拦。
如果下载工具把公证票据弄丢了，清一次隔离标记即可：

```sh
xattr -d com.apple.quarantine /usr/local/bin/drumai
```

## 运行要求

- macOS 13 或更高版本
- 不需要装 Node：二进制里内嵌了运行时

只造鼓点不需要装 App；`play`、`stop`、`preset`、`apply`、`live` 这些操控播放的命令
需要 App 正运行在模拟器或 Mac 上。

## 快速开始

```sh
drumai help                       # 全部命令
drumai kits                       # 内置鼓组
drumai kit kit-10                 # 某个鼓组的部件表（含索引）
drumai draft create --kit kit-10 --name demo --bpm 120 --bars 2
drumai render <draftId>           # 输出落地页链接与 drumai:// 深链
```

每条命令都支持 `--help`。输出是 JSON，便于配合 `jq` 使用。

## 语言

跟随系统语言，支持简体中文与英文。

```sh
drumai lang            # 看当前生效的语言
drumai lang en         # 切到英文
drumai --lang en help  # 只对这一次调用生效
```

## 卸载

```sh
brew uninstall drumai     # Homebrew 装的
rm /usr/local/bin/drumai  # 手动装的
rm -rf ~/.drumai          # 本地草稿与设置
```
