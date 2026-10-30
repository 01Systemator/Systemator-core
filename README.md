# Systemator Core

Systemator Core is a minimal, Git-backed space for an owner's persistent context. It stores addressable Markdown records and their links, lets the owner find them again, and preserves change history through Git. A ChatGPT project may be one interface to this space. The owner keeps their data in a separate repository.

This repository contains the shared instructions and data format for the **first Git-first pilot**. It contains no owner's records, credentials, or instance identity. A public repository alone is not an accepted release. The open-source license, independent acceptance, and versioned release are pending in Systemator Development tasks #10, #16, and #17.

## Connect an AI interface

**An entry instruction is required.** An AI model, agent, chat or other interface cannot infer which private instance belongs to you just from this public repository. Tell it where the shared Core rules are, where **your** data is, which Git adapter it may use, and what it may read and write. Without that binding, the interface is not connected to your Core instance. Keep the actual records in your private repository, not in the instruction text or chat history.

The entry instruction must name:

- this public engine repository and an exact commit of its `instructions/core.md` and `docs/format.md`;
- your private instance repository and its working branch; `core.json` there must point to the same engine commit;
- the authorized way to read and commit Git files, with access to your private repository;
- the rule that personal records live only in that private repository's `context/`, with a current-blob check before editing and a read-back after committing.

### Example: ChatGPT Project with the connected GitHub plugin

Paste the following into the **Project instructions** after replacing the placeholders. This is an example for ChatGPT, not a requirement to use ChatGPT or GitHub in every Core installation.

> My Systemator Core instance is the private GitHub repository `<account>/<my-private-instance>`. Use the connected GitHub plugin for its files. The public engine is `01Systemator/Systemator-core` at commit `<full-engine-commit-sha>`. Read `instructions/core.md` and `docs/format.md` from that exact engine commit, then read the private instance's `README.md` and `core.json` from `main`. Confirm that the instance is private and that `core.json` pins the same engine commit. Personal records live only in the private instance's `context/`. Check the current blob before changing a record; commit through the authorized Git adapter and read back the result from `main`. If the private repository or plugin is unavailable, report that and do not claim to have read or saved the data. Do not use public Issues, project memory, or a separate database as personal Core storage.

A new chat must actually follow that instruction; pasting it does not prove access or persistence. In the first chat, ask the assistant to read `core.json` and an existing card and report their IDs without changing anything. Then save one real record and independently read it back in another chat. If your ChatGPT Project cannot access the private repository, correct its connection or permissions before relying on it.

### Other models, agents and local environments

Adapt the same four bindings to the environment's own instruction mechanism: system prompt, project rule, agent configuration or startup file. Replace the GitHub plugin with an authorized Git adapter and replace repository addresses with the appropriate Git locations. Keep the public engine version pinned, the owner data private, the instance ID stable, and the write/read-back checks intact. This pilot documents the contract; operation with other adapters and offline synchronization still needs separate verification.

## Start

1. Create a separate **private** Git repository for your instance. Never put personal records or instance-specific configuration into this public repository.
2. Put a `core.json` identity file and a `context/` directory in the private repository, following [the format](docs/format.md). Keep the instance ID when moving or restoring the repository.
3. In the ChatGPT project instructions, give the exact private repository and the exact commit of this engine, then refer to [the Core instructions](instructions/core.md).
4. On the first visit, read the private `README.md` and `core.json`; read or write only relevant personal records in its `context/` directory. Verify committed changes by reading them back from the private repository.
5. Back up and restore the private Git repository independently. A single remote is not a verified backup.

GitHub is the first supported access route in this pilot. Git itself is the history and portable data format; local Git synchronization is future work. GitHub Issues and Projects are not the personal data store. The Core profile does not require a task manager, database, goals, plans, or agents.

The product profile contract and development coordination live in [Systemator Development](https://github.com/01Systemator/Systemator/blob/main/docs/systemator-core-contract.md). Work and Extended add capabilities separately. This repository is the shared Core source; a user's private repository is an instance of it.
