# Northstar Financial: AI Risk Tiering Methodology

**Fictional case study. Northstar Financial and all figures are invented for portfolio purposes.**

**Document ID:** NF-AIG-002 | **Version:** 1.0 (draft for AI Governance Committee approval) | **Owner:** AI Governance Office (2nd line)

## 1. Purpose and use

The risk tier decides how much governance a system receives: who approves it, how it is validated, and how closely it is monitored. It is assigned by the AI Governance Office from the intake record. It is never self-assigned by the submitting team.

**The tier measures inherent risk, before proposed controls.** If a tier depended on controls not yet built, teams could promise controls to earn a lower tier. Residual risk is assessed per risk in the Risk Register.

**Reassessment triggers:** material model or data change, a significant incident, evidence of over-reliance (see Automation below), a legal or regulatory change, closure of an open item that affected a score, and the scheduled review for the tier.

## 2. Why four tiers

Tiers exist to route governance effort, so the number of tiers should equal the number of distinct treatments. Northstar needs four:

1. Register and self-attest.
2. Second-line desk review.
3. Independent validation and committee approval.
4. Executive-level approval and a restricted rollout.

Three tiers would force either heavy process on moderate systems or light process on serious ones. Five would create distinctions no team could operate. The names describe severity and say nothing about whether a system is allowed.

**Relationship to frameworks (indicative only, verify before relying on it):**
- **NIST AI RMF** does not prescribe tiers. This method sits in the Map function (context and impacts), and the tier drives prioritization in Manage.
- **ISO/IEC 42001** expects documented AI risk and impact assessment. The standard is paywalled, so verify clause references against a licensed copy before citing them.
- **EU AI Act** is a benchmark, not a Northstar legal obligation. Its categories are legal classifications by use case, while this method scores risk. Rough alignment: Critical is a candidate for restriction or prohibition, High is comparable to Annex III high-risk, Moderate to limited risk, Low to minimal risk. This is not a legal determination.
- **OSFI E-23** expects risk-based rigour for models and does not prescribe tier names (Legal to confirm scope and timing).

## 3. Step 1: Gate check

If any answer is Yes, the use is out of appetite. It is not permitted without a documented exception from the Executive Risk Committee.

| Gate question | Basis |
|---|---|
| Does the system score or rank individuals on behaviour unrelated to its stated purpose (social scoring)? | Adapted from EU AI Act prohibited practices |
| Does it infer emotions of customers or employees? | Same |
| Does it use biometric data to infer protected characteristics? | Same |
| Does it use prohibited grounds under human rights legislation directly as decision inputs? | Legal maintains the list of grounds |

## 4. Step 2: Rate the seven impact dimensions (1 to 4)

Rate each dimension against the anchors. **Impact (I) is the highest single rating**, not the average, because averaging lets a severe harm in one dimension hide behind mild ratings in others.

| Dimension | 1 Minimal | 2 Limited | 3 Significant | 4 Severe |
|---|---|---|---|---|
| D1 Financial harm to individuals | None or trivial | Minor, quickly reversed | Material, reversible with effort (credit access, fees, credit file damage) | Severe or irreversible (insolvency, loss of housing) |
| D2 Fairness and discrimination | No decisions about individuals | Differential outcomes possible, low stakes | Consequential decision, differential impact on protected groups plausible, testing feasible even if limited | Consequential decision and testing not feasible, or existing evidence of disparity |
| D3 Privacy | No personal information | Limited, low-sensitivity personal information | Sensitive financial or identity information | Biometric, health, or minors' data, or covert profiling |
| D4 Rights and access to services | No effect | Discretionary product or convenience | Important but substitutable financial service | Essential service with no substitute, or physical safety |
| D5 Security consequence of compromise | Negligible | Limited disruption or low-sensitivity data | Could change customer outcomes, expose sensitive data, or disrupt a business line for days | Systemic loss, large-scale fraud, or disruption of a critical operation |
| D6 Regulatory and reputational exposure | No specific regulation | General legal obligations only | Regulated activity with specific supervisory or human rights expectations | Enforcement, public disclosure, or regulatory restriction likely if it fails |
| D7 Financial impact to Northstar (annual, illustrative thresholds, Finance to set) | Under $100k | $100k to $1M | $1M to $10M | Over $10M |

Submitter harm ratings in intake Section J are inputs. The assessor re-rates against these anchors and records any difference.

## 5. Step 3: Score the three amplifiers (1 to 4)

Amplifiers describe how widely and how likely the harm is to materialize.

**A. Degree of automation**

| Score | Description |
|---|---|
| 1 | Informational only, or the human decides independently before seeing the output |
| 2 | Human decides with the recommendation in view and reviews every output |
| 3 | Human reviews exceptions only, **or measured concordance with the recommendation exceeds 90 percent** (de facto automation) |
| 4 | No human review |

The 90 percent trigger is an initial value, to be calibrated against the pilot baseline.

**P. Affected population**

| Score | Individuals affected per year |
|---|---|
| 1 | Under 1,000 |
| 2 | 1,000 to 50,000 |
| 3 | 50,001 to 500,000 |
| 4 | Over 500,000 |

Add 1 (maximum 4) if the intake records vulnerable or protected groups as Possible or Yes.

**L. Likelihood of failure (uncertainty flags)**

Count the flags that apply:
1. LLM or generative component
2. Vendor-controlled or non-interpretable model
3. Trained on past human decisions, or known representativeness gaps
4. Weak ground truth for testing (outcomes lag over 6 months, or demographic data unavailable)
5. First production use of this kind at Northstar
6. Inputs likely to shift (economic, product, or population change)

| Flags | 0 to 1 | 2 | 3 to 4 | 5 to 6 |
|---|---|---|---|---|
| L | 1 | 2 | 3 | 4 |

## 6. Step 4: Calculate the tier

**Score = 2 x I + A + P + L** (range 5 to 20)

| Score | 5 to 8 | 9 to 12 | 13 to 16 | 17 to 20 |
|---|---|---|---|---|
| Formula tier | Low | Moderate | High | Critical |

**Why impact counts double:** severity of harm is the main driver of governance rigour, and the amplifiers modulate it. The weights are a judgement-based starting point (see Limitations).

**Impact window.** Impact alone implies a tier: I=1 Low, I=2 Moderate, I=3 High, I=4 Critical. The final tier must stay within one tier of the impact-implied tier. This stops a trivial system from becoming High through scale alone, and stops a severe-impact system from being scored down to Moderate.

**Floor rules.** Applied after the window. The highest result wins.

| Rule | Condition | Minimum tier |
|---|---|---|
| F1 | No human review (A=4) and I is 3 or more | Critical |
| F2 | The AI recommends or decides on eligibility, pricing, or access for individuals | High |
| F3 | Sensitive personal information is sent to a third-party AI service | Moderate |

**Overrides.** The AI Governance Office may raise a tier with a written note. Only the AI Governance Committee may lower one, with a written rationale. All overrides are logged, and downward overrides are a dashboard metric.

## 7. Governance treatment by tier

| Tier | Approval | Validation | Additional requirements | Monitoring | Reassessment |
|---|---|---|---|---|---|
| Low | AI Governance Office | Self-attestation, checked by the Office | None | Annual attestation | 24 months |
| Moderate | Head of AI Governance and business owner | Second-line desk review | Human oversight described in intake | Quarterly metrics review | 12 months |
| High | AI Governance Committee | Independent validation by Model Risk Management before deployment | Fairness testing and a Human Oversight Framework are mandatory | Monthly | 6 months during pilot, then 12 |
| Critical | Executive Risk Committee, Board Risk Committee informed | Independent validation plus Internal Audit review | As High, plus Legal opinion on regulator engagement before deployment. Restricted rollout only. | Weekly during rollout, then monthly | Quarterly |

## 8. Worked example: Beacon (Intake NF-AI-2026-014)

| Dimension | Rating | Basis |
|---|---|---|
| D1 Financial harm to individuals | 3 | Wrongly declined or approved applicants face material harm, reversible with effort (Section E) |
| D2 Fairness and discrimination | 3 | Consequential decision, likely proxies, no demographic data. Proxy testing is feasible but limited (Sections F, K) |
| D3 Privacy | 3 | Sensitive financial documents (Section F) |
| D4 Rights and access | 3 | Unsecured credit is important but substitutable (Section J) |
| D5 Security consequence | 3 | Tampering or compromise could change outcomes or expose documents (Section L) |
| D6 Regulatory and reputational | 3 | Credit decisioning (Section I) |
| D7 Financial impact to Northstar | 2 | Assumed $100k to $1M a year in credit loss and remediation |

**Impact (I) = 3**, so the impact-implied tier is High and the window is Moderate to Critical.

| Amplifier | Score | Basis |
|---|---|---|
| A Automation | 2 | Recommendation shown, human reviews every output, no concordance evidence yet |
| P Population | 4 | About 180,000 a year gives band 3, plus 1 because vulnerable groups are Possible |
| L Likelihood | 4 | Five flags: LLM, vendor model, past-decision training data, weak ground truth, first use |

**Score = 2 x 3 + 2 + 4 + 4 = 16, so the formula tier is High.** The window allows it, F2 also requires at least High, and F3 requires at least Moderate. **Assigned tier: High.** A score of 16 is the top of the High band, so one more point makes it Critical.

**Sensitivity analysis**

| Scenario | Change | Score | Tier |
|---|---|---|---|
| Base case | As above | 16 | High |
| Pilot scope | About 18,000 applications, Ontario only: P = 3 | 15 | High |
| LLM summarization removed | Flags 1 and 2 drop out: L = 3 | 15 | High |
| Evidence of rubber-stamping | Concordance above 90 percent: A = 3 | 17 | Critical |
| Legal rules out proxy-based fairness testing (OI-09) | D2 = 4, so I = 4 | 18 | Critical |

**What this shows:**
- The tier is driven by the credit decision context, not by the LLM. Removing the LLM does not lower it.
- A pilot does not change the tier, so pilot approval still needs the High-tier process.
- Two events would move Beacon to Critical: evidence that adjudicators are following recommendations without independent judgement, and a legal opinion blocking fairness testing. Both are monitored (Human Oversight Framework, Open Item OI-09).

## 9. Limitations

- **Weights and thresholds are judgement-based.** Before approval, a real programme would score 10 to 15 existing systems and compare the tiers to expert ranking. That calibration has not been done here.
- **This is inherent risk of the use case, not of individual risks.** Individual risks are assessed in the Risk Register.
- **Inputs depend on submitter accuracy.** The Governance Office validates them, but errors are possible.
- **Dollar thresholds are illustrative** and would come from Northstar's risk appetite statement.
- **This is not a legal classification.** Regulatory comparisons are indicative and need Legal review.
