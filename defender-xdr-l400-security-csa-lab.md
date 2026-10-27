# Architect and articulate Defender XDR business value for Caldova

**Microsoft Defender XDR | Level 400**

> [!IMPORTANT]
> Before you begin, replace `YOUR-TENANT-ID` in this guide and its portal URLs with the authorized demo tenant ID supplied by your facilitator. The original tenant ID, assigned-account address, and evidence IP address are omitted from this public copy. Incident and attack-path examples require the matching demo dataset. Validation notes below are carried forward from the source document, not independently revalidated during conversion.

A 60-minute hands-on lab for Security Cloud Solution Architects (CSAs)

| **Item** | **Detail** |
|----|----|
| Audience | Security CSAs advising existing Microsoft 365 E5 customers (L400, architecture and advisory) |
| Duration | 60 minutes (exactly) |
| Customer scenario | Caldova (fictional global manufacturing and logistics company) |
| Evidence source | Live Zava demo tenant (read-only) |
| Tenant ID | YOUR-TENANT-ID |
| Lab type | Investigation and advisory design; no infrastructure deployed, no changes made |
| Evidence checked | 2026-10-12 (Secure Score, incident list, attack paths) |

## Contents and navigation

Use this map to move through the lab. Times add up to exactly 60 minutes.

| **Section** | **Title** | **Minutes** |
|----|----|----|
| 1 | Overview, objectives, prerequisites, and scenario | (pre-read) |
| 2 | Exercise 0: Scenario briefing and tenant access | 5 |
| 3 | Exercise 1: Review Secure Score and posture | 10 |
| 4 | Exercise 2: Investigate Incident 74802 | 15 |
| 5 | Exercise 3: Analyze an attack path and blast radius | 12 |
| 6 | Exercise 4: Architecture map and prioritized adoption roadmap | 10 |
| 7 | Exercise 5: Incident-to-value narrative | 5 |
| 8 | Exercise 6: Three-minute customer pitch | 3 |
| 9 | Cleanup, troubleshooting, discussion guide, validation matrix | (reference) |
|  | **Total hands-on time** | **60** |

> **Pre-read:** Section 1 is a pre-read (about 5 minutes) and is not counted in the 60-minute lab clock. Facilitators should ask learners to read it before the session starts.

## 1. Overview

Caldova invested heavily in the Microsoft security stack, but its security operations center (SOC) still works across multiple consoles, correlates signals by hand, and sees alert volume grow faster than analyst capacity. In this lab you act as the Security CSA advising Caldova's leadership. You use a live demo tenant to gather representative evidence, then turn that evidence into an architecture, a roadmap, and an executive value story that emphasizes what Caldova already owns.

> **Important:** The live Zava demo tenant supplies **representative evidence** for the Caldova advisory exercise. Scores, incident counts, asset names, and attack-path details you see in the tenant are **not Caldova facts**. Never present tenant metrics to the Caldova personas as Caldova's own results.

### What you will do

- Review Microsoft Secure Score to understand posture strengths and gaps.

- Inspect a multi-stage incident correlated across Microsoft Defender XDR, Microsoft Sentinel, Microsoft Purview DLP, and Insider Risk Management.

- Analyze a critical cloud attack path to reason about trust boundaries and blast radius.

- Map gaps to an integrated Defender XDR architecture and build a phased adoption roadmap.

- Craft an incident-to-value narrative and a three-minute customer pitch.

### Objective alignment

| **Learning objective** | **Exercises** | **Output** |
|----|----|----|
| **LO2.** Design and defend an end-to-end Defender XDR architecture across endpoint, identity, email, collaboration, cloud apps, and vulnerability and exposure management, including signal flow, boundaries, dependencies, constraints, failures, and tradeoffs. | 1, 2, 3, 4 | Completed architecture map (Worksheet D) |
| **LO3.** Analyze a multi-stage attack using XDR signals, incidents, automated investigation, attack disruption, Secure Score, and exposure insights; identify attack paths, trust boundaries, gaps, blast radius, and response dependencies. | 1, 2, 3 | Worksheets A, B, C |
| **LO4.** Create a phased adoption roadmap that accounts for licensing, maturity, dependencies, ownership, governance, gates, and measurable technical and business criteria. | 1, 4 | Prioritized adoption roadmap (Worksheet E) |
| **LO5 (contribution).** Connect design decisions to outcomes, tradeoffs, residual risks, objections, and next actions. | 4, 5, 6 | Incident-to-value narrative (F); three-minute pitch (G) |

### Prerequisites

- Working knowledge of the Microsoft Defender portal, Defender workloads, Microsoft Entra concepts, and Microsoft 365 E5 security value.

- Sign-in to the Microsoft Defender portal for tenant YOUR-TENANT-ID with a role that can read Secure Score, incidents, and Exposure Management attack paths (for example, Security Reader). Use the credentials provided by your facilitator; do not record them in this document.

- A current Microsoft Edge or Google Chrome browser. Allow pop-ups and third-party storage for security.microsoft.com.

- Familiarity with this document's worksheets. Type directly into the tables, or print them.

> **Read-only lab:** No step requires you to create, edit, classify, resolve, assign, or delete anything. If a button would change tenant state (for example, **Manage incident**, **Classify**, **Assign to me**, or **Add to plan**), do not select it.

### Case study: Caldova

Caldova is a global manufacturing and logistics company with about 45,000 employees across North America, Europe, and Asia Pacific. It has invested in Microsoft 365 E5, Microsoft Defender for Endpoint, Microsoft Defender for Office 365, Microsoft Entra ID, Microsoft Sentinel, and third-party security tools.

| **Area** | **Caldova situation (from the case study)** |
|----|----|
| Endpoint coverage | About 31,000 managed endpoints report consistently; onboarding and reporting vary across the estate. |
| Operations | Multiple consoles and workflows; manual correlation; automation used inconsistently. |
| Alert load | Alerts grow while analyst capacity stays stable; Tier 1 escalates about 40% of investigations. |
| Visibility | Limited cross-domain visibility; inconsistent response; uneven use of exposure insights. |
| Leadership concerns | Rising SecOps cost; alerts outpace capacity; complex threats; unclear investment value; fear that more technology adds complexity. |

| **Persona** | **Role** | **Primary goals** |
|----|----|----|
| Elena Petrov | CISO | Reduce complexity and incident response time; show measurable risk reduction. |
| Marcus Chen | SecOps manager | Improve efficiency; reduce alert fatigue and manual work. |
| Priya Nair | Security Architect | Integrated architecture and consistent workflows. |
| Liam O'Connor | Executive Sponsor | Return on investment and adoption without proportional cost growth. |

**Advisory questions Caldova wants answered:**

- How effectively is Caldova using Defender XDR today?

- What are the key adoption gaps?

- Which capabilities should be prioritized next?

- Which investigation work remains manual?

> **Note:** The source case study lists one further question that is incomplete. This lab does not invent it. Ask your facilitator whether the customer has supplied the missing question.

## Timing summary

| **Exercise** | **Focus** | **Objectives** | **Minutes** | **Running total** |
|----|----|----|----|----|
| 0 | Scenario briefing and tenant access | Setup | 5 | 5 |
| 1 | Secure Score and posture | LO3, LO4 | 10 | 15 |
| 2 | Incident 74802: cross-workload story | LO2, LO3 | 15 | 30 |
| 3 | Attack path and blast radius | LO2, LO3 | 12 | 42 |
| 4 | Architecture map and prioritized roadmap | LO2, LO4, LO5 | 10 | 52 |
| 5 | Incident-to-value narrative | LO5 | 5 | 57 |
| 6 | Three-minute customer pitch | LO5 | 3 | 60 |
|  | **Total** |  | **60** | **60** |

> **Facilitator tip:** The Defender portal single-page app can take 20 to 30 seconds to load incident and attack path views. Open the Exercise 2 and Exercise 3 URLs in separate browser tabs during Exercise 0 so they warm up while you read.

## Exercise 0: Scenario briefing and tenant access (5 minutes)

Confirm that you can reach the three portal areas used in this lab and agree on the evidence rules before you start.

1. Open the Defender portal home page: https://security.microsoft.com/homepage?tid=YOUR-TENANT-ID.

2. Sign in with the credentials from your facilitator. Confirm the tenant ID in the URL matches the ID on the title page.

3. Open three browser tabs with these URLs and leave them loading:

   - Tab 1, Secure Score: https://security.microsoft.com/securescore?tid=YOUR-TENANT-ID

   - Tab 2, Incidents: https://security.microsoft.com/incidents?tid=YOUR-TENANT-ID

   - Tab 3, Attack paths: https://security.microsoft.com/attack-paths?viewId=attack-paths&tid=YOUR-TENANT-ID

4. Read the Caldova personas in Section 1 and choose which persona you will address in your final pitch (Exercise 6). Record your choice in Worksheet A.

5. Write the evidence rule in your own words: tenant data is representative, not Caldova's data.

**Expected result:** All three tabs load in the Defender portal without a sign-in prompt. If a page shows a spinner, wait up to 30 seconds.

> **Checkpoint 0:** You can name the three portal areas and the persona you will target. You can state that tenant data is illustrative.

## Exercise 1: Review Secure Score and posture (10 minutes)

Secure Score gives you a measurable posture baseline across identity, data, devices, and apps. For Caldova, where Liam O'Connor wants measurable value and Elena Petrov wants measurable risk reduction, Secure Score is the first quantified story to tell. Remember that endpoint onboarding at Caldova is inconsistent, which makes the Device category especially relevant.

### Steps

1. In Tab 1, open https://security.microsoft.com/securescore?tid=YOUR-TENANT-ID. In the navigation pane, the equivalent path is **Exposure management** \> **Secure score** (menu labels can vary slightly by portal release).

2. On the **Overview** page, record the overall score and points achieved in Worksheet A.

3. Locate the category breakdown (**Identity**, **Data**, **Device**, **Apps**). Record each percentage.

4. Locate the comparison with organizations of similar size. Record both values.

5. In **Actions to review** on the Overview page, note the status counts (**Regressed**, **To address**, **Planned**, **Risk accepted**, **Recently added**, and **Recently updated**).

6. In **Top recommended actions**, select **View all**. On the **Recommended actions** page, confirm that **List loaded** appears. The table is ranked by default. Record the top five actions, score impact, points achieved, status, license availability, category, and product in Worksheet A.

7. Select **Enable 'Local Security Authority (LSA) protection'** to open its flyout. Read the description, implementation status, points achieved, category, and product. Note the link to manage the action in threat and vulnerability management. Close the flyout without changing tags or status.

8. Decide which actions an architect would treat as quick wins (low user impact, existing licensing) and which need a governed rollout. Record your reasoning.

### Expected results (representative values seen on 2026-10-12)

| **Item** | **Value observed in the demo tenant** |
|----|----|
| Overall Secure Score | 56.59% (1,252.3 of 2,213 points) |
| Category breakdown | Identity 59.14%; Data 100%; Device 49.36%; Apps 73.57% |
| Similar-size organizations | Tenant 56.59 vs. 47.99 |
| Action status counts | Regressed 55; To address 211; Planned 5; Risk accepted 2; Recently added 0; Recently updated 0 |
| Top recommended actions | Turn on Microsoft Defender Antivirus real-time protection for Linux (+0.45%); PUA protection in block mode for Linux (+0.41%); Block rebooting machine in Safe Mode (+0.41%); Block copied or impersonated system tools (+0.41%); Update Defender Antivirus definitions for Linux (+0.41%); Disable outdated plugins (+0.36%); Enable LSA protection (+0.36%); Set UAC to automatically deny elevation (+0.36%) |

> **Note:** Scores change over time. Your numbers may differ slightly. The shape of the story (Device is the weakest category; most points come from endpoint configuration) is the learning point. Do not present these values as Caldova's.

### Worksheet A: Secure Score evidence

| **Measure** | **Your observation** | **Caldova implication (hypothesis)** |
|----|----|----|
| Overall score and points |  |  |
| Identity / Data / Device / Apps |  |  |
| Similar-size comparison |  |  |
| Regressed and To address counts |  |  |
| Top 5 actions (name, impact) |  |  |
| Persona you will target in Exercise 6 |  |  |
|  |  |  |
|  |  |  |

### CSA discussion prompts

- How would you explain a Device score below the other categories to Marcus Chen without it sounding like a failure of his team?

- Which top actions are configuration changes using products Caldova already owns? What governance gate (change board, pilot ring) would you propose?

- How do you reconcile 'Regressed' actions with Liam's concern about measurable value? What does regression tell you about operating model maturity?

> **Checkpoint 1:** Worksheet A has the score, category breakdown, comparison, and at least five prioritized actions with a quick-win versus governed-rollout call.

## Exercise 2: Investigate Incident 74802 (15 minutes)

Incidents correlate alerts from multiple workloads into one story. Incident 74802 shows signals from Defender XDR, Microsoft Sentinel, Microsoft Purview Data Loss Prevention (DLP), and Microsoft Purview Insider Risk Management in a single queue. This is the product of cross-workload correlation that Marcus Chen's analysts currently perform manually across consoles.

### Steps

1. In Tab 2, open https://security.microsoft.com/incidents?tid=YOUR-TENANT-ID. The navigation path is **Investigation & response** \> **Incidents & alerts** \> **Incidents**. Allow up to 30 seconds for the list to load.

2. Review the active filter set. The validated view showed **Alert severity: High, Medium**; **Incident severity: High, Medium**; **Incident assignment: Any**; **Priority score: 15-100**; **Service/detection sources: Any**; **Tags: Any**; and **Entities: Any**.

3. Locate incident **74802**: \*Multi-stage incident involving Execution & Exfiltration involving multiple users reported by multiple sources\*. Use the search box or the incident ID if it is not on the first page.

4. From the list row, record the severity, categories, impacted assets, service sources, detection sources, status, and workspace in Worksheet B.

5. Select the incident name. It redirects to an incident2/74802/overview address and opens the **Attack story** experience. Wait up to 30 seconds. In the header, confirm **High**, **Resolved**, the assigned account, **Unclassified**, and the last update time.

6. Select **View incident details**. Record the priority assessment, classification, categories, activity dates, creation time, workspace, incident description, impacted users and apps, evidence, and active-alert count. Close the pane without changing anything.

7. The incident graph can remain on **Loading incident graph** in this tenant. Do not wait for it or use it as evidence. Record the graph as **not available** if it does not load.

8. Identify the stages of the attack that the incident represents (execution, defense evasion, exfiltration). Map each stage to the Defender workload or Purview capability that would detect it.

9. Look for evidence of automated investigation, attack disruption, or automated response in the incident. Record whether it is present. If you cannot find it, record that as a finding rather than assuming.

10. Estimate which parts of this investigation Caldova's Tier 1 analysts would escalate today and which a unified incident queue plus automation could handle. Record in Worksheet B.

> **Fallback if the detail page stalls:** Wait 30 seconds and refresh once. If the page still stalls, complete the list-row fields, mark detail-only fields and the graph **not visible**, and continue. Do not claim detail evidence that you did not observe.

### Expected results (representative values seen on 2026-10-12)

| **Item** | **Value observed in the demo tenant** |
|----|----|
| Incident ID and name | 74802: Multi-stage incident involving Execution & Exfiltration involving multiple users reported by multiple sources |
| Severity and priority score | High; priority score 35 |
| Categories | Execution; Defense evasion; Exfiltration; Suspicious activity |
| Impacted assets | 165 accounts and 2 apps |
| Active alerts | 0 of 136 (all resolved) |
| Service sources | Microsoft Defender XDR; Microsoft Purview Data Loss Prevention; Microsoft Sentinel; Microsoft Purview Insider Risk Management |
| Detection sources | Custom detection; DLP; Scheduled detection; Insider Risk Management |
| Status, assignment, classification | Resolved; assigned to [assigned demo account]; header shows Unclassified; details pane shows Classification Not set (no determination field was observed) |
| Created and workspace | May 13, 2026, 5:25 AM; workspace CyberSOC |
| Detail-pane activity and evidence | First activity Oct 27, 2025, 8:00 PM; last activity Oct 11, 2026, 8:00 PM; evidence includes [demo evidence IP address] |

> **Validated limitation:** The Attack story page and **View incident details** pane load, but the incident graph remained on **Loading incident graph** after filters were reset and more than 45 seconds elapsed. The exercise does not depend on the graph.

### Worksheet B: Incident 74802 analysis

| **Question** | **Your finding** |
|----|----|
| Severity, categories, scope (assets) |  |
| Service sources and detection sources |  |
| Attack stages observed and the workload that detects each |  |
| Detail pane: priority, classification, dates, workspace, evidence |  |
| Incident graph (loaded or not available) |  |
| Automated investigation / attack disruption evidence (present, absent, not visible) |  |
| What a SOC would have to correlate manually without a unified incident |  |
| Gaps or dependencies (for example, Sentinel connection, DLP policy, Insider Risk policy) |  |
| Work that Tier 1 could resolve with automation |  |
|  |  |

### CSA discussion prompts

- Incident 74802 shows 165 impacted accounts. What response dependencies (identity, endpoint, data) must be in place to contain it? Who owns each at Caldova?

- Sentinel and Purview signals appear in the same Defender incident. How would you explain that unified workflow to Priya Nair, and what tradeoffs (data ingestion cost, role design, workspace layout) would you raise?

- Classification is **Not set** on a resolved incident. What does that imply for the quality of Caldova's metrics, such as mean time to respond and false-positive rate?

> **Checkpoint 2:** Worksheet B names the attack stages, the sources of each signal, the automation evidence you could or could not find, and at least two candidate cross-workload improvements for Caldova.

## Exercise 3: Analyze an attack path and blast radius (12 minutes)

Attack paths in Microsoft Security Exposure Management show how an attacker could move from an exposed entry point to a critical asset. Where an incident shows what already happened, an attack path shows what could happen. Combined, they let you tell a posture-plus-detection story to Elena Petrov.

### Steps

1. In Tab 3, open https://security.microsoft.com/attack-paths?viewId=attack-paths&tid=YOUR-TENANT-ID. The navigation path is **Exposure management** \> **Attack surface** \> **Attack paths**. Allow up to 30 seconds.

2. Confirm the **Status** filter is **New, Active**. Note the available filters: entry point type, target type, target criticality, risk level, type, risk factors, and number of hops. The current interface might not display a total item count.

3. Find the active **Critical** path named \*Internet exposed Azure VM with high severity vulnerabilities allows lateral movement to Critical Azure storage account with sensitive data\*. The title is not unique. Select the row whose entry point is **alpine-srv1**, target is **c5uj5lrp6r3dm**, affected-assets value is **5**, and recommendations value is **6**.

4. Record in Worksheet C the entry point, target, target criticality, path type, number of affected assets, number of recommendations, and risk factors.

5. Select the path to open its **Overview**. Wait for the graph to load.

6. Trace the graph from entry point to target. Write the node chain in Worksheet C.

7. Read the description and the attack story steps. Mark each hop where a **trust boundary** is crossed (internet to VM, VM to managed identity, managed identity to storage or Function App).

8. Decide where a single control would break the path most effectively (for example, remediate the vulnerability, remove internet exposure, or reduce the managed identity's permissions). Record your choice and the tradeoff.

### Expected results (representative values seen on 2026-10-12)

| **Item** | **Value observed in the demo tenant** |
|----|----|
| Default status filter | New and Active (the current interface might not display a total item count) |
| Path | Internet exposed Azure VM with high severity vulnerabilities allows lateral movement to Critical Azure storage account with sensitive data |
| Entry point and target | alpine-srv1 (Virtual machine) to c5uj5lrp6r3dm (Storage account) |
| Target criticality; type; status | Critical; Cloud path; Active |
| Affected assets; recommendations | 5 affected assets; 6 recommendations |
| Risk factors | Row: Internet exposure (+3); Overview: Internet exposure, Vulnerabilities, Sensitive data, Lateral movement |
| Graph chain | alpine-srv1 \> mi-serverless \> demo-func \> demo-func \> c5uj5lrp6r3dm |
| Attack story | Exploit the internet-facing VM; authenticate as managed identity; read storage; deploy, modify, or delete Function App code or configuration; run on the Function App and authenticate as its managed identity; read sensitive storage data |

> **Selection note:** Six visible rows shared the same path title during validation. Use the entry point, target, affected-assets value, and recommendations value in step 3 to select the validated row.

> **Scope note:** This path covers Azure resources, which are monitored through Microsoft Defender for Cloud and Exposure Management. It illustrates the architecture pattern. Do not assume Caldova has the same footprint; ask the customer.

### Worksheet C: Attack path and blast radius

| **Element** | **Your finding** |
|----|----|
| Entry point, target, criticality |  |
| Node chain |  |
| Trust boundaries crossed |  |
| Blast radius if the entry point is compromised (what else can the identity reach?) |  |
| Best single control to break the path, and its tradeoff |  |
| Which Defender capability would detect exploitation (incident side) |  |
| Response dependency (who must act, with which permission) |  |
|  |  |

### CSA discussion prompts

- How do you connect an attack path (potential) with Incident 74802 (actual) in one narrative for Elena? Where does the story change from posture to detection and response?

- The managed identity has broad permissions. Who typically owns that decision (cloud platform team, security, application owner), and how do you establish governance?

- How would you answer Liam if he asks whether he needs another product to see this? Which entitlements already cover it, and where would you verify licensing?

> **Checkpoint 3:** Worksheet C has the node chain, the trust boundaries, the recommended control, and a link to a detection capability.

## Exercise 4: Architecture map and prioritized adoption roadmap (10 minutes)

Use the evidence from Exercises 1 through 3 to design Caldova's target Defender XDR architecture and a phased roadmap. This is an offline design step. You do not need the portal.

### Steps

1. In Worksheet D, complete the architecture map: for each workload, note what Caldova owns, current state (from the case study), signal it contributes to the unified incident queue, and the gap or dependency.

2. Add the items you saw in the tenant that support each row (for example, Sentinel and Purview signals in Incident 74802, Device posture in Secure Score, attack paths from Exposure Management).

3. Identify at least three failure modes (for example, endpoints not onboarded, Sentinel connector disabled, automation levels left at default) and the tradeoff of the fix.

4. In Worksheet E, build a three-phase roadmap: **Quick wins (0 to 30 days)**, **Foundation (30 to 90 days)**, **Optimize (90 to 180 days)**. For each item, assign owner, dependency, gate, and measurable technical and business criteria.

5. Rank the items by value and effort. Mark which items require no new licensing.

### Worksheet D: Architecture map

| **Workload / layer** | **Caldova owns and state** | **Signal into unified incident** | **Gap, dependency, or constraint** |
|----|----|----|----|
| Endpoint: Microsoft Defender for Endpoint | Owned; about 31,000 endpoints report consistently, onboarding varies | Device alerts, automated investigation and response |  |
| Email and collaboration: Defender for Office 365 | Owned | Phishing and collaboration alerts |  |
| Identity: Microsoft Entra ID, Defender for Identity | Entra ID owned; confirm Defender for Identity and Entra ID Protection | Identity risk and lateral movement signals |  |
| Cloud apps: Microsoft Defender for Cloud Apps | Confirm entitlement and deployment | App and session signals |  |
| Vulnerability and exposure: Microsoft Defender Vulnerability Management, Exposure Management | Confirm entitlement | Exposure insights, attack paths, Secure Score |  |
| SIEM: Microsoft Sentinel in the Defender portal | Owned | Scheduled and custom detections in one queue |  |
| Data protection: Microsoft Purview DLP, Insider Risk Management | Confirm entitlement | Data exfiltration and insider signals |  |
| Response: automated investigation and response, attack disruption | Automation inconsistently used | Automatic containment and remediation |  |
| Third-party tools | Present | Connector or integration points |  |

### Worksheet E: Prioritized adoption roadmap

| **Phase** | **Initiative** | **Owner** | **Dependency / gate** | **Success measure (technical and business)** | **Priority** |
|----|----|----|----|----|----|
| Quick wins (0 to 30 days) |  |  |  |  |  |
| Quick wins (0 to 30 days) |  |  |  |  |  |
| Foundation (30 to 90 days) |  |  |  |  |  |
| Foundation (30 to 90 days) |  |  |  |  |  |
| Optimize (90 to 180 days) |  |  |  |  |  |
| Optimize (90 to 180 days) |  |  |  |  |  |
|  |  |  |  |  |  |

***Example starting points** (not required): complete endpoint onboarding verification; apply the highest-impact low-risk Secure Score actions; confirm Sentinel is connected to the Defender portal; review automated investigation levels; define an incident classification standard; track Tier 1 escalation rate and mean time to respond.*

> **Checkpoint 4:** Worksheet D has no empty gap column, and Worksheet E has at least six items, each with an owner and a measurable success measure.

## Exercise 5: Incident-to-value narrative (5 minutes)

Turn the evidence into a short story that leaders remember. A good narrative moves from what happened, to how the integrated platform handled it, to what it means in cost, risk, and time.

1. Complete Worksheet F using the structure shown. Write for Elena and Liam, not for analysts.

2. Use the tenant only as an illustration: say, 'In a representative environment, a multi-stage incident across endpoint, data, and SIEM signals appeared as one incident...'

3. State the Caldova-specific consequence in terms of the case-study pain points (for example, Tier 1 escalation of about 40%).

4. Close with an ask: one decision and one next step.

### Worksheet F: Incident-to-value narrative

| **Narrative element** | **Your text** |
|----|----|
| Situation (Caldova pain point) |  |
| Representative evidence (what the demo incident and attack path show) |  |
| What the integrated architecture changes |  |
| Outcome and measure (time, cost, risk) |  |
| Tradeoff, residual risk, or likely objection and response |  |
| Decision requested and next action |  |
|  |  |

> **Checkpoint 5:** Every row of Worksheet F is complete and none of the figures claim to be Caldova metrics unless they come from the case study.

## Exercise 6: Three-minute customer pitch (3 minutes)

Deliver (or rehearse with a partner) a three-minute executive pitch to your chosen persona. Use Worksheet G to plan roughly 30 seconds for the opening, 90 seconds for the story and architecture, 45 seconds for the roadmap, and 15 seconds for the ask.

### Worksheet G: Three-minute pitch

| **Segment** | **Time** | **Your talking points** |
|----|----|----|
| Opening: acknowledge the concern (complexity, cost, capacity) | 0:30 |  |
| Story: incident and attack path evidence, integrated architecture | 1:30 |  |
| Roadmap: first 30 days and the success measures | 0:45 |  |
| Ask: decision and next step | 0:15 |  |
|  |  |  |

### Self-assessment rubric

| **Criterion**                                    | **Yes / No / Partial** |
|--------------------------------------------------|------------------------|
| Emphasizes products Caldova already owns         |                        |
| Distinguishes tenant evidence from Caldova facts |                        |
| Addresses complexity and cost concerns directly  |                        |
| Names measurable outcomes and an owner           |                        |
| States a clear ask                               |                        |

> **Checkpoint 6:** You delivered or rehearsed the pitch inside 3 minutes and scored it against the rubric.

## Cleanup

No cleanup is required in the tenant. This lab is read-only and creates no resources, policies, incidents, tags, comments, or status changes. Sign out of the portal when you finish, and close any flyouts you opened without saving.

If you accidentally changed something (for example, an action status or an incident assignment), tell your facilitator immediately so the change can be reviewed. Do not attempt to fix it yourself.

## Troubleshooting

| **Issue** | **Likely cause** | **Resolution** |
|----|----|----|
| Incident list takes a long time (about 20 seconds) | Large incident queue in the demo tenant | Wait; do not refresh repeatedly. Keep the tab open from Exercise 0. |
| Incident 74802 not on the first page | Default filter or paging | Search by incident ID, or adjust the date range and status filters (viewing only). |
| Incident detail page stalls or shows console errors | Heavy single-page app and redirect to incident2 | Wait 30 seconds, refresh once, then continue with list-row data. |
| Attack path list is empty | Filter set to a different status or scope | Reset filters; confirm status **New** and **Active**. |
| Incident graph remains on 'Loading incident graph' | Known behavior observed for Incident 74802 | Reset the graph filters once; if it still does not load, record **not available** and use the list row plus **View incident details**. |
| Several attack paths have the same title | The path title is not unique | Select the row with entry **alpine-srv1**, target **c5uj5lrp6r3dm**, **5** affected assets, and **6** recommendations. |
| Values differ from this guide | Live tenant data changes | Use your own values; the exercise outcomes still apply. |
| Access denied or missing menu items | Role or licensing differences | Ask your facilitator for Security Reader access. |
| Portal asks you to sign in repeatedly | Browser blocks third-party cookies | Allow cookies for security.microsoft.com or use a private window. |

## Facilitator and CSA discussion guide

Use these prompts in the debrief after Exercise 6 or between exercises.

- **Complexity objection.** 'More technology increases complexity.' How does consolidation in one portal change that answer, and what did you see in Incident 74802 that supports it?

- **Value proof.** Which two measures (for example, Tier 1 escalation rate, time to contain, Secure Score Device category) would you commit to at 90 days?

- **Entitlement versus deployment.** Which capabilities in Worksheet D need a licensing verification before they appear in a customer proposal?

- **Residual risk.** What risks remain after your roadmap, and who accepts them?

- **Third-party tools.** Where do you keep them, replace them, or integrate them?

## Validation matrix

This table states exactly what was checked in the live tenant on 2026-10-12 and what was not. It uses these categories: **Fully validated** (the step and expected values were observed), **Validated with limitation** (the step works but part of it could not be confirmed or was unstable), and **Not required** (the step does not depend on tenant access).

| **Exercise / step** | **Status** | **What was observed or limitation** |
|----|----|----|
| Ex. 0: Open Defender portal and the three pages | Fully validated | Secure Score, Incidents, and Attack paths pages loaded in the tenant. |
| Ex. 1: Secure Score overview, categories, comparison | Fully validated | 56.59%, 1,252.3 of 2,213 points; Identity 59.14%, Data 100%, Device 49.36%, Apps 73.57%; similar-size 47.99. |
| Ex. 1: Action status counts and top recommended actions | Fully validated | Regressed 55; To address 211; Planned 5; Risk accepted 2; top actions as listed above. |
| Ex. 1: View all actions and open the LSA flyout | Fully validated | View all opened the ranked Recommended actions page. The LSA flyout showed description, 29/29 exposed devices, 0/8 points, Device, and Defender for Endpoint. |
| Ex. 2: Incident list and Incident 74802 row data | Fully validated | List loaded in about 20 seconds; row values matched the expected-results table. |
| Ex. 2: Incident detail header and details pane | Fully validated | Route redirected to incident2/74802/overview; the Attack story experience and View incident details pane exposed the values listed above. |
| Ex. 2: Incident graph | Validated with limitation | The graph remained on Loading incident graph after filters were reset and more than 45 seconds elapsed. It is not required. |
| Ex. 2: Evidence of automated investigation or attack disruption | Validated with limitation | Not confirmed in the tenant. Learners record present, absent, or not visible. |
| Ex. 3: Attack path list and exact row | Fully validated | List loaded with New and Active selected. Six visible rows shared the title; the row identifiers in step 3 selected the validated path. |
| Ex. 3: Path overview, graph, description, attack story | Fully validated | Graph chain and story steps matched the expected-results table. |
| Ex. 4: Architecture map and roadmap worksheets | Not required | Offline design work; no tenant access needed. |
| Ex. 5: Narrative worksheet | Not required | Offline writing; uses tenant evidence as illustration. |
| Ex. 6: Pitch | Not required | Offline rehearsal. |
| Read-only constraint and cleanup | Fully validated | Only navigation and reading were performed; no state changes were made. |
| Caldova facts | Not required | From the case study; not present in the tenant. |
| Incomplete fifth advisory question in the source | Not required | Intentionally not invented. |

### Remaining limitations

- Tenant values are live and may change; learners may see different numbers.

- The Incident 74802 graph did not load during validation and is not required. Use the list row and View incident details pane.

- The attack-path title is not unique; use the four row identifiers in Exercise 3 to select the correct row.

- Menu labels in the Defender portal change between releases; URLs with the tenant ID are the authoritative navigation.

- Licensing entitlements for Caldova must be verified with the customer; the lab does not assert them.

### References

- Microsoft Defender XDR documentation: https://learn.microsoft.com/defender-xdr/

- Microsoft Secure Score: https://learn.microsoft.com/defender-xdr/microsoft-secure-score

- Incidents in Microsoft Defender XDR: https://learn.microsoft.com/defender-xdr/incidents-overview

- Microsoft Security Exposure Management attack paths: https://learn.microsoft.com/security-exposure-management/work-attack-paths-overview

*The four public documentation reference URLs returned HTTP 200 when checked on October 27, 2026. This link check does not revalidate the live-tenant procedures.*
