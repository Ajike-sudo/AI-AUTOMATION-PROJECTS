# Project 1 — Invoice Data Extractor

**Company context:** Crestline Retail & Distribution Ltd — a mid-sized Nigerian retail/distribution business

## Problem

Crestline receives supplier invoices as PDFs/images dropped into a shared Drive folder. Finance re-keys vendor name, invoice number, dates, and amounts by hand into a tracking sheet before payment — slow, and error-prone on the field that matters most: the amount.

## Business Process Context

**Process:** Invoice Intake & Approval Processing
**Process owner:** Finance (Accounts Payable function) — owns the approval threshold policy and the accuracy of the underlying control
**System owner:** Operations — designs and maintains the automation that enforces the policy Finance sets

| Step | Action | Role |
|---|---|---|
| 1. Intake | Supplier invoice received into the system | Automated |
| 2. Extraction | Vendor, invoice #, dates, amount, line items captured | Automated (AI extraction) |
| 3. Routing | Invoice split by value against a materiality threshold | Automated (rule-based) |
| 4a. Low-value processing | Invoices ≤ ₦500,000 auto-logged; Junior Accountant notified by email | Junior Accountant (notified, monitors log, handles exceptions) |
| 4b. High-value approval | Invoices > ₦500,000 routed for explicit sign-off before being treated as approved | Senior Accountant |
| 5. Recording | Every invoice, either path, logged with timestamp and source reference | Automated (audit trail) |
| 6. Payment scheduling | Approved invoices move into the payment run | Finance (downstream of this process) |

**Why the threshold split matters:** this is a segregation-of-duties control based on materiality, not just a routing convenience. The Junior Accountant handles high-volume, lower-risk transactions, keeping operations moving without every invoice needing senior sign-off. The Senior Accountant's attention is reserved for transactions where an error or fraud would actually matter financially. The threshold itself is a Finance policy decision — Operations builds the system that enforces it, but doesn't set the number.

## Tools

`Google Drive` (trigger + file download) · `OpenAI` (structured extraction) · `Edit Fields` node (data normalization + approval logic) · `IF` node (threshold routing) · `Snr Accountant Approval` / `Jnr Accountant Processing Notif` (Gmail routing) · `Google Sheets` (audit log)

## Workflow

1. **Trigger** — new file lands in the "Incoming Invoices" Drive folder.
2. **Download** the file and pass it to OpenAI with a system prompt that returns strict JSON: vendor, invoice number, invoice date, due date, currency, total amount, line items. The prompt explicitly instructs the model to return `null` on unreadable fields rather than guess.
3. **Normalize** — the `Edit Fields` node flattens the JSON into individual fields (vendor, amount, dates, etc.) and keeps a reference back to the source file (id + name) for traceability.
4. **Threshold check** — the `IF` node checks whether `total_amount > ₦500,000`. Above threshold, the invoice branches to `Snr Accountant Approval`, an approval-request email for the Senior Accountant, before it's treated as approved. At or below threshold, it's logged directly and the `Jnr Accountant Processing Notif` node emails the Junior Accountant — informational, not requiring sign-off, since the invoice is already treated as processed.
5. **Audit log** — every invoice, regardless of branch, is appended to a Google Sheet with the extracted fields, `approval_required`, `status`, source file reference, and processing timestamp. Nothing bypasses the log.

## Other Possible Use Cases

The underlying pattern here — extract structured data from an incoming document, normalize it, apply a threshold-based decision, route for human approval where warranted, log everything — extends well beyond invoices:

- **Receipts / expense claims** — staff submit a photo of a receipt; the system extracts vendor, amount, and category, routes unusual or high-value claims for approval, and logs the rest automatically.
- **Delivery notes / goods received notes** — extract quantities delivered against what was ordered, flagging mismatches for review instead of relying on a manual tally at the warehouse.
- **Contracts or purchase orders** — pull key terms (value, dates, counterparty, renewal date) into a tracker the moment a signed PDF lands in a folder, instead of someone opening every document to log it by hand.
- **Vendor onboarding documents** — extract business registration details, bank information, or compliance certificates from submitted files, flagging incomplete or inconsistent submissions for manual review.
- **Any document-in, structured-data-out process with a materiality or risk threshold** — the reusable part is the shape of the workflow (capture → extract → normalize → route → log), not the invoice-specific prompt or threshold value.

## Lessons learned

- The single highest-cost failure mode here isn't sentiment or tone — it's a misread number. The design has to treat `total_amount` and `due_date` as the two fields worth extra scrutiny, not just "a field like any other."
- An approval threshold only works if it's paired with an audit trail. The threshold decides who reviews; the log is what makes the decision inspectable after the fact.
- Returning `null` on uncertain fields (instead of a best guess) is a small prompt change with an outsized effect — it turns "confidently wrong" into "visibly incomplete," which is the failure mode you want.

See `governance-notes.md` for the full risk writeup.
