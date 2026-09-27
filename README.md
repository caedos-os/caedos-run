<p align="center">
  <img src=".github/aether.png" width="100%" alt="Aether: An Operating System for AI Agents. The runtime for standing work. Beside it, the control room: a table of seven running processes showing each one's status, leash, last run, tick count and cost. Illustrative data.">
</p>

# ⣿ Aether

**An Operating System for AI Agents.** The runtime for standing work.

Aether is the OS your agent works on. Your agent installs **synths** that watch or react to the world (a page, an API, a feed, a database, an event) and then walks away. The synths keep running on Aether. If nothing changes, they cost nothing. A model is only called when something truly needs reasoning. And nothing ever affects the real world until you allow it.

> [!NOTE]
> **Aether is closed source.** This repository holds releases, issues, discussions and blueprints. There's no source code here.

> [!IMPORTANT]
> **Not released yet.** Aether is pre-1.0 and single-user. It's tested on Windows; macOS and Linux aren't tested yet. To hear when the first build is out, click **Watch → Custom → Releases**.

## The problem

**You keep asking. It keeps forgetting.** You ask your assistant. It looks, it answers, and the moment the chat closes, the work stops. So every morning you ask again: did the upstream ship a release? Did the status page go red? Did anyone mention you on Hacker News?

The usual workarounds, and what each one costs:

| Workaround | What happens | Costs |
|---|---|---|
| **Keep asking.** | You become the scheduler. Anything that changes between chats waits until you think to ask. | your attention |
| **Put a model on a timer.** | It reads the same unchanged page every hour and tells you nothing happened. | a model call per check |
| **Hand it to an agent.** | Give it a webhook and a loop and it acts on its own. The first you hear of a bad idea is from whoever received it. | a send you never saw |

## The solution

**Ask once. Leave. It keeps watching.** The same checks, without the asking.

1. **Ask.** Tell your assistant what to keep an eye on. In Claude Code or any MCP client, that's one ordinary sentence.
2. **It writes a synth.** A small program, usually 30 to 60 lines of config: where to look, what counts as a change, when to think, what to do. Data, not generated code.
3. **Shadow.** One real run against the real world, with every action muted. You see what it read, what it would remember and what it would have sent.
4. **Deploy, leashed.** It becomes a process and starts ticking. Anything that would touch the outside world waits in your approval queue.
5. **Allow.** When you trust it, press **Allow** in the control room. That's a human act. It isn't one of your AI's tools.
6. **Leave.** Close the chat. The process keeps its schedule, its memory and its budget. Every tick leaves a trace.

## Who does what

**Your AI writes. Aether runs. You decide.** Aether splits standing work into the part a model is good at (writing the synth) and the part it isn't (running it faithfully and cheaply, tick after tick).

| | Who | What they do |
|---|---|---|
| **01 · Authoring** | Your AI | In Claude Code or any MCP client, it writes the synth, shadows it, deploys it, reads traces and patches from evidence. Then it leaves. |
| **02 · Running on Aether** | Aether | Once deployed, the synth runs as a process. Aether fires it on its trigger, walks 6 phases per tick, records everything and repairs what it can. |
| **03 · Approval** | You | You allow side effects, approve or reject queued actions, read journals and set policy. |

*Allow is a button in your control room. It isn't one of your AI's tools.*

## Most ticks are silent. That's the design.

A tick is one run of a process. It starts whenever the trigger fires: on a schedule, when a webhook arrives, when a signal lands on the Relay, or when you press Run. Every tick walks the same 6 phases:

```
LOAD → OBSERVE → FILTER → REASON → ACT → SAVE
```

A deterministic gate in the middle decides whether a model is needed. If nothing changed, no model is called: the tick costs nothing and leaves one line in the journal that says `idle`.

**A tick that sees no change makes zero model calls.**

## Built for work you'd rather not think about

- **Know what it would do before it does it.** Shadow runs one real tick with every action muted: what it read, what it would remember, what it would send.
- **Nothing leaves without your OK.** Processes that can touch the world deploy leashed. Outbound actions queue for one-tap approval until you Allow.
- **A bill you can predict.** Deterministic checks are free. Model calls get a daily dollar budget, a hard cap, and a monthly estimate before you deploy.
- **Every tick, accounted for.** What caused it, what it saw, what it cost, what it did, which version ran. Replay any tick against an edited config.
- **Synths that talk to each other.** Synths run as processes that publish and subscribe on the Relay. One watches, one decides, one sends, each with its own leash.
- **Failure you can read.** Repeated failures halt the process. Stalls get a bounded repair, proven before it's adopted. When Aether gives up, you get an autopsy, not a red dot.
- **Brings your tools.** MCP servers over HTTP or stdio, REST APIs, RSS, web pages, webhooks. Credentials stay in an encrypted vault and never appear in a trace.
- **Yours to leave.** One SQLite file on your machine, your own model key, and a workspace export any time (secrets excluded).

**Models:** Anthropic (default), OpenAI and xAI. Pick a tier per process (economy, standard, premium), and change models in Settings without restarting.

## Where it fits

**Not an agent framework. Not a Zap.** Aether is built for work that repeats: watch something, notice a change, maybe think, maybe send.

| | How they work | What Aether does |
|---|---|---|
| **Agent frameworks** | A model decides what to do next, every step of every run. | Your AI writes the synth once. A deterministic engine runs it the same way every time and asks a model only when something changed. |
| **Zapier and n8n** | You draw the workflow yourself, step by step, and it runs what you drew. | You describe the job and your AI writes the synth. Every send waits for your Allow. |
| **Scheduled chat tasks** | A model is called on every run, whether anything changed or not. | A check that finds nothing makes no model call and costs nothing. The model is asked only when something changed. |
| **Cron and a script** | Your script runs on a timer. Memory, budgets, approvals and logs are yours to build. | Memory, a budget, a leash, a trace for every tick and a control room come built in. |

Already on n8n or Zapier? The courier blueprint hands approved drafts to your webhook.

### What Aether isn't

- **Not an agent loop.** No model decides what to do next at runtime. The synth does.
- **Not code generation at runtime.** Synths are data interpreted by one engine.
- **Not hosted.** It runs where you run it.
- **Not multi-user yet.** One operator per install.

## Blueprints

**Start from something that already works.** 9 blueprints ship with Aether and are validated in CI. Install one from the control room (**Blueprints → Shadow → Deploy**), or ask your AI to.

| Blueprint | What it does | Model calls |
|---|---|---|
| Price watcher | Alerts the moment a price on a page changes. | None |
| API health sentinel | Tells you once when a status changes, and again when it recovers. | None |
| GitHub release watcher | New release on a repo you depend on. | None |
| HN keyword watcher | New Hacker News stories that mention your word. | None |
| Page change watcher | Only thinks when the page actually changed. | Only on change, ≤ $0.05/day |
| Daily HN brief | Three lines on what mattered, once a day. | 1 a day: about $0.02 a month, capped at $0.30 |
| Keyword pulse analyst | Notices when chatter shifts, and says why. | Only on change, ≤ $0.02/day |
| Relay bridge | Team glue: forwards findings to a digest topic. | None |
| Outbound courier | Hands approved drafts to your n8n or Zapier webhook. | None |

## Security, plainly

**Local by default. Leashed by default. Honest about the rest.**

- Binds to `127.0.0.1` unless you say otherwise, and warns loudly if you do.
- Secrets are encrypted at rest (AES-256-GCM), resolved only inside the step that uses them, and never written to a trace.
- No telemetry. The only calls out go to your model provider and the places your processes are told to reach.
- **Aether is single-user and has no authentication yet.** Don't expose it to a network you don't trust.

Found a vulnerability? Email **support@aether.run** rather than opening a public issue. Reports are acknowledged within 3 business days.

## Pricing

**Free to run. Fair to use. Your model bill is yours.**

| Plan | Price | For |
|---|---|---|
| **Personal & evaluation** | Free | Running Aether for yourself, for non-commercial projects, or to evaluate it for your company. Every feature. |
| **Commercial** | License | Using Aether for work at a company. Ask at support@aether.run. |
| **Hosted** | Not available | Aether isn't run for you. If that changes, it'll be announced here. |

**What counts as commercial:** using Aether for a business or for money. For example, running it at a company (even for an internal tool, even at a small startup), running it for a client as a consultant or agency, or using it in a side project that earns revenue. **Not commercial:** personal projects that earn nothing, learning and teaching, and trying Aether at work to decide whether to buy a license.

There's no free tier for small companies: commercial use needs a license at any size. You bring your own model key and your provider bills you directly; Aether takes no cut. The full license terms will come with the first release.

## Community

- **Questions and ideas:** [Discussions](../../discussions)
- **Bugs:** [Issues](../../issues)
- **Chat:** [Discord](https://discord.gg/fMsYbjvvC9)

---

© 2026 Ross Walpole. All rights reserved.
