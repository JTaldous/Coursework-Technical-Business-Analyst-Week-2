# User stories

## Group 1: Access and Verification
### Story 1: [IDV-01] Multi-Factor Authentication (MFA) Login Gate
User Story: As a portal customer, I want to complete a secure Multi-Factor Authentication login so that my sensitive financial and banking data remains completely protected.
Business Value: Ensures absolute alignment with UK FCA data security regulations and prevents regulatory non-compliance fines (Daniel's priority).
Acceptance Criteria:
Given a user navigates to the Smart-Recovery portal login page,
When they enter their correct username, password, and the 6-digit SMS verification code sent to their registered mobile phone,
Then they are granted secure access to their account summary dashboard.
Dependencies: None.
Priority: High.

### Story 2: [IDV-02] 3x Failed Login Account Lockout Execution
User Story: As the Portal Security Engine, I want to temporarily lock an account after three consecutive failed MFA validation attempts so that unauthorised malicious access is blocked.
Business Value: Mitigates fraud risks and automatically flags potential security threats for Gareth's team.
Acceptance Criteria:
Given a user has failed their MFA input two times consecutively,
When they input an incorrect MFA verification code for the third time,
Then the portal must immediately lock online access for 24 hours and display a dedicated support hotline number on screen.
Dependencies: [IDV-01]
Priority: High.

### Story 3: [IDV-03] Real-Time Frontend Lockout Email Alert
User Story: As the Portal Security Engine, I want to automatically dispatch a real-time email alert to the collections team mailbox when a customer hits a 3x MFA lockout, so that agents can initiate immediate phone outreach.
Business Value: Eliminates the 24-hour batch delay, allowing Gareth's crew to rescue frustrated customers within minutes without risking legacy database instability (OPP-04).
Acceptance Criteria:
Given a customer account has been locked due to a 3x MFA failure,
When the 24-hour online block is applied by the portal,
Then the portal engine must bypass the database sync and immediately route an automated notification email containing the Account_ID, Timestamp, and Lockout_Reason directly to appropriate email, e.g. collections_alerts@bank.com.
Dependencies: [IDV-02]
Priority: High

## Group 2: Account Visibility
### Story 4: [VIS-01] Primary Balance and Overdue Arrears Summary Dashboard
User Story: As a portal customer, I want to view my current outstanding balance and immediate overdue amount clearly upon logging in so that I know exactly how much I need to pay without calling an agent.
Business Value: Directly executes OPP-01 (Automate Status Checks), eliminating basic data-lookup phone inquiries and reclaiming agent capacity.
Acceptance Criteria:
Given a customer has successfully authenticated via the MFA gate,
When the main account dashboard loads,
Then the system must clearly display their Total Outstanding Balance, Next Payment Due Date, and Overdue Amount.
Dependencies: [IDV-01]
Priority: High.

### Story 5: [VIS-02] Interactive Recent Actions and Payment History Ledger
User Story: As a portal customer, I want to view an interactive ledger of my recent payments and broken promises so that I can independently verify my historical account activity.
Business Value: Reduces transactional verification calls to the contact center, freeing up manual team hours.
Acceptance Criteria:
Given a customer is viewing the main account summary dashboard,
When they click on the "View Payment History" tab,
Then the UI must render a table displaying the last 12 months of transactions, categorised by date, amount, and payment status (Success / Pending / Missed).
Dependencies: [VIS-01]
Priority: Medium.

### Story 6: [VIS-03] Legacy Core Account Data Sync Interface
User Story: As the Portal Data Layer, I want to display data read from the most recent legacy database file ingestion so that the user sees their true, legally binding bank balance.
Business Value: Ensures financial accuracy and data alignment across old and new systems without relying on high-risk real-time APIs.
Acceptance Criteria:
Given the portal needs to populate a customer dashboard,
When the user logs in,
Then the dashboard must render financial data fields pulled directly from the local portal database updated by the latest morning batch processing.
Dependencies: [VIS-01], [REP-02]
Priority: High.

## Group 3: Promise-to-Pay / Payment-Plan Journey
### Story 7: [P2P-01] Full Overdue Balance Clearance Commitment (Single Date Setup)
User Story: As a portal customer, I want to select a single future date within 14 days to clear my total overdue balance in full so that I can log a basic promise-to-pay without talking to an agent.
Business Value: Automates basic arrangements, directly capturing the low-complexity revenue recovery opportunities mapped out in discovery.
Acceptance Criteria:
Given a customer initiates a Promise-to-Pay arrangement,
When they select "Pay in Full,"
Then the system must present a calendar picker capped at a maximum of 14 calendar days from the current date and capture their electronic signature.
Dependencies: [VIS-01]
Priority: High.

### Story 8: [P2P-02] Fixed 3-Month Installment Plan Selection Layout
User Story: As a portal customer, I want to select a structured, pre-approved 3-month payment plan so that I can clear my balance in predictable increments.
Business Value: Drives autonomous debt recovery without manual agent intervention, maximising collection yields.
Acceptance Criteria:
Given a customer chooses to set up a payment arrangement on the portal,
When they select the "3-Month Equal Installment" option,
Then the portal must calculate and display three equal monthly payment amounts, processing the first installment immediately upon confirmation.
Dependencies: [VIS-01]
Priority: High.

### Story 9: [P2P-03] Repayment Plan Terms and Electronic Consent Confirmation
User Story: As a portal customer, I want to review and digitally accept a structured Summary of Terms before confirming my repayment plan, so that I understand my payment legal obligations.
Business Value: Satisfies corporate compliance regulations regarding debt acknowledgement and collections auditing paths.
Acceptance Criteria:
Given a customer has chosen a repayment option ([P2P-01] or [P2P-02]),
When they click "Proceed to Confirmation,"
Then the system must display a simplified legal text block and block submission until the user selects the "I agree to these payment terms" checkbox.
Dependencies: [P2P-01], [P2P-02]
Priority: High.

## Group 4: Routing and Exception Handling
### Story 10: [ROUT-01] Real-Time Affordability Verification Guardrail
User Story: As the Portal Rules Engine, I want to cross-reference a customer's proposed payment plan total against their recorded monthly income profile, so that unsustainable arrangements are blocked automatically.
Business Value: Protects vulnerable customers from entering unviable financial plans, driving down downstream arrangement collapse rates.
Acceptance Criteria:
Given a customer selects a fixed payment plan,
When the calculated monthly payment amount exceeds 30% of their documented monthly income,
Then the portal must block the plan selection and display a manual escalation screen.
Dependencies: [P2P-02]
Priority: High.

### Story 11: [ROUT-02] Context-Rich Vulnerability Escalation and Metadata Routing
User Story: As a collections agent, I want the automated escalation file to explicitly state the reason for portal deflection (e.g., hardship, vulnerability, legal), so that I can tailor my manual phone conversation appropriately.
Business Value: Minimises agent prep time, protects vulnerable customers from repetitive questioning, and maintains strict adherence to UK FCA data guidelines.
Acceptance Criteria:
Given a customer triggers a manual escalation path via an affordability failure ([ROUT-01]) or a hardship checkbox selection,
When the system appends the account record to the morning batch queue file,
Then it must rigidly append a standardised string flag variable (Escalation_Reason_Code: "HARDSHIP_ABSOLUTE" or "AFFORDABILITY_FAILED") ensuring this metadata maps directly to the header field on the agent's desk.
Dependencies: [P2P-01], [P2P-02]
Priority: High.

### Story 12: [ROUT-03] Failed Affordability Portal Exception Screen
User Story: As a portal customer who has failed the automated affordability check, I want to see a clear, un-intimidating message detailing exactly how to contact a specialist, so that I don't abandon the debt recovery process.
Business Value: Prevents customer drop-off at friction points, safely moving high-risk exceptions into manual agent queues.
Acceptance Criteria:
Given a user has triggered the affordability check block ([ROUT-01]),
When the screen redirects,
Then the UI must explicitly state that a custom payment structure is required, hide standard automated buttons, and provide a direct text link to initiate an agent phone consultation.
Dependencies: [ROUT-01]
Priority: Medium.

## Group 5: Reporting and Audit Trail
### Story 13: [REP-01] Full-Spectrum Portal Activity Audit Log Logger
User Story: As a compliance officer, I want the portal backend to write every user interaction, click, and input into an un-editable security log so that we can easily pass corporate compliance audits.
Business Value: Delivers the iron-clad compliance history and tracking trail that Daniel requires to eliminate regulatory risk.
Acceptance Criteria:
Given a customer interacts with any element of the authenticated portal,
When an action is confirmed (e.g., failed MFA, plan selection, logout),
Then the system must append an immutable timestamped line to the portal log containing the Account ID, Action Type, IP Address, and System Output.
Dependencies: None.
Priority: High.

### Story 14: [REP-02] Midnight Secure SFTP Daily Batch Export Export Script
User Story: As the Portal Backend Infrastructure, I want to execute an automated script at midnight to compress and transmit the day’s customer data logs to the legacy database system via secure SFTP.
Business Value: Eliminates the need for risky real-time API overwriting (OPP-04), using a safe batch-transfer approach that completely protects the 20-year-old database from crashing.
Acceptance Criteria:
Given the system time reaches 00:00 AM daily,
When the automated batch window opens,
Then the system must bundle all un-synced portal records into an encrypted CSV file, establish a secure SFTP handshake with the core bank database server, drop the file into the designated import folder, and log a success flag.
Dependencies: [REP-01]
Priority: High.

### Story 15: [REP-03] Automated Executive Workload and Portal Yield Dashboard
User Story: As a collections manager, I want to view an automated internal reporting dashboard summarising total portal plans completed, broken promises, and automated agent escalations, so that I have complete visibility over operational metrics.
Business Value: Direct execution of OPP-03 and OPP-05, satisfying Gareth's tracking requirements by allowing him to see exactly how much phone capacity his team is reclaiming.
Acceptance Criteria:
Given a collections manager logs into the internal operational dashboard,
When the main view loads,
Then the interface must render charts visualising total volume throughput, total financial value recovered, and the total counts of broken promises routed to the morning workspace queues.
Dependencies: [REP-01]
Priority: Medium.

## Group 4: Routing and Exception Handling (New Addition)
### Story 16: [ROUT-04] Time-Based Portal Abandonment Session Monitor
User Story: As the Portal Workflow Engine, I want to monitor active portal sessions and track accounts that have initiated but abandoned a payment plan setup, so that they can be flagged for follow-up rather than left in limbo.
Business Value: Plugs an operational blind spot highlighted by Gareth, ensuring customers who disengage halfway through the digital journey are proactively caught and supported.
Acceptance Criteria:
Given a customer successfully authenticates and enters the payment-plan setup journey ([P2P-01] or [P2P-02]),
When the user leaves the session inactive or closes the portal without confirming a plan or triggering a hardship exclusion for more than 48 hours,
Then the system must mark the record status as "Incomplete Digital Arrangement - Abandoned" and generate a backend alert tag.
Dependencies: [IDV-01], [P2P-01], [P2P-02]
Priority: High

## Group 5: Reporting and Audit Trail (New Addition)
### Story 17: [REP-04] Abandoned Session Sync to Next-Day Agent Queue
User Story: As the Portal Data Layer, I want to include abandoned portal session tags in the midnight batch file export, so that these cases are automatically delivered to Gareth's manual team queue the following morning.
Business Value: Eliminates the risk of cases sitting quietly in limbo by giving agents visibility over digital drop-offs within 24–48 hours.
Acceptance Criteria:
Given an account has been flagged with an "Abandoned" status tag ([ROUT-04]),
When the midnight secure SFTP daily batch export script ([REP-02]) executes,
Then this account must be bundled into the encrypted CSV upload file with an "Escalate to Agent: Digital Drop-off" reason flag to populate an agent's morning workspace.
Dependencies: [ROUT-04], [REP-02]
Priority: High
