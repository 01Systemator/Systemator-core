# Standard private instance layout

Systemator Core uses one portable private-repository layout for every owner. The layout is both a plain Git repository and a usable Obsidian-style Markdown vault. Paths help people browse the data; stable UUIDs in record metadata remain the canonical identities.

## Required tree

A new instance creates this tree:

```text
README.md
core.json
context/
  README.md
  inbox/
  facts/
  thoughts/
  decisions/
  intentions/
  tasks/
  references/
  assets/
  templates/
```

Git does not preserve empty directories. A setup may place an empty `.gitkeep` in a category until the first file is added. These placeholder files are not records.

The directories have fixed meanings:

| Path | Meaning | Imported as a canonical record |
|---|---|---|
| `context/facts/` | Observed or supplied facts | Yes |
| `context/thoughts/` | Reflections, hypotheses and ideas | Yes |
| `context/decisions/` | Owner decisions | Yes |
| `context/intentions/` | Intended outcomes or commitments | Yes |
| `context/tasks/` | Concrete work items captured by Core | Yes |
| `context/references/` | References to external or internal material | Yes |
| `context/inbox/` | Unclassified source material awaiting deliberate classification | No |
| `context/assets/` | Attachments referenced by records | No |
| `context/templates/` | Copyable record templates | No |

`context/README.md` is the human entry and directory note. It is not a record. Every canonical Markdown file below the six record directories must follow [the Core record format](format.md). An interface must scan those directories recursively.

A task record is a portable context record, not a full task manager. Work may later import it into a managed work cycle. Core does not require a database, scheduler or automatic execution.

## Placement rules

- Put a canonical record under the directory matching its `kind`.
- Subdirectories inside a record directory are allowed for human navigation, for example `context/decisions/project-alpha/`.
- The front-matter `kind`, not the path, is authoritative. A mismatch is a validation error to correct deliberately.
- Moving or renaming a file never changes its record `id`.
- Use readable lowercase file names with hyphens. File names are not database keys.
- Keep unclassified imports in `context/inbox/`; do not silently treat them as canonical records.
- Keep attachments in `context/assets/` and refer to them with relative Markdown links. Never store credentials or secrets there.
- Additional owner-specific directories may exist, but they are outside the Core standard. Importers must ignore them unless a separately accepted extension contract says otherwise.

## Obsidian and Markdown tools

The private repository root may be opened directly as an Obsidian vault. Core records use flat YAML properties supported by Obsidian and ordinary Markdown bodies. The standard does not require Obsidian, its proprietary settings, community plugins or wikilinks.

For portability, canonical identity and record relationships are carried by UUID metadata. Human-facing Markdown links may be added in the body, but an Obsidian-only link is not a substitute for the metadata relation.

Editor-specific files such as `.obsidian/` are private local settings. They are not part of the portable Core data contract and must not be required by an importer.

## Database-ready boundary

The layout prepares records for deterministic import without making a database part of Core:

1. Discover canonical files only in the six record directories.
2. Parse and validate the flat YAML metadata.
3. Preserve `instance_id`, record `id`, timestamps, provenance, tags, aliases, links and Markdown body.
4. Map `instance_id` to an authorized Work tenant explicitly. `instance_id` is not a `tenant_id`.
5. Use `instance_id + id` as the stable import identity so that repeating an import does not create duplicates.
6. Resolve links only after their target records have been inventoried.
7. Report invalid, duplicate or unresolved records; never silently invent identities or discard source data.

Work owns its SQL schema and import implementation. It may store different record kinds in different tables or in a shared object model, but it must preserve the Core UUID and provenance or maintain an explicit unique mapping to them. Core does not promise that copying files directly into a database is sufficient.

## Existing data

Adopting this layout must not erase existing files. Inventory the current `context/`, validate each intended canonical record, move it to the matching standard directory, preserve its UUID and meaning, and read it back after commit. Material that cannot yet be classified remains in `context/inbox/` with its source intact.

Engine updates do not move private data automatically. A layout migration is a separate declared operation with backup, validation and read-back.
