# Project 01 — Customer Feedback Analyser
AI-powered workflow that reads customer feedback, classifies sentiment and category using OpenAI, and writes results back to Google Sheets automatically.

## Tools
- n8n
- OpenAI (gpt-4o-mini)
- Google Sheets

## Workflow
Manual Trigger → Google Sheets (Get Rows) → Basic LLM Chain → Code Node → Google Sheets (Update Row)
