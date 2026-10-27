# EEC Pro agent skill — for EPLAN EEC Pro

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

An agent-agnostic [skill](https://agentskills.io) for **EPLAN EEC Pro** development:
- the mechatronic model and typicals (variants, conditions);
- the formula language;
- configuration interfaces;
- commands, Groovy actions and P8 post-generation scripts;
- the EOX model file;
- generation through the **Job Server**: recipes, triggers, REST, multi-worker queues and timing.

It goes beyond the manual. It records what happens when EEC Pro is automated end to end:
- commands that report "successful" and change nothing;
- generation sequences that come out empty when split into separate actions;
- EOX checksums;
- why a job's project is not on the share until the job ends;
- where generation time actually goes.

For **EPLAN Electric P8** scripting (C#, the EPLAN API, Remote Client), use the companion skill
[eplan-scripting-skill](https://github.com/Canary-ECAD/eplan-scripting-skill). EEC Pro's main output is a P8
project, so the two pair well, but each installs on its own.

---

## Install

### Any agent (skills CLI)

```bash
npx skills add Canary-ECAD/eec-pro-skill
```

### Manually

```bash
git clone https://github.com/Canary-ECAD/eec-pro-skill
```

```powershell
# Windows - personal skills, available in every project.
# ~/.agents/skills is the universal dir most agents read; use ~/.claude/skills for Claude Code.
xcopy /E /I eec-pro-skill\skills\eec-pro-development "$env:USERPROFILE\.agents\skills\eec-pro-development"
```

```bash
# macOS / Linux — universal dir. Use ~/.claude/skills for Claude Code.
cp -r eec-pro-skill/skills/eec-pro-development ~/.agents/skills/
```

For a single project, copy into `<your-project>/.agents/skills/` (or `.claude/skills/`) instead.

---

## The documentation index (MCP)

The skill tells the agent to resolve every formula function, command and parameter against the official
documentation instead of guessing. This repo ships a [`.mcp.json`](.mcp.json) with the remote EEC Pro doc
index from [eplan-cloudflare-rags](https://github.com/Canary-ECAD/eplan-cloudflare-rags). It is already
deployed, so no local data is needed:

| Server | Endpoint | Covers | Search |
|---|---|---|---|
| `eecpro-rag` | `rageecpro.covaga.xyz/mcp` | EEC Pro 2026 docs | Semantic, 36 categories |

Or register it by hand:

```bash
claude mcp add eecpro-rag --transport http https://rageecpro.covaga.xyz/mcp
```

The same index answers over REST: `POST https://rageecpro.covaga.xyz/search` with
`{"query", "topK", "category"}`.

---

## What's inside

```
eec-pro-skill/
├── .mcp.json                     # the EEC Pro doc-index MCP server
└── skills/
    └── eec-pro-development/
        ├── SKILL.md              # model → configure → generate pipeline, lookup rules, golden rules
        └── references/
            ├── doc-categories.md # the 36-category map of the doc index
            ├── job-server.md     # recipes, triggers, workers, file coordination, REST, timing
            ├── eox-format.md     # EOX structure, shared libraries, checksums, safe editing
            └── groovy-actions.md # Groovy actions, generation sequence, ScriptArguments, P8 scripts
```

`SKILL.md` is the entry point. The agent pulls in only the reference that matches the task, so the skill
costs little context until it is needed.

## Coverage

**EPLAN EEC Pro 2026** documentation, with field notes from EEC Pro 2025.3 Job Server workers:
- mechatronic model, typicals, variants and conditions;
- formula language, Form-UI and import formats;
- scripting and commands;
- Job Server;
- all generation targets: ECAD/P8, Pro Panel, PLC, Graph2D, text/Office and SAP.

## Pairs well with

- **[eplan-scripting-skill](https://github.com/Canary-ECAD/eplan-scripting-skill)** — the P8 side: C#
  scripting, the EPLAN API, Remote Client, inserting the generated macros.
- **[eplan-cloudflare-rags](https://github.com/Canary-ECAD/eplan-cloudflare-rags)** — the remote
  documentation indexes.
- **[eplan-mcp](https://github.com/Canary-ECAD/eplan-mcp)** — the local MCP server that lets the agent
  *execute* inside a running EPLAN.

## License

MIT — see [LICENSE](LICENSE).

> **Disclaimer:** this is an independent community project by [Canary E-cad](https://github.com/Canary-ECAD).
> It is not affiliated with, endorsed by, or supported by EPLAN GmbH & Co. KG. EPLAN and EPLAN EEC Pro are
> trademarks of their respective owner.
