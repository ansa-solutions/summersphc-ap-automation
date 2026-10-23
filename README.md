# Summers PHC — AP Automation (n8n)

Accounts-payable automation built by ANSA Solutions on n8n. Vendor invoices and statements arrive by email; the flows read them with an LLM (z.ai GLM), match PO invoices to ServiceTitan, code non-PO invoices from a vendor account map, and post bills to Sage Intacct. Every document is tracked in a daily report table.

## Flows

| # | Flow | What it does |
|---|------|--------------|
| 01 | AP Orchestrator | Watches the AP inbox, extracts each PDF or email-body invoice, reads header and totals with GLM, resolves the location, routes each document to a lane, and records every step in the Daily Report table. |
| 02 | Direct Invoice (PO) | Finds the PO in ServiceTitan, extracts line items, and matches vendor, location, quantity and price. |
| 03 | Indirect Invoice (Non-PO) | Utilities, fuel, advertising and other non-PO bills: GL, department, vendor and location from the Vendor Account Map. |
| 04 | Sage Post | Creates the bill in Sage Intacct. |
| 05 | Exception Handler | Records exceptions and notifies the AP reviewer. |
| 06 | Error Handler | Alerts on workflow failures. |
| 07 | Statement Reconciliation | Reconciles vendor statements against open bills and credits in Sage. |
| 11 | Daily Report | Emails the previous day's processing report. |
| 12 | Sync Reference Data | Copies the reference Google Sheet into the n8n data tables (Tue–Sat). |
| 92 | Backup to GitHub | Commits every flow tagged `AP Automation` to this repo weekly (Sundays). |

## Design rules

- **No hard-coded settings.** Every URL, email address, model name, threshold and limit is read from the `summers.datastore.config.keys` data table at run time (`Load Config` → `Build Config`). Each flow fails loudly if a required key is missing. See [docs/configuration.md](docs/configuration.md).
- **Reference data lives in n8n data tables** (locations, departments, vendor account map), synced from a Google Sheet. See [docs/data-tables.md](docs/data-tables.md).
- **No secrets in workflows.** API keys sit in n8n credentials; Sage credentials come from Doppler at run time. Exports contain credential *names and IDs* only.
- **Nothing is posted on a guess.** Amounts the LLM returns must appear in the document text, printed totals must add up, and anything unclear becomes an exception for a person to review.

## Repository layout

```
workflows/   one JSON file per n8n workflow (written by flow 92)
config/      config.template.csv — every config key, with placeholders for environment-specific values
docs/        configuration, data tables, backup and restore
```

## Importing a flow

In n8n: **Workflows → Import from file**, choose the JSON from `workflows/`, then select the matching credentials on any node marked with a warning. Sub-flows are called by workflow ID, so after importing into a new instance update the IDs in the Orchestrator's *Run …* nodes.

## Contributing

`main` mirrors what is running in n8n (flow 92 commits weekly on Sundays and on demand). For a larger change, export the flow, commit it on a branch, and open a pull request for review before importing it into n8n.
