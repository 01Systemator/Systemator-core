# Systemator Core

Systemator Core is a minimal, Git-backed space for an owner's persistent context. It stores addressable Markdown records and their links, lets the owner find them again, and preserves change history through Git. A ChatGPT project may be one interface to this space. The owner keeps their data in a separate repository.

This repository contains the shared instructions and data format for the **first Git-first pilot**. It contains no owner's records, credentials, or instance identity. A public repository alone is not an accepted release. Systemator Core is open source under the [MIT License](LICENSE). Independent acceptance and a versioned release remain pending.

## Connect an AI interface

**An entry instruction is required.** An AI model, agent, chat or other interface cannot infer which private instance belongs to you just from this public repository. Tell it where the shared Core rules are, where **your** data is, which Git adapter it may use, and what it may read and write. Without that binding, the interface is not connected to your Core instance. Keep the actual records in your private repository, not in the instruction text or chat history.

The entry instruction must name:

- this public engine repository and an exact commit of its `instructions/core.md` and `docs/format.md`;
- your private instance repository and its explicit `instance_branch` (the canonical data branch); `core.json` there must point to the same engine commit;
- the authorized way to read and commit Git files, with access to your private repository;
- the rule that personal records live only in that private repository's `context/`, with a current-blob check before editing and a read-back after committing.

### Example: ChatGPT Project with the connected GitHub plugin

Paste the following into the **Project instructions** after replacing the placeholders. This is an example for ChatGPT, not a requirement to use ChatGPT or GitHub in every Core installation.

> My Systemator Core instance is the private GitHub repository `<account>/<my-private-instance>`. Use the connected GitHub plugin for its files. The canonical data branch is `<instance-branch>` (`instance_branch`); supply that branch explicitly on every instance read and write. The public engine is `01Systemator/Systemator-core` at commit `<full-engine-commit-sha>`. Read `instructions/core.md` and `docs/format.md` from that exact engine commit, then read the private instance's `README.md` and `core.json` from `<instance-branch>`. Confirm that the instance is private and that `core.json` pins the same engine commit. Personal records live only in the private instance's `context/`. Fetch the current blob from `<instance-branch>` before changing a record; commit through the authorized Git adapter to that branch and independently read back the result from the same branch. If a pull request is required, follow the branch and PR rules below; report an unmerged change as a candidate, not as saved canonical data. If the private repository or plugin is unavailable, report that and do not claim to have read or saved the data. Do not use public Issues, project memory, or a separate database as personal Core storage.

A new chat must actually follow that instruction; pasting it does not prove access or persistence. In the first chat, ask the assistant to read `core.json` and an existing card and report their IDs without changing anything. Then save one real record and independently read it back in another chat. If your ChatGPT Project cannot access the private repository, correct its connection or permissions before relying on it.

### Instance branch and write verification

The entry instruction must explicitly name `instance_branch`, the canonical data branch in the private instance repository. `main` is a possible value, not a default to assume. Read the instance identity and canonical records from that branch; always pass its ref to the adapter. The public engine commit is a separate binding and must not be used as an instance branch.

For direct writes, fetch the current file from `instance_branch`, write to that branch using the current blob SHA (or an equivalent concurrency precondition), then independently read the file back from the same branch. Compare its ID and intended content and report the resulting commit and branch. A failed or mismatching read-back means the save is unverified; inspect the state before retrying.

If repository policy requires a PR, create the change in a separate authorized candidate branch based on current `instance_branch`. Fetch/check existing files and verify writes in that candidate branch. Read-back there proves only a saved candidate. Target the PR at `instance_branch`; only after an authorized merge and independent read-back from `instance_branch` may the interface report the change as canonical. A closed, unmerged PR does not save canonical data.

If the branch is missing, inaccessible, or disagrees with the private entry instructions, resolve the binding before writing. Do not silently create, switch to, or fall back to the repository's default branch. An intentional branch change requires an explicit authorized binding update.

### Other models, agents and local environments

Adapt the same four bindings to the environment's own instruction mechanism: system prompt, project rule, agent configuration or startup file. Replace the GitHub plugin with an authorized Git adapter and replace repository addresses with the appropriate Git locations. Keep the public engine version pinned, the owner data private, the instance ID stable, and the write/read-back checks intact. This pilot documents the contract; operation with other adapters and offline synchronization still needs separate verification.

## Start

1. Create a separate **private** Git repository for your instance. Never put personal records or instance-specific configuration into this public repository.
2. Put a `core.json` identity file and a `context/` directory in the private repository, following [the format](docs/format.md). Keep the instance ID when moving or restoring the repository.
3. In the ChatGPT project instructions, give the exact private repository, its explicit `instance_branch`, and the exact commit of this engine, then refer to [the Core instructions](instructions/core.md).
4. On the first visit, read the private `README.md` and `core.json` from `instance_branch`; read or write only relevant personal records in its `context/` directory on that branch. Verify committed changes by reading them back from that same branch.
5. Back up and restore the private Git repository independently. A single remote is not a verified backup.

GitHub is the first supported access route in this pilot. Git itself is the history and portable data format; local Git synchronization is future work. GitHub Issues and Projects are not the personal data store. The Core profile does not require a task manager, database, goals, plans, or agents.

The product profile contract and development coordination live in [Systemator Development](https://github.com/01Systemator/Systemator/blob/main/docs/systemator-core-contract.md). Work and Extended add capabilities separately. This repository is the shared Core source; a user's private repository is an instance of it.

## License

Copyright (c) 2026 ASystemator. The shared Core source in this repository, including its instructions and data-format documentation, is available under the [MIT License](LICENSE) (SPDX: `MIT`).

You may use, copy, modify, and redistribute it, including commercially and as part of proprietary products, provided you retain the copyright and license notices as required by MIT. The software is provided without warranty; see the full license for its terms.

This license covers this repository's shared materials. It does not grant rights to an owner's private context or instance data. Work and Extended are separate products; their licensing is defined separately.
