# Email Outreach Automation with n8n

This repository contains a suite of **n8n** workflows designed to automate a multi-step cold email outreach campaign. It uses Google Sheets as a central tracking system and Gmail for deliverability.

## Project Structure

- **`workflow/`**: Contains the JSON exports of the n8n workflows.
  - `first-email-workflow.json`: Initial campaign outreach.
  - `first-follow-up-email-workflow.json`: First follow-up logic.
  - `second-follow-up-email-workflow.json`: Second follow-up logic.
  - `email-opt-out.json`: Automated unsubscribe webhook system.
- **`data/`**: Sample lead data structure (`sample_leads.csv`).
- **`screenshot/`**: Visual representations of the n8n nodes for each workflow.
- **`architecture.md`**: Detailed technical documentation of the system.

## Setup Instructions

1.  **Import Workflows:** Import the JSON files from the `workflow/` directory into your n8n instance.
2.  **Configure Credentials:** Set up OAuth2 credentials for:
    - Google Sheets (to read/write lead data).
    - Gmail (to send emails and check for replies).
3.  **Spreadsheet Setup:** Use the column structure provided in `data/sample_leads.csv` in your Google Sheet.
4.  **Activate Webhooks:** Ensure the `email-opt-out.json` workflow is active and the webhook URL is correctly updated in your email templates.

## Related Projects

- **[b2b-lead-generation](https://github.com/sah-automation/b2b-lead-generation)**: A specialized tool for sourcing, enrichment, and verifying B2B leads. This can be used to populate the `data/sample_leads.csv` structure used by these workflows.

For more details, see [architecture.md](./architecture.md).
