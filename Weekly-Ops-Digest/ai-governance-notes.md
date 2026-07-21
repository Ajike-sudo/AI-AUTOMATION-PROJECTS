# AI Governance Notes

A running record of risk findings, fixes, and open questions across automation projects in this portfolio.

## Purpose
This exists because most automation portfolios show the build and stop there, and most AI governance writing is theoretical — written by people who've never shipped a workflow that touches a real business's money or people. This file is where those two things meet, using the actual bugs and design decisions from this portfolio, not hypothetical risks borrowed from someone else's framework.

Project 01 already proved the point is real, not aspirational: the Weekly Ops Digest shipped an AI Agent that mislabeled a debtor increase as a decrease, and the fix wasn't a patch, it was rethinking which parts of the workflow should be allowed to reason versus which parts should be deterministic. That distinction — where AI judgment adds value versus where it introduces risk with no offsetting benefit — is the actual skill this file is building a record of. Every entry after this one should hold the same bar: real findings from real testing, not a checklist run through in the abstract.

## Framework
Four questions per project:
1. Where does this automation act or decide, rather than just inform?
2. What's the cost of it being wrong?
3. Is there a human checkpoint before impact, and if not, why not?
4. What did testing actually catch?

---

## Project 01 — Weekly Ops Digest (AI Agent)

**1. Where it acts**
Computes variances and sends a digest containing real financial figures — revenue, debtors — directly to leadership inboxes, unreviewed.

**2. Cost of being wrong**
A miscalculated or mislabeled variance on debtors or revenue could mislead a leadership decision, or erode trust in the automation once caught.

**3. Human checkpoint**
None currently. Digest sends automatically after generation. Open decision below.

**4. What testing actually caught**
- Found: an early version let the AI Agent compute percentage variances itself, guided only by a prompt instruction and a Calculator tool. This produced a contradictory result — a value that increased was labeled a decrease.
- Fix applied: moved all variance and multi-week pattern calculation (trend streaks, volatility, outlier detection) into deterministic JavaScript in the Code node. The AI Agent's role changed from computing numbers to narrating numbers it's given, which removes the failure mode instead of reducing its likelihood.
- Verified: re-ran with the fix. Single-week variances confirmed correct against source data.

**Still open**
- System message still instructs the AI to use a Calculator tool for computation — a leftover contradiction from before the fix. Tool should be disconnected and the instruction removed.
- No verification yet that the AI's narrative is actually incorporating the pattern-analysis flags (trend, volatility, outlier), rather than just restating single-week deltas. Needs a test case to confirm.
- No human review checkpoint before send. Decision needed: either build a lightweight approval step, or document this as a deliberate choice with reasoning.
- Pattern analysis needs a minimum data history (4+ weeks recommended) to avoid surfacing meaningless trend claims on short histories. Not yet enforced in code, only documented.
- Volatility (30% coefficient of variation) and outlier (1.5 standard deviations) thresholds are reasonable defaults, not yet tuned against real KPI variance.
