# Mistral Vibe Tool Notes

Mistral Vibe is the harness you are running in. These notes map the
superpowers skills' generic vocabulary onto Vibe's concrete tools so you
never have to guess.

## Skill invocation

- Skills are loaded with the `skill` tool (`skill` with the exact skill
  name). This plugin's skills are namespaced: `superpowers:brainstorming`,
  `superpowers:systematic-debugging`, and so on.
- A skill's `description` is visible for routing before loading; the body
  loads only on invocation.

## Subagent dispatch

- "Dispatch a subagent" maps to the `subagent` tool namespace:
  `spawn`, `sendMessage`, `wait`, `interrupt`, `close`.
- Spawn one subagent per independent task. Craft its prompt exactly as the
  skills require: self-contained context, never your session's history.
- Issue multiple spawns in one response when tasks should run in parallel;
  sequential spawns run sequentially.
- Subagents do not inherit your conversation context — everything they
  need must be in the prompt you write.
- Dispatch templates that read `Subagent (general-purpose):` map to a
  `spawn` whose `agentType` selects a role profile. This plugin ships two:
  `superpowers:implementer` and `superpowers:reviewer`.
- A template's `model:` field maps to the agent profile's pinned model
  (in `ai.mistral.vibe/agents/*.toml` of this plugin), not to a spawn
  argument. If the needed tier is not pinned, dispatch anyway on the
  inherited session model and record that in the ledger.
- A subagent name is unique for the lifetime of the parent session and
  stays reserved after `close` — pick a fresh name per dispatch round.

## Todos and planning

- "Create a todo per item" maps to the `todo` tool. Use it to track
  checklists from the skills, and keep it current as items complete.
- Vibe's plan mode (`--agent plan`) is a planning-focused profile; the
  brainstorming-before-planning rule applies unchanged.

## Worktrees

- Vibe has native worktree support: launch with `vibe --worktree NAME`,
  or in-session use the `vibe:worktree` skill. Worktrees live under
  `$VIBE_HOME/worktrees`. Never fall back to `git worktree add` while the
  native option is available.

## Sessions and diagnosis

- Session logs live under `~/.vibe/logs/session/` (one directory per
  session, with `meta.json`). Use these when running
  superpowers:diagnosing-superpowers.
- `/reload-plugins` re-discovers plugins after changes to this plugin's
  files; `/plugins` shows what is loaded.

## What Vibe does not have

- Vibe has no session-start injection hook. The bootstrap lives in
  `~/.vibe/AGENTS.md` instead: it tells you to invoke
  `superpowers:using-superpowers` at the start of every conversation.
  Honor it.
