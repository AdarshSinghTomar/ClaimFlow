# Design Decisions

Used Eclipse Temurin for  java 17 -- open source that's why
for DataBase used My sql - not postgre yet to learn about that 
Created .gitignore file whith secerts like ignoring logs, database passwords etc because git history  is permanent!!
## DD-002: Separate SUBMITTED and IN_APPROVAL

SUBMITTED means the claim has successfully entered the
approval workflow but no approver has acted yet.

IN_APPROVAL means at least one approval action has occurred
and the workflow is still active.

Reason:
This distinguishes untouched submitted claims from claims
already progressing through the approval chain.
## DD-003: Manager is represented as another Employee

Decision

A manager will not be modelled as a completely separate Manager entity.
Instead, an Employee can reference another Employee as their manager.

Example:

Riya → manager = Arjun
Arjun → manager = Meera
Meera → manager = CEO

Why?

A manager is not fundamentally a different type of person.

Arjun is still an employee of the company. He becomes Riya's manager because of the organisational relationship between them.

So the real business fact is:

Employee reports to Employee

rather than:

Employee belongs to Manager entity

What simpler/incorrect design could we have used?

We could have created separate concepts such as:

Employee
Manager
DepartmentHead
CEO

But this would duplicate common employee information such as name, email, grade and department.

It would also make promotions and reporting changes harder to represent.

What does this decision give us later?

This naturally creates a self-referencing employee hierarchy:

Employee
   ↓ manager
Employee
   ↓ manager
Employee

Later we can use this hierarchy to automatically calculate the approval path:

Riya → Arjun → Meera

Revision line

Manager is a relationship, not a separate person type; therefore an Employee points to another Employee as manager.

## DD-004: Claim and ExpenseLine are separate concepts

Decision

A Claim represents the complete reimbursement request, while each individual expense inside the claim is represented separately as an ExpenseLine.

Example:

Claim CF-0192
├── Hotel ₹15,000
├── Taxi ₹1,200
├── Meal ₹900
└── Meal ₹1,400

Why?

A single claim can contain any number of expenses.

The claim contains information that applies to the whole request:

claim number
employee
purpose
trip dates
status

An ExpenseLine contains information about one specific expense:

expense date
amount
vendor
receipt
category

These are different business concepts.

What simpler design could we have used?

We could have stored fixed fields inside Claim:

hotelAmount
taxiAmount
mealAmount

But this immediately fails when there are:

3 meals
5 taxi rides
2 hotel charges

We would need fields like:

meal1
meal2
meal3
taxi1
taxi2
...

which is not scalable.

What does this decision give us later?

We get a natural:

One Claim
   ↓
Many ExpenseLines

relationship.

It also allows each expense line to be validated independently by policy rules.

Revision line

Claim is the overall reimbursement request; ExpenseLine is one individual expense, because a claim can contain a variable number of expenses.

## DD-005: Common expense data is separated from expense-type-specific data

Decision

All expenses share a common conceptual base, while special expense types keep only the information that belongs to them.

Common information:

date
amount
vendor
receipt
category

Hotel-specific information:

check-in
check-out
number of nights
city

Travel-specific information:

from
to
mode

Why?

Hotel, travel, meal and other expenses are all expenses, so they naturally share some information.

But they are not identical.

A hotel needs check-in/check-out information.

A travel expense needs origin, destination and travel mode.

A meal does not need either of those.

What simpler design could we have used?

We could create one huge ExpenseLine containing every possible field:

amount
vendor
checkIn
checkOut
nights
fromPlace
toPlace
travelMode
...

But then most objects would contain meaningless values.

Example:

MealExpense
checkIn = null
checkOut = null
travelMode = null
fromPlace = null

The model would become confusing and weakly structured.

What does this decision give us later?

This business requirement will naturally lead us toward a Java inheritance hierarchy such as:

ExpenseLine
├── HotelExpense
├── TravelExpense
├── MealExpense
└── OtherExpense

and later we will decide how that hierarchy should be represented in the database.

Revision line

Keep shared expense facts once, but keep hotel/travel-specific facts only where they make business sense.

## DD-006: DepartmentBudget is separate from Department

Decision

Budget information will not be stored directly as a single field inside Department.

Instead, each department will have separate budget records for different fiscal years.

Conceptually:

DepartmentBudget
├── Department
├── Fiscal Year
├── Allocated Amount
└── Consumed Amount

Why?

A department continues to exist for many years, but its budget changes every financial year.

Example:

Sales 2026 → ₹5,00,000
Sales 2027 → ₹6,50,000
Sales 2028 → ₹7,20,000

The budget therefore describes:

Department + Year

not simply Department.

What simpler design could we have used?

We could store:

Department
budget = ₹5,00,000

But next year we would overwrite that value.

Then the system could no longer answer:

What was the Sales budget in 2026?

Historical budget information would be lost.

What does this decision give us later?

We can maintain historical budgets, compare spending across years, and enforce:

one budget per department per fiscal year

It also prepares us for safe final-approval budget checking.

Revision line

Department is permanent organisational data; budget is time-dependent financial data, so budget belongs in a separate yearly record.

## DD-007: Previous approval rounds are preserved after send-back and resubmission

Decision

When a claim is sent back and later resubmitted, the previous approval steps will not be deleted or overwritten.

Instead, each submission cycle will have its own approval round.

Example:

Round 1
Arjun → APPROVED
Meera → SENT_BACK

Round 2
Arjun → APPROVED
Meera → APPROVED
Finance → PENDING

Why?

Approval actions are part of the history of the claim.

Suppose Meera sends Riya's claim back because the hotel invoice number is missing.

Riya corrects the claim and submits again.

If we simply delete the first approval steps, we lose information such as:

Who approved the original submission?
Who sent it back?
Why was it sent back?
When did this happen?

That would weaken the audit trail.

What simpler design could we have used?

The simplest implementation would be:

send back
   ↓
delete old approval steps
   ↓
create fresh approval steps

Technically, the new workflow would work.

But the system would destroy historical evidence.

What does this decision give us later?

Each ApprovalStep can carry a roundNumber.

So we preserve:

Claim
├── Approval Round 1
├── Approval Round 2
└── Approval Round 3

This makes send-back/resubmission auditable and allows us to understand exactly how the claim travelled through the organisation.

Revision line

Resubmission starts a new approval round instead of deleting the old one, because workflow history must survive.