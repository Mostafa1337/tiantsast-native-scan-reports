# securoid-merged — validation lab app

This folder contains a self-contained validation package for the TIANTSAST-NATIVE engine:

| File | What it is |
|---|---|
| `securoid-merged.apk` | The lab application itself (268 KB) — install in an emulator or scan it with any tool |
| `securoid-merged.html` | The TIANTSAST-NATIVE scan report for this exact APK (20 findings, grouped HTML, evidence embedded) |

## What the app contains

`securoid-merged` is a purpose-built **validation lab**, not a production app, not a capture-the-flag challenge, and **not a record of real findings**. It reproduces well-documented Android vulnerability patterns from **public security research by other authors** — rebuilt deliberately so the engine can be checked against known ground truth. No original discoveries are claimed.

Patterns included (all reachable from exported entry points):

- **JavaScript bridge → command execution (ActionBridge, CRITICAL 9.5)** — a `@JavascriptInterface` method feeding page-controlled data into `Runtime.exec`. This is the classic dangerous-bridge pattern documented by other researchers, included as a lab flow — **not a real-world finding and not discovered by the author**
- **JavaScript bridge → arbitrary file write** — page-controlled `Base64` content written through `FileOutputStream` under the app's cache directory
- **Intent redirection via `Intent.parseUri`** — attacker URLs loaded into a WebView and parsed into dispatched Intents inside `WebViewClient` callbacks (`shouldOverrideUrlLoading`), including limited/constrained variants. Most of these redirection shapes come from **other researchers' published findings**, reproduced here as lab targets — not discovered by the author
- **WebView exposure shapes** — attacker-controlled `loadUrl` with JavaScript enabled, HTML injection, bridge exposure without a dangerous sink

## Scan results (20 findings)

- code-exec (CRITICAL) — bridge → `Runtime.exec`
- file (HIGH) — bridge → `FileOutputStream` write
- redirection — 4× Limited Intent Redirection via `parseUri` in WebViewClient callbacks + bridge-driven dispatch
- webview — unsafe URL navigation, HTML injection, bridge exposure, content-injection candidates
- A suppressed section lists internal-provenance flows the engine deliberately does not present as vulnerabilities

## Reproducing

Open `securoid-merged.html` in any browser for the full source-to-sink traces, or re-scan the APK with your own tooling — every finding names its exported entry point, the ICC hops, and the exact DEX statements between source and sink.
