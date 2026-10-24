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

- **WF1 — Intake, Triage and Escalation:** Receives submissions, classifies reports, creates tickets, and acknowledges students. Routes issues to responsible owners and escalates overdue tickets.
- **WF3 — Weekly Summary:** Summarizes complaint trends for campus authorities.

## Repository Structure

- `workflows/` — Exported n8n workflow JSON files
- `docs/` — Architecture, setup, and testing documentation

## Setup

See [`docs/setup-guide.md`](docs/setup-guide.md) for configuration instructions.

## Project Status

Hackathon project in development.
