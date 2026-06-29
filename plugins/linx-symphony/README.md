# LinX Symphony

Portable LinX Symphony skill for system-evolution control-plane coordination.

`symphony` coordinates system-evolution work, worker delegation, delivery
review, and evidence feedback. It may consume captured Ideas, but Capture is a
separate `linx-capture` plugin.

The plugin is platform-neutral. Codex, Claude Code, and LinX package adapters
may consume it, but the Symphony skill source lives here.

Symphony can run without login in portable local mode. In LinX, use `/login` or
`linx login` when shared Pod state is needed; before login LinX may keep pending
local-first control records for later Pod replay. In Codex or Claude Code, use
`xpod auth login` for direct Pod writes; without xpod auth, keep local control
records/reports and do not claim they are cross-device Pod authority.
