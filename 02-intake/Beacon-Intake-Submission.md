
# Northstar Financial: AI Use-Case Intake Submission

**Fictional case study. Northstar Financial, Meridian Language Systems, and all figures are invented for portfolio purposes.**

**Form ID:** NF-AIG-001 v1.0 | **Intake ID:** NF-AI-2026-014 | **System:** Beacon (Application Review Assistant)

**Status:** Accepted to AI inventory with open items (see Open Items Register)

## Section A: Submission record

| Field | Entry |
|---|---|
| Intake ID | NF-AI-2026-014 |
| Submission date / version | 2026-03-16 / v1.0 |
| Submitted by | Director, Credit Analytics Engineering, for Consumer and Small Business Lending |
| Lifecycle stage at submission | Build |
| Target deployment date | Pilot 2027-01-18 (two adjudication teams, Ontario applicants only). Wider rollout not before 2027-06-01 and only after passing the approval gate. |

Retroactive intake: No.

## Section B: Ownership and accountability

| Field | Entry |
|---|---|
| AI system name | Beacon (Application Review Assistant) |
| Business owner | SVP, Consumer and Small Business Lending |
| Technical owner | Director, Credit Analytics Engineering |
| Model owner (per OSFI E-23 model risk definitions) | Head of Credit Risk Analytics |
| Business line / cost centre | Consumer and Small Business Lending, unsecured credit |
| Second-line contacts consulted | Model Risk Management, Privacy Office, Compliance. Legal and Cybersecurity not yet consulted (see OI-02, OI-04, OI-09). |

## Section C: Purpose and business context

| Field | Entry |
|---|---|
| Problem being solved and current process | Adjudicators manually read every unsecured loan and line-of-credit application and its supporting documents. Handling is slow and inconsistent between adjudicators. Applications are worked in arrival order with no prioritization by complexity or risk. |
| Intended purpose (one sentence) | Beacon assists credit adjudicators by summarizing application documents and recommending a review path (Standard Review, Enhanced Review, or Recommend Decline for Review) so that adjudicator attention goes where it is most needed. |
| Explicitly out-of-scope uses | Issuing final credit decisions. Setting pricing or credit limits. Use on secured lending, mortgages, or commercial lending. Use in collections or marketing. Use in employee evaluation, including ranking adjudicators. |
| Expected benefit and how it will be measured | Lower average handling time and more consistent review depth. Measured against the pre-pilot baseline for handling time, override rate, and loss rate. No target is accepted if fairness or override indicators breach agreed thresholds. |
| Non-AI alternative considered, and why rejected | (1) Rules-based triage on income and bureau thresholds: tested, but it missed complex files and produced rigid routing. (2) Hiring more adjudicators: higher cost and does not fix inconsistency. Both remain the fallback. |
| New, replacement, or extension? | New. Beacon sits alongside the existing loan origination system and does not replace any current model. |

## Section D: Users and affected individuals

| Field | Entry |
|---|---|
| Internal users | Credit adjudicators (about 220) and their team leads |
| Training provided to users | Planned before pilot: product training, and training on model limitations and automation bias. Not yet designed. |
| **[T]** Affected individuals | Retail customers; Small business |
| Estimated individuals affected per year | About 180,000 applications per year at full rollout. Pilot volume is roughly 10 percent. |
| **[T]** Includes vulnerable or protected groups? | Possible. Applicants include newcomers with thin credit files, older applicants, and applicants in rural and lower-income areas. Protected characteristics are not collected, so the exact composition is unknown. |
| **[T]** Are affected individuals aware AI is involved? | No. Current application disclosures do not mention AI. See OI-01 and OI-09. |

## Section E: Decisions influenced and automation

| Field | Entry |
|---|---|
| Decisions influenced | Which review path an application receives, and the adjudicator's final approve or decline decision on unsecured loans and lines of credit. |
| **[T]** Decision type | Recommendation on an eligibility, pricing, or access decision |
| **[T]** Degree of automation | Human reviews every output |
| **[T]** Effect on the individual if the output is wrong | Denial of a material service or right |
| Can an affected individual contest or appeal? | Yes, through the existing credit decision review and complaints process. Adverse-action reasons must come from the adjudicator's own assessment and may not cite the Beacon score alone. |
| Is an adverse outcome reversible? | Yes with effort. The applicant can reapply or request review, but a bureau inquiry is recorded and there is delay. |

Note from the submitter: the Recommend Decline path is the highest-risk output. If adjudicators follow it at very high rates, Beacon becomes the de facto decision-maker. This is flagged for the oversight design.

## Section F: Data

| Field | Entry |
|---|---|
| Data sources | Internal application data; internal account history for existing customers; credit bureau data; applicant-supplied documents (pay stubs, bank statements); adjudicator notes. No synthetic data in production. |
| Training, validation, and inference data | Training: about six years of historical applications and outcomes (about 1.1M records). Validation: out-of-time holdout of the most recent 12 months. Inference: live application data plus uploaded documents at submission. |
| **[T]** Personal information involved | Sensitive financial or identity data |
| **[T]** Volume | Over 1M (training set). Inference is about 180k applications per year. Highest applicable band selected. |
| Sensitive attributes or likely proxies | Race and ethnicity are not collected. Present in source data: age, name, preferred language, marital status, postal code (FSA), employer. Design intent is to exclude age, name, language, and marital status from model features. FSA and employer are under review as proxy risks. Feature exclusion is not yet verified (OI-03). |
| Lawful basis and consent (PIPEDA; Law 25 for Quebec residents) | Consent is collected through the credit application. The consent wording predates AI. Whether it covers model training and LLM processing of documents is UNKNOWN (OI-01). Pilot excludes Quebec applicants until Law 25 review is complete. |
| Purpose limitation | Historical application data collected for credit adjudication is being reused for model training. Privacy Office to confirm this is a consistent purpose (OI-01). |
| Data retention and deletion | Model inputs, scores, and reason codes retained to match credit record retention (assumed 7 years, Privacy to confirm). LLM vendor retention: see Section G. |
| Data residency and cross-border transfer | All Northstar hosting and the vendor endpoint are in a Canadian region. Vendor sub-processor locations are UNKNOWN (OI-05). |
| Data lineage documented? | Partial. Training data lineage is documented. Feature-level lineage for bureau attributes is incomplete. |
| Known data quality or representativeness gaps | Past approval decisions reflect past adjudicator judgment, including any historical bias. Declined applicants have no repayment outcome (reject inference problem). Thin-file, newcomer, rural, and Quebec applicants are underrepresented. Degree not yet quantified. |

## Section G: Model and technology

| Field | Entry |
|---|---|
| Model type | Hybrid: classical ML (Review Priority Model) plus LLM (Document Summarization Assistant) |
| Build type | Review Priority Model: built in-house. Summarization Assistant: vendor product. Workbench: built in-house. |
| Model or provider and version | Review Priority Model: gradient-boosted decision trees, v0.9 candidate. Summarization: Meridian Language Systems (fictional vendor). The model version is pinned at contract, and vendor change-notice terms are under negotiation (OI-04). |
| Hosting location and tenancy | Review Priority Model: Northstar private cloud tenancy, Canadian region. LLM: vendor-hosted dedicated tenancy, Canadian region. |
| Does any component use an LLM or generative AI? | Yes |
| If yes: prompt data, vendor retention, vendor training | Prompts contain pay stubs, bank statements, and adjudicator notes (names, addresses, employer, transactions). Vendor default retention is 30 days for abuse monitoring. A zero-retention addendum has been requested but not agreed (OI-04). The contract draft prohibits use of Northstar data for vendor training. This is not yet verified. |
| LLM tool use, write access, or retrieval | None. No tool calling. No write access to any system. No retrieval beyond the documents of the single application under review. Output is shown to the adjudicator only. |
| Integration points | Loan origination system; document management system; credit bureau interface; model monitoring platform; identity and access management; security logging (SIEM). |
| Explainability method available | Review Priority Model: reason codes plus post-hoc explanation (SHAP). LLM summaries: not explainable, so each summary statement must cite its source document location. |

## Section H: Human involvement

| Field | Entry |
|---|---|
| Who reviews outputs, and override authority | Credit adjudicators with delegated lending authority. They can override any Beacon recommendation, and only they can approve or decline. |
| Is the reviewer qualified to challenge the output? | Yes on credit judgment. No on model limitations until training is built. |
| Time available per review | Current average about 25 minutes per file (submitter estimate, to be measured). Beacon targets about 20 percent reduction. Time pressure is a known automation bias risk. |
| Are overrides tracked and reviewed? | Not yet. Workbench capture of override and rationale is required before pilot (OI-06). |
| How is over-reliance prevented? | Proposed, not yet approved: mandatory written rationale when following or overriding Recommend Decline, blind re-review of a random sample, and override-rate monitoring. To be designed in the Human Oversight Framework. |

## Section I: Regulatory and legal exposure

| Field | Entry |
|---|---|
| **[T]** Regulated activity | Credit decisioning |
| Applicable regimes | OSFI E-23 (model risk; confirm scope and effective date with Legal); PIPEDA; Law 25 (Quebec, excluded from pilot); Canadian Human Rights Act and provincial human rights codes (Legal to confirm which apply by province); provincial consumer credit law; FCAC guidance |
| EU AI Act benchmark classification (informational only) | High-risk (Annex III, creditworthiness evaluation of natural persons). Beacon's prioritization role is arguably in scope, so it is treated as high-risk for design purposes. Application timelines for high-risk obligations have been under revision: verify current status. Not a legal obligation for Northstar. |
| Privacy Impact Assessment | Required. Not started (OI-02). |
| Regulator notification or engagement | UNKNOWN. Compliance and Legal to advise on OSFI engagement (OI-09). |
| Jurisdiction-dependent items needing legal confirmation | (1) Law 25 notice requirement for automated decisions, and whether Beacon's assistive role triggers it. (2) Lawfulness of inferring protected attributes for fairness testing under PIPEDA. (3) Applicability and timing of OSFI E-23. No federal AI statute currently applies (AIDA did not pass). |

## Section J: Potential harms

| Harm | Rating | Rationale |
|---|---|---|
| **[T]** Financial harm to individuals | High | Wrongly declined or wrongly approved applicants face lost access to credit or unaffordable debt. |
| **[T]** Discrimination or unfair treatment | High | Model trained on historical decisions, with likely proxies for protected groups and no direct demographic data to test with. |
| **[T]** Privacy harm or data exposure | High | Full financial documents are processed by a third-party LLM. |
| **[T]** Harm to individual rights or access to essential services | Moderate | Unsecured credit is important but not an essential service, and alternatives exist. |
| Financial loss to Northstar | Moderate | Credit losses if the model mis-prioritizes risk, and remediation cost. |
| Reputational or regulatory harm to Northstar | High | A discrimination finding or a privacy incident would draw supervisory and public scrutiny. |
| Operational disruption | Moderate | Adjudication would slow if Beacon were switched off, but manual fallback exists. |

## Section K: Fairness and explainability

| Field | Entry |
|---|---|
| Groups for which differential outcomes are a risk | Indigenous applicants; racialized applicants; newcomers to Canada; women; older applicants; applicants with disabilities; applicants in rural or lower-income postal areas. |
| Fairness testing method | Protected attributes are not collected, so testing relies on proxies: geographic analysis against census demographics, name-based analysis, and synthetic matched-pair tests. These methods are imperfect and must never be used to make individual decisions. Legal to confirm lawfulness (OI-09). |
| Fairness metrics and thresholds proposed | Compare approval, Enhanced Review, and Recommend Decline rates across proxy-inferred groups. A screening ratio of 0.80 is proposed as an investigation trigger only. It is a US employment heuristic and not a Canadian legal standard. Thresholds are to be set by the AI Governance Committee (OI-07). |
| Who must understand an output | Adjudicator (plain-language reason codes); applicant (adverse-action reasons given by the adjudicator); auditor and regulator (model documentation and testing evidence). |
| Can reasons for an individual outcome be produced on request? | Review Priority Model: yes. LLM summaries: partially, via source citations. |

## Section L: Security

| Field | Entry |
|---|---|
| Threat model completed? | In progress |
| Access controls and privileged access | Role-based access through identity management. Privileged access for model engineers through privileged access management. Vendor staff have no access to production data (to be verified, OI-04). |
| Adversarial risks considered | Training data poisoning; model extraction; evasion through manipulated inputs or altered documents; prompt injection through uploaded documents (OWASP LLM guidance); insecure output handling; sensitive information disclosure. |
| Logging of inputs, outputs, and decisions | Inputs, scores, reason codes, LLM prompts and outputs (with personal information controls), and adjudicator actions are all logged. |
| Vendor security assessment status | Not started (OI-04). |
| Incident response integration | Beacon will use the enterprise security incident process. No AI-specific playbook exists yet. |

## Section M: Performance, testing, and monitoring

| Field | Entry |
|---|---|
| Success and failure metrics | Success: reduced handling time, stable loss rates, override rates within agreed bounds, fairness indicators within thresholds. Failure: threshold breach on any fairness or performance indicator. |
| Validation plan and independent validator | Independent validation by Model Risk Management (second line) before pilot, including a fairness assessment and a review of the LLM summarization accuracy. |
| Baseline (current human process) performance | Average handling time about 25 minutes (estimate). Decision consistency between adjudicators is UNKNOWN (OI-08). |
| Drift and performance monitoring | Monthly feature and score drift checks. Quarterly performance backtesting as repayment outcomes mature. Outcomes lag 6 to 12 months, so early monitoring relies on drift and override data. |
| Retraining triggers and change control | Annual scheduled review, plus triggers on drift or fairness threshold breach. All changes go through the model change process with re-validation. |
| Monitoring owner | Head of Credit Risk Analytics (first line), with oversight by Model Risk Management and the AI Governance Office. |

## Section N: Third parties and resilience

| Field | Entry |
|---|---|
| Vendors and sub-processors | Meridian Language Systems (LLM, fictional); Northstar's existing Canadian-region cloud provider; the existing external credit bureau (not AI-related). Vendor sub-processors UNKNOWN (OI-05). |
| Contractual controls | Requested and under negotiation: audit rights, data use restrictions, 24-hour incident notice, 30-day model change notice. None are agreed yet (OI-04). |
| Concentration risk | UNKNOWN. The same vendor is shortlisted for another Northstar use case (OI-10). |
| Fallback if the AI is unavailable or must be switched off | The LLM summarization can be disabled independently of the Review Priority Model using a feature flag. Full fallback is the current manual queue. A runbook is required before pilot. |
| Exit and retirement plan | 90-day exit target. Model artifacts and logs retained per retention policy. The contract requires vendor data deletion with a certificate. |

## Section O: Attestation and sign-off

| Role | Name | Decision | Date |
|---|---|---|---|
| Business owner | SVP, Consumer and Small Business Lending | Attested | 2026-03-16 |
| Technical owner | Director, Credit Analytics Engineering | Attested | 2026-03-16 |
| AI Governance Office (validation) | AI Governance Analyst | Accepted to inventory with open items | 2026-03-20 |
| Assigned risk tier | AI Governance Office | High (score 16). See 03-risk-assessment/Risk-Tiering-Methodology.md, section 8 | 2026-03-20 |

## Open Items Register


| ID | Open item | Owner | Follow-up date |
|---|---|---|---|
| OI-01 | Confirm application consent wording covers model training and LLM processing; assess purpose limitation | Privacy Office and Legal | 2026-04-15 |
| OI-02 | Complete Privacy Impact Assessment | Privacy Office | 2026-05-01 |
| OI-03 | Feature audit: verify sensitive attributes are excluded and assess FSA and employer as proxies | Credit Analytics | 2026-04-30 |
| OI-04 | Vendor security assessment, zero-retention addendum, and contract terms | Cybersecurity and Procurement | 2026-05-15 |
| OI-05 | Obtain vendor sub-processor list and locations | Procurement | 2026-04-30 |
| OI-06 | Build override and rationale capture into the Workbench | Engineering | 2026-06-01 |
| OI-07 | Set fairness metrics and thresholds | AI Governance Committee | 2026-05-15 |
| OI-08 | Measure baseline handling time and decision consistency | Credit Operations | 2026-05-01 |
| OI-09 | Legal opinion: Law 25 notice, regulator engagement, and inference of protected attributes for testing | Legal | 2026-04-30 |
| OI-10 | Check vendor concentration risk across Northstar | Vendor Risk | 2026-05-15 |
