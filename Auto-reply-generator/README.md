# Project 02 — Auto Reply Generator

AI-powered workflow that reads customer complaints, uses OpenAI to write a unique personalised apology email for each customer, and sends it automatically via Gmail.

## Tools
- n8n
- OpenAI (gpt-4o-mini)
- Gmail
- Google Sheets

## Workflow
Manual Trigger → Google Sheets (Get Rows) → Filter (Rating ≤ 2) → Loop Over Items → Basic LLM Chain → Send Email → Edit Fields
