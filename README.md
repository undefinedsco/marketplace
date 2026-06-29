# Undefineds Plugin Marketplace

Shared plugin marketplace for Undefineds tools.

This repository is the marketplace index. Plugin sources may live in other
Undefineds repositories; the current `solid-modeling` plugin is sourced from
`undefinedsco/models`.

## Install

Codex:

```bash
codex plugin marketplace add undefinedsco/marketplace --ref main
codex plugin add solid-modeling@undefineds
codex plugin add xpod-cli@undefineds
codex plugin add linx-capture@undefineds
codex plugin add linx-symphony@undefineds
```

Claude Code:

```bash
claude plugin marketplace add undefinedsco/marketplace
claude plugin install solid-modeling@undefineds
claude plugin install xpod-cli@undefineds
claude plugin install linx-capture@undefineds
claude plugin install linx-symphony@undefineds
```

## Plugins

- `solid-modeling` - shared Solid/RDF modeling guidance from
  `github.com/undefinedsco/models/plugins/solid-modeling`.
- `xpod-cli` - spec-aligned Xpod CLI guidance from
  `github.com/undefinedsco/xpod/plugins/xpod-cli`.

- `linx-capture` - portable Capture skill from this marketplace repository.
- `linx-symphony` - portable Symphony skill from this marketplace repository.
