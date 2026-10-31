# Manual first setup (local Git)

This route creates a private Core instance using Git and a text editor. The repository can also be opened directly as an Obsidian vault. Keep all instance files private.

## 1. Create the repository and standard layout

```sh
mkdir my-systemator-core
cd my-systemator-core
git init -b main
mkdir -p context/{inbox,facts,thoughts,decisions,intentions,tasks,references,assets,templates}
python -c "import uuid; print(uuid.uuid4())"
```

Keep the generated UUID as this instance's `instance_id`. Create a new UUID for every record.

Git does not preserve empty directories. Preserve the complete standard tree in the first commit:

```sh
touch context/{inbox,facts,thoughts,decisions,intentions,tasks,references,assets,templates}/.gitkeep
```

Remove a category's placeholder when its first real file is added. The names and meanings remain those in [the standard layout](storage-layout.md).

## 2. Choose and pin the engine

Choose the accepted stable tag `v1.0.0`, verify its exact full commit SHA and read `instructions/core.md`, `docs/format.md` and `docs/storage-layout.md` at that commit. Never use a moving branch or abbreviated SHA as the active pin.

## 3. Create instance files

Create root `README.md` with the owner, scope, canonical branch and authorized access route. Do not put credentials in it.

Create root `core.json`:

```json
{
  "format": "systemator-core-context",
  "format_version": "1.0",
  "profile": "Core",
  "instance_id": "<the UUID generated above>",
  "owner": "<stable owner identifier>",
  "scope": "personal",
  "engine": {
    "repository": "01Systemator/Systemator-core",
    "commit": "<full engine commit SHA>"
  }
}
```

Create `context/README.md` as a human navigation note. It is not a canonical record. Copy the record header from `docs/format.md` into `context/templates/record.md`; templates and inbox material are not canonical records.

## 4. Create the first real record

Choose one real fact, thought, decision, intention, task or reference. Put it under the matching standard directory, generate a fresh record UUID and complete every required metadata field. The record `instance_id` must equal `core.json.instance_id`. Use real UTC timestamps and accurate provenance.

For example, a decision belongs at `context/decisions/<readable-name>.md`. The filename is convenient for people; the UUID is the identity.

## 5. Commit and verify

```sh
git add README.md core.json context/
git commit -m "Initialize private Systemator Core instance"
git status --short
git show HEAD:core.json
git show HEAD:context/decisions/<readable-name>.md
```

Replace the example record path with the path you created. Confirm that `git status --short` is empty and both committed files contain the intended values. This proves local persistence, not remote backup or restore.

## 6. Open in Obsidian when desired

Open the repository root as an Obsidian vault. YAML metadata appears as note properties. Obsidian settings and plugins are optional and are not part of the portable Core format.

Do not convert UUID relations into editor-only links. You may add ordinary Markdown links for navigation while keeping canonical IDs in metadata.

## 7. Connect an AI interface

Configure the interface with the private repository, explicit `instance_branch`, exact engine commit and an authorized Git adapter. Ask it to read `README.md`, `core.json` and the first record from the explicit branch. Do not claim connection or persistence until those reads succeed.

Git is required by the Core contract. Local Git is sufficient; GitHub and other cloud providers are optional remotes. Core does not require registration, a database or an automatic updater.
