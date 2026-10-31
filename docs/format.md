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

The private instance must be a Git repository. The baseline is an owner-controlled local repository; the owner may select a cloud Git service as an optional backend or remote in private instance settings. The public Core contract does not mandate a hosting provider. Changing that private provider choice does not change card IDs or the data format. Context files without Git version control are not a supported Core storage mode. Git history preserves committed revisions during ordinary operation; repository backup and recovery remain the owner's responsibility. Core defines no database storage.

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

## Existing context and migration

Adopting Core does not require moving the existing `context/` directory or converting every file in it. Keep existing files and subdirectories in the private Git repository. Do not overwrite, rename, or treat an existing note as a canonical card simply because it is stored under `context/`.

Use the card format above for new canonical records. If an existing record is deliberately converted, preserve its meaning, provenance, and reliable stable ID. If it has no reliable ID, do not guess one or overwrite the source: create a new card with a fresh UUID and retain a reference to the source where useful. Convert records deliberately and verify each result by reading it back from the canonical `instance_branch`. Existing, unconverted files may remain alongside cards as source material; disclose when their format limits reliable search or interpretation.

Moving or renaming the private repository, changing its Git provider, or organizing paths does not create a new instance or new card identities. Preserve `instance_id`, card IDs, links, and owner data. Keep the canonical branch explicit in the interface entry. No data migration is implied by fetching a new engine commit or by adopting this documentation.

A minimal instance uses a private root `README.md`, one root `core.json`, and a private `context/` directory for owner records. A `context/README.md` may preserve an otherwise empty directory; it is a directory note, not a card. Owners may add subdirectories or other private files without changing the Core identity or card format.

## Complete card example

The following is illustrative content for `context/guide-before-video.md`, not a real owner's record. Replace both illustrative UUIDs, the timestamps, source and body when creating a real card. The link must use your actual instance ID or an existing relevant card ID.

```markdown
---
id: "72f317e6-7b9a-4b02-94b4-985424d3841c"
kind: "decision"
created: "2026-10-31T00:00:00Z"
updated: "2026-10-31T00:00:00Z"
source: "Owner's explicit decision in the current conversation"
links:
  - "170036d8-4964-4301-b17b-997d6c5a5fc1"
---

# Publish the guide before recording the video

For the current project, publish the short guide first. Record the video
afterwards, using the guide as its basis.

This is the owner's decision, not an assistant's proposal.
```

Use actual UTC creation/update timestamps in ISO-8601 form, not the example date. The filename is a convenience; the card UUID is its identity. `context/README.md` is a directory note, not a card: interfaces must not parse it as an owner record merely because it ends in `.md`.
