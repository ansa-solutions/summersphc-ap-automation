# Configuration

All settings live in the n8n data table **`summers.datastore.config.keys`** (columns `category`, `key`, `value`). Values are text; `Build Config` turns `true`/`false` into booleans, numbers into numbers, and JSON (`[...]`, `{...}`) into arrays and objects. A starting set is in [`config/config.template.csv`](../config/config.template.csv); values shown as `<...>` are environment-specific.

## GLOBAL_CONFIG
| Key | Purpose |
|---|---|
| `extraction_endpoint` | z.ai chat-completions URL used for every GLM call |
| `glm_extraction_model` | Model for header and line extraction |
| `glm_fallback_model` | Model for the location fallback |
| `glm_header_max_tokens` / `glm_lines_max_tokens` / `glm_fallback_max_tokens` | Output token limits per call |
| `st_api_base` / `st_auth_url` | ServiceTitan API and token URLs (integration or production) |
| `sage_gateway_url` | Sage Intacct XML gateway |
| `doppler_secrets_url` | Doppler endpoint that returns the Sage credentials |
| `ap_inbox`, `currency`, `environment` | Informational |

## INV_PROC_CONFIG (invoice processing)
| Key | Purpose |
|---|---|
| `validation_tolerance` | Dollar tolerance for total checks |
| `line_price_tolerance` | Per-line price tolerance against the PO |
| `allow_st_price_update` | Let the flow update zero-priced PO lines in ServiceTitan |
| `vendor_match_stopwords` | Words ignored when comparing vendor names |
| `sage_send_termname`, `sage_search_page_size` | Sage posting options |
| `email_body_min_chars` | Minimum email-body length to treat a PDF-less email as an invoice |
| `indirect_require_confirmed_map` | Only post non-PO bills whose account mapping the client confirmed |
| `indirect_amount_basis` | `current_charges` (default) or `invoice_total` for non-PO bills |

## LOCATION_CONFIG
| Key | Purpose |
|---|---|
| `conflict_policy` | `strict`: disagreeing location sources become an exception. `po_wins`: PO code wins, flagged for review |
| `enable_llm_fallback` | Ask the LLM to pick a branch when no rule matched (PO invoices only) |

## ROUTING_CONFIG
| Key | Purpose |
|---|---|
| `advertising_patterns`, `utility_patterns` | Regex lists that label non-PO bills as advertising or utility |

## STMT_RECON_CONFIG (statements)
| Key | Purpose |
|---|---|
| `tolerance_dollars` | Reconciliation tolerance |
| `lookback_days`, `default_window_days` | Date window for open bills |
| `suffix_match_min` | Minimum digits when matching invoice-number suffixes |
| `sage_page_size`, `vendor_search_page_size`, `max_vendor_ids` | Sage query limits |

## EMAIL_CONFIG
| Key | Purpose |
|---|---|
| `statement_notify_to` | Daily Report and statement reviews (To) |
| `exception_notify_to` | Exception notifications |
| `error_notify_to` | Workflow-failure alerts |
| `CC_to` | Extra Daily Report recipients |
| `notify_on_reconciled`, `silent_statuses` | Which outcomes send email |
| `internal_domains` | Domains treated as internal |
| `vendor_draft_enabled`, `signature_team`, `signature_company` | Vendor email drafts |

## SHEET_SYNC_CONFIG (flow 12)
| Key | Purpose |
|---|---|
| `spreadsheet_id`, `location_tab_gid`, `department_tab_gid`, `vendor_map_tab_gid` | Source Google Sheet and tabs |
| `vendor_map_header_row` | Header row of the vendor-map tab |
| `delete_missing_rows`, `max_delete_ratio` | Delete rows removed from the sheet, with a safety cap |
| `dry_run` | Report changes without writing |

## GITHUB_CONFIG (flow 92)
| Key | Purpose |
|---|---|
| `owner`, `repo`, `branch` | Target repository |
| `workflows_path` | Folder for workflow files |
| `tag_filter` | Only flows with this n8n tag are backed up |
| `exclude_tags` | Tags never backed up (default `["Test"]` — test flows can hold real document text) |
| `block_on_sensitive` | Hold back any file that contains an email address or secret-like value |
| `allowed_email_domains` | Email domains allowed in workflow files |
