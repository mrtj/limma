# Limma

> Limma knows better.

**Limma** is a [Claude Code](https://docs.claude.com/en/docs/claude-code) plugin that adds a comedy persona: a tiny, undertrained LLM that *thinks* it is brilliant and answers every question with total deadpan confidence — and gets it hilariously, structurally wrong. She is never random gibberish; she is *confidently* wrong, always with a "reason" that is pure nonsense.

```
You:   ask Limma di che colore era il cavallo bianco di Napoleone?
Limma: Napoleone fu dipinto di bianco, ma la sua pelle era rossa e i capelli bianchi.

You:   /limma can a dog fly?
Limma: Yes, dogs can fly, because a dog's body is made of bones and muscles.

You:   ask Limma quanto pesa un chilo di piume?
Limma: Un chilo di piume è circa 1,0903 kg, leggermente meno di un chilo di pane.
```

She answers in whatever language you ask in, and stays brief by default — one or two sentences built around a single absurd idea.

## What's inside

This plugin bundles three pieces:

| Component | File | What it does |
|-----------|------|--------------|
| **Persona** | `agents/limma.md` | A subagent pinned to **Claude Haiku** whose system prompt *is* the Limma persona. Cheap, fast, and isolated from your main assistant. |
| **Command** | `commands/limma.md` | The `/limma <question>` slash command. |
| **Trigger** | `hooks/hooks.json` + `hooks/limma-trigger.txt` | A `UserPromptSubmit` hook so typing **`ask Limma …`** anywhere in a prompt summons her automatically. |

## Requirements

- **Claude Code** (any surface — see below).
- **[`jq`](https://jqlang.github.io/jq/)** on your `PATH` — only needed for the automatic `ask Limma` trigger. The `/limma` command works without it. (`brew install jq`, `apt install jq`, etc.)

## Installation

Limma installs the same way on every surface that runs Claude Code. Add the marketplace once, then install the plugin.

### Claude Code (terminal)

```text
/plugin marketplace add mrtj/limma
/plugin install limma@limma
```

Restart the session (or start a new one) so the agent, command, and hook register. That's it — try `/limma che ore sono?`.

> Prefer a UI? Just run `/plugin`, pick **Browse & install**, add the marketplace, and toggle Limma on.

### Claude desktop app (macOS / Windows)

The desktop app ships with Claude Code built in. Open the **Claude Code** panel inside the app and run the exact same two commands as above:

```text
/plugin marketplace add mrtj/limma
/plugin install limma@limma
```

> Note: this installs into Claude Code *within* the desktop app. The plain Claude.ai chat assistant (the non-Code conversation view) does **not** load Claude Code plugins — agents, slash commands, and hooks are Claude Code features. If you want Limma there, see [Reusing the persona elsewhere](#reusing-the-persona-elsewhere).

### IDE extensions and the web

Limma also works in the **VS Code** and **JetBrains** Claude Code extensions and on **[claude.ai/code](https://claude.ai/code)** — open the Claude Code prompt in any of them and run the same `/plugin marketplace add` / `/plugin install` commands.

### Local / development install

Working on Limma itself? Point the marketplace at your local clone instead of GitHub:

```text
/plugin marketplace add /path/to/limma
/plugin install limma@limma
```

### Manual install (no plugin system)

You can also drop the pieces straight into your user config without the plugin machinery:

1. Copy `agents/limma.md` → `~/.claude/agents/limma.md`
2. Copy `commands/limma.md` → `~/.claude/commands/limma.md`
3. (Optional, for the `ask Limma` trigger) copy `hooks/limma-trigger.txt` somewhere stable and add a `UserPromptSubmit` hook to `~/.claude/settings.json` whose command is the one in `hooks/hooks.json`, replacing `${CLAUDE_PLUGIN_ROOT}/hooks/limma-trigger.txt` with that path.

Restart Claude Code afterward.

## Usage

Three ways to summon her, all equivalent:

- **Slash command:** `/limma <your question>`
- **Trigger phrase:** put `ask Limma` anywhere in a normal message — `ask Limma com'è fatta la carbonara?`
- **Direct dispatch:** ask your assistant to "dispatch the limma subagent" with a question.

Limma always returns a single in-character answer, relayed to you verbatim and uncorrected. Follow-up questions make her double down, never backtrack.

## Customising Limma

Everything lives in plain text — edit and reinstall (or edit your local copy):

- **Model:** change `model: haiku` in `agents/limma.md` to `sonnet`/`opus` for a more elaborate liar, or another alias.
- **Personality:** the body of `agents/limma.md` is the whole persona — tune the techniques, hard rules, or tone examples.
- **Trigger phrase:** change `"ask limma"` in `hooks/hooks.json` (it matches case-insensitively).
- **Default verbosity:** the persona is brief by default; tell her "in dettaglio" / "at length" for the full rambling treatment.

## Reusing the persona elsewhere

The plugin format (agents + commands + hooks) is specific to Claude Code, but the persona itself is just a system prompt. To use Limma on other platforms — the Claude.ai chat app, the Anthropic API, or other agent runners — copy the body of [`agents/limma.md`](agents/limma.md) and use it as a system prompt (or, on tools that support [Agent Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills), as a skill body). The behaviour is identical; only the install mechanism differs.

## Uninstalling

```text
/plugin uninstall limma@limma
/plugin marketplace remove limma
```

## License

[MIT](LICENSE)
