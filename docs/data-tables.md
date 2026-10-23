# Data tables

The flows use five n8n data tables. Their contents are client data and are **not** stored in this repository; only their structure is documented here.

| Table | Used by | Maintained by |
|---|---|---|
| `summers.datastore.config.keys` | every flow | by hand (see [configuration.md](configuration.md)) |
| `Location_Master` | 01, 03 | flow 12 from the Google Sheet; `active` and `code_aliases` by hand |
| `Department_Code` | 02 | flow 12; `location_code` by hand |
| `Vendor_Account_Map` | 03 | flow 12; `sage_entity_id`, `department_code`, `sage_vendor_id` by hand |
| Daily Report (`New Business Processing Datatable`) | 01, 05, 07, 11 | the flows |

## Location_Master
`location_number` (number), `location_name`, `ship_to_street`, `city`, `state`, `zip`, `location_code` (PO suffix, e.g. `BB`), `sage_entity_id`, `st_tenant_id`, `active` (boolean — only active locations are processed), `code_aliases` (other PO suffixes for the same location, comma-separated).

## Department_Code
`business_unit_id` (ServiceTitan), `business_unit_name`, `department_code` (Sage), `department_name`, `location_code`.

## Vendor_Account_Map
`vendor_name`, `account_number`, `location_code`, `sage_entity_id`, `gl_account`, `department_code`, `sage_vendor_id`, `status`, `confirmed_by_client` (boolean).

## Daily Report
One row per invoice or statement:
- **Document:** `document_id`, `document_type`, `subject`, `attachment_name`, `message_id`
- **Invoice:** `vendor_name`, `invoice_number`, `po_number`, `total_amount`, `payment_terms`
- **Routing:** `lane`, `location_id`, `location_name`, `location_method` (how the location was found)
- **ServiceTitan:** `st_po_number`, `st_po_id`, `st_po_status`
- **Sage:** `gl_code`, `gl_name`, `sage_bill_id`
- **Outcome:** `processing_status`, `exception_reason`, `warnings`
- **Match detail:** `invoice_price_mismatch`, `invoice_qty_mismatch`, `invoice_location_mismatch`, `invoice_vendor_mismatch`, `account_number_match`, `utility_invoice_match_status`, `line_count` (number), `variance_amount` (number), `match_details`
