# AI-enabled internal request triage system
Microsoft Power Automate | AI Builder | NLP | Microsoft 365

## Overview
An end-to-end no-code AI automation pipeline that intelligently triages internal workplace requests using natural language processing. The system analyses the sentiment of submitted requests, applies conditional routing logic, and automatically escalates urgent or distressed submissions while eliminating manual processing entirely.
This project demonstrates how pre-trained NLP models can be integrated into real business workflows with appropriate governance and human oversight controls.

## The Problem
Organisations processing large volumes of internal requests face a common challenge:

- Admin staff spend hours manually reading, judging urgency, and forwarding requests
- Urgent or emotionally distressed messages are not always identified promptly
- No consistent process as routing depends on individual judgement
- No audit trail unless manually maintained

**Estimated manual processing time: ~2.5 hours per day for 20 requests**

## The Solution
- Form Submission
- Retrieve Response → AI Sentiment Analysis
- Condition (Negative OR High Urgency?)
    - TRUE  → URGENT email to department
    - FALSE → Standard email to department
- Confirmation email to submitter
- Log to Excel audit trail
**Estimated automated processing time: ~15 minutes per day for 20 requests**
**Estimated time saving: 90%**


![Power Automate](https://img.shields.io/badge/Microsoft-Power%20Automate-0066FF?style=flat&logo=microsoft&logoColor=white)
![AI Builder](https://img.shields.io/badge/AI-Builder-742774?style=flat&logo=microsoft&logoColor=white)
![Status](https://img.shields.io/badge/Status-Complete%20%26%20Tested-1D9E75?style=flat)
![NLP](https://img.shields.io/badge/NLP-Sentiment%20Analysis-FF6B35?style=flat)


## Architecture

### Components

| Component | Tool | Purpose |
|---|---|---|
| Trigger | Microsoft Forms | Captures staff request submissions |
| Data retrieval | Power Automate | Pulls all form field responses |
| AI model | AI Builder (Sentiment Analysis) | NLP classification of message tone |
| Routing logic | Power Automate Condition | Dual-trigger: sentiment OR urgency |
| Notifications | Outlook (V2) | Department routing + submitter confirmation |
| Audit trail | Excel (SharePoint) | Logs all submissions for governance |

### Full flow overview
![Full Flow](screenshots/Workflow.png)

### AI Model Detail

The sentiment analysis uses Microsoft AI Builder's pre-trained NLP model, which applies transformer-based text classification to return one of three outputs:

- **Positive** - constructive or satisfied tone
- **Neutral** - factual, no strong emotional content  
- **Negative** - frustration, dissatisfaction, or urgency detected

The model analyses the free-text description field and the output directly controls the routing decision.

---

## Governance Design

A key design principle of this project is that **AI informs, humans decide**. Governance controls include:

### Dual-trigger condition
The urgent routing pathway fires if:
- AI detects **negative** sentiment, **OR**
- Submitter manually selects **High** urgency

This safeguard addresses a known limitation of sentiment models, a calm but time-sensitive message may not be detected as negative, but the submitter can still flag it correctly.

### Human validation points
- Every output is an email to a human inbox, no autonomous action is taken
- The URGENT flag signals priority but the human decides the response
- No direct action is taken on the submitter's behalf without human involvement

### Audit trail
Every submission is logged to a SharePoint Excel file with:
- Submission timestamp
- Submitter name and department
- Request title and description
- Urgency level selected
- AI sentiment classification
- Routing pathway taken

### Excel audit trail
![Audit Trail](screenshots/Excel%20form%20for%20Audit%20trial.png)

### Data compliance
All data remains within the Microsoft 365 environment. No data is transmitted to third-party services. AI Builder operates within Microsoft's GDPR-compliant cloud infrastructure.

---

## Risk Analysis

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| AI misclassification | Medium | Low | Human reviews all URGENT emails before action |
| False negative (urgent but calm tone) | Medium | Medium | Dual trigger includes manual urgency field |
| Over-reliance on automation | Low | Medium | Regular audit log review, spot checks |
| Sensitive content misrouting | Low | High | Separate confidential pathway (future improvement) |
| Model accuracy on unusual language | Medium | Low | Structured form fields guide clearer submissions |

---

## Test Scenarios

Three test scenarios were run to validate the flow:

**Test 1 - Negative sentiment detected**
> *"We are short-staffed and unable to complete some urgent tasks. Please provide some help."*
- AI output: Negative
- Route: URGENT email

### URGENT email received
![Urgent Email](screenshots/urgent%20email.png)

**Test 2 - Neutral / positive message**
> *"Can you provide some new training on AI"*
- AI output: Positive
- Route: Standard email

 ### Standard email received
![Non Urgent Email](screenshots/non%20urgent%20email.png)

**Test 3 - Calm message, High urgency selected**
> *"Please could someone look at the server issue. It is affecting today's deadline."*
- AI output: Neutral
- Urgency field: High
- Route: URGENT email ✓ (dual trigger working correctly)

 

### Test run - succeeded
![Test Run](screenshots/test%20run.png)
---

## Key Learnings

- Pre-trained NLP models are highly effective for sentiment classification but require human safeguards for edge cases
- Dual-condition logic significantly improves routing reliability over single AI-only decisions
- Governance design (audit trails, human checkpoints, fallback routing) is as important as the automation itself
- No-code tools like Power Automate allow data scientists to deploy AI models into production environments rapidly without infrastructure overhead

---

## Relation to Data Science

This project applies concepts from my MSc Applied Data Science in a practical deployment context:

- **Natural Language Processing** - understanding how transformer-based sentiment models classify text
- **Model limitations and bias** - recognising where pre-trained models can fail and designing mitigations
- **Pipeline design** - structuring data flow from input to output with validation at each stage
- **Responsible AI** - governance, human oversight, GDPR compliance, and audit trails

---

## Tools and Technologies

`Microsoft Power Automate` `AI Builder` `Microsoft Forms` `Outlook` `Excel` `SharePoint` `Natural Language Processing` `Sentiment Analysis` `No-Code Automation` `Microsoft 365`

---

## Files in This Repository

```
├── README.md                                  — This file
├── Workflow_Automation_Design_Document.docx   — Full design document
├── screenshots/
│   ├── full_flow_overview.png                 — Complete flow in Power Automate
│   ├── run_history_succeeded.png              — Successful test run evidence
│   ├── urgent_email_received.png             — URGENT routing email output
│   ├── standard_email_received.png           — Standard routing email output
│   └── excel_audit_log.png                   — Audit trail in Excel
```

---

## Author

Zubeen Khalid
MSc Applied Data Science  
Anglia Ruskin University
ISO 42001 certified

---
