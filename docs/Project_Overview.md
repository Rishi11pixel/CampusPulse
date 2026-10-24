# CampusPulse — Project Overview

## 1. Introduction

CampusPulse is an AI-powered campus complaint management and triage system designed to help educational institutions organize student-reported issues, prioritize urgent complaints, and improve follow-up.

## 2. Problem Statement

Campus complaints about maintenance, connectivity, facilities, and safety can be difficult to track when reports are handled manually. Urgent issues may be overlooked, and students may not know whether their complaints have been received or acted upon.

## 3. Proposed Solution

CampusPulse automates complaint intake and uses AI-assisted classification to organize reports. It records tickets, acknowledges submissions by email, and supports prioritization and escalation workflows.

## 4. Key Features

- Complaint intake through Google Forms.
- Centralized ticket storage in Google Sheets.
- AI-assisted categorization and triage using Groq.
- Email acknowledgements through Gmail.
- Priority-based handling of urgent and overdue issues.
- Weekly reporting on active tickets and ticket statistics.
- A workflow foundation for tracking resolution and escalation.

## 5. Technology Stack

- **Google Forms:** Collects complaint submissions.
- **Google Sheets:** Stores complaint and ticket records.
- **n8n Cloud:** Connects services and automates workflows.
- **Groq:** Provides the language model used for AI-assisted processing.
- **Gmail:** Sends acknowledgements and reports.

## 6. Intended Users

- Students submitting campus complaints.
- Campus maintenance and IT teams.
- Campus administrators responsible for reviewing issues.

## 7. Expected Benefits

- More organized complaint tracking.
- Faster identification of potentially urgent reports.
- Consistent acknowledgement of submissions.
- Better visibility into outstanding issues.
- Easier review of ticket patterns and weekly workload.

## 8. Project Scope

CampusPulse supports automated intake, AI-assisted triage, ticket tracking, and reporting. Actual resolution still depends on the responsible campus staff. AI classifications and recommendations should be reviewed where appropriate, especially for safety-related complaints.

## 9. Project Status

This repository contains exported n8n workflow files and project documentation. Setup requirements, credentials, and recipient addresses must be configured for each deployment.
