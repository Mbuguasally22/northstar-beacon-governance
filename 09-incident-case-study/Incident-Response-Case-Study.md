# Northstar Financial: AI Incident Response Case Study for Beacon

**Fictional case study. Northstar Financial and all figures, dates and events are invented for portfolio purposes.**

**Document ID:** NF-AIG-006 | **Version:** 1.0 (draft for AI Governance Committee approval) | **Owner:** AI Governance Office (2nd line) | **Applies to:** Beacon (Intake NF-AI-2026-014), risk tier High | **Incident ID:** AI-INC-2027-001 (illustrative)

## 1. Purpose and how to read it

This document walks through one incident in Beacon's pilot, from the first signal to the end of post-incident monitoring, and ends with the formal incident report (section 19). It is written in the past tense as a worked example, to show how Northstar's incident controls behave when a fault occurs. It doubles as the scenario for the tabletop exercise that CTL-26 requires before the pilot (section 16).

The scenario: Beacon starts giving materially lower scores and less favourable review paths to applicants from one demographic group, with no change to the model. Dates assume the pilot start in the intake (2027-01-18). Every figure is invented. It is a scenario and not a prediction.

Sections 3 to 13 follow the eleven steps: detection, triage, containment, investigation, root cause, risk assessment, stakeholder escalation, remediation, regulatory and privacy considerations, documentation and post-incident monitoring. Each names the controls (CTL), RACI rows (INC) and gates (G8, G9) it relies on.

## 2. The scenario in brief

| Item | Detail |
|---|---|
| System and stage | Beacon in pilot (gate G7 monitoring stage): two adjudication teams, Ontario applicants only, about 18,000 applications a year, or roughly 350 a week. Tier High |
| The group | Applicants in postal areas (FSAs) where census data shows a high share of recent immigrants, cross-checked with name-based analysis. It is a statistical group inferred for testing (CTL-02). It is not a label on any applicant, and Northstar does not collect ethnicity or immigration status |
| Reference group | All other Ontario pilot applicants |
| Fairness ratio | The group's Standard Review rate divided by the reference group's Standard Review rate. Standard Review is what remains after Enhanced Review and Recommend Decline, so this is the same data as the rates named in R-01, arranged so that a lower ratio means worse treatment. Baseline at pilot start: about 0.95 (59 percent against 62 percent) |
| Formal triggers (R-01, CTL-03) | Ratio below 0.80 in any month, or below 0.90 for two consecutive months. In the pilot the report shows a weekly value and a rolling four-week value. The formal triggers are read on the rolling value, with "month" meaning a four-week period |
| Severity scale | Severity 1: confirmed material harm to individuals at scale, a confirmed breach of a binding obligation, or loss of control of the system. Severity 2: probable or confirmed harm to a group of individuals (including a protected group), or failure of a key control with harm potential, and still ongoing. Severity 3: control failure or near miss with no identified harm. Severity 4: minor and contained. CTL-26 says the playbook sets severity levels. This scale is an illustration |
| Incident ID | AI-INC-2027-001 |

## 3. Detection

Three signals appeared before the incident was opened. All three reached the AI incident intake queue (CTL-33).

| # | Date | Signal | Raised by | Disposition at the time | What it missed |
|---|---|---|---|---|---|
| 1 | Mon 2027-03-29 | Weekly ratio 0.79 (week of Mar 22), rolling four-week 0.89. First weekly value below 0.80 | Weekly fairness report (CTL-03) | AI Governance Office analyst: monitor. About 70 group applications a week, so a single weekly value is noisy (margin about 0.10) | The fault was already two weeks old. On its own the call was reasonable |
| 2 | Report Thu 2027-04-01, closed Fri 04-02 | March drift report: PSI 0.14 on `months_since_oldest_tradeline`, above the 0.10 watch level and below the 0.25 investigation level. Score PSI 0.05 | Monthly drift report (CTL-12) | Data Science: watch item, attributed to spring changes in the applicant mix, review next month | CTL-12 does not require a raw-input or fairness check before a watch item is closed. The feature is on the proxy list in the feature register (CTL-01), which the drift team did not consult |
| 3 | Mon 2027-04-05, 09:40 | Weekly ratio 0.70 (week of Mar 29), the second weekly value below 0.80. Rolling four-week 0.83 | Weekly fairness report (CTL-03) | Incident opened | The formal trigger had not fired: 0.83 is above 0.80, and only one four-week period was below 0.90 |

| Week of | Group ratio (weekly) | Rolling four-week | Status |
|---|---|---|---|
| Mar 1 | 0.95 | 0.95 | Normal |
| Mar 8 | 0.94 | 0.95 | Normal |
| Mar 15 | 0.90 | 0.94 | Normal on paper. Fault began Wed Mar 17 |
| Mar 22 | 0.79 | 0.89 | Signal 1 |
| Mar 29 | 0.70 | 0.83 | Signal 3. Incident opened |

**Why signal 3 became an incident.** The analyst opened the intake queue (CTL-33), saw signals 1 and 2 together, and matched the feature named in signal 2 against the feature register, where it is recorded as a moderate proxy for newcomer status. That link, made by a person, turned three routine alerts into an incident. The formal triggers never fired.

Time from fault start to detection: 19 days. Time from the first signal to the incident: 7 days.

## 4. Triage

The analyst reviewed the queue at 09:40 and finished triage at 10:30, which is 50 minutes against a target of one business day for high-severity alerts.

| Question | Finding |
|---|---|
| Is it noise? | No. From the start of the fault (Mar 17) to Apr 5 there were 196 group and 716 reference applications. Standard Review rate was 41 percent against 60 percent, a ratio of 0.68, well outside the sampling margin |
| Did anything change on our side? | No. Model, prompt, threshold and configuration versions match the approved register (daily version check clean, CTL-32; artifact hashes clean, CTL-25) |
| Is the LLM involved? | No sign. Canary and citation checks were clean (CTL-20, CTL-11). Summaries remain usable |
| What do the inputs show? | The share of records with `months_since_oldest_tradeline` = 0 rose from 2 percent to 21 percent from Mar 17. The feature is on the proxy list as a moderate proxy for newcomer status (CTL-01) |
| Is harm ongoing? | Yes. New files were being scored that morning, and 55 group files had already been routed to Recommend Decline |

**Provisional severity: Severity 2** (probable harm to a group, ongoing, cause unknown). It was not Severity 1 because harm was not yet confirmed and no binding obligation had been shown to be breached. The analyst opened an incident bridge and notified the AI Governance Office lead and the Deputy CRO, acting for the CRO, at 11:15. That is 1 hour 35 minutes after detection, against a target of 4 hours (CTL-33).

## 5. Containment

| Option | Assessment | Decision |
|---|---|---|
| A. Suspend only the Recommend Decline display (the CTL-15 mechanism) | Fast, but the score, the Enhanced Review routing and the Standard Review priority are also wrong for affected files. Harm would continue | Rejected |
| B. Keep Beacon running and manually re-review every file from the group | Needs group membership to decide how an individual file is treated. Group inference is for testing only and must never be used to decide individual applications (CTL-02) | Rejected |
| C. Suspend the Review Priority Model output for the two pilot teams and revert to the manual queue (CTL-21 runbook) | Stops all exposure. The manual queue is proven: the last fallback drill reached 86 percent of normal throughput against the 70 percent floor (CTL-34) | Chosen |
| D. Also suspend LLM summarization | No evidence it is involved. It has its own feature flag. Keeping it preserves support for adjudicators | Not needed |

**Who decided.** No defined trigger allowed the Head of Credit Adjudication or the CISO to suspend alone (those cover confirmed injection, artifact tampering and logging failure), so the decision went to the Deputy CRO, acting for the CRO (INC-3). The decision was recorded at 14:05, which is 4 hours 25 minutes after detection against a 24-hour target. Output stopped at 14:31, 26 minutes after the decision, against a runbook target of 30 minutes (illustrative).

**Also done on day one.** Files scored but not yet decided were flagged for manual assessment without Beacon output (mandatory trigger MT-08 in the Human Oversight Framework). Evidence was preserved: raw bureau payloads, feature tables, score logs, version hashes and Workbench logs.

Containment stops new harm. It does not correct decisions already made, which is what section 10 does.

## 6. Investigation

The investigation ran from Apr 5 to Apr 6 under INC-4, with Data Science and Engineering doing the analysis and the AI Governance Office leading.

| Hypothesis | Test | Result |
|---|---|---|
| Unapproved change to the model, thresholds, prompts or configuration | Daily version check and hash register (CTL-32, CTL-25) | Ruled out. Everything matches the approved register |
| Tampering or manipulated inputs | Integrity checks and sampling of raw applications | Ruled out |
| A real shift in who is applying | Compare the application mix by FSA, income band and product over 8 weeks | Ruled out. The mix is unchanged |
| Adjudicator behaviour | Path assignment happens before an adjudicator touches a file | Ruled out as a cause of the change in paths |
| A fault in an input | Compare the distribution of every model input over time, then compare parsed values with the raw bureau payloads | **Confirmed.** One input, `months_since_oldest_tradeline`, is 0 for files whose raw payload shows an older tradeline |

Root cause confirmed Tue Apr 6 at 16:00.

**The mechanism.** From Wed Mar 17 the credit bureau sent the oldest-tradeline open date in a new format for accounts from a subset of data furnishers. Northstar's ingestion routine could not read it, set the field to missing, and the feature calculation turned missing into 0 months. The model treats 0 months as no credit history, which is a strong risk signal.

**Why one group was hit harder.** The feature is a moderate proxy for newcomer status, because newcomers have short Canadian credit histories. The furnishers that migrated first report mostly newer products, such as secured cards and digital lender accounts, which newcomers and younger applicants use heavily. The fault touched 118 of the group's 196 files (60 percent) and 53 of the reference group's 716 files (7 percent).

## 7. Root cause

| Level | Finding |
|---|---|
| Direct cause | The ingestion routine turned an unreadable date into a missing value, and then into 0 months, without raising an error |
| Why it reached the model | Third-party feeds were validated for schema type only. Nothing checked the share of zero or missing values in model inputs, and nothing stopped scoring when a key input degraded |
| Why one group was hit harder | A retained proxy feature. The AI Governance Committee kept `months_since_oldest_tradeline` with a dated decision under CTL-01, because it predicts well and the pre-deployment matched-pair gap was within tolerance. That decision assumed the feature was correct. It did not ask what happens when the input degrades |
| Why testing did not catch it | Pre-deployment fairness testing (CTL-02) used historical data in the old format. The fault began after go-live |
| Why change control did not catch it | CTL-31 and CTL-32 cover the model, thresholds, prompts, LLM configuration and vendor model updates. No control covers a change in an upstream data feed |
| Why the bureau was outside oversight | Section N of the intake lists the bureau as "not AI-related", so third-party controls (CTL-19) covered Meridian only. The bureau had sent a format change notice to its subscribers six weeks earlier. It went to a Credit Data Operations mailbox and never reached the Beacon team |
| Why detection took 19 days | Three signals owned by different teams and dispositioned separately. A weekly value too noisy to act on alone. A formal trigger that reads a rolling four-week value, which dilutes a fast fault. A drift procedure with no fairness check |

**Root cause statement.** Beacon depended on an upstream data feed that sat outside change control and third-party oversight, and one of its important inputs is a proxy for a group Northstar must not disadvantage.

**What the root cause is not.** It is not a model defect, not bias learned from historical data and not an LLM problem. That matters because retraining the model would not have fixed it.

## 8. Risk assessment

| Measure | Value |
|---|---|
| Applications scored in the exposure window (Mar 17 to Apr 5) | 912 (196 group, 716 reference) |
| Files touched by the data fault | 171 (118 group, 53 reference) |
| Group Standard Review rate, as scored and corrected | 41 percent and 59 percent |
| Reference Standard Review rate, as scored and corrected | 60 percent and 62 percent |
| Fairness ratio, as scored and corrected | 0.68 and 0.95 |
| Group files routed to Recommend Decline, as scored and corrected | 55 (28 percent) and 27 (14 percent) |
| Adjudicator overrides of Recommend Decline on group files | 14 of 55 (25 percent), against a pilot baseline of 12 percent |
| Decline decisions issued on fault-touched files | 52 (39 group, 13 reference) |
| Declines reversed or revised at independent re-review | 17 (13 group, 4 reference). 35 upheld |
| Types of harm | Financial harm (declined or delayed credit) and unfair treatment of a group. No privacy exposure |

**What the numbers say about oversight.** Adjudicators overrode Recommend Decline on group files at twice the pilot baseline, and 11 of the 14 override rationales cited income or payment history the recommendation had not reflected. Human review reduced the harm. It did not prevent it, and no adjudicator raised a pattern, because the framework only asks for file-level action.

| Item | Assessment |
|---|---|
| Register mapping | An instance of R-01 (discrimination through proxies) and R-06 (drift). A new risk is proposed: R-17, upstream data feed change or degradation (likelihood 3, impact 4, inherent 12 High) |
| Tier reassessment | Methodology flag 6 (inputs likely to shift) now applies. L stays 4 because 5 to 6 flags score 4. Score stays 16. Tier stays High |
| Confirmed or probable harm | 17 applicants had a decline reversed or revised. The wider effect on 171 files is delay and routing. Recorded as Severity 2, and not raised to Severity 1 because no binding obligation was shown to be breached |
| Limit of the estimate | Group membership is inferred, so the true number of newcomer applicants affected cannot be known. Remediation therefore used the data fault and not the group label to decide who is contacted |

## 9. Stakeholder escalation

| When | Who | What | Rule |
|---|---|---|---|
| Mon 04-05, 11:15 | AI Governance Office lead and Deputy CRO (acting for the CRO) | Provisional Severity 2 and request for a suspension decision | CTL-33: within 4 hours |
| Mon 04-05, 14:05 | Deputy CRO | Suspension decision recorded | CTL-33: decision within 24 hours. INC-3 |
| Mon 04-05, 17:00 | Business owner (SVP), Chief Compliance Officer, Chief Privacy Officer, Legal, CISO, Head of Model Risk Management, Head of Credit Adjudication | Incident bridge. Facts and actions so far. Instruction to preserve evidence | INC-2, INC-4 |
| Tue 04-06 | AI Governance Committee chair. Pilot team leads and adjudicators | Committee notified. Adjudicators briefed on the manual process and on what to tell applicants | R-01: notify the Committee within 5 business days (done in 1) and pause rollout expansion |
| Tue 04-06 | Credit bureau | Formal notice of the fault and request for its format change documentation | Vendor Risk |
| Wed 04-07 | Executive Risk Committee, OSFI relationship team, Internal Audit | Briefing. Voluntary notice to OSFI. Internal Audit informed | Pre-pilot engagement memo (CTL-29): notify OSFI of Severity 1 or 2 AI incidents within 2 business days |
| Thu 04-08 | AI Governance Committee (extraordinary meeting) | Approves the remediation plan and the fix path | APR-1, CHG-3 |
| Fri 04-16 | Affected applicants | Contact complete (section 10) | INC-6 |
| Next quarterly meeting | Board Risk Committee | Reported in the quarterly AI risk report | Severity 2 does not need an immediate Board notice. A Severity 1 would |

Meridian, the LLM vendor, was not notified because it was not involved. Communications stated facts only, did not speculate on cause before Apr 6 and made no statement about applicants' ethnicity, which Northstar does not hold.

## 10. Remediation

**Track 1: fix the cause**
- Apr 6 to 7: the parser was patched to read both date formats. Any unreadable date now fails the record and blocks scoring, and does not default to a value (fail closed).
- Apr 9: the patch went through gate G9 as a material change (CHG-1, CHG-3). Model Risk Management re-validated the fix and replayed the 912 files from the exposure window, giving a ratio of 0.95. The AI Governance Committee approved the release. The builder and the releaser were different people (CTL-31).

**Track 2: fix outcomes for people**
- All 171 fault-touched files were re-scored with corrected data (Apr 7 to 9). The scope was set by the data fault and not by group membership, so both groups were covered.
- A senior panel re-reviewed all 52 decline decisions on these files without Beacon output (Apr 8 to 12). 17 were reversed or revised and 35 were upheld.
- By Apr 16 all 52 applicants had been contacted. The 17 received an apology, a reconsidered decision, a corrected adverse-action record and priority handling. The 35 received a written explanation that an independent review had upheld the decision, with reasons from the adjudicator's own assessment (CTL-10). The other 119 fault-touched applicants were contacted only where Enhanced Review had delayed them by more than 2 business days (Compliance decision).
- Documented extra costs, such as higher interest paid after taking credit elsewhere, are reimbursed on request (Business owner with Legal; illustrative).

**Track 3: fix the controls.** See sections 14 and 15.

**Resumption (gate G8) on Mon Apr 19**, after 10 business days of suspension:

| G8 criterion | Evidence | Met |
|---|---|---|
| Root cause identified | Investigation report, Apr 6 | Yes |
| The failed control re-tested | Replay test of the fix (ratio 0.95). An interim daily script checks the zero-rate of the feature and the fairness ratio. The permanent feed validation control is not built yet (A-5, due 2027-05-28) | Yes, with conditions |
| Affected individuals remediated where required | Panel review complete Apr 12. Contact complete Apr 16 | Yes |
| Breach reporting decisions recorded | Section 11 | Yes |
| Legal sign-off | Apr 16 | Yes |
| Any fix released through G9 first | Apr 9 | Yes |
| Decision by the authority that suspended | Deputy CRO, acting for the CRO | Yes |

Beacon resumed for the same two pilot teams only, with no expansion and with the heightened monitoring in section 13.

## 11. Regulatory and privacy considerations

Items marked "Legal to confirm" are jurisdiction-dependent and are not assumed.

| Question | Assessment | Owner | Status |
|---|---|---|---|
| Is this a breach of security safeguards under PIPEDA? | No. No personal information was lost, accessed or disclosed without authority. The fault made a derived input wrong. Recorded as not reportable as a security breach (Legal to confirm) | Chief Privacy Officer | Decided 2027-04-16 |
| Was inaccurate personal information used to make decisions about individuals? | Yes, for 171 applications. This engages the PIPEDA accuracy principle (Legal to confirm). Response: records corrected, applicants contacted, controls added | Chief Privacy Officer with Legal | Actions under way |
| Did the outcome disadvantage a group protected by human rights law? | Possibly. Adverse-effect discrimination in a service is the risk. The group is inferred and cannot be confirmed from data Northstar holds. The applicable statute and grounds (federal for a federally regulated bank, and provincial codes may also be raised) are for Legal to confirm. Response: treat it as potentially engaged, remediate on the fault and not on group membership, and record the analysis | Legal with Compliance | Assessment complete. Complaint handling ready (CTL-28) |
| Do adverse-action and consumer credit notice rules apply to the corrected decisions? | Yes for the 17 revised and the 35 upheld declines. Notices were issued or corrected with reasons from the adjudicator's independent assessment (CTL-10). Federal and provincial notice requirements: Legal to confirm | Compliance | Done 2027-04-16 |
| Must a regulator be told? | OSFI was notified voluntarily on Apr 7 under the pre-pilot engagement memo (CTL-29). Compliance assessed whether OSFI's technology incident reporting expectations applied (confirm scope and status) and whether the Financial Consumer Agency of Canada should be told (Compliance decision) | Chief Compliance Officer | OSFI done. FCAC decision recorded |
| Does Quebec Law 25 apply? | No. Quebec applicants are excluded from the pilot (CTL-18). Recorded so it is reassessed before any Quebec expansion | Chief Privacy Officer | Recorded |
| Contractual position with the bureau | Notice given Apr 6 and rights reserved. Format change notice and data quality commitments to be negotiated (P-6) | Head of Vendor Risk with Legal | Open |
| EU AI Act (benchmark only) | Would be assessed under Art. 73 as a potential serious incident involving obligations that protect fundamental rights. Not a Northstar legal obligation | AI Governance Office | Informational |

## 12. Documentation

The AI Governance Office keeps one case file (INC-7). Retention follows the schedule Privacy approves (CTL-17).

| Record | Kept by | Note |
|---|---|---|
| Incident ticket with timestamps, severity and bridge log | AI Governance Office | Timeline in section 19 comes from this |
| Evidence pack: raw bureau payloads, feature tables, score logs, version hashes, Workbench logs | Data Science and Engineering | Preserved on day one, access-controlled |
| Investigation report and root cause analysis | AI Governance Office | Sections 6 and 7 |
| Decision records: severity, suspension, G9 release, resumption | AI Governance Office | Names the decider, date and evidence for each |
| Panel re-review records and applicant communications | Head of Credit Adjudication and Compliance | Includes corrected adverse-action records |
| Regulator and bureau correspondence | Chief Compliance Officer and Head of Vendor Risk | |
| Legal and Privacy assessments | Legal and Chief Privacy Officer | Section 11 |
| Post-incident review and action tracker | AI Governance Office | Review held 2027-04-28 |
| Updates to the inventory, Risk Register, Control Matrix and intake | AI Governance Office | Proposed in section 15 |

## 13. Post-incident monitoring

Heightened monitoring runs for 8 weeks from the restart, to 2027-06-14. All thresholds here are starting values.

| Measure | Frequency and threshold | Control |
|---|---|---|
| Fairness ratio, group against reference, 5-day rolling with confidence interval | Daily. Below 0.85: same-day review by the AI Governance Office. Below 0.80: suspension decision under INC-3 | CTL-03 (tightened) |
| Zero rate, missing rate and PSI for the top 10 features | Daily. Zero or missing rate above twice baseline, or PSI above 0.10: fairness cut and raw-input check before any disposition | CTL-12 (tightened, P-2) |
| Bureau format canary file | Daily from 2027-05-07 (A-6). Any parse difference blocks scoring and opens an incident | Proposed CTL-35 |
| Blind re-review sample | 10 percent for 8 weeks, up from the 5 percent starting rate | CTL-07 |
| Override and concordance on Recommend Decline | Weekly. Existing thresholds (concordance above 90 percent, override below 3 percent) plus an override rate above 20 percent as an early warning of model error | CTL-08 (tightened) |
| Manual fallback readiness | Drill before any expansion. Floor 70 percent of normal throughput | CTL-34 |

**Exit criteria.** All of these must be true at the exit review on 2027-06-14: no rolling four-week ratio below 0.90, no unresolved data alarm, and actions A-4 to A-6 closed. If any fails, heightened monitoring is extended. Pilot expansion stays paused until the exit criteria are met and the next scheduled reassessment (2027-07-18) has taken place.

## 14. How the controls performed

| Control | Expected | What happened | Assessment |
|---|---|---|---|
| CTL-03 weekly fairness report | Detect fairness drift. Formal trigger on the four-week value | Produced all three readings. The formal trigger never fired (0.89, then 0.83) | Partly effective. It saw the problem, but the trigger is too slow for a fast fault |
| CTL-12 drift monitoring | Detect input instability | PSI 0.14 reached watch level and was closed as seasonal on Apr 2 without a raw-input or fairness check | Operated as designed. Design gap: no required check before a watch item is closed |
| CTL-33 one intake queue | Put every signal in one place with a logged disposition | All three signals reached one queue. Two owners dispositioned them separately. The analyst's review connected them | Partly effective. It made detection possible and did not stop isolated dispositions |
| CTL-06, CTL-08 and mandatory trigger MT-04 (human oversight) | Adjudicators challenge the recommendation | Overrides ran at 25 percent against a 12 percent baseline. No one escalated a pattern | Partly effective. It reduced harm and is not a detection mechanism |
| CTL-01 feature register | Proxy features need a Committee decision | Decision made and recorded, but it did not consider data degradation | Operated. Design gap |
| CTL-02 pre-deployment fairness testing | Find disparities before go-live | Passed on historical data. The fault appeared later | As designed. Cannot cover future data faults |
| CTL-31 and CTL-32 change control | Only approved versions run | Version check clean. The change was upstream of both controls | Gap: no control covers upstream feed changes |
| CTL-19 third-party oversight | Vendor terms and change notice | Covered Meridian only. The bureau was listed as not AI-related and its notice reached a mailbox outside Beacon | Gap |
| CTL-21 switch-off runbook | Stop output quickly and keep lending running | Output stopped 26 minutes after the decision. Manual queue at 86 percent of normal throughput | Effective |
| CTL-26 and CTL-33 playbook, severity and authority | Clear roles and timing | Notification in 1 hour 35 minutes, suspension decision in 4 hours 25 minutes, review inside 30 days | Effective |

**The pattern.** The response controls worked. The prevention and early-detection controls had design gaps that a written control set cannot show until a fault occurs.

## 15. Proposed changes to earlier artifacts

These are proposals. None has been applied to the Risk Register, Control Matrix, intake, Human Oversight Framework, RACI or lifecycle document. They will be applied together in one review pass.

| ID | Proposed change | Applies to | Owner | Due |
|---|---|---|---|---|
| P-1 | New control (proposed CTL-35): validate third-party data feeds at ingestion for schema, null and zero rate and distribution on model inputs. Fail closed for the top 10 features. Add a daily canary file in the bureau format. Route feed change notices to the AI Governance Office | New; R-06, proposed R-17 | Director, Credit Analytics Engineering | 2027-05-28 |
| P-2 | Amend CTL-12: a drift alert on the score or a top-10 feature needs a raw-input check and a fairness cut by proxy group before it can be closed as seasonal or noise | CTL-12 | Head of Credit Risk Analytics | 2027-05-14 |
| P-3 | Amend CTL-03: during the pilot, two consecutive weekly ratios below 0.80 open an incident, and weekly values are shown with confidence intervals. The formal monthly triggers stay | CTL-03 | AI Governance Office | 2027-05-14 |
| P-4 | Amend CTL-33: alerts on the same system within 14 days are linked into one case, open dispositions are reviewed across the queue each week, and a "seasonal" or "noise" disposition must state the check performed | CTL-33 | AI Governance Office | 2027-05-14 |
| P-5 | Amend the Human Oversight Framework: when three or more adjudicators in a team-week cite MT-04 or override for the same data field or reason, the Workbench raises an item in the intake queue | Framework section 5 | Head of Credit Adjudication | 2027-05-14 |
| P-6 | Bring the credit bureau feed into third-party oversight: amend CTL-19 and intake Section N, agree a change-notice term, and route notices to the AI Governance Office | CTL-19, intake | Head of Vendor Risk | 2027-05-14 |
| P-7 | Amend CTL-01: keeping a proxy feature needs a data degradation analysis (what happens if the feature is wrong or missing) and a monitoring plan for that feature | CTL-01 | Head of Credit Risk Analytics | 2027-06-11 |
| P-8 | Update the Risk Register (add R-17, add upstream feed change as a cause on R-01 and R-06), the intake (v1.1) and the Control Matrix | Register, intake, matrix | AI Governance Office | 2027-05-14 |
| P-9 | Add RACI rows for the decisions the lifecycle document assigns without one, including resumption at G8 as used in this incident | RACI | AI Governance Office | 2027-05-14 |

## 16. Using this as a tabletop exercise

CTL-26 requires a tabletop before the pilot using this scenario. Run it for 90 minutes with participants from Risk, Data Science, Engineering, Credit Adjudication, Privacy, Legal, Compliance and Cybersecurity. Ideally the facilitator sits outside the team that wrote the playbook.

- Give participants the three signals in section 3 one at a time. Do not reveal the cause (section 7) until the investigation stage.
- Ask: at which signal would you open an incident, and why?
- Ask: who decides to suspend Beacon, and what happens if the CRO cannot be reached?
- Ask: how do you decide who to contact, given that group membership is a statistical inference?
- Ask: what must be true before Beacon resumes?
- Ask: which of your dispositions would you defend to a regulator?
- Ask: which controls failed, and what would you change?
- Record the answers as the exercise report, which is the CTL-26 evidence, and track every action to closure.

## 17. Framework alignment (indicative, verify before citing)

- **NIST AI RMF:** MANAGE 2.3 (respond to and recover from previously unknown risks), MANAGE 2.4 (mechanisms to supersede or deactivate an AI system), MANAGE 4.3 (incidents communicated and tracked) and GOVERN 4.3 (incident identification and information sharing). Check the wording against the Playbook.
- **ISO/IEC 42001:** Clause 10.2 (nonconformity and corrective action) and Annex A.3 (internal organization). Verify clause references against a licensed copy.
- **EU AI Act (benchmark only):** Article 73 (serious incident reporting) and Article 72 (post-market monitoring). Verify current status and timelines.
- **Canadian expectations:** PIPEDA accuracy and accountability principles, human rights legislation on adverse-effect discrimination, and OSFI expectations on model risk (E-23) and technology incidents (confirm scope, status and effective dates). Legal to confirm.

## 18. Limitations

- Every date, count and rate is invented and was chosen to be internally consistent. Real incidents are messier, and real numbers would carry wider uncertainty.
- One scenario does not show how the programme handles a Severity 1 event, an incident in the LLM component, or one that spans two systems.
- The scenario was written by the same author as the controls, so it tests them less than an independent exercise would. A real tabletop should use injects the participants have not seen.
- The group is inferred from census and name analysis. It is a statistical construct, and any statement about who was affected is an estimate.
- The root cause was chosen to show gaps in monitoring and third-party oversight. Other causes, such as a retrain that learned a proxy, would stress different controls.
- Regulatory and privacy conclusions are labelled for Legal to confirm because they depend on jurisdiction and facts.
- The severity scale, the response times and the heightened monitoring thresholds are illustrations and starting values.
- Proposed changes in section 15 are not applied. Until they are, the earlier artifacts describe the programme as it stood before this incident.

## 19. Incident report: AI-INC-2027-001

| Field | Entry |
|---|---|
| System | Beacon (Intake NF-AI-2026-014), tier High, pilot (two teams, Ontario) |
| Severity | 2 |
| Status | Containment and remediation complete. Heightened monitoring to 2027-06-14 |
| Fault window | 2027-03-17 to 2027-04-05 |
| Detected | 2027-04-05, 09:40 |
| Suspended and resumed | Suspended 2027-04-05 at 14:31. Resumed 2027-04-19 (10 business days) |
| Incident owner (INC-2) | AI Governance Office lead |
| Accountable for remediation (INC-6) | SVP, Consumer and Small Business Lending |
| Suspension authority (INC-3) | Deputy CRO, acting for the CRO |
| Report date | 2027-04-30, after the post-incident review on 2027-04-28 |

**Summary.** A change in the credit bureau's data format made one Beacon input default to zero for 171 applications, 118 of them from a proxy-inferred group of applicants in areas with a high share of recent immigrants. The group's fairness ratio fell from about 0.95 to 0.68, and 55 group files were routed to Recommend Decline against 27 after correction. Adjudicator overrides limited the harm. 52 decline decisions were made on affected files, and 17 were reversed or revised on independent re-review. Beacon's output was suspended on 2027-04-05 and resumed on 2027-04-19 after a fix, remediation and a G8 review. The cause was an upstream data feed outside change control and third-party oversight, feeding a retained proxy feature. Three monitoring signals over 7 days were dispositioned separately before an analyst connected them.

**Timeline**

| When | Event | Reference |
|---|---|---|
| 2027-03-17 | Bureau begins sending the new date format. Parsing fault starts with no alert | Section 6 |
| 2027-03-29 | Weekly ratio 0.79. Logged and set to monitor | CTL-03, CTL-33 |
| 2027-04-01 to 02 | Drift report shows PSI 0.14 on the feature. Closed as seasonal | CTL-12, CTL-33 |
| 2027-04-05 09:40 | Weekly ratio 0.70. Analyst links the three signals and opens the incident | CTL-33 |
| 2027-04-05 10:30 | Triage complete. Provisional Severity 2 | Section 4 |
| 2027-04-05 11:15 | AI Governance Office lead and Deputy CRO notified | CTL-33 |
| 2027-04-05 14:05 | Suspension decision | INC-3 |
| 2027-04-05 14:31 | Output stopped. Manual queue running | CTL-21 |
| 2027-04-06 16:00 | Root cause confirmed. Committee chair and bureau notified | INC-4 |
| 2027-04-07 | Executive Risk Committee briefed. OSFI notified voluntarily | CTL-29 |
| 2027-04-09 | Fix released through G9 after re-validation and replay test | CHG-3 |
| 2027-04-12 | Panel re-review complete: 17 of 52 declines reversed or revised | Section 10 |
| 2027-04-16 | Applicant contact complete. Legal sign-off | INC-5, INC-6 |
| 2027-04-19 | Beacon resumes under G8 with conditions and heightened monitoring | G8 |
| 2027-04-28 | Post-incident review held | INC-7 |

**Response metrics**

| Metric | Result | Target | Met |
|---|---|---|---|
| Fault start to detection | 19 days | None set | Finding |
| First signal to incident opened | 7 days | None set | Finding |
| Alert to triage decision | 50 minutes | Inside 1 business day | Yes |
| Detection to notification of AI Governance Office and CRO | 1 hour 35 minutes | Within 4 hours | Yes |
| Detection to suspension decision | 4 hours 25 minutes | Within 24 hours | Yes |
| Decision to output stopped | 26 minutes | 30 minutes (runbook, illustrative) | Yes |
| Notice to the AI Governance Committee | 1 business day | Within 5 business days (R-01) | Yes |
| Applicants with a decline contacted | 9 business days | 10 business days (illustrative) | Yes |
| Post-incident review | 23 days after detection | Within 30 days | Yes |

**Corrective actions**

| ID | Action | Owner | Due | Status |
|---|---|---|---|---|
| A-1 | Patch the parser and fail closed on unreadable dates | Director, Credit Analytics Engineering | 2027-04-09 | Done |
| A-2 | Re-score 171 files and re-review 52 declines | Head of Credit Adjudication | 2027-04-12 | Done |
| A-3 | Contact affected applicants and correct records | SVP, Consumer and Small Business Lending, with Compliance | 2027-04-16 | Done |
| A-4 | Bring the bureau feed into third-party oversight (P-6) | Head of Vendor Risk | 2027-05-14 | Open |
| A-5 | Build the feed validation control (P-1) | Director, Credit Analytics Engineering | 2027-05-28 | Open |
| A-6 | Add the daily bureau format canary file (P-1) | Director, Credit Analytics Engineering | 2027-05-07 | Open |
| A-7 | Amend CTL-12, CTL-03 and CTL-33 (P-2, P-3, P-4) | Head of Credit Risk Analytics and AI Governance Office | 2027-05-14 | Open |
| A-8 | Add pattern escalation to the oversight framework (P-5) | Head of Credit Adjudication | 2027-05-14 | Open |
| A-9 | Test a variant without the feature or with its influence capped. Committee decision on the feature (P-7) | Head of Model Risk Management | 2027-06-11 | Open |
| A-10 | Update the intake, Risk Register, Control Matrix and RACI (P-8, P-9) | AI Governance Office | 2027-05-14 | Open |
| A-11 | Exit review of heightened monitoring | AI Governance Office | 2027-06-14 | Open |
| A-12 | Report to the Board Risk Committee | Chief Risk Officer | Next quarterly meeting | Open |

**Sign-off**

| Role | Name | Decision | Date |
|---|---|---|---|
| Incident owner | AI Governance Office lead | Report issued | 2027-04-30 |
| Accountable for remediation | SVP, Consumer and Small Business Lending | Accepted | 2027-04-30 |
| Executive sponsor | Deputy CRO, acting for the CRO | Accepted | 2027-04-30 |
| Privacy | Chief Privacy Officer | Reviewed | 2027-04-30 |
| Legal | Legal counsel | Reviewed | 2027-04-30 |
| Model Risk | Head of Model Risk Management | Reviewed | 2027-04-30 |

This report closes the containment and remediation phase. Monitoring continues until the exit review on 2027-06-14.
