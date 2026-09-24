# Northstar Beacon: AI risk and governance case study

This is a fictional case study. Northstar Financial, Beacon and every number in this repository are invented.

## What this is

I wanted to show how AI risk and governance work in practice, so I built a full governance program around one system. Northstar is a made-up Canadian bank. Beacon is an AI assistant that helps credit adjudicators review loan applications. It recommends a review path, and a person makes the decision.

The work follows Beacon from intake to retirement: how it is classified, what could go wrong, which controls cover each risk, who owns what, how people stay in charge of the decision, what happens when something breaks, and what leadership sees on a dashboard. Every risk links to controls, and every control has a way to test it.

## Live dashboard

[Open the AI governance dashboard](https://mbuguasally22.github.io/northstar-beacon-governance/10-dashboard/northstar-ai-governance-dashboard.html). It runs on mock data that matches the documents below.

## The documents

| Document | What it is |
|---|---|
| [Intake form (blank template)](02-intake/AI-Intake-Form-Template.md) | The form a team completes before an AI system enters the inventory |
| [Beacon intake submission](02-intake/Beacon-Intake-Submission.md) | The completed form for Beacon, with open items and owners |
| [Risk tiering method](03-risk-assessment/Risk-Tiering-Methodology.md) | How risk is scored and why Beacon lands in the High tier |
| [Risk register](04-risk-register/Risk-Register.csv) | 16 risks with likelihood, impact, controls and monitoring |
| [Control matrix](05-control-matrix/Control-Matrix.csv) | 34 controls with owners, evidence and how an auditor would test each one |
| [Human oversight framework](06-human-oversight/Human-Oversight-Framework.md) | How adjudicators review Beacon's output and when they must override or escalate |
| [Governance RACI](07-raci/AI-Governance-RACI.md) | Who does what from intake to retirement |
| [Lifecycle governance](08-lifecycle/Lifecycle-Governance.md) | Eleven stages, each with a decision gate |
| [Incident case study](09-incident-case-study/Incident-Response-Case-Study.md) | A fairness incident from first signal to post-incident review, with a report |
| [Dashboard specification](10-dashboard/Dashboard-Specification.md) | The 18 metrics behind the dashboard and what each tells leadership |
| Framework mapping | In progress |
| Executive recommendation | In progress |
| Executive summary | In progress |

## Frameworks used

NIST AI RMF, ISO/IEC 42001, the OWASP Top 10 for LLM Applications (2025 edition), and Canadian privacy law and guidance (PIPEDA, Quebec Law 25, OSFI). The EU AI Act is used only as a design benchmark, because it is not a legal obligation for a Canadian bank. The references are my own indicative mapping and not a compliance claim, so check clause numbers against the source documents before relying on them.

## What I would challenge in this work

- Fairness testing without demographic data relies on proxies. It can flag a problem but it cannot prove or rule out discrimination.
- The thresholds are starting values. They have not been tested against real data.
- This is a design. Nothing was built or tried with real staff.
- Several legal points are marked for a lawyer to confirm.

## How this was made

I built this with AI assistance (Claude). I chose the scenario and scope and directed the design.
