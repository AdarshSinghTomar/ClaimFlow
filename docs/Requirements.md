# ClaimFlow Requirements

## Actors

### Employee
- Create an expense claim.
- Add, edit and remove expense lines while the claim is editable.
- View own claims.
- Submit own claim.
- View policy violations.
- Provide justification for WARN violations.
- Correct and resubmit a SENT_BACK claim.

### Approver
- View approval steps currently assigned to them.
- Approve an actionable step.
- Reject an actionable step.
- Send an actionable claim back for correction.
- Must not approve their own claim.
- Must not act out of approval order.

### Finance Officer
- Perform final approval.
- Verify department budget before final approval.
- View department budget status.

## System Responsibilities

- Validate claims against policy.
- Distinguish BLOCK and WARN violations.
- Prevent submission when BLOCK violations exist.
- Require justification for WARN violations.
- Determine approval requirements from claim amount.
- Determine approvers from the employee hierarchy.
- Enforce claim state transitions.
- Enforce approval ordering.
- Prevent self-approval.
- Prevent budget overspend.

## Later Blueprint Features

- Withdraw claim.
- Duplicate receipt detection.
- Audit history.
- Resubmission approval rounds.
- Reimbursement.
- CSV payout export.
- Advanced reports.