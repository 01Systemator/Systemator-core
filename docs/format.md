# Systemator Core data format

Format name: `systemator-core-context`. Schema version: `1.0`. This is the accepted data format for Systemator Core v1.0.0.

## Instance identity

The **private** repository contains exactly one root `core.json`:

```json
{
  "format": "systemator-core-context",
  "format_version": "1.0",
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

Keep `instance_id` unchanged across moves, renames, reinstalls, backups and restores. A new independent instance receives a different ID. The owner field identifies whose data this is; its relationship with a shared registry or a Work tenant is reconciled separately. Never copy another owner's IDs. The engine commit pins the rules and format used by this instance.

## Repository binding and layout

The interface entry explicitly names the private repository and its `instance_branch` (canonical data branch). Read `core.json` and canonical records from that branch with an explicit ref. Candidate PR branches contain proposed changes until an authorized merge and independent read-back.

Every instance follows [the standard private layout](storage-layout.md). The repository is usable as a plain Markdown/Obsidian vault, while Git preserves committed history. Local Git is sufficient; a cloud Git provider is optional. A directory without Git is not a supported Core instance.

## Canonical records

Canonical records are UTF-8 Markdown files stored recursively in the standard record directories under `context/`. Each record begins with flat YAML metadata:

```yaml
---
record_format: "systemator-core-record"
record_format_version: "1.0"
id: "<stable record UUID>"
instance_id: "<the instance UUID from core.json>"
kind: "fact"
title: "<short human-readable title>"
status: "active"
created_at: "<ISO-8601 UTC>"
updated_at: "<ISO-8601 UTC>"
source_type: "owner_statement"
source_actor: "<person, interface or source>"
source_ref: "<stable source reference or null>"
tags:
  - "<tag>"
aliases: []
links:
  - "<another record UUID>"
---
```

The Markdown body contains the record's substantive content. Metadata values are atomic and flat so common Markdown tools, including Obsidian, can read them without nested-property support.

### Required fields

| Field | Rule |
|---|---|
| `record_format` | Exactly `systemator-core-record` |
| `record_format_version` | `1.0` for this release |
| `id` | Fresh stable UUID; never reused for another record |
| `instance_id` | Must equal root `core.json.instance_id` |
| `kind` | One of `fact`, `thought`, `decision`, `intention`, `task`, `reference` |
| `title` | Short human-readable title, also used as the first H1 heading |
| `status` | A valid status for the selected kind |
| `created_at` | Original UTC creation time |
| `updated_at` | UTC time of the current substantive revision |
| `source_type` | How the content entered Core |
| `source_actor` | Who or what supplied it |
| `source_ref` | Stable reference when available; use `null` when none exists |
| `tags` | YAML list; may be empty |
| `aliases` | YAML list; may be empty |
| `links` | YAML list of record UUIDs; link to the instance itself is unnecessary because `instance_id` is explicit |

Use one concrete `kind`, not a list. The record directory must match that kind. A title, tag, alias or file path never replaces the UUID.

### Status values

For `fact`, `thought`, `decision` and `reference`, use `active`, `superseded` or `archived`.

For `intention` and `task`, use `open`, `in_progress`, `waiting`, `done`, `cancelled` or `archived`.

Core captures tasks as context records. Work may later add priority, assignment, dependencies, acceptance criteria, execution and evidence without changing the original Core record identity.

### Provenance and links

Use a concise controlled `source_type`, such as `owner_statement`, `conversation`, `document`, `import`, `observation` or `external_reference`. Preserve enough information in `source_actor` and `source_ref` to distinguish observed facts, owner decisions and assistant proposals.

`links` contains stable UUIDs from the same authorized instance. Missing targets, duplicate IDs and cross-instance IDs are inconsistencies to report. Ordinary Markdown links may supplement UUID relations for human navigation.

## Complete record example

Illustrative path: `context/decisions/publish-guide-before-video.md`.

```markdown
---
record_format: "systemator-core-record"
record_format_version: "1.0"
id: "72f317e6-7b9a-4b02-94b4-985424d3841c"
instance_id: "170036d8-4964-4301-b17b-997d6c5a5fc1"
kind: "decision"
title: "Publish the guide before recording the video"
status: "active"
created_at: "2026-10-31T00:00:00Z"
updated_at: "2026-10-31T00:00:00Z"
source_type: "owner_statement"
source_actor: "Owner"
source_ref: "conversation:2026-10-31"
tags:
  - "publishing"
aliases: []
links: []
---

# Publish the guide before recording the video

For the current project, publish the short guide first. Record the video afterwards, using the guide as its basis.

This is the owner's decision, not an assistant proposal.
```

Replace every illustrative UUID, timestamp and source value. Use actual UTC timestamps.

## Change, import and migration rules

Git commit history preserves revisions. Keep the same record `id` when correcting or moving a record; update `updated_at` for substantive changes. Do not rewrite provenance to make a proposal look like an owner decision.

Search indexes, Obsidian metadata caches and database projections are derived and rebuildable. They are not additional canonical Core stores.

A Work importer follows [the database-ready boundary](storage-layout.md#database-ready-boundary). It must preserve Core identity and provenance, explicitly map the instance to an authorized tenant, remain idempotent, and report conflicts. Core v1.0.0 defines the portable source contract, not a Work SQL schema or an automatic migration service.

Keep private data out of the public engine. Do not put passwords, tokens or other secrets in Git. Export and restore require an independent copy plus read-back, not merely a successful commit.
