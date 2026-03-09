#System Architecture

##Overview
This automation system extracts email outreach data from Google Sheet, check if already not sent or not replied, and automatically send it in a daily schedule manner.
The system combines n8n nodes including schedule node, google sheet nodes, limit & Switch node, and gmail nodes to create a fully automated 3 email scequence (2 follow-ups) email automation system.

##Emails High Level Architecture (Slight diference in first vs follow-up)
Schedule
↓
Google Sheet 
↓
Switch
↓
Loop
↓
Gmail
↓
Google Sheet

##Components
1. Domain, Google Workspace and Google Cloud
Email architechture setup including:
- n8n hosted on google cloud VM
- Domain
- Email ids
- DKIM/SPF/DMARC setup
- Email warming (manyreach)

2. n8n workflow Automation
Used to run automated schedule emails sensing workflow.
Functions:
- Schdule when workflow need to run.
- Extact outreach data from google sheet.
- send email with opt-out link.

##Output:
Email sent and google sheet get updated.

Data Flow
1. Workflow start at scheduled time.
2. It fetch outreach data from google sheet with label "Sent" as "No".
3. With daily limit, it sent emails.
4. Finally, google sheet row get updated as Sent.

##Technologies Used
Automation Tools:
- n8n orchestration tool.
Data
- Google Sheet

##Scalability Considerations
The system is designed to handle large number of emails.
