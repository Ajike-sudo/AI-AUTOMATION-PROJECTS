Outgrower Onboarding Pipeline
Problem Statement Manual outgrower registration required the team to assign Managers, send welcome emails, and track onboarding status by hand. No accountability system existed for Manager follow-up.
Solution Automated end-to-end onboarding pipeline triggered by Google Form submission.
Tools Used
Google Forms
Google Sheets
n8n Cloud
Gmail
Workflow Architecture
Main Workflow:
[Google Sheets Trigger] → [Set Node] → [Update Row] → [Gmail Welcome] → [Gmail Manager Notification] → [Wait 48hrs] → [Get Rows] → [IF Status = Active?] → END or [Escalation Email]
Webhook Chain (same canvas, disconnected):
[Webhook] → [Set] → [Get Rows] → [Filter] → [Update Row] → [Respond to Webhook]
What It Automates
Outgrower data captured and stored automatically on form submission
Manager assigned based on outgrower location using conditional logic
Welcome email sent to outgrower instantly with next steps
Manager notified with full outgrower details and Mark as Contacted button
Status updated to Active when Manager clicks button
Escalation email fires to Manager if no action taken within 48 hours
Key Logic
Manager Assignment: Location-based expression assigns correct Manager and email automatically
Status Tracking: Webhook updates outgrower status to Active in Google Sheets on button click
48-Hour Accountability: Wait node pauses workflow, checks status after 48 hours, escalates to Head of Operations if still New.
Outcome Zero manual intervention required from form submission to active outgrower status. Full accountability loop with automatic escalation built in.
