# Core instructions for an interface

These instructions apply when a project is connected to a Systemator Core instance. The project-specific entry must supply the **private instance repository** and a pinned engine commit. A chat, project memory, and this public engine repository are not the owner's data store.

1. Read the project entry. Verify that the private repository exists and is private. Read its `README.md` and `core.json`. If the engine ref or owner scope is unclear, report that uncertainty before writing.
2. Use the instance's `context/*.md` as the source for owner records. Retrieve only the relevant cards and preserve their IDs, provenance, and links. Separate stored facts from current inference.
3. When the owner asks to preserve a decision, fact, thought, intention, or reference, create or update a context card in the **private** repository. Use the format in `docs/format.md` at the pinned engine ref. Do not turn every conversation into a record. A task manager or issue is not required.
4. Before replacing a card, fetch its current blob and check whether it changed. Preserve its ID; update metadata and content, and commit through the available Git adapter. If concurrent changes conflict, reconcile them before writing.
5. After a write, independently read back the card from the private repository's default branch and report the commit or the failure. Git history records revisions. Never claim restoration or a new accepted release from a successful write alone.
6. Never copy personal context, identity, secrets, or private links to the public engine repository, public Issues, Development logs, or another owner's instance. Keep engine changes separate from owner-data changes.
7. A ChatGPT project may point to the same private instance as other projects. Its instruction is an entry point, not a copy of the data. Read permissions are checked for each available tool and project.

Initial transport: connected GitHub plugin. Local Git and other remotes can be added without changing the owner data format or IDs, after synchronization and recovery are tested. Keep Core within the minimal context contract; Work and Extended add other capabilities.
