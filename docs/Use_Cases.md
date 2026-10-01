# ClaimFlow Use Cases

## UC-01 Create Claim
Actor: Employee

Goal:
Create a new expense claim.

Successful result:
A new claim exists in DRAFT state.


## UC-02 Submit Claim
Actor: Employee

Preconditions:
- Claim belongs to employee.
- Claim is editable.

Main flow:
1. Employee requests submission.
2. System validates claim policies.
3. BLOCK violations stop submission.
4. WARN violations require justification.
5. System calculates claim total.
6. System determines required approval path.
7. System creates approval steps.
8. Claim becomes SUBMITTED.

Successful result:
Claim enters approval workflow.


## UC-03 Act on Approval
Actor: Approver

Preconditions:
- Approval step belongs to current approver.
- Step is currently actionable.
- Approver is not claimant.

Actions:
- Approve
- Reject
- Send back


## UC-04 Final Approval
Actor: Finance Officer

Preconditions:
- Required hierarchy approvals are complete.
- Claim is eligible for final approval.

Main flow:
1. Finance requests approval.
2. System checks remaining department budget.
3. Budget is consumed only if sufficient.
4. Claim becomes APPROVED.

Failure:
Insufficient budget means approval does not occur.


## UC-05 Resubmit Sent-Back Claim
Actor: Employee

Precondition:
Claim is SENT_BACK.

Main flow:
1. Employee corrects claim.
2. Employee resubmits.
3. Policy rules run again.
4. New approval process begins.