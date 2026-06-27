# Project 04 — Smart Farmer Lead Qualifier

## What This Does
An AI Agent that reads each farmer registration, reasons about farm size, crop type, location, and status, assigns a qualification tier (Hot/Warm/Cold), and automatically triggers immediate email outreach for Hot leads, logs Warm leads to a follow-up sheet, and archives Cold leads.

## The Problem It Solves
All farmer registrations were treated equally regardless of commercial potential. A 0.3-acre inactive farmer and a 60-acre cashew farmer received the same follow-up, wasting the sales team's time on low-value leads while high-value prospects waited.

## Tools Used
- n8n
- OpenAI (gpt-4o-mini)
- Google Sheets
- Gmail

## Workflow
Manual Trigger → Google Sheets (Get Rows) → Loop Over Items → AI Agent (qualify + score) → Code Node (parse JSON) → Switch (Hot/Warm/Cold) → [Hot: Send Email] / [Warm: Sheets Append] / [Cold: Edit Fields]

## AI Nodes Used
- AI Agent with qualification rules in System Prompt

## Key Lessons
- All three Switch branches must connect back to the Loop input — not just the email branch
- Reference the exact node name when using $('Node Name') in Code nodes — check the canvas label
- Switch value fields must be in plain text mode (fx OFF) not expression mode
- The AI Agent only classifies — HTML email formatting is handled separately in the Send Email node

## Business Context
Built for Legacy Farms — qualifying farmer registrations across Osogbo, Benue, and Ibadan into Hot, Warm, and Cold tiers based on farm size, crop type, and farmer status.

## Dummy Data
15 farmer registrations with mixed Yoruba-Hausa names, farm sizes from 0.3 to 60 acres, all three crop types (Cassava, Maize, Cashew), and dummy @mailtest.com emails.
