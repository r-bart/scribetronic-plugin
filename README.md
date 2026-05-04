# scribetronic-plugin

**Claude Code plugin marketplace for [scribetronic](https://github.com/r-bart/scribetronic).**

Ships 22 writing skills, workflow hooks, and the editorial pipeline for Claude Code. The companion `scribetronic` npm CLI handles project scaffolding (calendar, ideas, publish-config); this repo handles everything Claude Code loads at runtime.

---

## Install

In Claude Code:

```
/plugin marketplace add r-bart/scribetronic-plugin
/plugin install scribetronic@scribetronic
```

Or via the CLI (which registers the marketplace and scaffolds the editorial calendar in one step):

```bash
npx scribetronic init
```

After install, every skill is namespaced — invoke as `/scribetronic:write`, `/scribetronic:agenda`, etc. The bare `/write` form also works.

---

## What's inside

```
plugins/scribetronic/
├── .claude-plugin/plugin.json     # plugin metadata + version
├── skills/                        # 22 SKILL.md files
│   ├── agenda/                    # orchestrator: weekly cadence
│   ├── write/                     # orchestrator: full pipeline
│   ├── write-publish/             # orchestrator: publish + archive
│   ├── long-form-*/               # 6 long-form types
│   ├── short-form-*/              # 8 short-form types
│   └── (5 shared utilities)
└── hooks/hooks.json               # SessionStart + Stop reminders
```

### Skills

| Bucket | Count | Examples |
|---|---|---|
| Orchestrators | 3 | `agenda`, `write`, `write-publish` |
| Long-form | 6 | `long-form-weekly-newsletter`, `long-form-monthly-devlog`, `long-form-hot-take`, `long-form-how-to`, `long-form-launch-retro`, `long-form-manifesto` |
| Short-form | 8 | `short-form-x-vs-y`, `short-form-listicle`, `short-form-observation`, `short-form-motivational`, `short-form-present-vs-future`, `short-form-thread-from-longform`, `short-form-carousel-li`, `short-form-voice-adjustments` |
| Shared | 5 | `writing-style`, `editing-pass`, `ai-slop-check`, `style-extract`, `style-refine` |

Full reference: [scribetronic/docs/skills.md](https://github.com/r-bart/scribetronic/blob/main/docs/skills.md).

### Hooks

| Hook | Trigger | What it does |
|---|---|---|
| `SessionStart` (startup) | Claude Code session opens | If `scribetronic/calendar/<week>/plan.md` exists, prints today's resolved slot in 1–2 lines. Silent in non-scribetronic projects. |
| `Stop` | Claude Code session ends | If drafts under `scribetronic/calendar/*/drafts/` were touched, reminds about `/editing-pass` + `/ai-slop-check` before `status: ready`. |

Both hooks use the Haiku model and stay quiet when irrelevant — zero cost in non-scribetronic sessions.

---

## Versioning

This repo's `plugin.json:version` tracks the [scribetronic CLI](https://github.com/r-bart/scribetronic) lockstep — same `vX.Y.Z` tags. When a new CLI version ships, `scripts/sync-plugin-repo.sh` (in the CLI repo) copies the latest skill bundle here and bumps `plugin.json:version` to match.

## Cache and updates

Claude Code caches the marketplace clone under `~/.claude/plugins/cache/`. To force a refresh:

```
/plugin marketplace update scribetronic
/plugin update scribetronic
```

This pulls the latest commit on this repo's default branch.

## Contributing

Skill content (`SKILL.md` files) is **mirrored from the CLI repo on each release** — do not edit `plugins/scribetronic/skills/*` here directly. To propose a new skill or a change, open a PR against [r-bart/scribetronic](https://github.com/r-bart/scribetronic) at `packages/cli/templates/claude-code/.claude/skills/`. The next release will sync it across.

Direct edits welcome for: `marketplace.json`, `plugin.json`, `hooks/hooks.json`, this README.

## License

[MIT](./LICENSE) © 2026 Roberto Díaz
