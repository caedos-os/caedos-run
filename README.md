<p align="center">
  <img src=".github/aether.png" width="100%" alt="Aether's control room: a table of seven running processes showing each one's status, leash, last run, tick count and cost. Illustrative data.">
</p>

# ⣿ Aether

**An Operating System for AI Agents.** The runtime for standing work.

Aether is the OS your agent works on. Your agent installs **synths** that watch or react to the world (a page, an API, a feed, a database, an event) and then walks away. The synths keep running on Aether. If nothing changes, they cost nothing. A model is only called when something truly needs reasoning. And nothing ever affects the real world until you allow it.

> [!NOTE]
> **Aether is closed source.** This repository holds releases, issues, discussions and blueprints. There's no source code here.

> [!IMPORTANT]
> **Not released yet.** Aether is pre-1.0 and single-user. It's been tested on Windows; macOS and Linux are untested so far. To hear when the first build is out, click **Watch → Custom → Releases**.

## How it works

1. **Ask.** Tell your assistant what to keep an eye on. In Claude Code or any MCP client, that's one ordinary sentence.
2. **It writes a synth.** A small program, usually 30 to 60 lines of config: where to look, what counts as a change, when to think, what to do. Data, not generated code.
3. **Shadow.** One real run against the real world, with every action muted. You see what it read, what it would remember and what it would have sent.
4. **Deploy, leashed.** It becomes a process and starts ticking. Anything that would touch the outside world waits in your approval queue.
5. **Allow.** When you trust it, press **Allow** in the control room. That's a human act. It isn't one of your AI's tools.
6. **Leave.** Close the chat. The process keeps its schedule, its memory and its budget. Every tick leaves a trace.

## Most ticks are silent

A tick is one run of a process. It starts on a schedule, when a webhook arrives, when a signal lands on the Relay, or when you press Run. Every tick walks the same six phases:

```
LOAD → OBSERVE → FILTER → REASON → ACT → SAVE
```

A deterministic gate in FILTER decides whether a model is needed. **A tick that sees no change makes zero model calls.** It costs nothing and leaves one line in the journal: `idle`.

## What's built in

- **Shadow runs.** Know what it would do before it does it.
- **The leash.** Processes that can touch the world deploy leashed. Outbound actions queue for one-tap approval until you Allow.
- **A bill you can predict.** Deterministic checks are free. Model calls get a daily dollar budget, a hard cap, and a monthly estimate before you deploy.
- **Every tick, accounted for.** What caused it, what it saw, what it cost, what it did, which version ran. Replay any tick against an edited config.
- **Synths that talk to each other.** Processes publish and subscribe on the Relay. One watches, one decides, one sends, each with its own leash.
- **Failure you can read.** Repeated failures halt the process. Stalls get a bounded repair, proven before it's adopted. When Aether gives up, you get an autopsy, not a red dot.
- **Brings your tools.** MCP servers over HTTP or stdio, REST APIs, RSS, web pages, webhooks. Credentials stay in an encrypted vault and never appear in a trace.
- **Yours to leave.** One SQLite file on your machine, your own model key, and a workspace export any time (secrets excluded).

**Models:** Anthropic (default), OpenAI and xAI, on your own key. Pick a tier per process (economy, standard, premium), and switch models in Settings without restarting.

## Blueprints

Aether ships with 9 blueprints, each validated in CI. Clone one, shadow it, deploy it. Or ask your AI to.

| Blueprint | What it does | Model calls |
|---|---|---|
| Price watcher | Alerts the moment a price on a page changes. | None |
| API health sentinel | Tells you once when a status changes, and again when it recovers. | None |
| GitHub release watcher | New release on a repo you depend on. | None |
| HN keyword watcher | New Hacker News stories that mention your word. | None |
| Page change watcher | Only thinks when the page actually changed. | Only on change, ≤ $0.05/day |
| Daily HN brief | Three lines on what mattered, once a day. | One a day, ≤ $0.01/day |
| Keyword pulse analyst | Notices when chatter shifts, and says why. | Only on change, ≤ $0.02/day |
| Relay bridge | Team glue: forwards findings to a digest topic. | None |
| Outbound courier | Hands approved drafts to your n8n or Zapier webhook. | None |

## Security, plainly

- Binds to `127.0.0.1` unless you say otherwise.
- Secrets are encrypted at rest (AES-256-GCM), resolved only inside the step that uses them, and never written to a trace.
- No telemetry. The only calls out go to your model provider and the places your processes are told to reach.
- **Aether is single-user and has no authentication yet.** Don't expose it to a network you don't trust.

Found a vulnerability? Email **support@aether.run**. Please don't open a public issue.

## Pricing

- **Free** for personal and evaluation use.
- **Commercial use** needs a license. Ask at support@aether.run.
- **Your model bill is yours.** You bring your own key, and Aether takes no cut.

The full license terms will come with the first release.

## Community

- **Questions and ideas:** [Discussions](../../discussions)
- **Bugs:** [Issues](../../issues)
- **Chat:** [Discord](https://discord.gg/fMsYbjvvC9)

---

© 2026 Ross Walpole. All rights reserved.
