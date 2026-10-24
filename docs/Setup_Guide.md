# CampusPulse — Setup Guide

## 1. Prerequisites

Before setting up CampusPulse, make sure you have:

- A Google account.
- Access to Google Forms and Google Sheets.
- An n8n Cloud account.
- A Groq account and API key.
- Gmail access for sending notifications and reports.

## 2. Repository Structure

The repository contains the exported n8n workflows and documentation.

```text
CampusPulse/
├── workflows/
│   ├── CampusPulse_WF1_Intake_triage_Escalation.json
│   └── CampusPulse_WF3_Weekly_Summary.json
├── docs/
│   ├── PROJECT_OVERVIEW.md
│   ├── SYSTEM_ARCHITECTURE.md
│   ├── SETUP_GUIDE.md
│   └── DEMO_GUIDE.md
└── README.md
```

File names may differ slightly depending on the exported workflow files in the repository.

## 3. Prepare the Google Form and Spreadsheet

1. Create or open the Google Form used to collect campus complaints.
2. Link the form to a Google spreadsheet.
3. Confirm that the form responses are appearing in the spreadsheet.
4. Create or verify the following worksheet tabs:
   - `Tickets`
   - `Owners`
   - `Config`
   - `Processed_Submissions`
5. Ensure the column headers match the fields referenced by the imported workflows.

The `Tickets` sheet should contain the ticket information used by the reporting workflow, including ticket ID, email, category, severity, location, timestamp, summary, status, escalation flag, and resolution timestamp where applicable.

The `Processed_Submissions` sheet should contain the submission key and processing information required by WF1's duplicate check.

**Important:** The exact headers and data types must match the workflow configuration. Use the spreadsheet structure from the original project rather than inventing new column names.

## 4. Configure n8n Credentials

In n8n, configure the required credentials:

### Google Sheets

1. Open the Credentials section in n8n.
2. Create or connect a Google Sheets OAuth2 credential.
3. Authorize access to the spreadsheet.
4. Select the correct spreadsheet and worksheet in each Google Sheets node.

### Groq

1. Create an API key through your Groq account.
2. Add the key using the supported Groq credential type in n8n.
3. Select the credential in the Groq Chat Model node.
4. Verify that the configured model is available to your account.

Never put API keys directly into workflow code or public repository files.

### Gmail

1. Create or connect a Gmail OAuth2 credential in n8n.
2. Authorize the account used for automated emails.
3. Select the credential in the Gmail nodes.
4. Set the appropriate recipient addresses for acknowledgements and reports.

## 5. Import the Workflows

Repeat these steps for each workflow:

1. Open n8n.
2. Create a new workflow or open the workflow import option.
3. Import the relevant JSON file from the `workflows/` folder.
4. Review every Google Sheets node and select the correct spreadsheet and worksheet.
5. Reconnect Google Sheets, Groq, and Gmail credentials.
6. Replace any redacted placeholders with your own configuration values.
7. Save the workflow.

Redacted exports are intended for sharing. They may require reconfiguration before they can run.

## 6. Configure and Test WF1 — Intake & Triage

1. Confirm that the Google Sheets Trigger watches the correct form-response worksheet.
2. Confirm that the duplicate-check node queries `Processed_Submissions`.
3. Verify that the AI processing nodes are connected correctly.
4. Verify that ticket records are appended to `Tickets`.
5. Confirm that the acknowledgement email uses the reporter's email address.
6. Submit a clearly identifiable test complaint using an email address you control.
7. Check the workflow execution, the resulting ticket, the acknowledgement email, and the processed-submission record.

Use synthetic complaint details during testing. Avoid submitting real sensitive student information.

## 7. Configure and Test WF2 — Escalation & Resolution

1. Import the WF2 JSON export.
2. Reconnect all required credentials.
3. Review the escalation threshold and any settings sourced from `Config`.
4. Verify owner lookup and recipient mappings.
5. Verify the resolution trigger and status-update logic.
6. Test each branch with synthetic tickets before enabling automated notifications.

The exact setup depends on the current WF2 export. Confirm the node configuration and test results rather than assuming every planned behavior is already connected.

## 8. Configure and Test WF3 — Weekly Insights

1. Confirm that the Google Sheets node reads the `Tickets` worksheet.
2. Confirm that the Code node receives the ticket rows.
3. Check that the Groq Chat Model and Basic LLM Chain credentials are configured if those nodes are used.
4. Verify the report recipient in the `Build Readable Email` Code node.
5. Confirm that the Gmail node uses the correct credential.
6. Run the workflow manually using test data.
7. Inspect the generated report and verify that its counts and priority ordering match the spreadsheet.
8. Confirm that the email arrives at the intended recipient.

The exported WF3 code builds an HTML report directly from calculated ticket data. Verify the actual connections in n8n before assuming the separate LLM Chain is part of the report-sending path.

The scheduled trigger is configured for Monday at 9:00 AM. Check the workflow timezone in n8n before relying on the scheduled run.

## 9. Enable Automation

After successful testing:

1. Confirm credentials and recipients.
2. Confirm that test data is excluded or removed according to the project's testing policy.
3. Review the workflow's error handling and notification behavior.
4. Activate the workflows that should run automatically.
5. Submit a test complaint and verify the end-to-end result.

Do not enable live escalation or student notifications until the recipients, thresholds, and status-update behavior have been verified.

## 10. Troubleshooting

### Google Sheets returns no rows

- Check the selected spreadsheet and worksheet.
- Confirm that the expected records exist.
- Verify column headers and filter conditions.

### Groq requests fail

- Check the selected credential.
- Verify that the API key is valid.
- Confirm model availability and account limits.

### Gmail does not send

- Check the Gmail credential and authorization.
- Confirm that the recipient field contains a valid email address.
- Inspect the n8n execution for node errors.

### Tickets are not created

- Inspect the trigger execution.
- Review the duplicate-check result.
- Confirm the AI-processing and ticket-append connections.

### The weekly report is incorrect

- Check the input rows and timestamp formats.
- Verify ticket status and severity values.
- Review the priority rules in the Code node.
- Compare the report with the calculated data before distributing it.

## 11. Security Checklist

- Never commit API keys, OAuth tokens, passwords, or private student data.
- Use redacted workflow exports in public repositories.
- Keep spreadsheet sharing restricted to authorized people.
- Use test records for public demonstrations.
- Verify all automated email recipients before activating workflows.

## 12. Final Verification

Before considering the deployment ready, confirm that:

- [ ] Credentials are connected.
- [ ] Spreadsheet IDs and worksheet selections are correct.
- [ ] Required column headers match the workflow mappings.
- [ ] WF1 creates a ticket and sends the expected acknowledgement.
- [ ] Duplicate-check behavior has been tested.
- [ ] WF1 has also been tested for escalation and resolution.
- [ ] WF3 produces a correct report and sends it to the intended recipient.
- [ ] Scheduled triggers use the intended timezone.
- [ ] No secrets or private student data are present in the public repository.
