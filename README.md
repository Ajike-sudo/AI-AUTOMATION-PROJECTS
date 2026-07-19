# AI Automation & Agent Workflow Portfolio

Welcome! My name is Oluwatobiloba Onisemo, and this repository is a curated collection of AI automation and AI agent workflows I've designed and built with n8n.

I approach automation from an operations leadership seat, and this portfolio contains several projects — each one solving a real business operations problem: lead qualification, customer feedback, KPI reporting, and onboarding accountability. Every project is a fully functional, end-to-end build with its own detailed README covering the problem, architecture, and lessons learned.

## 🛠️ Tech Stack & Skills

This portfolio showcases my experience with the following tools and patterns, integrated via n8n:

* **Core Automation:** n8n (Workflows, Webhooks, Schedule Triggers, Wait/Escalation Logic, Switch Routing, Loops)
* **AI & LangChain:** AI Agents, Basic LLM Chain, Summarization Chain, Persistent Memory, Calculator Tool, Text Classification, Sentiment Analysis
* **AI Models:** OpenAI (GPT-4o, GPT-4o-mini)
* **Cloud & APIs:** Google Workspace (Sheets, Gmail, Forms), Webhooks, HTTP Request

---

## 📂 Workflow Showcase

Below is a summary of the projects in this repository. Each one includes a detailed `README.md` in its folder with a full breakdown of the problem it solves, the workflow architecture, and key lessons from the build.

| Workflow | Description & Key Features | Core Technologies |
| :--- | :--- | :--- |
| **[Smart Farmer Lead Qualifier](./Lead%20Qualifier)** | An AI Agent that reasons over farmer registrations — farm size, crop type, location, status — assigns Hot/Warm/Cold tiers, and routes each differently: instant outreach, follow-up log, or archive. | `AI Agent`, `OpenAI`, `Google Sheets`, `Gmail`, `Switch Routing` |
| **[Weekly Ops Digest — AI Agent](./weekly_ops_digest(RCN))** | Runs every Monday at 7AM, pulls weekly KPIs, computes exact week-on-week variances with a Calculator tool, and emails leadership a formatted HTML digest — zero manual reporting. | `AI Agent`, `Memory`, `Calculator Tool`, `Google Sheets`, `Gmail` |
| **[Outgrower Onboarding Pipeline](./outgrower-onboarding)** | End-to-end onboarding from form submission: automatic manager assignment by location, welcome emails, one-click status updates via webhook, and a 48-hour escalation loop if no manager action. | `Google Forms`, `Webhooks`, `Wait Node`, `Gmail`, `Conditional Logic` |
| **[Auto Reply Generator](./Auto-reply-generator)** | Detects low-rating customer complaints and sends each customer a unique, personalised apology email — no templates, no manual drafting. | `Basic LLM Chain`, `OpenAI`, `Google Sheets`, `Gmail` |
| **[Customer Feedback Analyser](./Customer_Feedback_Analyzer)** | Reads customer feedback at scale, classifies sentiment and category with AI, and writes structured results back to the sheet automatically. | `Basic LLM Chain`, `OpenAI`, `Google Sheets` |
| **[Menu Review Summariser](./Menu%20Review%20Summariser)** | Groups scattered reviews by menu item and produces a one-paragraph intelligence brief per dish — praise, complaints, and one kitchen recommendation. Same pattern transfers to bank complaints, patient feedback, and employee surveys. | `Summarization Chain`, `OpenAI`, `Google Sheets` |
| **[Weekly Ops Digest — v1](./weekly-ops-digest)** | The first version of the ops digest, built with a direct OpenAI API call before rebuilding it as an AI agent — kept here to show the architectural progression. | `HTTP Request`, `OpenAI API`, `Google Sheets`, `Gmail` |

## 💬 Let's Connect!

I'm building at the intersection of operational leadership and AI automation. If you'd like to discuss my work, potential collaborations, or opportunities, feel free to reach out.

* **LinkedIn:** https://www.linkedin.com/in/oluwatobilobaonisemo
* **Email:** onisemotobiloba@gmail.com
