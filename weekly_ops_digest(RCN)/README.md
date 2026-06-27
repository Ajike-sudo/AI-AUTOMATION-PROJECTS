# Project 03 — Weekly Ops Digest (AI Agent)

## What This Does
An AI Agent that runs every Monday at 7AM, pulls weekly KPI data from Google Sheets, uses a Calculator tool to compute exact percentage variances between weeks, and delivers a formatted HTML leadership digest to the inbox automatically — zero manual reporting.

## The Problem It Solves
Leadership had no structured weekly visibility into operational performance. KPI reporting was manual, inconsistent, and never compared week-on-week. Variances went unnoticed until problems became serious.

## Tools Used
- n8n
- OpenAI (gpt-4o-mini)
- Google Sheets
- Gmail
- Simple Memory
- Calculator Tool

## Workflow
Schedule Trigger (Monday 7AM) → Google Sheets (Get Rows) → Code Node (format KPI data) → AI Agent (analyse + write HTML digest) → Send Email

## AI Nodes Used
- AI Agent with Memory and Calculator tool

## Key Lessons
- AI Agent Memory allows the agent to compare against previous weeks over time
- Calculator tool ensures percentage changes are mathematically accurate not estimated
- Prompting the AI to return HTML directly removes the need for a separate formatting node
- Session key must be a fixed string not an expression for memory to persist across runs

## Business Context
Built for Legacy Farms — an agribusiness tracking outgrower onboarding, RCN sales, revenue, leads, and outstanding debtors weekly.

## Dummy Data
8 weeks of KPI data with deliberate dips in weeks 4, 6, and 8 to give the AI meaningful variances to flag.
