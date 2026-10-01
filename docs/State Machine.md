# ClaimFlow Claim State Machine

## States

- DRAFT
- SUBMITTED
- IN_APPROVAL
- SENT_BACK
- APPROVED
- REJECTED
- WITHDRAWN
- REIMBURSED

## Meaning

DRAFT:
Claim is being prepared and can be edited.

SUBMITTED:
Claim has successfully entered the approval workflow,
but no approver has acted yet.

IN_APPROVAL:
At least one approval action has occurred and the claim
is still being processed.

SENT_BACK:
Approver requested correction. Claim becomes editable again.

APPROVED:
All required approvals including Finance are complete.

REJECTED:
Claim has been denied.

WITHDRAWN:
Employee cancelled the claim.

REIMBURSED:
Approved amount has been paid.

## MVP Allowed Transitions

DRAFT -> SUBMITTED

SUBMITTED -> IN_APPROVAL
SUBMITTED -> SENT_BACK
SUBMITTED -> REJECTED

IN_APPROVAL -> SENT_BACK
IN_APPROVAL -> REJECTED
IN_APPROVAL -> APPROVED

SENT_BACK -> SUBMITTED

APPROVED -> REIMBURSED

## Terminal States

REJECTED
WITHDRAWN
REIMBURSED