# Lessons from Herdr, Orca and T3 Code

What users and reviewers complain about in similar apps, and what Tay does about it. Gathered in
November 2026 from the Hacker News thread on Herdr, independent reviews, and a Reddit comment
quoted in one of them. Reddit itself couldn't be searched directly, so this isn't a Reddit
survey. Re-check before relying on any point; all three apps ship fast.

## Herdr

Terminal-first agent multiplexer ("tmux rebuilt around agents"), with a native desktop app since
October 2026 and a background server, herdr-core.

- **Agent status can be wrong.** It marks a pane as blocked only when it recognises that agent's
  approval prompt by reading the screen, so unfamiliar prompts show as idle. Codex detection
  regressed in v0.9.5.
- **Hard to see what an agent is doing.** Users want tool calls and where an agent is stuck, not
  just a needs-attention indicator.
- **Keybindings feel scattered** ("all over the place"), likely from avoiding clashes with tmux.
- **Awkward next to tmux.** tmux users have to switch terminal tabs to reach their agents, and
  several asked why tmux isn't enough.
- **Missing tmux features:** no predefined layouts (like tmuxp), a weaker copy mode, no quick
  pane picker, few plugins.
- **Young:** pre-1.0, several releases a week with occasional breaking changes, less polished
  on Windows.
- **Some found it too complex**, and a few wanted a proper GUI instead of a terminal app.

## Orca

Electron-style "agent development environment" from Stably (YC-backed, MIT). Parallel agents,
each in its own git worktree.

- **Heavy:** about a 250 MB installer and 400–800 MB RAM at idle with a few agents running.
- **Parallel agents multiply quota use.** Three Claude Code agents use about three times the
  limit, which surprises new users.
- **Bugs from shipping daily.** Reviewers advise pinning a version for teams. Over 4,000 open
  issues at the time of the review.
- **Remote machines need Orca's own agent installed**, so plain SSH servers and locked-down work
  machines are a problem.
- **The mobile app depends on the desktop** being awake.
- **Built for parallel work.** One-agent-at-a-time users may be better served by a normal
  editor, and reviewing many diffs becomes the bottleneck.

## T3 Code

From Theo and the T3 team (MIT, "very very early"). Node server, React UI as a web app or
Electron desktop app, mobile apps, pairing over Tailscale or T3 Connect.

- **Early and rough.** An alpha review found `~` paths broken, missing file diffs and no
  security on the web server mode. Most outside contributions aren't accepted yet.
- **"A design layer over Claude Code"**: more taste and workflow than new capability.
- **Claude limits drain faster** through the Agent SDK than through Claude Code directly (T3
  says this is an Anthropic-side bug; unconfirmed).
- **Loose security defaults.** New threads start with full access, pairing tokens get every
  permission, and the MCP server lets a prompt-injected agent start other agents.
- **Permission modes differ per agent**, so "Auto" doesn't mean the same thing everywhere.
- **Worktrees use a lot of disk.** One user reportedly cleared 500 GB.
- **Other:** usage telemetry on by default, no local model support, the host must stay on for
  remote use.

## What Tay does about it

Each point is also tracked in `docs/ROADMAP.md`.

1. **Status from structured events, never screen-scraping.** Use ACP's typed events (tool calls,
   permission requests, turn end) to set working / needs you / done. Herdr's biggest complaint.
   *(M6)*
2. **Show what the agent is doing.** Tool calls as cards and the current step in the sidebar
   line, not just a dot. *(M6, M8)*
3. **Safe permission defaults that mean the same thing for every agent.** New chats start in
   Ask. Tay maps each mode to ACP permission handling itself, rather than trusting each agent's
   own idea of "auto". *(M6)*
4. **Agents driving Tay are limited too.** `tay mcp` can't start new agents or change permission
   modes unless you allow it. *(M6)*
5. **Stay light, and measure it.** Native Rust is an advantage over Electron apps. Track idle
   memory and startup time from the first window onwards. *(M3 onwards)*
6. **One consistent, Vim-like keymap** designed up front as a single table, not added piecemeal.
   *(M3, M8)*
7. **Saved and predefined layouts**, so a chat can open straight into a named arrangement.
   *(M8)*
8. **Worktree housekeeping.** Show each worktree's disk use and offer to remove it when a chat
   is finished or merged. *(M8)*
9. **Make quota use visible.** Show context and usage per chat so running several agents isn't
   a surprise. *(M6)*
10. **Secure remote access.** Trust Tailscale identity and give paired clients only the
    permissions they need. No all-powerful pairing tokens. *(Later)*
11. **No telemetry.** Nothing leaves the machine except what the agents themselves send.
12. **Plain SSH should be enough for remote environments**, without installing a Tay agent on
    the server, if that turns out to be practical. *(Later)*
