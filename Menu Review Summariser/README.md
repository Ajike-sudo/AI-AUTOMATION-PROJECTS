# Project 05 — Menu Review Summariser

## What This Does
Groups all customer feedback by food item, feeds each group to a Summarization Chain, and produces a one-paragraph intelligence brief per dish covering what customers like, what they complain about, and one kitchen recommendation.

## The Problem It Solves
Dozens of customer reviews per menu item were scattered across a spreadsheet with no one reading them systematically. Patterns — recurring complaints about a specific dish, consistent praise for another — stayed invisible and never informed menu decisions.

## Tools Used
- n8n
- OpenAI (gpt-4o-mini)
- Google Sheets
- Summarization Chain

## Workflow
Manual Trigger → Google Sheets (Get Rows) → Code Node (group by food item) → Loop Over Items → Summarization Chain → Google Sheets (Append — Menu Intelligence)

## Real World Use Cases
- Banks summarising customer complaint themes per product
- Hospitals extracting patterns from patient feedback per department
- E-commerce summarising product reviews per SKU
- HR teams synthesising employee survey responses per team

## Key Lessons
- Summarization Chain uses {text} as the placeholder — not an n8n expression
- Output field is called text not output — map accordingly in Append Row
- Group data in a Code node before the Loop so each iteration handles one subject, not one row

## Dummy Data
50-row Legacy Restaurant feedback sheet with Food column added manually.
