# CampusPulse

**AI-Powered Campus Complaint Triage and Escalation System**

CampusPulse helps colleges receive, classify, prioritize, and route student complaints and feedback.

## Problem

Campus issues such as hostel maintenance, classroom equipment failures, and safety hazards can be difficult to track and escalate consistently.

## Solution

CampusPulse uses Google Forms, Google Sheets, n8n, and Groq to process student submissions, classify issues, create tickets, and send acknowledgement emails. High-priority issues can be routed for escalation.

## Architecture

- **Input:** Google Forms
- **Database:** Google Sheets
- **Automation:** n8n
- **AI classification:** Groq
- **Notifications:** Gmail

## Workflows

- **WF1 — Intake and Triage:** Receives submissions, classifies reports, creates tickets, and acknowledges students.
- **WF2 — Escalation:** Routes issues to responsible owners and escalates overdue tickets.
- **WF3 — Weekly Summary:** Summarizes complaint trends for campus authorities.

*Update the workflow descriptions to reflect the features that are implemented and tested.*

## Repository Structure

- `workflows/` — Exported n8n workflow JSON files
- `docs/` — Architecture, setup, and testing documentation

## Setup

See [`docs/setup-guide.md`](docs/setup-guide.md) for configuration instructions.

## Security

Do not commit API keys, credentials, personal student information, or production data. Use environment-specific credentials in n8n.

## Project Status

Hackathon project in development.
