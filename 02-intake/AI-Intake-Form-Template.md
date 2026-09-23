# Northstar Financial: AI Use-Case Intake Form

**Form ID:** NF-AIG-001 | **Version:** 1.0 | **Owner:** AI Governance Office (2nd line)
**Policy reference:** Interim AI Governance Policy, s. 4 (Inventory and Intake)

**Purpose:** No AI system, including vendor tools, embedded AI features, and pilots, may be procured, built, or deployed without a completed intake. Submission creates the AI inventory record.

**How to complete it:** Items marked **[T]** feed the risk-tiering score and must use the closed options provided. Free-text answers there will be returned. The AI Governance Office validates every submission, and self-declared answers are never final.

## Section A: Submission record

| Field | Entry |
|---|---|
| Intake ID | *(assigned by AI Governance Office)* |
| Submission date / version | |
| Submitted by | |
| Lifecycle stage at submission | Idea / Proof of concept / Build / Vendor evaluation / Live (retroactive) |
| Target deployment date | |

Retroactive intakes are tracked separately. The share of systems found after deployment is a governance KPI.

## Section B: Ownership and accountability

| Field | Entry |
|---|---|
| AI system name | |
| Business owner (accountable executive, VP level or above) | |
| Technical owner | |
| Model owner (per OSFI E-23 model risk definitions) | |
| Business line / cost centre | |
| Second-line contacts consulted (Risk, Privacy, Compliance, Legal, Cyber) | |

## Section C: Purpose and business context

| Field | Entry |
|---|---|
| Problem being solved and current process | |
| Intended purpose (one sentence) | |
| Explicitly out-of-scope uses | |
| Expected benefit and how it will be measured | |
| Non-AI alternative considered, and why rejected | |
| Is the use case new, a replacement, or an extension of an existing system? | |

## Section D: Users and affected individuals

| Field | Entry |
|---|---|
| Internal users (roles, headcount) | |
| Training provided to users | |
| **[T]** Affected individuals | Employees / Retail customers / Small business / Vendors / Public / Other |
| Estimated individuals affected per year | |
| **[T]** Includes vulnerable or protected groups? | No / Possible / Yes (describe) |
| **[T]** Are affected individuals aware AI is involved? | Yes, notified / No / Not applicable |

## Section E: Decisions influenced and automation

| Field | Entry |
|---|---|
| Decisions influenced | |
| **[T]** Decision type | Informational / Prioritization or routing / Recommendation on an eligibility, pricing, or access decision / Fully automated decision |
| **[T]** Degree of automation | Assistive only / Human reviews every output / Human reviews exceptions only / No human review |
| **[T]** Effect on the individual if the output is wrong | Negligible / Inconvenience / Financial harm / Denial of a material service or right |
| Can an affected individual contest or appeal the outcome? How? | |
| Is an adverse outcome reversible? | Yes easily / Yes with effort / No |

## Section F: Data

| Field | Entry |
|---|---|
| Data sources (internal, bureau, vendor, public, synthetic) | |
| Training, validation, and inference data described separately | |
| **[T]** Personal information involved | None / Business contact only / Personal / Sensitive financial or identity data |
| **[T]** Volume | Under 10k records / 10k to 1M / Over 1M |
| Sensitive attributes present, directly or as likely proxies (postal code, name, language, age, marital status) | |
| Lawful basis and consent for this use (PIPEDA; Law 25 where Quebec residents are involved) | |
| Purpose limitation: is any data used beyond its original collection purpose? | |
| Data retention and deletion | |
| Data residency and cross-border transfer | |
| Data lineage documented? | Yes / Partial / No |
| Known data quality or representativeness gaps | |

## Section G: Model and technology

| Field | Entry |
|---|---|
| Model type | Rules / Classical ML / Deep learning / LLM / Generative / Hybrid |
| Build type | Built in-house / Vendor product / Vendor model fine-tuned in-house / Open-source |
| Model or provider and version | |
| Hosting location and tenancy | |
| Does any component use an LLM or generative AI? | Yes / No |
| If yes: what data enters prompts? Are prompts or outputs retained by the vendor? Is data used for vendor training? | |
| LLM: can the model call tools, write to systems, or retrieve from internal data? | |
| Integration points with other Northstar systems | |
| Explainability method available | Inherently interpretable / Post-hoc (e.g., SHAP) / Reason codes / None |

## Section H: Human involvement

| Field | Entry |
|---|---|
| Who reviews outputs, and what authority do they have to override? | |
| Is the reviewer qualified to challenge the output independently? | |
| Time available per review | |
| Are overrides tracked and reviewed? | |
| How is over-reliance on the output prevented? | |

## Section I: Regulatory and legal exposure

| Field | Entry |
|---|---|
| **[T]** Regulated activity | None / Consumer-facing financial service / Credit decisioning / Employment / Other |
| Applicable regimes | OSFI E-23 / PIPEDA / Law 25 (Quebec) / Canadian human rights legislation / Provincial consumer credit law / FCAC guidance / Other |
| EU AI Act benchmark classification (informational, not a legal obligation for Northstar) | Prohibited / High-risk (Annex III) / Limited risk / Minimal |
| Privacy Impact Assessment completed or required? | |
| Regulator notification or engagement needed? | |
| Jurisdiction-dependent items needing legal confirmation | |

## Section J: Potential harms

For each, rate **None / Low / Moderate / High**, with a short rationale.

| Harm | Rating | Rationale |
|---|---|---|
| **[T]** Financial harm to individuals | | |
| **[T]** Discrimination or unfair treatment | | |
| **[T]** Privacy harm or data exposure | | |
| **[T]** Harm to individual rights or access to essential services | | |
| Financial loss to Northstar | | |
| Reputational or regulatory harm to Northstar | | |
| Operational disruption | | |

## Section K: Fairness and explainability

| Field | Entry |
|---|---|
| Groups for which differential outcomes are a risk | |
| Fairness testing method, given whether protected attributes are collected | |
| Fairness metrics and thresholds proposed | |
| Who must be able to understand an output, and at what level (adjudicator, customer, auditor, regulator)? | |
| Can the reasons for an individual outcome be produced on request? | |

## Section L: Security

| Field | Entry |
|---|---|
| Threat model completed? | Yes / In progress / No |
| Access controls and privileged access | |
| Adversarial risks considered (data poisoning, model extraction, evasion, prompt injection, insecure output handling; OWASP LLM Top 10 where applicable) | |
| Logging of inputs, outputs, and decisions | |
| Vendor security assessment status | |
| Incident response integration | |

## Section M: Performance, testing, and monitoring

| Field | Entry |
|---|---|
| Success and failure metrics | |
| Validation plan and independent validator | |
| Baseline (current human process) performance | |
| Drift and performance monitoring approach and frequency | |
| Retraining triggers and change control | |
| Monitoring owner | |

## Section N: Third parties and resilience

| Field | Entry |
|---|---|
| Vendors and sub-processors | |
| Contractual controls (audit rights, data use restrictions, incident notification, model change notice) | |
| Concentration risk: is this vendor critical elsewhere at Northstar? | |
| Fallback if the AI is unavailable or must be switched off | |
| Exit and retirement plan | |

## Section O: Attestation and sign-off

The business owner attests the answers are complete and accurate.

| Role | Name | Decision | Date |
|---|---|---|---|
| Business owner | | Attested | |
| Technical owner | | Attested | |
| AI Governance Office (validation) | | Complete / Returned | |
| Assigned risk tier | | *(from methodology)* | |
