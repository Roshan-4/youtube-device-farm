<div align="center">

<img src="assets/logo.svg" alt="" width="96" height="96">

# mobile-agent-testers

**20 AI testers on 20 different phones, in parallel, on GitHub's free runners.**

[![Tests](https://github.com/pierridotite/mobile-agent-testers/actions/workflows/tests.yml/badge.svg)](https://github.com/pierridotite/mobile-agent-testers/actions/workflows/tests.yml)
[![Stars](https://img.shields.io/github/stars/pierridotite/mobile-agent-testers?style=flat)](https://github.com/pierridotite/mobile-agent-testers/stargazers)
[![License: MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE)
![iOS + Android](https://img.shields.io/badge/platforms-iOS%20%2B%20Android-blue)
![Claude Code plugin](https://img.shields.io/badge/Claude%20Code-plugin-orange)

**[Examples](docs/examples.md)** · **[Dashboard demo](https://pierridotite.github.io/mobile-agent-testers/)** · **[Compare](docs/compare.md)** · **[FAQ](docs/faq.md)** · **[Personas](personas.json)** · **[Contributing](CONTRIBUTING.md)**

<sub>English · [简体中文](i18n/zh-CN/README.md) · [日本語](i18n/ja/README.md) · [Español](i18n/es/README.md) · [Português](i18n/pt-BR/README.md) · [Tiếng Việt](i18n/vi/README.md) · [Türkçe](i18n/tr/README.md)</sub>

<a href="https://www.linkedin.com/in/pierre-raffalli/"><picture><source media="(prefers-color-scheme: dark)" srcset="assets/linkedin-follow-dark.svg"><img src="assets/linkedin-follow-light.svg" alt="Follow Pierre Raffalli on LinkedIn" width="160" height="54"></picture></a>
<a href="https://discord.gg/WuaCANscr8"><picture><source media="(prefers-color-scheme: dark)" srcset="assets/discord-join-dark.svg"><img src="assets/discord-join-light.svg" alt="Join us on Discord" width="160" height="54"></picture></a>
<a href="https://trendshift.io/repositories/371956?utm_source=trendshift-badge&amp;utm_medium=badge&amp;utm_campaign=badge-trendshift-371956" target="_blank" rel="noopener noreferrer"><img src="https://trendshift.io/api/badge/trendshift/repositories/371956/daily?language=Python" alt="pierridotite%2Fmobile-agent-testers | Trendshift" width="250" height="55"/></a>

</div>

<p align="center"><a href="assets/localsend-20.mp4"><img src="assets/localsend-20.gif" alt="Real screen recordings of 20 AI testers using LocalSend at the same time: 10 Android emulators and 10 iOS Simulators in 14 languages, from a small Pixel to a 13-inch iPad. When a tester finishes, its phone shows how many issues it found. Opens the video." width="100%"></a><br>
<sub>Real screen recordings of the LocalSend run, idle time cut. <a href="assets/localsend-20.mp4">Full video (MP4)</a> · <a href="docs/runs/localsend-20.md">The run, device by device</a></sub></p>

No test scripts to write. Each tester is a Claude Code agent with its own iOS
Simulator or Android emulator on GitHub Actions. It plays a persona (iPhone SE
in Swedish at the largest text size, Pixel 7 at 200%, iPad in German, Arabic
right to left...), uses your app like a new user (or runs through the feature you
point it at) and writes a bug report with screenshots. Flutter, React Native, Expo, native Kotlin and Swift, Kotlin
Multiplatform, Capacitor and .NET MAUI.

GitHub's runners are free for public repos. You only pay for the model, with your
Claude subscription or API key: about $0.15 to $0.55 per phone in [these runs](docs/runs/).

<p align="center"><a href="https://github.com/pierridotite/explore-compile-replay"><img src="assets/ecr-now-building.svg" alt="Next: every bug becomes a free test. Explore-Compile-Replay" width="340" height="54"></a><br>
<sub>What I'm building next: when an agent finds a bug, Explore-Compile-Replay turns its path into a test that reruns on every PR, with no model. Early draft.</sub></p>

## Quick start

```bash
gh repo create my-testers --public --template pierridotite/mobile-agent-testers --clone
cd my-testers && ./setup.sh           # key pair + secrets + Claude token (Windows: .\setup.ps1)
gh workflow run agent-testers.yml     # demo: 2 Android + 2 iOS testers, ~20 min
bash scripts/collect.sh <run id>      # decrypts reports, groups duplicates: runs/<id>/SUMMARY.md
```

Then on your own app:

```bash
gh workflow run agent-testers.yml -f source_repo=you/app -f app_id=com.you.app \
  -f android_personas=anna,malik -f ios_personas=clara,jonas
```

Or let Claude set it up for you: `/plugin marketplace add pierridotite/mobile-agent-testers`, then `/plugin install mobile-agent-testers@mobile-agent-testers`.

Every input (builds, briefs, bug reports, cheaper models, build arguments, video) and the supported frameworks: [docs/configuration.md](docs/configuration.md).

<details>
<summary>What the demo run finds</summary>

The demo app has [11 deliberate bugs](demo-app/BUGS.md). In [one run](docs/runs/), the 4 demo testers found 9 of them (2 to 7 minutes per agent, about 21 minutes for the whole run, builds included), plus real issues nobody planted (a button under the Android nav bar, no data kept after a relaunch). Model usage: $0.87 for the 4.

</details>

## What it does

One use per run, set with the inputs above.

| Use | What you get |
|---|---|
| **Explore like new users** (default) | Each persona tries to reach its goal and reports what gets in the way, with numbered screenshots. |
| **Run one feature end to end** | Your brief sends every agent through a whole mechanic: taps, swipes, typing, relaunches, deep links. [AnkiDroid's](briefs/ankidroid.md) found the Back field of a new card hidden behind the keyboard. |
| **Reproduce a bug report** | Each phone answers yes, no or unclear, with screenshots ([brief](briefs/reproduce.md)). |
| **Get an improvement report** | Where each persona hesitated, what it expected, what to change first ([brief](briefs/improve.md)). |
| **Compare two versions** | The same personas on two branches or builds, then one table of goals, scores and issues ([how](docs/compare-versions.md)). |
| **See every screen on one map** | The testers' paths and a heat map on each screen, with your comments: `python3 scripts/dashboard.py runs/<id>` ([demo](https://pierridotite.github.io/mobile-agent-testers/), [video](assets/dashboard-demo.mp4)). |

Any Anthropic-compatible model works, from Claude Haiku to OpenRouter, DeepSeek, Kimi, GLM, MiniMax or Qwen; findings are only as good as the model's reading of screenshots ([docs/cheaper-models.md](docs/cheaper-models.md)). Maestro or Appium run the tests you write; these agents look for what nobody thought to test ([how it compares](docs/compare.md)).

## Real runs

Open-source apps, with nothing planted.

| App | Phones | What the agents found | Time, model usage |
|---|---|---|---|
| [LocalSend](https://github.com/localsend/localsend) (Flutter, 100k stars) | 20: 10 Android, 10 iOS, 14 languages | 46 distinct bugs, 1 blocker: on an iPhone SE in Swedish at the largest text size, you can't send a text message at all | 69 min, $4.62 |
| [AnkiDroid](https://github.com/ankidroid/Anki-Android) (Kotlin, 12k stars, release APK) | 10 Android | 43 distinct issues, among them answer buttons cut in half at 200% font and "Get started" left in English in three languages | under 10 min, $4.69 |
| [Flutter's animations sample](https://github.com/flutter/packages/tree/main/packages/animations/example) | 1 Android, 1 iOS | The same typo, reported by both agents on their own, and an app that ignores dark mode | 19 min, $0.47 |

34 of LocalSend's 46 bugs had nothing to do with text size or accessibility: broken flows, silent failures, missing translations, tablet layouts. Screenshots, every finding and the commands to run them again: [docs/examples.md](docs/examples.md).

## Contributing

Open an issue first to say what you take, so two people don't build the same thing. [CONTRIBUTING.md](CONTRIBUTING.md) says where each idea starts and how to test it offline. Questions: [Discord](https://discord.gg/WuaCANscr8).

<details>
<summary>Ideas where help is welcome</summary>

- **Other agent CLIs**: OpenCode, Codex CLI, Gemini CLI, Goose. The tools are plain scripts and the mission is a Markdown brief, so the agent is one command per platform.
- **Web apps**: the same personas on a mobile browser with Playwright, for PWAs and mobile sites.
- **Physical Android phones** over adb, on a self-hosted runner.
- **New personas**: color blindness filters, a slow or flaky network.
- **Findings to issues**: turn the summary into draft GitHub issues with their screenshots, or a Slack or Discord message.
- **Other CI**: GitLab CI, Bitrise, Codemagic.
- **More build systems**: NativeScript, Unity, Godot, each one case in `scripts/build/`.
- **Translations** of the README and docs.
- **Model comparison**: the demo app's 11 planted bugs make a small benchmark. A table of what each model finds and costs.

</details>

## Hosted version for teams (exploring)

The workflow here stays free, open source and complete. I'm exploring a hosted version for teams with private apps: macOS runners included (on a private repo, GitHub counts each macOS minute ten times) and a web dashboard to share the reports with the whole team, product managers included.

Would that help your team? [Answer three questions](https://github.com/pierridotite/mobile-agent-testers/issues/new?template=teams.yml) or [write to me on LinkedIn](https://www.linkedin.com/in/pierre-raffalli/). Already ran it on your app? [Tell me what it found and what got in the way](https://github.com/pierridotite/mobile-agent-testers/issues/new?template=usage-feedback.yml).

## Star history

[![Star History Chart](https://api.star-history.com/svg?repos=pierridotite/mobile-agent-testers&type=Date)](https://star-history.com/#pierridotite/mobile-agent-testers&Date)

## Credits

[Claude Code](https://docs.claude.com/en/docs/claude-code) ·
[AXe](https://github.com/cameroncooke/axe) ·
[android-emulator-runner](https://github.com/ReactiveCircus/android-emulator-runner)

## Contributors

Thanks to everyone who has contributed. Want to help? [Start here](CONTRIBUTING.md).

<a href="https://github.com/pierridotite/mobile-agent-testers/graphs/contributors"><img src="https://contrib.rocks/image?repo=pierridotite/mobile-agent-testers" alt="Contributors" /></a>

Made by Pierre Raffalli · [LinkedIn](https://www.linkedin.com/in/pierre-raffalli/) · [GitHub](https://github.com/pierridotite) · [Discord](https://discord.gg/WuaCANscr8) · MIT license
