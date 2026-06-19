# Wireframes
## Landing Page


## Identity verification 1- login details
Related user story ID:
- [IDV-02] (3x Lockout Enforcement)
- [IDV-03] (Real-Time Email Alert)
Key data shown:
- Personal id input box 
- Security number input box
Validation or business rule:
- System matches inputs against dencrypted legacy master data from the morning sync file ([VIS-03]).
- If input is correct, user moves on to MFA where they will be sent a text
- Lockout Rule: If the token input fails 3 consecutive times, freeze the screen for 24 hours ([IDV-02]) and immediately route a notification email containing the metadata to Gareth's team mailbox ([IDV-03]) to prevent a 24-hour batch delay.
Next steps in the journey:
- Upon successful validation, route the user directly to the second identity verification page
- If validation is not successful, user will be routed to an agent.



## Identity verification 2- MFA
Related user story ID:
- [IDV-01] (MFA Gate Setup)
- [IDV-02] (3x Lockout Enforcement)
- [IDV-03] (Real-Time Email Alert)
Key data shown:
- Phone number box with last three digits of number
- 6 digit code input box
Validation or business rule:
- User sent a text as part of MFA
- Lockout Rule: If the token input fails 3 consecutive times, freeze the screen for 24 hours ([IDV-02]) and immediately route a notification email containing the metadata to Gareth's team mailbox ([IDV-03]) to prevent a 24-hour batch delay.
Next steps in the journey:
- Upon successful validation, route the user directly to the Account Summary Dashboard.
- If validation is not successful, user will be routed to an agent.
## Account Summary
Related user story ID:
- [VIS-01] (Balance Dashboard Display).
- [VIS-02] (Recent Actions and Payment History)
Key data shown:
- Customer Name
- Account Status
- Total Outstanding Balance (rigidly split into Principal Debt vs. Frozen Fees to satisfy Daniel's audit trail guidelines
Validation or business rule:
- Data is read-only; no manual inputs are allowed on this screen. A background session timer initializes here: if the screen is abandoned for more than 48 hours without an action, trigger the "Silent Abandonment" tag ([ROUT-04]).
Next steps in the journey:
- User clicks the “pay” Call-to-Action button to proceed to the "Choose Next Action" selector screen.
- User might also select the payment history option where they can view the last 12 month’s transactions 

## Choose next action
Related user story ID:
- [P2P-01] (Full Overdue Balance Clearance)
- [P2P-02] (3-Month Installment Plan)
Key data shown:
- Pay in full option
- Pay in installments option
Validation or business rule:
- Payment choice: Customer can choose whether to pay off their debt in one go or in monthly installments over a three month period
Next steps in the journey:
- If user clicks “Pay in full ” they will proceed to schedule the payment for some time in the next 14 days
- If user clicks “Pay in installments ” they will proceed to “promise-to-pay” screen

## Promise-to-pay
Related user story ID:
- [P2P-02] (3-Month Installment Setup) 
- [ROUT-01] (Affordability Validation Gate)
Key data shown:
- Calculated monthly payment amount (Total Balance/ 3), calendar due dates for installments 1, 2, and 3
- A mandatory income layer confirmation tick-box.
Validation or business rule:
- Affordability Rule: The customer must check the confirmation box stating their net income covers the installment amount.
- Exception Branch: If the customer unchecks the box or selects "I cannot afford these monthly payments" the confirmation path is instantly blocked. The system dynamically re-routes them to the Routed-to-Agent/Unsupported Case Page ([ROUT-02]), packaging a HARDSHIP_ABSOLUTE reason code string flag.
Next steps in the journey:
- If approved, move to Confirmation
- If failed/rejected, route immediately to the manual escalation holding screen

## Confirmation or next steps
Related user story ID:
- [P2P-02] (3-Month Installment Setup) 
- [P2P-03] (Repayment Plan Terms and Electronic Consent Confirmation)
Key data shown:
- Confirmation of a successful payment
- Summary of payment plan that can be printed or downloaded
Next steps in the journey:
- None

## Routed-to-agent- ineligible payment-plan case
Related user story ID:
- [ROUT-01] (Affordability Validation Gate)
- [ROUT-03] (Failed Affordability Exception Screen)
Key data shown:
- Escalation message
- Reference number
- Customer support contact number
- Reason for case escalation
Validation or business rule:
- Informs the customer as to why their case needs to be escalation and provides them with some extra information to help them with their next steps
Next steps in the journey:
- Customer gets in contact with agent
- Agent gets in contact with customer

## Failed Verification

Related user story ID:
- [IDV-02] (3x Lockout Enforcement)
Key data shown:
- Personal id input box 
- Security number input box
Validation or business rule:
- Error message: an explanation as to why the account has been locked- for security reasons following three incorrect sign in attempts
Next steps in the journey:
- Customer will be redirected to the “routed-to-agent” screen, with agent contact information
- Agents will be notified of the locked account 