# IAM-Access-Request-Tracker
IAM accesss request and approval simulator - request/review/workflow with segregation-of-duties flagging.
# AccessGuard — Access Request & Approval Ledger

A single-page simulation of an IAM access-governance workflow: an employee requests access to a system, the request enters a review queue, a reviewer approves or denies it, and every action is written to an audit log.

**Live demo:** (link shared separately)

## The problem it models

Most breaches and audit findings trace back to access that was never reviewed properly — someone got admin rights for a one-off task and kept them, or an approval happened with no record of who signed off. This project models the core control that fixes that: every access grant has a requestor, a justification, a reviewer, and a timestamp, and nothing gets access without going through that path.

## Key design decisions

- **Privileged access is flagged automatically.** Selecting "Privileged / Admin" as the access level surfaces a segregation-of-duties warning before the request is even submitted, and the request card in the queue carries a visible "Elevated" badge. The idea: high-risk requests should never look identical to routine ones in the reviewer's queue.
- **The audit log is append-only in the UI.** Every submission and every decision writes a new line; nothing is edited or removed. That mirrors how real audit trails work — you don't fix a mistake by deleting the record, you add a correcting entry.
- **State persists per-browser via `localStorage`.** No backend — this is a front-end demo, so state lives in the reviewer's own browser. A real implementation would move this to a database with role-based access to the approval action itself (a reviewer shouldn't be able to approve their own request).
- **Visual language:** deep navy/slate base with a teal accent for "verified" states and amber/red reserved for risk and denial, so severity is legible at a glance without relying on text alone.

## What I'd build next (backend version)

- Real auth, so "who's the reviewer" isn't just a label
- A rule engine blocking self-approval and enforcing dual-control on privileged requests
- Role-based entitlement catalog instead of a free-text system dropdown
- Exportable audit log (CSV/PDF) for compliance reporting

## Stack

Plain HTML/CSS/JS, no build step, no dependencies. `localStorage` for persistence. Built this way so it's easy to read end-to-end in one sitting — the whole app is one file.

## Files

- `accessguard.html` — the full application (open directly in a browser)
