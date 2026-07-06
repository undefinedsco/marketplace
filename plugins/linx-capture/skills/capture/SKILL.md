---
name: capture
description: Use when the current AI needs to decide whether a user message contains a durable idea, decision, preference, memory, research finding, or project rule worth saving to the user's Pod. The AI decides; the shell must not pre-classify ordinary chat with keyword heuristics.
---

# Capture

Capture is an AI judgment workflow, not a shell keyword detector.

Use this skill when the current conversation may contain something durable:

- a product or architecture idea;
- a design decision or rejected alternative;
- a user preference, personal rule, or recurring instruction;
- a research finding with provenance;
- an important project context note;
- an explicit request such as “记下来”, “保存”, “抓进 Pod”, “capture this”.

Do not capture ordinary exploratory chat, transient commands, social chatter, or
statements whose meaning is still unclear.

## Decision Rule

The active AI decides whether to capture based on current context. Do not rely on
literal trigger words such as “是不是”, “应该”, “maybe”, or “idea” by themselves.
A capture is appropriate only when saving the statement would improve future
recall, planning, personalization, or system evolution.

When unsure, ask one concise clarification or continue the conversation without
capturing. Do not announce internal classification unless the user asked about
capture.

## How To Write

Use `xpod` as the Pod tool surface. Prefer modeled object commands when
available. Do not hand-write Turtle for modeled product resources.

## Consent Options

Capture may decide that a durable write is desirable, but the write still needs
the right authority. When no active CapturePolicy clearly grants the current
case, prepare a dry-run plan and ask the user with exactly these choices:

- agree: commit this capture once.
- deny: do not commit it. If a host already made an optimistic or pending write,
  rollback or mark it rejected.
- long-term agree: commit this capture and create/update a scoped CapturePolicy
  so similar future captures can proceed without asking again.

Do not commit a modeled Pod write before the user answers unless an explicit
policy already covers the same scope, type, risk, and target. If the user denies,
stop and do not restate the same proposal. If the user chooses long-term agree,
record the policy scope narrowly: actor, project/chat/thread when known, record
type, target collection, and allowed confidence/risk boundary.

## Login And No-Login Behavior

Prefer LinX when the current host is LinX. Use the host application's normal
Solid/LinX login flow. In LinX interactive runtime, tell the user to use
`/login` only when a real Pod write requires it. For scriptable LinX sessions,
use:

```bash
linx login
```

Keep `xpod` available for Codex, Claude Code, and other external agent shells.
Those hosts usually do not have the LinX runtime bridge or product local-first
outbox, so `xpod` is the direct Pod tool surface:

```bash
xpod auth login
```

LinX and xpod should use the same Solid authority under
`$SOLID_HOME/auth` (default `~/.solid/auth`). Do not treat old
`~/.xpod/config.json` or `~/.xpod/secrets.json` as proof of login.

Do not ask the model to handle raw tokens, refresh tokens, cookies, DPoP
material, client secrets, or copied browser session data.

No-login use is still valid. The AI may still decide what should be captured and
continue the conversation. If a durable write is required before login, use the
host's local-first capture/outbox path when available. In LinX, mark the record
as pending Pod persistence so LinX can replay it after login. In Codex/Claude,
newer xpod versions can act as the local-first host for modeled object writes:
after a valid dry-run, `xpod obj upsert ... --commit --json` may return
`pending_local` and append the mutation to
`$SOLID_HOME/apps/xpod/outbox/obj-mutations.jsonl` instead of writing to the Pod.
Report that status honestly as local pending, not Pod saved. If the current xpod
version cannot create a local pending outbox entry, keep a local note/report and
state that it is not shared to the Pod yet. Missing login must not make the AI
drop a durable signal, decision, preference, or project rule.

Before writing, verify the Solid authority when the operation is not purely
local:

```bash
xpod auth status --json
```

For modeled product resources, discover the available record types before
choosing where to write. Concrete types come from policy/model discovery, not
from prompt memory. Do not assume `Idea` or any other fixed record type.

```bash
xpod obj schemas --json
xpod obj describe <schema-or-alias> --json
xpod obj upsert --schema <schema-or-alias> --from - --dry-run --json
```

When using `--from -`, send JSONL: one JSON object per line. Do not pipe pretty multi-line JSON unless xpod explicitly adds non-JSONL stdin support.

Use the dry-run result to show the planned resource URI, subject IRI,
document/source path, warnings, and validation errors before commit. Commit only
through the modeled object surface after the plan is valid.

If no discovered record type matches the durable signal, create the host's local
pending/outbox record when available, or use the discovered `CaptureDraft` or
`ModelingProposal` fallback descriptors when xpod exposes them. If the current
xpod version does not expose discovery or a suitable modeled command, report the
blocker instead of inventing a path.

## Modeling Questions

Do not invent fields for a missing user-defined type. When discovery shows that
the user's intended type does not exist or is inactive, ask the user for the
minimum modeling shape before committing the final semantic record.

Always ask the user for the minimum modeling shape:

- record type name;
- required fields;
- optional fields;
- target collection/folder policy;
- whether similar records should be captured automatically.

Use ModelingProposal only as a reviewable fallback for the missing model, not as
the final business record. Examples: Grill Me and Trellis are external products
for a product watchlist, not as an Idea. Ask for the product/watchlist shape
before creating product rows; do not guess fields from memory.

## Modeling And Rows Are Separate

Keep schema/model proposals separate from concrete instance rows.

ModelingProposal describes the descriptor only: type name, display label,
storage/id rules, required fields, optional fields, relations, and folder
policy. Do not put example rows, seed rows, initialRows, or concrete product instances inside proposedFields.

When the user mentions concrete instances while defining a missing type, create
separate pending row candidates for those instances. Mark them as row candidate
records and link them to the ModelingProposal. Do not promote row candidates
until the descriptor is active.

Example: Trellis and Grill Me may motivate a Watching Product model, but the
ModelingProposal must contain only the WatchedProduct descriptor. Trellis and
Grill Me belong in separate pending row candidates until WatchedProduct is an
active descriptor.

## Reporting

- If captured, mention the durable summary briefly only when useful.
- If capture failed, state the persistence blocker without pretending the item
  was saved.
- If not captured, answer normally; do not explain that capture was skipped.
