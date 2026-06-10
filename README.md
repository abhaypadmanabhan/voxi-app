# Voxi

**Local hold-to-talk dictation for macOS.** Hold the Fn key, speak, release. Clean text lands at your cursor in about 130 milliseconds, transcribed entirely on-device.

[**Download for macOS**](https://github.com/abhaypadmanabhan/voxi-app/releases/latest) · [Website](https://voxi-landing.vercel.app) · [Install & FAQ](https://voxi-landing.vercel.app/faq)

> Public beta. Requires macOS 14+ on Apple Silicon. Notarized by Apple.

---

## Why it exists

macOS dictation is slow to start, needs a network round-trip for its best models, and types word by word. Voxi is built around one interaction: hold a key, think out loud, release, and the finished sentence is already at your cursor.

## How it works

```
Fn down ──► mic capture (16 kHz) ──► Swift ASR sidecar ──► formatter ──► paste at cursor
            AVAudioEngine             Parakeet TDT v2          rule-based,
            non-activating pill UI    on the Neural Engine     optional LLM pass
```

- **App**: pure SwiftUI menu-bar app, 4.8 MB DMG. A non-activating floating pill shows live state without stealing focus from the app you're typing in.
- **ASR**: NVIDIA Parakeet TDT 0.6B v2 running on the Apple Neural Engine via [FluidAudio](https://github.com/FluidInference/FluidAudio). No GPU spin-up, ~34 MB idle memory.
- **Formatting**: deterministic rule formatter by default (instant); optional opt-in LLM pass for tone-aware cleanup. The formatter knows the frontmost app, so dictating into a terminal behaves differently than dictating into an email.
- **Paste**: synthetic Cmd+V only when an editable text field has focus; otherwise the transcript stays on the clipboard and the pill says so.

## Measured performance

| Metric | Value |
|---|---|
| Speech-to-text per utterance | ~125 ms |
| Hotkey release → text pasted (p50) | ~130 ms |
| First utterance after launch | ~65 ms STT (model pre-warmed at startup) |
| Sidecar idle memory | ~34 MB |
| App download | 4.8 MB DMG |

Everything runs locally. No accounts, no audio upload, no telemetry. [Privacy policy](https://voxi-landing.vercel.app/privacy).

## Why is the core closed?

Voxi is in active solo development and the core is closed source for now. This repo is the public face: release downloads, the changelog, architecture notes, and benchmarks. If you're curious about specific engineering details (the Fn event-tap handling, the Swift 6 actor-isolation traps, ANE warm-up strategy), I'm happy to talk: open an issue or email abhaykerala+voxi@gmail.com.

## Releases & updates

DMGs are published on the [Releases](https://github.com/abhaypadmanabhan/voxi-app/releases) page. The app updates itself via Sparkle; the appcast is served from the latest release's assets.

---

© 2026 Padzy. Voxi ships open-source components under their own licenses: [acknowledgements](https://voxi-landing.vercel.app/acknowledgements).
