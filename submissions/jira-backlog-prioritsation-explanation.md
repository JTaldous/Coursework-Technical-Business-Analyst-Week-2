## Tier 1: Foundational Security & Core Ingestion (Highest Priority)
Stories Included: [IDV-01] (MFA Gate), [IDV-02] (3x Lockout), [REP-02] (Midnight SFTP Batch Export Script), [VIS-03] (Legacy Data Sync)
Critical Dependencies: These stories have zero technical dependencies; they are the dependencies.
Workflow/ROI Connection: You cannot build a customer-facing portal without a secure front door (IDV-01). Furthermore, because we officially excluded real-time database writes to protect the 20-year-old system (OPP-04), the midnight SFTP batch engine (REP-02) must be established immediately to ensure data actually flows between systems.

## Tier 2: Core Self-Service ROI Engines (High Priority)
Stories Included: [VIS-01] (Balance Dashboard), [P2P-01] (Pay in Full Setup), [P2P-02] (3-Month Installment Setup), [P2P-03] (Terms & Consent)
Critical Dependencies: Requires Tier 1 validation ([IDV-01]) and data ingestion ([VIS-03]) to be functional so the dashboard can render accurate financial balances.
Workflow/ROI Connection: These stories drive the core financial value of the ROI model. By automating status checks (OPP-01) and enabling self-service promise-to-pay setups (OPP-05), these features actively deflect high-volume, low-complexity phone calls, immediately plugging the bank's labor leakage.

## Tier 3: Guardrails, Exception Routing, and Observability (Medium Priority)
Stories Included: [ROUT-01] (Affordability Check), [ROUT-02] (Vulnerability Routing), [ROUT-04] (Time-Based Abandonment Monitor), [REP-01] (Audit Logs), [REP-04] (Abandoned Session Sync)
Critical Dependencies: Requires Tier 2 payment paths to exist before you can wrap validation rules or disengagement monitors around them.
Workflow/ROI Connection: This tier addresses Daniel's and Gareth's feedback directly. While automated analytics (OPP-03) originally showed a standalone negative ROI, embedding compliance audit logging (REP-01) protects the bank from catastrophic FCA fines. Additionally, the abandonment tracking system (ROUT-04) directly satisfies Gareth's requirement for daily operational tracking, catching "silent drop-offs" before they turn into missed revenue.

# Strategic Justification

Risk Mitigation (Tier 1): The login framework and the scheduled midnight SFTP batch pipeline are prioritised. This addresses the legacy architecture risks flagged in OPP-04. It ensures we protect the core database from cracking without forcing the team to build real-time APIs.
Value Realization (Tier 2): Next, we deploy the account dashboard and structured payment selections. These features target the core financial assumptions of our business case, specifically OPP-01 (Status Checks) and OPP-05 (P2P Capture). By shifting routine payment scheduling to autonomous self-service channels, we eliminate the primary agent labour leak.
Governance & Visibility (Tier 3): Finally, we give the journey affordability guardrails and daily observability tracking. This ensures strict adherence to UK FCA data rules (satisfying Daniel's governance mandates) and establishes the morning dashboard updates that Gareth requires to monitor active team workloads and catch portal drop-offs within a tight 24-to-48-hour loop.
