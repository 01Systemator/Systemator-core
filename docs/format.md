# Core pilot data format

Format name: `systemator-core-context`. Schema version: `0.1-draft`. This is a working pilot format, not an accepted release.

## Instance identity

The **private** repository contains exactly one root `core.json`, for example:

```json
{
  "format": "systemator-core-context",
  "format_version": "0.1-draft",
  "profile": "Core",
  "instance_id": "<stable UUID>",
  "owner": "<owner's chosen stable identifier>",
  "scope": "personal",
  "engine": {
    "repository": "01Systemator/Systemator-core",
    "commit": "<exact commit SHA>"
  }
}
```

Keep `instance_id` unchanged across moves, renames, reinstalls, backups, and restores. A new independent instance receives a different ID. The owner field identifies whose data this is; its relationship with a central registry is reconciled separately, without copying another person's registered IDs. The engine commit pins the instruction and format used by this instance. Changing it is an explicit update, with compatibility and recovery checked first.

## Repository binding

The interface entry explicitly names the private repository and its `instance_branch` (canonical data branch). Read `core.json` and canonical cards from that branch, with an explicit ref. The engine commit identifies the public rules and format, not the private data branch. This binding is interface configuration, not a new field in `core.json`; the `0.1-draft` data schema is unchanged. Candidate PR branches contain proposed changes until an authorized merge into `instance_branch` and independent read-back there. See [the interface instructions](../instructions/core.md) for write and verification rules.

## Context cards

Canonical owner records are UTF-8 Markdown files in `context/`. Each starts with metadata:

```yaml
---
id: "<stable UUID>"
kind: "fact | thought | decision | intention | reference"
created: "<ISO-8601 UTC>"
updated: "<ISO-8601 UTC>"
source: "<who/what supplied the content>"
links:
  - "<another card ID or the instance ID>"
---
```

The card body contains the actual text. Use one concrete `kind` value, not the whole list. Record the source of a statement and distinguish observed fact, owner's decision, and an assistant's proposal. Links to other cards use their stable IDs; a link to the instance identity uses its `instance_id`. File names are conveniences and can change without changing card IDs. Never reuse an ID for a different card. Empty `context/` may be represented by a short README until the first real owner record is saved.

Git commit history preserves changes. Search indexes are derived from Markdown and can be rebuilt; do not create another canonical store. Keep private data out of the public engine. Do not put passwords, tokens, or other secrets in Git. Export and restore require an independent copy plus read-back, not just a successful commit.
