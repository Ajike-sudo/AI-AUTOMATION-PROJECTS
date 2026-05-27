Weekly Ops Digest Generator
Problem Statement Leadership had no structured visibility into weekly operational performance. KPI reporting was manual, inconsistent, and time-consuming with no AI-driven analysis or priority actions.
Solution Automated weekly ops digest that pulls KPI data from Google Sheets every Monday, sends it to OpenAI for analysis, and delivers a formatted leadership brief directly to the team's inbox — zero manual input required.
Tools Used
Google Sheets
n8n Cloud
OpenAI API (GPT-4o)
Gmail
Workflow Architecture
[Schedule Trigger] → [Google Sheets Get Rows] → [Code Node] → [HTTP Request OpenAI] → [Gmail Send Digest]

What It Automates
Triggers every Monday at 7:00 AM automatically
Pulls all KPI rows from Hastom Weekly KPIs sheet
Formats data into a structured prompt using the Code node
Sends prompt to GPT-4o for analysis
GPT-4o identifies what's on-track, off-track and generates priority actions
Formatted digest delivered to leadership inbox every Monday morning
Key Logic
Code Node: formats raw Google Sheets rows into a clean readable prompt before sending to OpenAI
OpenAI Prompt: instructs GPT-4o to act as a Hastom ops analyst, flag variances and recommend actions
HTML Formatting: markdown response from OpenAI converted to clean HTML before sending via Gmail
KPIs Tracked
Outgrowers onboarded
RCN sold
RCN bought
RCN harvested
Revenue (₦)
Leads generated
Outstanding debtors
Outcome Leadership receives a professional AI-written ops digest every Monday morning with zero manual effort. Variances are flagged automatically and priority actions generated based on real data.
