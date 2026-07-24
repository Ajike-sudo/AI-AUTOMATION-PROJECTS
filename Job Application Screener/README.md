# Project 2 — Job Application Screener

**Company context:** Crestline Retail & Distribution Ltd — a mid-sized Nigerian retail/distribution business

## Problem

Crestline hires across multiple roles (Sales Associate, Customer Service Representative, Procurement Officer, Warehouse Supervisor) at different times. Applications arrive with a CV and a cover letter, but reviewing every application by hand is slow and inconsistent — different reviewers weigh experience, education, and skills differently, and there's no structured, comparable record of why a candidate was ranked where they were.

## Business Process Context

**Process:** Candidate Screening & Shortlisting
**Process owner:** HR — owns the scoring rubric, the role requirements, and the final hiring decision
**System owner:** Operations — designs and maintains the automation that applies HR's criteria consistently

| Step | Action | Role |
|---|---|---|
| 1. Intake | Candidate submits name, email, role, CV, and cover letter via Google Form | Automated |
| 2. Extraction | Years of experience, education, and key skills extracted from the CV | Automated (AI extraction) |
| 3. Requirements lookup | The specific role's required experience, education, skills, and cover letter expectations are pulled from a Role Requirements reference sheet | Automated |
| 4. Scoring | Candidate is scored 0–100 against that role's specific requirements, across four criteria, each with stated reasoning | Automated (AI scoring) |
| 5a. Strong Fit (≥75) | Candidate logged to Shortlist; HR notified by email to arrange an interview | HR |
| 5b. Consider (60–74) | Candidate logged to Screening Results only — no email, awaiting HR's own review | HR |
| 5c. Below threshold (<60) | Candidate logged to Screening Results; an automated rejection email is sent directly to the candidate | Automated, no human review |
| 6. Recruiter decision | Every logged row carries a "Recruiter Decision" field, defaulted to "Pending review" and never set by the AI | HR |

**Why this matters:** this process sits on real hiring outcomes, which makes it a meaningfully higher-stakes automation than a purely internal log (see Project 1). The scoring is role-aware rather than generic — a candidate's experience and skills are judged against that specific role's stated requirements, pulled from a maintained reference sheet, not inferred by the AI on the fly.

## Tools

`Google Forms` (candidate intake) · `Google Sheets Trigger` (`Application Submission`) · `Edit Fields` (field normalization) · `Google Drive` (`Download file` — CV retrieval) · `CV Extraction` (OpenAI — structured data from the CV) · `Role Requirement Lookup` (Google Sheets — role-specific requirements) · `Applicant Scoring` (OpenAI — role-aware rubric scoring) · `Score Result` (Edit Fields — normalization) · `If Score >= 75` / `If Score <60` (branching logic) · `Shortlisted Applicants` / `Screened Applicants` (Google Sheets — audit log) · `Notify HR to set Interview` / `Rejection Mail` (Gmail)

## Workflow

1. **Intake** — a candidate submits the Google Form (Full Name, Email Address, Role Applied For, CV Upload, Cover Letter). This lands as a row in the `Applications` sheet.
2. **Trigger** — `Application Submission` fires on the new row.
3. **Normalize + download** — `Edit Fields` extracts the CV's file ID from the uploaded link; `Download file` retrieves the actual CV from Google Drive.
4. **CV extraction** — `CV Extraction` reads the CV and returns structured JSON: years of experience, highest education, education field, and key skills. The prompt explicitly instructs the model to return `null` on fields it can't determine rather than guess.
5. **Role requirements lookup** — `Role Requirement Lookup` reads the `Role Requirements` sheet and pulls the one row matching the candidate's selected role, giving the required experience, education, key skills, and cover letter expectations for that specific role.
6. **Scoring** — `Applicant Scoring` compares the candidate's CV data and cover letter against that role's specific requirements (not generic standards), scoring four criteria — experience match, education fit, skills match, cover letter fit — each 0–25, with a stated reason per criterion. The prompt explicitly instructs the model not to weigh protected characteristics.
7. **Normalize** — `Score Result` flattens the score into individual fields and carries the candidate's name, email, and role forward.
8. **Branch on tier:**
   - **`If Score >= 75`** — Strong Fit candidates are appended to `Shortlisted Applicants`, and `Notify HR to set Interview` emails HR with the candidate's details and reasoning.
   - **Below 75** — candidates are appended to `Screened Applicants` regardless of what happens next.
   - **`If Score <60`** — candidates scoring below 60 additionally trigger `Rejection Mail`, sent directly to the candidate. Candidates scoring 60–74 are logged only, with no email in either direction, awaiting HR's own review.
9. **Audit log** — every candidate, every tier, carries a `Recruiter Decision` field defaulted to `"Pending review"` and never set by the AI.

## Other Possible Use Cases

The pattern here — extract from an unstructured document, look up context-specific requirements, score against them, and branch on the result — extends beyond hiring:

- **Vendor or supplier qualification** — score prospective suppliers against category-specific requirements (certifications, capacity, pricing) pulled from a reference sheet, rather than one generic bar for every vendor type.
- **Grant or scholarship application screening** — score applicants against program-specific eligibility criteria that differ by program, the same way this workflow scores candidates against role-specific requirements.
- **Internal role-change or promotion requests** — compare an employee's stated experience against the requirements of the role they're requesting to move into.
- **Any scoring process where "what counts as a good fit" genuinely differs by category** — the reusable part is the requirements-lookup-then-score shape, not the hiring-specific prompt or thresholds.

