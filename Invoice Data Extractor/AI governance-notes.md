# AI Governance Notes — Project 1: Invoice Data Extractor

**1. What decision does the AI make?**
It reads an unstructured invoice (PDF/image) and produces the structured facts a payment decision depends on: vendor, invoice number, dates, and total amount. It does not itself decide to pay — but it decides what "the invoice says," and everything downstream trusts that.

**2. What happens if it gets it wrong?**
A misread amount or due date has direct financial cost — an overpayment, a missed due date, or a payment to the wrong reference. This is the highest-stakes field type in the project: a wrong number is worse than a wrong category label elsewhere in the portfolio.

**3. What's the human checkpoint?**
Any invoice above ₦500,000 routes to the Senior Accountant before it's treated as approved — the email is explicit that the figures are an AI extraction, not a verified amount, and asks for confirmation against the source document. Below-threshold invoices are auto-logged, not auto-paid; the Junior Accountant receives a notification email so every invoice has a named human aware of it, even though sign-off isn't required at this value. This workflow stops at data extraction and logging, it does not itself trigger payment.

**4. What data does it touch, and what's the audit trail?**
Every invoice processed — approved-threshold or not — is appended to the Invoice Log sheet with the full extracted field set, the approval flag, status, source file reference, and a timestamp. Nothing is discarded after processing; any later dispute can be traced back to the original file and the exact fields the model extracted from it.
