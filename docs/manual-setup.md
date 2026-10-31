# Manual first setup (local Git)

This route is for an owner who can use Git and a text editor. It creates a private Core instance without an AI installer, database, or cloud Git account. Use a machine and repository location you control. Keep all instance files private.

## 1. Create the instance repository

In Git Bash, macOS, or Linux, choose a new empty directory and run:

```sh
mkdir my-systemator-core
cd my-systemator-core
git init -b main
python -c "import uuid; print(uuid.uuid4())"
```

Keep the generated UUID as this instance's `instance_id`. If Python is unavailable, use another standard UUID v4 generator. Do not reuse an example UUID.

## 2. Choose and pin the engine

Choose one exact full commit SHA from `01Systemator/Systemator-core`. A published stable release tag is the preferred source once one exists. Until the first stable release is published, `main` is a changing pilot branch: if you choose a pilot commit, record the full SHA and treat your instance as a pilot.

Read `instructions/core.md` and `docs/format.md` at that exact commit. Do not put a branch name or abbreviated SHA in the instance pin.

## 3. Create the instance files

Create a root `README.md` that names the owner, scope, canonical branch (`main` here), and how you will access this private repository. Do not put credentials or secrets in it.

Create a root `core.json` and replace every placeholder:

```json
{
  "format": "systemator-core-context",
  "format_version": "0.1-draft",
  "profile": "Core",
  "instance_id": "<the UUID generated in step 1>",
  "owner": "<your stable owner identifier>",
  "scope": "personal",
  "engine": {
    "repository": "01Systemator/Systemator-core",
    "commit": "<the full engine commit SHA chosen in step 2>"
  }
}
```

Create `context/README.md` with a short note such as: `Owner context cards live here; this file is not a context card.`

## 4. Add your first real record

Create `context/<descriptive-name>.md` for one fact, decision, intention, thought, or reference you actually want to preserve. Use a new UUID for the card, the real UTC timestamps, an accurate source, and the instance ID in `links`. Follow the card example in `docs/format.md` at your pinned engine commit. Do not put passwords, tokens, or other secrets in a card.

## 5. Commit and verify

Review the files to make sure the owner data is private and `core.json` contains the right UUID and full engine SHA. Then commit and read the committed copies back:

```sh
git add README.md core.json context/README.md context/<descriptive-name>.md
git commit -m "Initialize private Systemator Core instance"
git status --short
git show HEAD:core.json
git show HEAD:context/<descriptive-name>.md
```

Check that `git status --short` is empty and the output of both `git show` commands contains the intended values and record. This confirms the files are committed in this local repository; it does not prove that an AI interface is connected or that a backup can be restored.

## 6. Connect an AI interface when ready

To use an AI, configure its project or entry instructions with the private repository, canonical branch, exact engine commit, and the authorized Git adapter available in that environment. The entry must explicitly say what the AI may read and write. Use the copy-ready ChatGPT example in the main README as a starting point, adapting it to the actual interface and adapter. Confirm access by asking the interface to read `README.md`, `core.json`, and the first card from the explicit branch. Do not claim connection or persistence until the interface actually reads the files back.

Git is required by the Core contract. A local repository is sufficient. GitHub and other cloud Git services are optional remotes selected by the owner. Core does not require registration, a database, or an automatic updater.
