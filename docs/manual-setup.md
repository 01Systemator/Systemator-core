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

Choose the accepted stable tag `v1.0.0` from `01Systemator/Systemator-core` and verify its exact full commit SHA. Pin that full SHA in the instance. The `main` branch remains a changing source branch and must not be used as an active stable pin.

Read `instructions/core.md` and `docs/format.md` at that exact commit. Do not put a branch name or abbreviated SHA in the instance pin.

## 3. Create the instance files

Create a root `README.md` that names the owner, scope, canonical branch (`main` here), and how you will access this private repository. Do not put credentials or secrets in it.

Create a root `core.json` and replace every placeholder:

```json
{
  "format": "systemator-core-context",
  "format_version": "<copy the supported version from docs/format.md at the pinned engine commit>",
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

Create `context/first-record.md` for one fact, decision, intention, thought, or reference you actually want to preserve. Generate a second, fresh UUID for this card (for example, run the Python command from step 1 again); it must differ from the instance ID. Use the real UTC timestamps, an accurate source, and the instance ID in `links`. Follow the card example in `docs/format.md` at your pinned engine commit, including the exact format fields and version it defines. Do not put passwords, tokens, or other secrets in a card. The filename is literal in the commands below; if you choose another name, replace it there too.

## 5. Commit and verify

Review the files to make sure the owner data is private and `core.json` contains the right UUID and full engine SHA. Then commit and read the committed copies back:

```sh
git add README.md core.json context/README.md context/first-record.md
git commit -m "Initialize private Systemator Core instance"
git status --short
git show HEAD:core.json
git show HEAD:context/first-record.md
```

If Git reports that your author identity is not configured, set a repository-local identity and retry the commit, for example `git config user.name "Your Name"` and `git config user.email "you@example.com"`. These identify the commit author; they are not repository credentials. Check that `git status --short` is empty and the output of both `git show` commands contains the intended values and record. This confirms the files are committed in this local repository; it does not prove that an AI interface is connected or that a backup can be restored.

## 6. Connect an AI interface when ready

To use an AI, configure its project or entry instructions with the private repository, canonical branch, exact engine commit, and the authorized Git adapter available in that environment. The entry must explicitly say what the AI may read and write. Use the copy-ready ChatGPT example in the main README as a starting point, adapting it to the actual interface and adapter. Confirm access by asking the interface to read `README.md`, `core.json`, and the first card from the explicit branch. Do not claim connection or persistence until the interface actually reads the files back.

Git is required by the Core contract. A local repository is sufficient. GitHub and other cloud Git services are optional remotes selected by the owner. Core does not require registration, a database, or an automatic updater.
