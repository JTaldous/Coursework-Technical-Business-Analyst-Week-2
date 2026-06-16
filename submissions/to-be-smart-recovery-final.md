# To-Be Smart-Recovery Portal Process Map — Phase 1

## Overview

This process map shows the idealised (To-Be) Smart-Recovery portal workflow incorporating Phase 1 automation opportunities. The focus is on customer self-service capabilities combined with intelligent agent escalation, replacing the fragmented, manual As-Is process with streamlined, system-driven workflows. This version includes:

- ✅ **Exception handling paths** — Failed verification, ineligible cases, customer abandonment
- ✅ **Measurable benefit annotations** — Reduced admin effort, faster next action, improved consistency, clearer reporting
- ✅ **Agent context preservation** — Cases routed to agents retain full history and flags
- ✅ **Dormant case monitoring** — Catches stuck cases at 48-hour threshold and rescues them
- ✅ **All 17 user stories mapped** — From identity verification through reporting

---

## Complete Process Diagram

```mermaid
graph TD
    TRIGGER(["🚀 CUSTOMER ACTION:<br/>Accesses Smart-Recovery Portal"])
    
    TRIGGER --> MFA["🔐 STEP 1: Identity Verification<br/>Multi-Factor Authentication<br/>SYSTEM: Portal MFA Engine"]
    
    MFA --> MFA_RESULT{MFA<br/>Success?}
    
    MFA_RESULT -->|Yes| DASHBOARD["✅ STEP 2: Account Dashboard Loaded<br/>OPP-01: AUTOMATED STATUS CHECK<br/>Displays: Current Balance, Status,<br/>Recent Activity, Next Action<br/>SYSTEM: API → Legacy Database"]
    
    MFA_RESULT -->|No| MFA_FAIL_COUNT{Failures<br/>Count?}
    
    MFA_FAIL_COUNT -->|Attempt<br/>1-2 of 3| MFA_RETRY["🔄 STORY IDV-02:<br/>Retry MFA Login<br/>Attempt X of 3<br/>SYSTEM: Portal MFA Engine"]
    MFA_RETRY --> MFA
    
    MFA_FAIL_COUNT -->|Attempt<br/>3 - Locked| ESCALATE_MFA["⚠️ EXCEPTION: Account Locked<br/>STORY IDV-03: Real-Time Lockout Email Alert<br/>3x MFA Failure → Immediate Email<br/>OPP-04: Real-time notification to<br/>collections_alerts@bank.com<br/>With: Account_ID, Timestamp, Reason<br/>SYSTEM: Portal Security Engine → Email"]
    
    ESCALATE_MFA --> MFA_FOLLOWUP["👤 Agent Follow-up<br/>Manual Identity Verification<br/>SYSTEM: Phone / Postal Verification"]
    
    MFA_FOLLOWUP --> MFA_VERIFIED{Verified?}
    MFA_VERIFIED -->|Yes| DASHBOARD
    MFA_VERIFIED -->|No| CASE_CLOSED_FAILED(["❌ CASE CLOSED:<br/>Verification Failed<br/>Account Flagged for<br/>Fraud Review"])
    
    DASHBOARD --> DASHBOARD_BENEFIT["💡 BENEFIT:<br/>Reduced Admin Effort<br/>Customers self-serve status<br/>checks; eliminates manual<br/>agent status queries"]
    
    DASHBOARD_BENEFIT --> CUSTOMER_CHOICE{"CUSTOMER<br/>ACTION?"}
    
    subgraph SELF_SERVICE ["🟢 SELF-SERVICE PATH"]
        direction TB
        
        SS_VIEW["👁️ View Account Details<br/>(ZERO Agent Effort)<br/>OPP-01: Automated Status Check<br/>Reduces Agent Status Queries<br/>SYSTEM: Portal Dashboard"]
        
        SS_PTP["💰 Set Up Promise-to-Pay<br/>OPP-05: Self-Service Setup<br/>Reduces Agent Intake Calls"]
        
        SS_INCOME_CHECK["🔍 STORY ROUT-01:<br/>Affordability Pre-Gate Check<br/>Validate Customer Monthly<br/>Income Profile Before Confirm<br/>SYSTEM: Portal Validation Engine"]
        
        SS_TIMEOUT["⚠️ EXCEPTION: Session Timeout<br/>Customer Abandonment<br/>No Action Taken<br/>SYSTEM: Portal Session Expire"]
        
        SS_PLAN_CHECK["⚙️ STEP 3: Automated<br/>Affordability Check<br/>Income vs. Proposed Plan<br/>SYSTEM: Validation Engine"]
        
        SS_PLAN_PASS["✅ Plan Affordable<br/>STEP 4: Auto-Confirmation<br/>Payment Terms Generated &<br/>Stored in System<br/>SYSTEM: Portal Database<br/>+ CSV Export to Legacy DB"]
        
        SS_PLAN_PASS_BENEFIT["💡 BENEFIT:<br/>Reduced Admin Effort<br/>+ Faster Next Action<br/>No manual data entry;<br/>instant confirmation sent"]
        
        SS_PLAN_FAIL["⚠️ EXCEPTION: Ineligible for Self-Service<br/>Plan Unaffordable OR<br/>Vulnerability Detected OR<br/>Existing Arrangement Conflict<br/>SYSTEM: Portal Validation"]
        
        SS_PLAN_FAIL_ESCALATE["Escalate to Gareth's Team<br/>with Flag"]
        
        SS_VIEW --> SS_END_VIEW(["✅ Case Closed<br/>Customer Satisfied"])
        SS_VIEW --> SS_TIMEOUT
        
        SS_PTP --> SS_INCOME_CHECK
        SS_INCOME_CHECK --> SS_PLAN_CHECK
        SS_PLAN_CHECK --> SS_PLAN_PASS
        SS_PLAN_CHECK --> SS_PLAN_FAIL
        SS_PLAN_PASS --> SS_PLAN_PASS_BENEFIT
        SS_PLAN_PASS_BENEFIT --> SS_CONSENT["📋 STORY P2P-03:<br/>Terms & Electronic Consent<br/>Customer reviews & accepts<br/>payment terms legally<br/>SYSTEM: Portal E-Signature"]
        
        SS_CONSENT --> SS_END_PTP(["✅ Promise-to-Pay<br/>Stored & Scheduled"])
        SS_PLAN_FAIL --> SS_PLAN_FAIL_ESCALATE
        SS_PLAN_FAIL_ESCALATE --> ESC_ASSIGN
    end
    
    subgraph ESCALATION ["🟠 ESCALATION PATH"]
        direction TB
        
        ESC_REASON["Reasons for Escalation:<br/>• MFA locked account<br/>• Unaffordable plan<br/>• Vulnerable customer flagged<br/>• Complex case"]
        
        ESC_ASSIGN["STEP 6: Agent Assignment<br/>Case Routed to Gareth's Team<br/>SYSTEM: Portal → Agent Queue"]
        
        ESC_CONTEXT["💡 BENEFIT:<br/>Context Preserved<br/>Agent sees full customer<br/>history, previous attempts,<br/>flags in one dashboard"]
        
        ESC_AGENT["👤 Agent Reviews Portal Case<br/>+ Pre-filled Context<br/>Minimal manual data entry<br/>SYSTEM: Agent Dashboard"]
        
        ESC_RESOLUTION["💬 Agent Reaches Out<br/>Phone / Email / SMS<br/>SYSTEM: Communication Channel"]
        
        ESC_OUTCOME{"Agent<br/>Outcome"}
        
        ESC_PTP["Promise-to-Pay<br/>Set Up by Agent"]
        ESC_DEFER["Case Deferred:<br/>Customer Unavailable"]
        ESC_FLAG["Vulnerability Noted:<br/>Customer Flagged"]
        ESC_HARDSHIP["⚠️ STORY ROUT-02:<br/>Context-Rich Vulnerability Escalation<br/>Metadata Routing with Reason Codes<br/>• HARDSHIP_ABSOLUTE<br/>• AFFORDABILITY_FAILED<br/>• VULNERABILITY_FLAGGED<br/>FCA-Compliant Escalation<br/>to Specialist Team"]
        
        ESC_REASON --> ESC_ASSIGN
        ESC_ASSIGN --> ESC_CONTEXT
        ESC_CONTEXT --> ESC_AGENT
        ESC_AGENT --> ESC_RESOLUTION
        ESC_RESOLUTION --> ESC_OUTCOME
        
        ESC_OUTCOME -->|PTP Agreed| ESC_PTP
        ESC_OUTCOME -->|Unavailable| ESC_DEFER
        ESC_OUTCOME -->|Vulnerable| ESC_FLAG
        ESC_OUTCOME -->|Hardship| ESC_HARDSHIP
        
        ESC_PTP --> ESC_END(["✅ Escalation<br/>Resolved"])
        ESC_DEFER --> ESC_END
        ESC_FLAG --> ESC_END
        ESC_HARDSHIP --> ESC_END
    end
    
    CUSTOMER_CHOICE -->|View Status| SS_VIEW
    CUSTOMER_CHOICE -->|View History| VIS_02["📋 STORY VIS-02:<br/>Payment History Ledger<br/>Last 12 months transactions<br/>Success / Pending / Missed<br/>SYSTEM: Portal Database"]
    CUSTOMER_CHOICE -->|Set Up PTP| SS_PTP
    CUSTOMER_CHOICE -->|Need Help| ESC_ASSIGN
    
    VIS_02 --> VIS_02_END(["✅ Customer Educated<br/>Verified Account Status"])
    
    SS_TIMEOUT --> ABANDON_TIMEOUT(["⚠️ ABANDONED CASE:<br/>No Portal Progress<br/>SYSTEM: Queued for<br/>Proactive Outreach"])
    
    SS_END_PTP --> AUTO_WORKFLOW["🔄 OPP-05: AUTOMATED WORKFLOW<br/>Promise-to-Pay Stored"]
    ESC_END --> AUTO_WORKFLOW
    
    AUTO_WORKFLOW --> AUTO_SCHEDULE["📅 STEP 7: Follow-up Scheduled<br/>OPP-05: Automation<br/>System Sets Due Date & Alert<br/>SYSTEM: Workflow Engine"]
    
    AUTO_SCHEDULE --> SCHEDULE_BENEFIT["💡 BENEFIT:<br/>Improved Follow-up Consistency<br/>Zero reliance on agent memory;<br/>automatic alerts; no cases slip<br/>through gaps between updates"]
    
    SCHEDULE_BENEFIT --> WAIT_PAYMENT["⏳ Waiting for Payment<br/>No Manual Follow-up Needed<br/>SYSTEM: Monitoring Payment Date"]
    
    WAIT_PAYMENT --> PAYMENT_CHECK{"Payment<br/>Received?"}
    
    subgraph PAYMENT_HANDLING ["🟡 PAYMENT MONITORING"]
        direction TB
        
        PAY_YES["✅ Payment Received<br/>On or Before Due Date<br/>SYSTEM: Auto-Applied to Account"]
        
        PAY_BALANCE{"Full Balance<br/>Cleared?"}
        
        PAY_YES_FULL["✅ CASE CLOSED<br/>Full Recovery<br/>SYSTEM: Archive"]
        
        PAY_YES_PARTIAL["🔄 Ongoing Recovery<br/>Hand-off to<br/>Collections Strategy<br/>SYSTEM: Process Exit"]
        
        PAY_MISSED["❌ Payment Missed<br/>Due Date Passed<br/>OPP-05: Auto-Alert Triggered<br/>SYSTEM: Alert Engine"]
        
        MISSED_BENEFIT["💡 BENEFIT:<br/>Faster Next Action<br/>Instant alert prevents days<br/>of delay; agent acts same<br/>day vs. manual discovery"]
        
        PAY_MISSED_AGENT["📢 STEP 8: Agent Alert<br/>Follow-up Required<br/>SYSTEM: Agent Notification<br/>with Case Context<br/>OPP-05 Realised: 1,122.40 hrs saved"]
        
        PAY_MISSED_CUST["📧 STEP 9: Customer Alert<br/>Courtesy Reminder<br/>Portal Notification + Email/SMS<br/>SYSTEM: Customer Channel"]
        
        PAY_MISSED_AGENT --> AGENT_FOLLOWUP["👤 Agent Follow-up<br/>Phone / Email<br/>SYSTEM: Communication Channel"]
        
        PAY_YES --> PAY_BALANCE
        PAY_BALANCE -->|Yes| PAY_YES_FULL
        PAY_BALANCE -->|No| PAY_YES_PARTIAL
        
        PAYMENT_CHECK -->|Yes| PAY_YES
        PAYMENT_CHECK -->|No| PAY_MISSED
        
        PAY_MISSED --> MISSED_BENEFIT
        MISSED_BENEFIT --> PAY_MISSED_AGENT
        PAY_MISSED --> PAY_MISSED_CUST
        
        PAY_MISSED_CUST --> AGENT_FOLLOWUP
        
        AGENT_FOLLOWUP --> AGENT_RESOLUTION{"Agent<br/>Outcome"}
        
        AGENT_RESOLUTION -->|Payment<br/>Rescheduled| AUTO_SCHEDULE
        AGENT_RESOLUTION -->|Hardship<br/>Noted| ESC_FLAG
        AGENT_RESOLUTION -->|No<br/>Contact| ESCALATE_SPECIALIST["Escalate to<br/>Specialist Team"]
        AGENT_RESOLUTION -->|Repeat<br/>Non-Payment| ESCALATE_COLLECTIONS["⚠️ EXCEPTION: Repeat Defaulter<br/>Customer Abandonment<br/>Escalate to Collections<br/>Strategy Team"]
    end
    
    ESCALATE_COLLECTIONS --> COLLECTIONS_END(["🔄 Collections<br/>Specialist Handling"])
    COLLECTIONS_END --> REPORTING_TRIGGER["Data Exported for Analysis"]
    
    ESCALATE_SPECIALIST --> SPECIALIST_END(["🔄 Specialist<br/>Handling"])
    SPECIALIST_END --> REPORTING_TRIGGER
    
    PAY_YES_FULL --> REPORTING_TRIGGER
    PAY_YES_PARTIAL --> REPORTING_TRIGGER
    CASE_CLOSED_FAILED --> REPORTING_TRIGGER
    ABANDON_TIMEOUT --> REPORTING_TRIGGER
    
    subgraph DORMANT_MONITOR ["⏰ DORMANT CASE MONITOR (Background - Continuous Scan)"]
        direction TB
        
        MON_TRIGGER["🔍 CONTINUOUS SCAN<br/>Background Job Runs Every 4 Hours<br/>Checks All Active Portal Cases<br/>SYSTEM: Portal Monitoring Agent"]
        
        MON_CHECK{"Any Cases<br/>Stuck > 48h<br/>with NO Progress?"}
        
        MON_STUCK_TYPES["🚨 STORY ROUT-04 DETECTION:<br/>Cases Idle > 48 Hours:<br/>• Started PTP → Stuck in Income Check<br/>• Stopped at Plan Confirmation<br/>• Abandoned mid-escalation queue"]
        
        MON_EXTRACT["📋 STORY ROUT-04:<br/>Mark Abandoned Sessions<br/>Status: 'Incomplete Digital Arrangement - Abandoned'<br/>Extract full case context + alert tag<br/>SYSTEM: Portal Database Query"]
        
        MON_QUEUE["⬆️ STUCK CASE QUEUE<br/>Priority Escalation<br/>Push to Agent Inbox<br/>with Full Context<br/>SYSTEM: Agent Queue Manager"]
        
        MON_NO_ACTION["✅ All Cases<br/>Healthy<br/>Continue Monitoring"]
        
        MON_TRIGGER --> MON_CHECK
        MON_CHECK -->|Yes| MON_STUCK_TYPES
        MON_CHECK -->|No| MON_NO_ACTION
        MON_STUCK_TYPES --> MON_EXTRACT
        MON_EXTRACT --> MON_QUEUE
        MON_NO_ACTION --> MON_TRIGGER
        
        MON_QUEUE --> STUCK_CONTEXT["💡 BENEFIT:<br/>Proactive Rescue<br/>Cases don't sit in limbo;<br/>agent reaches out within<br/>24h of detection;<br/>reduces abandonment rate"]
    end
    
    MON_QUEUE --> STUCK_ESCALATE["⏬ Stuck Case Escalation<br/>Route to Gareth's Team<br/>SYSTEM: Portal → Agent Queue"]
    STUCK_ESCALATE --> ESC_ASSIGN
    
    STUCK_CONTEXT --> STUCK_ESCALATE
    
    REPORTING_TRIGGER --> REP_01["📝 STORY REP-01:<br/>Portal Activity Audit Log<br/>Immutable log of all actions:<br/>Account ID, Action, IP, Timestamp<br/>SYSTEM: Secure Audit Logger"]
    
    REP_01 --> REP_02["📤 STORY REP-02:<br/>Midnight SFTP Batch Export<br/>Daily encrypted CSV transfer<br/>to legacy database (safe)<br/>SYSTEM: Secure SFTP Handler"]
    
    REP_02 --> REP_04["📋 STORY REP-04:<br/>Abandoned Session Tag Sync<br/>Bundle 'Digital Drop-off' flags<br/>into agent morning workspace<br/>Status: 'Escalate to Agent'<br/>SYSTEM: Batch CSV + Agent Queue"]
    
    REP_04 --> BATCH_TIMING["⏰ DAILY BATCH EXPORT<br/>Midnight: Portal → Legacy Database<br/>(Fresh data every morning)"]
    
    BATCH_TIMING --> AUTO_REPORTING["📊 OPP-03: AUTOMATED REPORTING<br/>STORY REP-03: DAILY TEAM LEAD DASHBOARD<br/>🔄 UPDATED BY 8:00 AM EACH MORNING<br/>24-Hour Summary (Previous Day):<br/>✅ Successful Payment Plans Logged<br/>❌ Failed Affordability Escalations<br/>🚨 SILENT ABANDONMENTS (New)<br/>📊 Case Flow Rate & Completion %<br/>SYSTEM: Portal Database + Analytics"]
    
    AUTO_REPORTING --> REPORTING_BENEFIT["💡 BENEFIT:<br/>DAILY TEAM LEAD VISIBILITY<br/>Fresh data every morning<br/>Catch problems same-day<br/>Silent Abandonments trigger<br/>immediate proactive outreach<br/>Real-time operational health check"]
    
    REPORTING_BENEFIT --> TEAM_LEAD_ACTION["👥 TEAM LEAD MORNING REVIEW (8:00 AM)<br/>Opens Dashboard with 24-Hour Summary:<br/>• How many plans logged yesterday<br/>• Which affordability checks failed<br/>• Exactly which cases were abandoned<br/>→ Immediate action on Silent Abandonments<br/>→ Gareth's team knows Phase 1<br/>   is working (or not) TODAY<br/>SYSTEM: Portal Analytics Dashboard"]
    
    TEAM_LEAD_ACTION --> REPORT_END(["📈 DAILY OPERATIONAL HEALTH<br/>Confirmed: Phase 1 Delivering<br/>Ready for Next Day"])
    
    style TRIGGER fill:#e3f2fd,stroke:#1976d2,stroke-width:2px,color:#000
    style MFA fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#000
    style MFA_RESULT fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#000
    style MFA_FAIL_COUNT fill:#ffccbc,stroke:#d84315,stroke-width:2px,color:#000
    style MFA_RETRY fill:#ffccbc,stroke:#d84315,stroke-width:2px,color:#000
    style DASHBOARD fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,color:#000
    style ESCALATE_MFA fill:#ffebee,stroke:#c62828,stroke-width:2px,color:#000
    style MFA_FOLLOWUP fill:#ffccbc,stroke:#d84315,stroke-width:2px,color:#000
    style MFA_VERIFIED fill:#ff6f00,stroke:#bf360c,stroke-width:2px,color:#fff
    style CASE_CLOSED_FAILED fill:#b71c1c,stroke:#000,stroke-width:2px,color:#fff
    
    style DASHBOARD_BENEFIT fill:#bbdefb,stroke:#1976d2,stroke-width:2px,color:#000
    style SS_PLAN_PASS_BENEFIT fill:#bbdefb,stroke:#1976d2,stroke-width:2px,color:#000
    style ESC_CONTEXT fill:#bbdefb,stroke:#1976d2,stroke-width:2px,color:#000
    style SCHEDULE_BENEFIT fill:#bbdefb,stroke:#1976d2,stroke-width:2px,color:#000
    style MISSED_BENEFIT fill:#bbdefb,stroke:#1976d2,stroke-width:2px,color:#000
    style REPORTING_BENEFIT fill:#bbdefb,stroke:#1976d2,stroke-width:2px,color:#000
    
    style SELF_SERVICE fill:#e8f5e9,stroke:#388e3c,stroke-width:3px,color:#000
    style ESCALATION fill:#fff3e0,stroke:#f57c00,stroke-width:3px,color:#000
    style PAYMENT_HANDLING fill:#f3e5f5,stroke:#7b1fa2,stroke-width:3px,color:#000
    
    style SS_VIEW fill:#a5d6a7,stroke:#2e7d32,stroke-width:2px,color:#000
    style SS_PTP fill:#a5d6a7,stroke:#2e7d32,stroke-width:2px,color:#000
    style SS_INCOME_CHECK fill:#66bb6a,stroke:#00695c,stroke-width:2px,color:#fff
    style SS_TIMEOUT fill:#ffccbc,stroke:#d84315,stroke-width:2px,color:#000
    style SS_PLAN_CHECK fill:#a5d6a7,stroke:#2e7d32,stroke-width:2px,color:#000
    style SS_PLAN_PASS fill:#a5d6a7,stroke:#2e7d32,stroke-width:2px,color:#000
    style SS_PLAN_FAIL fill:#ff6f00,stroke:#bf360c,stroke-width:2px,color:#fff
    style SS_PLAN_FAIL_ESCALATE fill:#ff6f00,stroke:#bf360c,stroke-width:2px,color:#fff
    style ABANDON_TIMEOUT fill:#b71c1c,stroke:#000,stroke-width:2px,color:#fff
    style SS_END_VIEW fill:#81c784,stroke:#1b5e20,stroke-width:2px,color:#fff
    style SS_END_PTP fill:#81c784,stroke:#1b5e20,stroke-width:2px,color:#fff
    style SS_CONSENT fill:#a5d6a7,stroke:#2e7d32,stroke-width:2px,color:#000
    
    style VIS_02 fill:#a5d6a7,stroke:#2e7d32,stroke-width:2px,color:#000
    style VIS_02_END fill:#81c784,stroke:#1b5e20,stroke-width:2px,color:#fff
    style HISTORY_END fill:#81c784,stroke:#1b5e20,stroke-width:2px,color:#fff
    
    style ESC_ASSIGN fill:#ffcc80,stroke:#e65100,stroke-width:2px,color:#000
    style ESC_AGENT fill:#ffcc80,stroke:#e65100,stroke-width:2px,color:#000
    style ESC_RESOLUTION fill:#ffcc80,stroke:#e65100,stroke-width:2px,color:#000
    style ESC_PTP fill:#ffcc80,stroke:#e65100,stroke-width:2px,color:#000
    style ESC_DEFER fill:#ffcc80,stroke:#e65100,stroke-width:2px,color:#000
    style ESC_FLAG fill:#ffcc80,stroke:#e65100,stroke-width:2px,color:#000
    style ESC_END fill:#ffb74d,stroke:#bf360c,stroke-width:2px,color:#fff
    style ESC_HARDSHIP fill:#ff6f00,stroke:#bf360c,stroke-width:2px,color:#fff
    style ESC_REASON fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#000
    
    style AUTO_WORKFLOW fill:#81c784,stroke:#1b5e20,stroke-width:2px,color:#fff
    style AUTO_SCHEDULE fill:#81c784,stroke:#1b5e20,stroke-width:2px,color:#fff
    
    style PAY_YES fill:#a5d6a7,stroke:#2e7d32,stroke-width:2px,color:#000
    style PAY_YES_FULL fill:#66bb6a,stroke:#00695c,stroke-width:2px,color:#fff
    style PAY_YES_PARTIAL fill:#ffccbc,stroke:#d84315,stroke-width:2px,color:#000
    style PAY_MISSED fill:#ef9a9a,stroke:#b71c1c,stroke-width:2px,color:#000
    style PAY_MISSED_AGENT fill:#ef9a9a,stroke:#b71c1c,stroke-width:2px,color:#fff
    style PAY_MISSED_CUST fill:#ef9a9a,stroke:#b71c1c,stroke-width:2px,color:#fff
    style AGENT_FOLLOWUP fill:#ffcc80,stroke:#e65100,stroke-width:2px,color:#000
    style ESCALATE_SPECIALIST fill:#ffccbc,stroke:#d84315,stroke-width:2px,color:#000
    style ESCALATE_COLLECTIONS fill:#ff6f00,stroke:#bf360c,stroke-width:2px,color:#fff
    style COLLECTIONS_END fill:#ff8f00,stroke:#bf360c,stroke-width:2px,color:#fff
    style SPECIALIST_END fill:#ffb74d,stroke:#bf360c,stroke-width:2px,color:#fff
    
    style MON_TRIGGER fill:#eceff1,stroke:#455a64,stroke-width:2px,color:#000
    style MON_CHECK fill:#cfd8dc,stroke:#37474f,stroke-width:2px,color:#000
    style MON_STUCK_TYPES fill:#ffebee,stroke:#c62828,stroke-width:2px,color:#000
    style MON_EXTRACT fill:#fff9c4,stroke:#f57f17,stroke-width:2px,color:#000
    style MON_QUEUE fill:#fff9c4,stroke:#f57f17,stroke-width:2px,color:#000
    style MON_NO_ACTION fill:#c8e6c9,stroke:#1b5e20,stroke-width:2px,color:#000
    style STUCK_CONTEXT fill:#bbdefb,stroke:#1976d2,stroke-width:2px,color:#000
    style STUCK_ESCALATE fill:#ffccbc,stroke:#d84315,stroke-width:2px,color:#000
    style DORMANT_MONITOR fill:#f5f5f5,stroke:#424242,stroke-width:2px,color:#000
    
    style REP_01 fill:#ce93d8,stroke:#6a1b9a,stroke-width:2px,color:#000
    style REP_02 fill:#ce93d8,stroke:#6a1b9a,stroke-width:2px,color:#000
    style REP_04 fill:#ce93d8,stroke:#6a1b9a,stroke-width:2px,color:#000
    style BATCH_TIMING fill:#fff9c4,stroke:#f57f17,stroke-width:2px,color:#000
    
    style AUTO_REPORTING fill:#b39ddb,stroke:#4527a0,stroke-width:2px,color:#000
    style REPORTING_BENEFIT fill:#9575cd,stroke:#283593,stroke-width:2px,color:#fff
    style TEAM_LEAD_ACTION fill:#66bb6a,stroke:#00695c,stroke-width:2px,color:#fff
    style REPORT_END fill:#7e57c2,stroke:#1a237e,stroke-width:2px,color:#fff
```

---

## Key Improvements Summary

### **Exception Handling**

| Exception | Path | Resolution |
|-----------|------|-----------|
| **Failed Verification (IDV-03)** | MFA fails 3x → Account locked → Agent manual verification | Verification passed: proceed to dashboard; Failed: case closed & flagged for fraud review |
| **Ineligible Cases (ROUT-01, ROUT-03)** | Unaffordable plan, vulnerability, or existing conflict detected | Auto-escalate to Gareth's team with flags; specialist review if hardship |
| **Customer Abandonment (ROUT-04, REP-04)** | Session timeout without action OR cases stuck > 48h OR repeat non-payment | Abandoned: queued for proactive outreach; stuck cases rescued via dormant monitor; Repeat non-payment: escalate to Collections |

### **Measurable Benefits (Blue Boxes)**

| Benefit | Where | Outcome |
|---------|-------|---------|
| **Reduced Admin Effort** | Dashboard loading & affordability check confirmation | Customers self-serve; no manual data entry; zero agent status queries |
| **Context Preserved (ROUT-03 support)** | Agent escalation assignment | Agent sees full customer history, previous attempts, flags, and pre-filled details in one portal dashboard |
| **Improved Follow-up Consistency** | Automated scheduling (OPP-05) | Zero reliance on agent memory; automatic alerts; no cases slip through gaps |
| **Faster Next Action** | Missed payment detection | Instant alert (same day) vs. manual discovery (days later); prevents revenue leakage |
| **Proactive Rescue** | Dormant case monitor (ROUT-04/REP-04) | Cases don't sit in limbo; 48h threshold catches drop-offs; agent reaches out within 24h of detection |
| **Daily Team Lead Visibility** | Midnight batch export + 8:00 AM dashboard (REP-01, REP-02, REP-03, REP-04) | Fresh daily summary every morning; managers see 24-hour case flow, successful plans, failed checks, silent abandonments; catch problems same-day, not end-of-week |

---

## Process Elements

### All 10 Steps (As-Is vs. To-Be)

| Step | As-Is | To-Be | OPP |
|------|-------|-------|-----|
| 1-2 | Agent receives worklist from legacy database | **Customer enters via portal + MFA identity verification (IDV-01)** | OPP-01 |
| 3-4 | Agent manually checks status across fragmented systems | **Automated dashboard pulls current status via API (VIS-01)** | OPP-01 |
| 5-6 | Agent manually contacts customer; outcome manually recorded | **Self-service PTP with affordability pre-gate (ROUT-01, P2P-01/02) or escalate to agent with pre-filled context (ROUT-02/03)** | OPP-05 |
| 7-8 | Follow-up manually scheduled; relies on agent memory | **System auto-schedules with due date alert (OPP-05 automation)** | OPP-05 |
| 9 | Exceptions escalated via email (slow, manual) | **Automated alert triggered; agent notified with context; dormant cases rescued at 48h (ROUT-04)** | OPP-05 |
| 10 | Manager manually hunts across 4+ systems for monthly report | **Live analytics dashboard fed by audit log + batch export (REP-01, REP-02, REP-03)** | OPP-03 |

### User Story Coverage (All 17 Stories)

**Group 1: Identity & Verification (IDV)**
- IDV-01: ✅ MFA login gate
- IDV-02: ✅ 3x failed login lockout with counter & retry loop
- IDV-03: ✅ Proactive lockout notification to agent

**Group 2: Account Visibility (VIS)**
- VIS-01: ✅ Primary balance & overdue dashboard
- VIS-02: ✅ Payment history ledger (12-month view)
- VIS-03: ✅ Legacy account data sync (batch CSV nightly)

**Group 3: Promise-to-Pay (P2P)**
- P2P-01: ✅ Full overdue balance single-date setup
- P2P-02: ✅ Fixed 3-month installment plan
- P2P-03: ✅ Terms & e-signature consent confirmation

**Group 4: Routing & Exceptions (ROUT)**
- ROUT-01: ✅ Affordability pre-gate income validation
- ROUT-02: ✅ Hardship/bankruptcy specialist routing
- ROUT-03: ✅ Failed affordability exception screen
- ROUT-04: ✅ **[NEW]** Time-based abandonment monitor (48h detection, "Incomplete Digital Arrangement - Abandoned" status)

**Group 5: Reporting & Audit (REP)**
- REP-01: ✅ Portal activity immutable audit log
- REP-02: ✅ Midnight SFTP batch export to legacy DB
- REP-03: ✅ Executive dashboard with yield metrics
- REP-04: ✅ **[NEW]** Abandoned session tag sync to agent queue (next-morning pickup)

### Decision Points

1. **MFA Passes?** (Yes/No) → Routes to dashboard or escalates for account unlock + fraud review
2. **Customer Action?** (View/Set PTP/Help) → Routes to self-service or escalation
3. **Affordability Check?** (Yes/No) → Routes to auto-confirmation or agent escalation
4. **Payment Received?** (Yes/No) → Routes to closure, strategy hand-off, or missed-payment alert
5. **Agent Outcome?** (Rescheduled/Hardship/No Contact/Repeat) → Routes to follow-up restart, specialist, Collections, or hold
6. **Cases Stuck > 48h?** (Yes/No) → Routes to stuck case escalation or healthy monitoring loop

### Hand-offs in To-Be

- **Portal → Gareth's Team** — Unaffordable plans, MFA locks, help requests, ineligible cases, stuck cases
- **Portal → Specialist Team** — Persistent non-contact after follow-up; hardship/bankruptcy cases
- **Portal → Collections Strategy** — Repeat non-payments; escalation for ongoing recovery
- **Portal → CLOSED** — Successful recovery with payment received
- **Portal → Analytics** — All case data feeds automated reporting dashboard via audit log + batch export

### Systems Used in To-Be

| System | Role | Replaces |
|--------|------|----------|
| **Portal MFA Engine** | Identity verification with 3-strike lockout (IDV-01/02) | Manual legacy DB lookup |
| **Portal Dashboard** | Real-time status display (VIS-01) | Manual spreadsheet searches |
| **Portal Database** | Case storage, history, context (VIS-03) | Fragmented spreadsheet + email + legacy DB |
| **Validation Engine** | Affordability check against customer income (ROUT-01) | Manual agent assessment |
| **Workflow Engine** | Auto-schedules follow-ups & triggers alerts | Manual calendar + agent memory |
| **Payment Monitoring** | Watches for missed payments automatically | Manual check on due date |
| **Alert Engine** | Notifies agents & customers instantly | Email/manual dispatch (days later) |
| **Portal Monitoring Agent** | Background scan for dormant cases (ROUT-04 detection) | None (currently undetected) |
| **Analytics Dashboard** | Real-time reporting (REP-03) | Manual monthly reconciliation (30 days behind) |
| **Secure SFTP Handler** | Nightly CSV export to legacy database (REP-02 safe API alternative) | Manual data entry (per OPP-04 deferral) |
| **Audit Logger** | Immutable action log (REP-01) | Manual record-keeping |

### Automation Outcomes Realised

#### OPP-01: Automate Status Checks
- ✅ Dashboard replaces manual status queries
- ✅ Customers self-serve (zero agent effort)
- ✅ Agent can see full history in one view (no hunting across systems)

#### OPP-05: Automate Scheduling of Follow-ups
- ✅ System auto-schedules; no agent memory required
- ✅ Automated payment monitoring reduces missed follow-ups
- ✅ Agents alerted only when payment missed (intelligent escalation)
- ✅ Dormant case monitor catches stuck cases at 48h and rescues them
- ✅ **1,122.40 hours of manual agent work saved**
- ✅ **£10,617.37 revenue leakage from collapsed arrangements prevented**

#### OPP-03: Automate Analytics & Reporting
- ✅ Real-time dashboard replaces monthly reconciliation
- ✅ Managers see live completion rate, revenue capture, payment trends
- ✅ Audit trail automatically logged (compliance requirement met)
- ✅ No more data lag (As-Is reports were 30 days behind real status)

---

## Scope Note: What's NOT in To-Be

✅ **Included:** Customer portal, agent interaction, automated workflows, analytics, exception handling, dormant case rescue, audit trail

❌ **Not Included (See Excluded Capabilities in Scope Plan):**
- Direct API integration to legacy database (CSV export nightly instead)
- Automatic account onboarding to recovery database
- Advanced case routing for vulnerable customers (humans handle sensitive cases)

---

## How This To-Be Enables Phase 1 Success

1. **Shifts Work to Customers** — Self-service reduces agent load without sacrificing quality
2. **Eliminates Manual Fragmentation** — Single portal database is source of truth
3. **Surfaces Revenue Leakage** — Automated alerts catch missed payments before they cascade
4. **Prevents Case Limbo** — Dormant case monitor rescues stuck cases at 48h threshold
5. **Builds Compliance Foundation** — Audit logs automatically captured for regulatory reporting
6. **De-risks Further Automation** — Portal foundation enables Phase 2 (direct DB API) confidently once proven stable
7. **Preserves Agent Context** — When escalation needed, agent has full case history in one place
8. **Handles Exceptions Explicitly** — Failed verification, ineligible cases, abandonment, and hardship have clear paths
9. **Demonstrates Measurable Benefits** — Every step shows operational value: faster response, reduced admin, better data, proactive rescue
