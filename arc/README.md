# ARC Whispr — setup guide

Voice dictation that runs on your own Mac. Hold a key, talk, and your words
appear wherever your cursor is. Nothing you say leaves your computer.

## ⚠️ This is an AI-first setup. Don't drive — ride along.

Don't try to work through this alone. Open **Claude Code** next to you and let
it guide you. It will check your computer, tell you exactly what to click, and
confirm each step actually worked.

**Your job:** download, click, and answer questions.
**Claude's job:** check your machine, choose your settings, catch mistakes.

### Start here

1. Open Claude Code.
2. Paste this and hit Enter:

```
Read arc/AGENT-GUIDE.md from https://github.com/arc-web/arc-whispr and walk me
through setting up OpenWhispr on this computer, one step at a time. Check my
machine before recommending anything.
```

3. Do what it says. When a screen confuses you, screenshot it and paste it in.

---

## What you're installing

We don't ship our own app. You install the **official OpenWhispr app**, signed
and notarized by Apple, and set it up **inside the app**, the way its makers
intended. This repo is just the map.

| | |
|---|---|
| App | [OpenWhispr](https://github.com/OpenWhispr/openwhispr) — free, open source (MIT) |
| Official site | [openwhispr.com](https://openwhispr.com) |
| Signed by | Gizmo Labs Inc. (T832773L2J), notarized by Apple |
| This setup needs | Apple Silicon Mac (M1 or newer), macOS 15.5+ (required by the Orukeet dictation model) |
| Account | Not needed. Choose **"Continue without an account."** |
| Cost | Free. No API key. |

## The setup, at a glance

You'll go through these screens in the app. Claude explains each one as you go.

| # | Screen | What you pick |
|---|---|---|
| 1 | Welcome | **Continue without an account** (small link under the buttons) |
| 2 | Permissions | **Microphone** (required) + **Accessibility** (strongly recommended) |
| 3 | Hotkey | **Right Option** |
| 4 | Setup choice | **Local** |
| 5 | Dictation model | **Oruk → Orukeet** |
| 6 | Assistant model | Depends on your Mac's memory — see below |

![Permissions screen](screenshots/02-permissions.png)

## Which assistant model? It depends on your memory

Check your memory: **Apple menu → About This Mac** → look for **Memory**.

The dictation model (Orukeet) is the same for everyone. The *assistant* model
is what changes. The app sizes the assistant's working memory from
`(your RAM ÷ 2) − 2 GB`. Pick a model that fits inside that, with room to spare.

| Your Mac's memory | Assistant budget | Pick this | Size |
|---|---|---|---|
| **8 GB** | 2 GB | **Gemma 3 1B** ✅ *tested by ARC* | 0.81 GB |
| 16 GB | 6 GB | Gemma 3 4B | 2.49 GB |
| 24 GB | 10 GB | Gemma 4 E4B QAT | 5.2 GB |
| 32 GB | 14 GB | GPT-OSS 20B | 12.1 GB |
| 64 GB or more | 30 GB+ | Gemma 4 31B QAT | 17.9 GB |

> **Don't trust the "Recommended" labels in the app** if you have 8 or 16 GB.
> They assume a bigger machine. On 8 GB, five of the six recommended models are
> over budget. An over-budget model still loads, but it squeezes your memory and
> slows the whole Mac down.

Only the 8 GB row has been tested on real hardware. The rest are calculated from
the app's own memory formula (`src/helpers/llamaContextPolicy.js`). If yours
feels slow, go one row down.

## The four mistakes that cost us time

1. **Don't copy the app — drag it.** Drag OpenWhispr into Applications in
   Finder. If it gets copied some other way, macOS can run it from a hidden temp
   folder, and the Accessibility permission may stop working without warning.
2. **Don't press the hotkey until setup is finished.** Until you choose your
   local models, the app's default is its cloud service. Dictating early sends
   your words online. (ARC's own first test did exactly this.)
3. **Don't use F-keys with hold-to-talk.** On a Mac, F5/F6 can't detect you
   letting go. Use **Right Option** for hold-to-talk, or switch to **Tap** mode.
4. **Don't use the Globe (🌐) key.** To take it over, the app changes a macOS
   system setting while it runs. It restores the setting afterwards, but Right
   Option does the same job without touching system settings.

## After setup — two settings to check

In **Settings**, confirm these two:

- **Automatic updates: off** — the app still checks for updates; this just
  stops it downloading them on its own.
- **Activation mode** matches your key — Hold for Right Option, Tap for F-keys.

## Is it working?

Ask Claude: *"Check my last dictation stayed local."* It reads the app's history
and confirms your words were handled by `nvidia / orukeet-v0.1.0` — your Mac —
not `openwhispr-cloud`.
