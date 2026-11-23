# Antigravity Gem

*A full AI coding environment for Android — in your pocket.*

**Antigravity Gem** runs four coding agents on your phone — Antigravity (`agy`), Claude Code, OpenCode and Codex — with a real Linux terminal, a code editor, a Git client and a browser the agent can control, all in one app.

By [Aazim Ameen](https://github.com/aazimameen1122).

## Download

Get the latest APK from [Releases](https://github.com/aazimameen1122/antigravity-gem/releases). The app package is `aazim.gems`; it sets up its own Linux environment and can be installed next to Termux.

- `AntigravityGem-v<version>.apk` — normal build (arm64 phones)
- `AntigravityGem-v<version>-chroot.apk` — with the Ubuntu chroot bridge (rooted phones)
- `AntigravityGem-v<version>-x86_64.apk` — emulators and Intel/AMD Chromebooks

## What it does

- **Four agents, one app:** Antigravity, Claude Code, OpenCode (200+ providers, your own keys) and Codex, with streaming replies, tool approvals, diffs and subagent progress as native chat UI.
- **Real Linux terminal:** a full Termux-based environment with `apt`/`pkg`, bash, Python, Node.js and Git — no root, no proot, full speed.
- **Code editor and Git:** syntax highlighting, tabs, find/replace, and clone/commit/pull/push from a source-control view.
- **Built-in browser** for previewing local dev servers, controllable by the agent.
- **Jarvis voice assistant:** run the whole app hands-free by voice.
- **Build on the phone:** set up Kotlin/Compose, Flutter and React Native builds from Settings.
- **Plugins and MCP servers:** one-tap MCP presets (Browser, Terminal, Android, Notes and more) and an installable plugin system.

## Building

Releases are built by GitHub Actions (`.github/workflows/release.yml`): JDK 21, Go, and the signing key from the `RELEASE_KEYSTORE_BASE64` repository secret. Locally:

```
git submodule update --init termux-am
./gradlew assembleRelease
```

## Notes on this fork

See `PORTING.md` for the package-name decision, signing, what still points upstream (Linux rootfs and plugin content sources), and the Phase 2 roadmap (skill catalogue, offline voice, preview improvements).

## License

GPL-3.0 — see `LICENSE`. Antigravity Gem is a fork of [AntiGem](https://github.com/Santoshkurmi/antigem) by Santosh Kurmi, used and redistributed under the GNU General Public License v3.0.
