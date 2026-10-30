# Systemator Core

Systemator Core is a minimal, Git-backed space for an owner's persistent context. It stores addressable Markdown records and their links, lets the owner find them again, and preserves change history through Git. A ChatGPT project may be one interface to this space. The owner keeps their data in a separate repository.

This repository contains the shared instructions and data format for the **first Git-first pilot**. It contains no owner's records, credentials, or instance identity. A public repository alone is not an accepted release. The open-source license, independent acceptance, and versioned release are pending in Systemator Development tasks #10, #16, and #17.

## Start

1. Create a separate **private** Git repository for your instance. Never put personal records or instance-specific configuration into this public repository.
2. Put a `core.json` identity file and a `context/` directory in the private repository, following [the format](docs/format.md). Keep the instance ID when moving or restoring the repository.
3. In the ChatGPT project instructions, give the exact private repository and the exact commit of this engine, then refer to [the Core instructions](instructions/core.md).
4. On the first visit, read the private `README.md` and `core.json`; read or write only relevant personal records in its `context/` directory. Verify committed changes by reading them back from the private repository.
5. Back up and restore the private Git repository independently. A single remote is not a verified backup.

GitHub is the first supported access route in this pilot. Git itself is the history and portable data format; local Git synchronization is future work. GitHub Issues and Projects are not the personal data store. The Core profile does not require a task manager, database, goals, plans, or agents.

The product profile contract and development coordination live in [Systemator Development](https://github.com/01Systemator/Systemator/blob/main/docs/systemator-core-contract.md). Work and Extended add capabilities separately. This repository is the shared Core source; a user's private repository is an instance of it.
