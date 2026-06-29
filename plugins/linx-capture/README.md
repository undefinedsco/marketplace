# LinX Capture

Portable Capture skill for deciding whether conversation facts should become durable user-owned context.

Use it for ideas, preferences, decisions, findings, project rules, and explicit save requests. It is independent from Symphony; Symphony may use Capture, but does not own it.

Capture can run without login. In LinX, use `/login` or `linx login` when Pod
persistence is needed; before login LinX may keep pending local-first capture
records for later Pod replay. In Codex or Claude Code, use `xpod auth login`
for direct Pod writes; without xpod auth, keep a local note/report and do not
claim the item was saved to the Pod.
