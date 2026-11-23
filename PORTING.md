# Antigravity Gem — porting notes

Fork of Santoshkurmi/antigem v2.0.3 (commit e721833), GPL-3.0. Started 2026-11-23.

## Package decision
Display name is "Antigravity Gem"; `applicationId` is **`aazim.gems`**
(Aazim's call, 2026-11-23; debug variant `aazim.gemd`). AntiGem relocates
Termux packages by rewriting `com.termux` to the app package byte-for-byte,
so the new package had to be exactly 10 characters — `aazim.gems` is.
The audit covered: `applicationId` + debug variant in `app/build.gradle.kts`,
`TermuxRelocation.RELEASE_PACKAGE`, `TERMUX_PACKAGE_NAME` in
`termux-am-build/build.gradle.kts` (the `am` binary's intent package), the
own-prefix paths in `assets/bin/` scripts, and the Go relocate tests.
The Kotlin **namespace and all source packages stay `ai.antigem.*`** — those
are class names, not the app identity, and manifest component/action names
built on them stay consistent. On-device verification (first-run setup,
terminal, `am`) is still the real verdict.
Also fixed on the way: upstream's "Unofficial build: analytics disabled"
developer Toast fired on every cold start of any fork-signed build — removed;
upstream TelemetryDeck analytics now stays off silently.

## Signing
Releases are signed with this fork's own permanent key (`app/release.jks`,
gitignored, generated 2026-11-23, cert SHA-256
34:A4:21:AF:24:3A:77:3E:D6:71:F3:30:24:D8:EA:FB:1A:CD:CC:F5:F7:27:66:9A:1A:7A:60:65:D4:91:1C:D0).
Never replace this key: Android updates require the same certificate.
The signing-config passwords still match upstream's hardcoded defaults;
changing them is a tracked follow-up.

## What still points upstream (deliberate)
- Linux rootfs download (`LocalEnvironmentManager.HARDCODED_ROOTFS_URL`) and
  the chroot prebuilt repo — these artefacts only exist in upstream releases.
- Mini-app store repo (`Santoshkurmi/antigem_mini_apps`), plugin repos and the
  REA default source — content sources, not app identity.
Repointing any of these before equivalent artefacts exist in this repo would
break first-run setup.

## What points here now
- App update check: `raw.githubusercontent.com/aazimameen1122/antigravity-gem/standalone/version.json`
- Update APK URL: `github.com/aazimameen1122/antigravity-gem/releases/download/v<version>/AntigravityGem-v<version><suffix>.apk`
  — release assets MUST be named exactly `AntigravityGem-v<version>.apk`
  (plus `-chroot` / `-x86_64` suffix variants) or in-app updates will 404.
- Crash-report issue link and Settings GitHub links: this repo.

## Phase 2 — porting Aazim's earlier Antigravity work
From the KuatoDev-based Antigravity app (com.aazim.antigravity, v1.1.3):
1. **61-skill catalogue** — preload into AntiGem's skills/plugin system
   (Settings -> skills; plugin repos in `PluginRepositories`).
2. **Offline voice (Vosk)** — single mic button, no language picker,
   English vs Hindi/Hinglish auto-detect per segment, Hindi output
   transliterated to Roman (never Devanagari). AntiGem's Jarvis uses
   device/online voices; the Vosk path would plug in as an alternative
   speech recognizer. No offline Urdu Vosk model exists — do not fake it.
3. **Preview auto-port scan** — probe last-working port first, then
   3000/8000/5173/8080/5000/4200/5500/9000, and show a friendly
   "No server is running" screen instead of a raw WebView error, in
   AntiGem's in-app browser preview.
