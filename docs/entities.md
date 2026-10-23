# ClaimFlow Conceptual Model

## Employee

Represents a person working in the company.

Important information:
- employee code
- name
- email
- department
- grade
- manager
- active status
- finance status

Important insight:
An employee's manager is also an Employee.
This creates a self-referencing organisational hierarchy.


## Department

Represents an organisational unit.

Important information:
- name
- department head


## Grade

Represents an employee level.

Important information:
- code
- rank



##ApprovalBand
We said:
₹4,000 claim
→ maybe one approval level

₹19,050
→ two hierarchy levels

₹80,000
→ more approval / Finance

ApprovalBand
├── minimum amount
├── maximum amount
├── hierarchy levels required
└── finance required
## Claim

Represents one reimbursement request submitted by one employee.

Important information:
- claim number
- claimant
- purpose
- trip dates
- status
- total
- submission time

A Claim contains multiple ExpenseLines.


## ExpenseLine

Represents one individual expense inside a Claim.

Common information:
- expense date
- amount
- vendor
- receipt number
- description
- category
- optional city

Different expense types may require additional information.


## PolicyLimit

Represents a monetary policy limit determined by factors such as:
- category
- employee grade
- city tier
- effective date


## ApprovalStep

Represents one required approval action.
ApprovalStep
├── claim
├── approver
├── step number
├── round number
├── step type
├── status
├── action time
└── comments

Important information:
- approver
- order
- approval round
delete old approval steps
create new steps

Works.
But then we've destroyed history.
We can no longer answer:
Who approved the first submission?

So instead:
Round 1
Arjun APPROVED
Meera SENT_BACK

Round 2
Arjun APPROVED
Meera APPROVED
Finance PENDING
- status
- comments
- acted time


## PolicyViolation

Represents the result of a policy rule being violated.
PolicyViolation
├── claim
├── maybe expense line
├── rule code
├── severity
├── message
└── justification

Important information:
- rule
- severity
- message
- affected line
- justification


## DepartmentBudget

Represents one department's budget for one fiscal year.


## Reimbursement

Represents the actual payout for an approved claim.
Reimbursement
├── claim
├── amount
├── paid date
└── bank reference


## ClaimStatusHistory

Represents a historical claim status change.