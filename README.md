# Superpowers for Mistral Vibe

Native Vibe plugin port of [obra/superpowers](https://github.com/obra/superpowers)
(v7.0.0, MIT). Superpowers is a complete software development methodology for
coding agents: brainstorming, writing and executing plans, test-driven
development, systematic debugging, code review, git worktrees, and
subagent-driven development.

> Unofficial port by Sascha Lichtenstein. Not affiliated with Jesse Vincent /
> obra — all credit for the skill content belongs to the original project.

## Contents

- `skills/` — all 15 upstream skills, unchanged except for the Vibe-specific
  adaptations listed below. Runtime names are `superpowers:<skill-name>`.
- `ai.mistral.vibe/agents/` — two subagent profiles used by the dispatch
  templates: `superpowers:implementer` and `superpowers:reviewer`. Pin cheaper
  or more capable models per role via `activeModel` in those TOML files.

## Vibe adaptations

This is a Vibe-only port: references to other harnesses (Claude Code, Codex,
Gemini, Copilot, and their tool notes) were removed. In detail:

1. `using-superpowers` reads `references/vibe-tools.md` (the only platform
   reference kept). It maps the skills' generic vocabulary onto Vibe's tools
   (skill tool, todo tool, subagent namespace, `vibe --worktree`, session
   logs under `~/.vibe/logs/session/`).
2. `writing-skills` points personal skills at `~/.vibe/skills/`.
3. `using-git-worktrees` names Vibe's native worktree options.
4. `requesting-code-review` dispatches via the subagent namespace.
5. `brainstorming/visual-companion.md` launches the companion server the
   Vibe way.
6. Files that only made sense on other harnesses were dropped:
   `writing-skills/anthropic-best-practices.md` (vendored Anthropic docs),
   `writing-skills/examples/CLAUDE_MD_TESTING.md` (Claude-specific worked
   example), `systematic-debugging/CREATION-LOG.md`, and the per-harness
   tool notes under `using-superpowers/references/`.

## Bootstrap difference

Upstream injects the `using-superpowers` skill at session start via a
SessionStart hook. Vibe has no session-start injection hook, so the bootstrap
lives as a section in `~/.vibe/AGENTS.md` instead: it requires invoking the
`superpowers:using-superpowers` skill before any response. If you remove this
plugin, remove that section as well.

## Installing from this repository

Clone into your Vibe plugins directory:

```bash
git clone https://github.com/saschalichtenstein/superpowers-vibe.git ~/.vibe/plugins/superpowers
```

Then add the bootstrap section to `~/.vibe/AGENTS.md`:

```markdown
## Superpowers (Vibe-Plugin)

<EXTREMELY_IMPORTANT>
You have superpowers. At the start of every conversation, before ANY response
or action — including clarifying questions, exploring the codebase, or
checking files — invoke the skill `superpowers:using-superpowers` and follow
it exactly. If there is even a 1% chance a skill might apply to what you are
doing, you absolutely must invoke the skill first.
</EXTREMELY_IMPORT>

User instructions (AGENTS.md, direct requests) take precedence over skills.
Only skip skill workflows when your human partner explicitly says so.
When the `superpowers` plugin is removed, delete this section too.
```

Restart Vibe or run `/reload-plugins`.

## Updating from upstream

Copy `skills/` from a fresh clone of obra/superpowers, then re-apply the
adaptations above: keep only `references/vibe-tools.md` under
`using-superpowers/references/`, delete `writing-skills/anthropic-best-practices.md`,
`writing-skills/examples/CLAUDE_MD_TESTING.md`, and
`systematic-debugging/CREATION-LOG.md`, and search for `vibe` in `skills/`
to find the adapted spots.

## License

MIT. This port is derived from obra/superpowers by Jesse Vincent, MIT-licensed
(see `LICENSE`). Skill content is taken from that project; the Vibe-specific
adaptations are part of this port.
