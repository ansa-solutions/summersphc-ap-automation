# Backup and restore

## How the backup works (flow 92)
1. Runs nightly at 11:45 PM, or on demand with **Run Now**.
2. Reads every workflow tagged `AP Automation` through the n8n API (pinned test data excluded) and skips flows tagged `Test`.
3. Keeps only what is needed to rebuild a flow (nodes, connections, settings, tags) and sorts nodes by name so diffs stay readable.
4. Scans each file for email addresses and secret-like values. Any hit holds that file back and fails the run, so nothing sensitive reaches this public repo.
5. Commits only files whose content changed, one commit per file.

Setup: an **n8n API** credential (Settings → n8n API → create key) and a **GitHub** credential with a fine-grained token limited to this repository (Contents: read and write). Settings are in `GITHUB_CONFIG` (see [configuration.md](configuration.md)).

## Restoring a flow
1. Download the file from `workflows/` (any commit in history).
2. In n8n, open the existing flow → **⋯ → Import from file** to replace it, or create a new workflow and import.
3. Re-select credentials if prompted, save, and publish if it was active.
