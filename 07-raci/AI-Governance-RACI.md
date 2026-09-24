# Northstar Financial: AI Governance RACI for Beacon

**Fictional case study. Northstar Financial and all figures are invented for portfolio purposes.**

**Document ID:** NF-AIG-004 | **Version:** 1.0 (draft for AI Governance Committee approval) | **Owner:** AI Governance Office (2nd line) | **Applies to:** Beacon (Intake NF-AI-2026-014), risk tier High

## 1. Purpose and how to read it

This RACI shows who does what across Beacon's life, from intake to retirement. It is written for a High-tier system (section 6 shows what changes at other tiers). Each row is one activity and has exactly one Accountable role, so there is always one place to go for a decision.

| Code | Meaning |
|---|---|
| R | Responsible: does the work |
| A | Accountable: owns the outcome and makes or signs the decision. One per row |
| A/R | Accountable and also does the work |
| C | Consulted: gives input before the decision or action (two-way) |
| I | Informed: told after the decision or action (one-way) |
| - | No role in this activity |

## 2. Columns

The twelve columns are functions and not named people. Named role-holders and delegations of authority belong in a separate register.

| Code | Function | Includes | Line of defence |
|---|---|---|---|
| BX | Board / executive leadership | Board Risk Committee, Executive Risk Committee, Chief Risk Officer | Oversight |
| GC | AI Governance Committee | Cross-functional committee set up under the AI governance charter | Governance body |
| RK | Risk | AI Governance Office, Model Risk Management, Vendor Risk | 2nd |
| CP | Compliance | Chief Compliance Officer, complaints team | 2nd |
| LG | Legal | Legal counsel | 2nd (advisory) |
| PV | Privacy | Chief Privacy Officer, Privacy Office | 2nd (also operates controls) |
| CS | Cybersecurity | CISO, security operations | Shared (operates controls and advises) |
| DS | Data Science | Credit Risk Analytics. The model owner is the Head of Credit Risk Analytics | 1st |
| EN | Engineering | Credit Analytics Engineering | 1st |
| PR | Product | Product management for Beacon and the Workbench | 1st |
| BO | Business owner | SVP Consumer and Small Business Lending, Head of Credit Adjudication, Credit Operations | 1st |
| IA | Internal Audit | Internal Audit | 3rd |

## 3. Design rules

1. **One Accountable role per row.** If two roles both feel accountable, nobody is. Where the same role also does the work it is marked A/R.
2. **Builders do not approve or validate their own work.** Validation (TST-2) and re-validation (CHG-2) are A/R for Risk, and release of a material change (CHG-3) is approved by the AI Governance Committee. Data Science and Engineering are never Accountable for either.
3. **Internal Audit stays independent.** It is A/R only for testing controls (TST-5). Everywhere else it is Informed or absent, and it is never Consulted or Responsible in designing a control it will later test.
4. **The first line owns the risk.** The business owner is Accountable for the intake attestation, adjudicator training, oversight quality, remediation, scope changes and retirement (INT-1, DEP-5, MON-3, INC-6, CHG-6, RET-1).
5. **Stopping Beacon is a named decision.** INC-3 gives the decision to the Chief Risk Officer and lets the Head of Credit Adjudication or the CISO act alone on defined triggers, so nobody waits for a meeting while harm continues.
6. **Approval authority rises with the tier.** Low and Moderate approvals sit with the AI Governance Office (with the business owner for Moderate), High with the AI Governance Committee and Critical with the Executive Risk Committee (section 6).

## 4. RACI matrix

Rows are grouped by lifecycle area. The Ref column points to the controls in the Control Matrix (CTL) or to the artifact that defines the activity.

### Intake

| ID | Activity | Ref | BX | GC | RK | CP | LG | PV | CS | DS | EN | PR | BO | IA |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| INT-1 | Complete and attest the intake form | Intake form NF-AIG-001 | - | - | C | C | C | C | C | C | R | R | A | - |
| INT-2 | Validate the intake, create the inventory record and track open items to closure | OI-01 to OI-10 | I | R | A/R | C | R | R | R | R | R | C | R | I |

### Risk classification

| ID | Activity | Ref | BX | GC | RK | CP | LG | PV | CS | DS | EN | PR | BO | IA |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| CLS-1 | Score the impact and amplifiers, assign the tier and reassess it after a trigger event | Methodology NF-AIG-002 | - | I | A/R | C | C | C | C | C | C | C | C | I |
| CLS-2 | Raise a tier | Methodology section 6 | - | I | A/R | - | - | - | - | - | - | - | I | - |
| CLS-3 | Lower a tier (Committee only, with written rationale) | Methodology section 6 | I | A | R | - | C | - | - | C | - | - | C | I |

### Approval

| ID | Activity | Ref | BX | GC | RK | CP | LG | PV | CS | DS | EN | PR | BO | IA |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| APR-1 | Approve the pilot and each rollout expansion, including the residual risks accepted | Methodology section 7, CTL-04 | I | A | R | C | C | C | C | C | C | C | R | I |
| APR-2 | Sign off at each gate that no blocking legal item is open | CTL-27 | - | I | I | C | A/R | C | - | - | - | - | I | - |
| APR-3 | Complete the Privacy Impact Assessment and sign it off | CTL-18 | - | I | C | C | C | A/R | C | C | C | C | C | I |
| APR-4 | Decide whether and how to engage OSFI and other regulators | CTL-29 | A | C | C | R | C | - | - | - | - | - | C | I |
| APR-5 | Set fairness metrics and thresholds | OI-07, CTL-02, CTL-03 | I | A | R | C | C | C | - | C | - | - | C | I |

### Testing

| ID | Activity | Ref | BX | GC | RK | CP | LG | PV | CS | DS | EN | PR | BO | IA |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| TST-1 | Verify feature exclusions, audit proxies and run pre-deployment fairness testing | CTL-01, CTL-02 | - | C | C | - | C | C | - | A/R | C | - | I | I |
| TST-2 | Independently validate Beacon before the pilot | CTL-04 | - | I | A/R | - | - | - | - | C | C | - | I | I |
| TST-3 | Threat-model and red-team the pipeline | CTL-23, CTL-24 | - | I | C | - | - | - | A/R | C | R | I | I | I |
| TST-4 | Build the summary test set and set accuracy tolerances | CTL-30 | - | I | R | - | - | C | - | A/R | C | - | C | I |
| TST-5 | Independently test the controls using the matrix test procedures | Control Matrix | I | I | C | C | C | C | C | C | C | C | C | A/R |

### Deployment

| ID | Activity | Ref | BX | GC | RK | CP | LG | PV | CS | DS | EN | PR | BO | IA |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| DEP-1 | Build the Workbench capture, logging and flags, and release approved versions | CTL-06, CTL-14, CTL-15, CTL-31, OI-06 | - | I | C | - | - | C | C | R | A/R | R | C | - |
| DEP-2 | Complete vendor due diligence, security assessment and contract terms, and repeat them annually | CTL-19 | - | I | A/R | I | R | R | R | - | C | C | C | I |
| DEP-3 | Verify the vendor's data-use terms in operation | CTL-16 | - | I | R | - | C | A/R | C | - | R | - | - | I |
| DEP-4 | Test the switch-off runbook and the exit plan | CTL-21 | - | I | R | - | C | C | C | C | A/R | C | R | I |
| DEP-5 | Train adjudicators and grant Workbench access | CTL-09 | - | I | C | - | - | - | - | R | R | C | A/R | I |

### Monitoring

| ID | Activity | Ref | BX | GC | RK | CP | LG | PV | CS | DS | EN | PR | BO | IA |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| MON-1 | Produce fairness, drift and performance reports | CTL-03, CTL-05, CTL-12 | - | I | I | - | - | - | - | A/R | R | - | I | I |
| MON-2 | Review monitoring results, challenge and escalate, report to the Committee and leadership, and keep the register and matrix current | Dashboard, Risk Register, Control Matrix | I | I | A/R | R | C | R | R | R | R | C | R | I |
| MON-3 | Monitor oversight quality and run blind re-review, competency checks and fallback drills | CTL-06, CTL-07, CTL-08, CTL-15, CTL-34 | - | I | C | - | - | - | - | C | R | - | A/R | I |
| MON-4 | Sample summary quality and citations in live use | CTL-11, CTL-30 | - | I | C | - | - | - | - | A/R | R | - | R | I |
| MON-5 | Monitor privacy (prompt log scans, retention jobs) and handle access requests and complaints | CTL-17, CTL-28 | - | I | I | R | C | A/R | C | - | R | - | R | I |
| MON-6 | Monitor security: access recertification, artifact integrity checks and probing alerts | CTL-22, CTL-24, CTL-25 | - | I | I | - | - | - | A/R | I | R | - | - | I |
| MON-7 | Maintain the obligations register, assess OSFI E-23 alignment and track regulatory change | CTL-27, CTL-29 | - | I | R | A/R | C | C | - | - | - | - | - | I |

### Incidents

| ID | Activity | Ref | BX | GC | RK | CP | LG | PV | CS | DS | EN | PR | BO | IA |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| INC-1 | Maintain the AI incident playbook and run the annual exercise | CTL-26 | I | I | R | C | C | C | A/R | C | C | - | C | I |
| INC-2 | Operate the AI incident intake and triage alerts | CTL-33 | I | I | A/R | C | C | R | R | R | R | - | C | I |
| INC-3 | Decide whether to suspend Beacon or its LLM component | CTL-15, CTL-21, CTL-33 | A | I | R | I | C | C | R | C | R | I | R | I |
| INC-4 | Investigate the incident and find the root cause | CTL-33 | I | I | A/R | C | C | C | C | R | R | - | C | I |
| INC-5 | Assess breach reporting duties and notify regulators or individuals (Legal to confirm duties) | R-07, R-15 | I | I | C | R | R | A/R | C | - | - | - | C | I |
| INC-6 | Remediate and verify the fix | CTL-33 | I | I | R | - | C | C | C | R | R | C | A | I |
| INC-7 | Run the post-incident review and update the register and dashboard | CTL-33 | I | I | A/R | C | C | C | C | R | R | C | R | I |

### Model changes

| ID | Activity | Ref | BX | GC | RK | CP | LG | PV | CS | DS | EN | PR | BO | IA |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| CHG-1 | Operate the change process: classify changes, approve minor changes and check that live versions match approved versions | CTL-31, CTL-32 | - | I | C | - | - | - | C | R | A/R | C | C | I |
| CHG-2 | Re-validate a material change and re-run the fairness test before release | CTL-02, CTL-04, CTL-13 | - | I | A/R | - | - | - | - | R | C | - | I | I |
| CHG-3 | Approve the release of a material change | CTL-31 | I | A | R | C | C | C | C | R | R | C | C | I |
| CHG-4 | Detect and accept vendor model updates using the canary test set | CTL-20 | - | I | R | - | - | - | I | C | A/R | - | I | I |
| CHG-5 | Decide to retrain or redevelop and manage the retraining path | CTL-13 | - | I | C | - | - | C | - | A/R | R | - | C | I |
| CHG-6 | Extend Beacon to a new population, province, product or use (amended intake and re-tiering) | Intake form, APR-1 | I | C | R | C | C | C | C | C | C | R | A | I |

### Retirement

| ID | Activity | Ref | BX | GC | RK | CP | LG | PV | CS | DS | EN | PR | BO | IA |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| RET-1 | Decide to retire or replace Beacon | Lifecycle (Artifact 8) | I | C | C | C | C | C | C | C | C | R | A | I |
| RET-2 | Execute the exit plan: vendor data return, deletion certificate and return to the manual queue | CTL-21 | I | I | R | I | C | R | R | C | A/R | C | R | I |
| RET-3 | Apply retention and deletion to data, logs and model artifacts | CTL-17 | - | - | I | I | C | A | C | R | R | - | - | I |
| RET-4 | Close the inventory record and file the retirement report | AI inventory | I | I | A/R | - | - | I | - | - | - | - | C | I |

## 5. Decision rights at a glance

| Decision | Decided by (A) | Ref |
|---|---|---|
| Assign the risk tier | AI Governance Office (Risk) | CLS-1 |
| Raise a tier | AI Governance Office (Risk) | CLS-2 |
| Lower a tier | AI Governance Committee, with written rationale | CLS-3 |
| Approve the pilot and rollout expansion (High tier) | AI Governance Committee | APR-1 |
| Set fairness metrics and thresholds | AI Governance Committee | APR-5 |
| Decide on regulator engagement | Executive Risk Committee | APR-4 |
| Suspend Beacon or the LLM component | Chief Risk Officer or delegate. The Head of Credit Adjudication or the CISO may act alone on defined triggers | INC-3 |
| Release a material change | AI Governance Committee | CHG-3 |
| Extend Beacon to a new population, province, product or use | Business owner, after re-tiering by Risk | CHG-6 |
| Retire or replace Beacon | Business owner | RET-1 |

## 6. How the RACI changes by tier

| Tier | Approval (APR-1) | Independent review before deployment | Other differences |
|---|---|---|---|
| Low | AI Governance Office (Risk) | Self-attestation, checked by the AI Governance Office | Most testing and monitoring rows shrink to an annual attestation |
| Moderate | Head of AI Governance (Risk) with business owner co-approval | Second-line desk review | Quarterly metrics review. No Committee involvement |
| High | AI Governance Committee | Independent validation by Model Risk Management (TST-2) | This RACI |
| Critical | Executive Risk Committee. The Committee becomes R and the Board Risk Committee is informed | Independent validation plus Internal Audit review, so IA becomes R in APR-1 | Restricted rollout and weekly monitoring during rollout |

## 7. Checks

- Every row has exactly one A and at least one R (44 rows).
- Accountabilities per function: Risk 11, Business owner 6, Privacy 5, Data Science 5, Engineering 5, AI Governance Committee 4, Cybersecurity 3, Board / executive leadership 2, Compliance 1, Legal 1, Internal Audit 1, Product 0. Total 44.
- Internal Audit is A/R in one row only (TST-5) and is never Consulted or Responsible in any other row.
- Every control owner in the Control Matrix is Accountable or Responsible in a row that references its control, except CTL-10 (see the notes below).
- Rows per lifecycle area: Intake 2, Risk classification 3, Approval 5, Testing 5, Deployment 5, Monitoring 7, Incidents 7, Model changes 6, Retirement 4.

Where the RACI and the Control Matrix need a note:

- **CTL-10** (reason codes and adverse-action reasons) is owned by Data Science in the matrix, but the faithfulness test happens inside independent validation (TST-2) and the adverse-action notice standard sits with the business owner. It has no row of its own.
- **CTL-31** (change control) is owned by Engineering, which is Accountable in CHG-1. Minor changes are approved by the model owner in Data Science, shown as R.
- **CTL-33** (incident intake) names the Chief Risk Officer as owner. The CRO is Accountable for the suspension decision (INC-3), while the AI Governance Office runs the intake queue (INC-2), as the control text says.

## 8. Framework alignment (indicative, verify before citing)

- **NIST AI RMF:** GOVERN 2.1 (roles and responsibilities documented and clear) and GOVERN 2.3 (executive leadership takes responsibility for AI risk decisions). Check the wording against the Playbook.
- **ISO/IEC 42001:** Clause 5.3 (roles, responsibilities and authorities) and Annex A.3 (internal organization). Verify clause references against a licensed copy.
- **EU AI Act (benchmark only):** Article 17 expects an accountability framework within the quality management system, and Article 26 sets deployer duties including human oversight by competent people. Verify current status and timelines.
- **Canadian privacy and prudential expectations:** the PIPEDA accountability principle expects a designated accountable person, which is why Privacy has its own Accountable rows. OSFI expectations on model risk governance and lines of defence apply (Legal to confirm scope and effective dates).

## 9. Limitations

- Only twelve functions are shown. Procurement, HR, Finance, Fraud, Complaints and Credit Operations also have roles in practice and are absorbed into the nearest column (Procurement into Risk and Legal, complaints into Compliance, Credit Operations into Business owner).
- Risk holds 11 of 44 accountabilities, the highest concentration. Some of them (incident investigation in INC-4 and post-incident review in INC-7) are operating work that a mature programme would move to the first line with second-line challenge. In a small AI Governance Office this is also a capacity bottleneck.
- Product has no Accountable rows on purpose. It delivers requirements and Workbench design while risk decisions sit with the business owner and control owners. A reviewer may reasonably ask whether Product should own scope and requirements (CHG-6).
- Privacy and Cybersecurity both operate controls and advise on them. Independent challenge of their controls comes from Internal Audit testing (TST-5) and AI Governance Office review (MON-2), and not from a separate function.
- One decision right is new here: material changes return to the AI Governance Committee (CHG-3). The Control Matrix requires re-validation for material changes but does not name an approver. The Lifecycle Governance artifact will use the same rule.
- This is function-level. Real programmes also keep named role-holders, deputies and delegations of authority, and the RACI is only as good as those are current.
- This is a design and has not been tested with real staff. Workload per function was not estimated.
