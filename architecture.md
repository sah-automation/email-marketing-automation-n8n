# Email Outreach Automation Architecture

This project automates a multi-step cold email outreach campaign using **n8n**. It integrates with Google Sheets for lead management and tracking, and uses Gmail for email delivery and reply monitoring.

## System Overview

The system consists of four primary n8n workflows that work together to manage the lifecycle of an outreach campaign.

### 1. Initial Outreach Workflow
- **File:** `workflow/first-email-workflow.json`
- **Trigger:** Scheduled Cron (Monday-Friday).
- **Process:**
  - Fetches leads from a Google Sheet that haven't received the first email.
  - Applies a limit (e.g., 32 leads per run) to maintain sender reputation.
  - Sends a personalized "Email #1" via Gmail.
  - Updates the Google Sheet with a `Yes` status and the Gmail `Message ID`.
  - Implements a randomized delay (3-7 seconds) between emails to mimic human behavior.

### 2. Follow-up Workflows (Email #2 & #3)
- **Files:** 
  - `workflow/first-follow-up-email-workflow.json`
  - `workflow/second-follow-up-email-workflow.json`
- **Trigger:** Scheduled Cron.
- **Process:**
  - Checks if a reply has been received for previous emails by monitoring the Gmail thread.
  - If no reply is detected, it sends a follow-up email as a **threaded reply** to the original message.
  - Updates the tracking sheet with sent status and new message IDs.

### 3. Opt-Out (Unsubscribe) System
- **File:** `workflow/email-opt-out.json`
- **Trigger:** HTTP Webhook (`/unsubscribe`).
- **Process:**
  - Receives a unique token via the unsubscribe link in the email footer.
  - Locates the lead in Google Sheets using the token.
  - Updates the `Opted Out` status to `Yes`.
  - Redirects the user to a confirmation landing page.

## Data Schema

The project relies on a centralized Google Sheet (or CSV equivalent) with the following key columns:

| Column Name | Description |
|-------------|-------------|
| `Email Address` | Prospect's contact email. |
| `First Name` | Used for personalization. |
| `Email#1 Body/Subject` | Templates for the first email. |
| `Email#1 Sent` | Tracking flag (Yes/No). |
| `Message ID` | ID of the last sent message for threading follow-ups. |
| `Replied` | Campaign status indicating if the prospect responded. |
| `Token` | Unique identifier for the unsubscribe system. |
| `Opted Out` | Flag to stop all future communications. |

## Key Features

- **Spam Protection:** Randomized delays and batched sending frequencies.
- **Personalization:** Dynamic injection of lead data into email templates.
- **Threaded Conversations:** Maintains a single conversation thread for better engagement.
- **Timezone Awareness:** Campaigns are scheduled to run during the prospect's active hours.
- **Automated Opt-Out:** Fully automated unsubscription handling.

## Lead Sourcing

To populate the tracking spreadsheet with high-quality prospects, this project can be integrated with:
- **[b2b-lead-generation](https://github.com/sah-automation/b2b-lead-generation)**: A repository focused on scraping, enrichment, and verifying B2B contact information, specifically designed to match the data schema required by these n8n workflows.

## Visual Documentation

Refer to the `screenshot/` directory for visual representations of the n8n workflow node structures:
- `first-email-workflow.png`
- `first-follow-up-email-workflow.png`
- `second-follow-up-email-workflow.png`
