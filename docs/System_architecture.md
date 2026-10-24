# CampusPulse — System Architecture

## 1. Overview

CampusPulse uses n8n Cloud to connect Google Forms, Google Sheets, Groq AI, and Gmail into an automated campus complaint management workflow.

The system is organized around three workflows:

- **WF1 — Intake & Triage:** Processes incoming complaints, checks for duplicate submissions, creates tickets, and sends acknowledgement emails.
- **WF2 — Escalation & Resolution:** Intended to support follow-up, escalation, and complaint resolution.
- **WF3 — Weekly Insights:** Reads ticket records, calculates ticket statistics, prioritizes active issues, and prepares a weekly report.

## 2. Technology Components

| Component | Responsibility |
|---|---|
| Google Forms | Collects student complaint submissions |
| Google Sheets | Stores form responses, tickets, owner information, configuration, and processed-submission records |
| n8n Cloud | Runs the automation workflows and connects the services |
| Groq AI | Supports AI-assisted complaint classification and report generation |
| Gmail | Sends acknowledgement emails and automated reports |

## 3. WF1 — Intake & Triage

### Processing flow

1. A student submits a complaint through Google Forms.
2. The response is added to the form-response spreadsheet.
3. The n8n Google Sheets trigger detects the new row.
4. The workflow creates a normalized submission key using the reporter's email, complaint description, and location.
5. The workflow checks `Processed_Submissions` for a matching key.
6. A submission that passes the duplicate check continues to AI-assisted triage.
7. The workflow processes the complaint and prepares structured ticket information.
8. The ticket is appended to the `Tickets` sheet.
9. Gmail sends an acknowledgement to the reporter.
10. The workflow records the processed submission in `Processed_Submissions`.

### Duplicate-check limitation

The submission key is based on email, description, and location. Consequently, an identical complaint from the same email and location can be treated as a duplicate, even if it is submitted again later. Different descriptions, locations, or email addresses can produce different keys.

## 4. WF2 — Escalation & Resolution

WF2 is intended to handle complaint follow-up, escalation, and resolution.

The planned behavior includes routing urgent complaints to the appropriate responsible person, checking unresolved tickets against an escalation threshold, and supporting a resolution action.

The exact behavior depends on the current WF2 implementation and its configured credentials, spreadsheet columns, and triggers. Confirm that each branch works before treating it as production-ready.

## 5. WF3 — Weekly Insights

WF3 can be started manually or by a weekly schedule configured for Monday at 9:00 AM in the workflow's configured timezone.

### Processing flow

1. The workflow reads records from the `Tickets` sheet.
2. The Code node excludes ticket IDs beginning with `CP-TEST-` from its metrics.
3. It separates resolved tickets from active tickets.
4. It calculates ticket totals, category counts, severity counts, average resolution time where valid timestamps are available, and escalation counts.
5. It identifies potential urgent issues using severity and safety-related keyword rules.
6. It identifies issues open for more than seven days.
7. It sorts active tickets by priority: urgent, overdue, then normal.
8. The workflow builds an HTML email containing active issues, suggested next actions, and weekly statistics.
9. Gmail sends the report to the configured recipient.

### Important reporting notes

- Priority rules are based on the workflow's code and should not be interpreted as a substitute for human safety assessment.
- The repeat-complaint rule groups eligible active issues by normalized location and issue family and counts distinct email addresses. It marks a group as repeated when the count exceeds ten.
- The average resolution time requires valid creation and resolution timestamps.
- The report's suggested actions are recommendations; generating the report does not itself resolve tickets or change their status.

## 6. Data Storage

The Google Sheets database contains these tabs:

- `Form responses` — incoming form submissions.
- `Tickets` — complaint records used by the workflows.
- `Owners` — intended storage for responsible-person information.
- `Config` — intended storage for workflow configuration values.
- `Processed_Submissions` — records used by WF1's duplicate check.

Column names and tab names must match the configuration in the imported workflows.

## 7. Security and Configuration

Before deploying CampusPulse:

- Connect Google Sheets and Gmail credentials in n8n.
- Configure the Groq API credential securely in n8n.
- Set the correct spreadsheet and worksheet references.
- Verify report recipient and escalation email addresses.
- Do not commit API keys, passwords, OAuth tokens, private student information, or unredacted workflow exports to a public repository.
- Use synthetic complaints when demonstrating the project publicly.

## 8. Reliability and Limitations

- AI-generated categories and summaries may be imperfect.
- Keyword-based urgency rules may miss risks or flag harmless reports.
- Duplicate detection can suppress a later identical submission from the same reporter.
- Automated emails depend on valid credentials and recipient addresses.
- Weekly metrics depend on consistent ticket fields and valid timestamps.
- Campus staff remain responsible for investigating and resolving reported issues.

## 9. Architecture Summary

Google Forms collects complaints, Google Sheets stores the records, n8n coordinates processing and follow-up, Groq AI supports classification and reporting, and Gmail communicates updates.

CampusPulse is designed to improve visibility and consistency in campus complaint handling, while leaving real-world investigation and resolution to the appropriate campus personnel.
