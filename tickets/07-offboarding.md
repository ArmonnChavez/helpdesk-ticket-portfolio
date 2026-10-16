# Ticket #7: Offboarding, Sara Ek Leaves Today (Close All Access)

**Category:** User Lifecycle / Identity and Access
**Difficulty:** Hard
**Priority:** Medium (30-minute SLA)
**Environment:** Windows domain (northwind.local) and Nexus 365 admin center

## Issue

HR reported that Sara Ek (Sales) had her last day today at 5 PM. The ticket asked for full offboarding:

- Disable the AD account
- Remove her from `Sales-GG`
- Move the account to the Disabled OU
- Revoke her M365 license
- Forward her mail to `anna@northwind.com`

## Troubleshooting Steps

1. Opened Event Viewer and reviewed the offboarding event confirming her last working day and the 17:00 deadline for the checklist.
2. In Active Directory Users and Computers, located Sara's account in the Sales OU.
3. **Disabled the account** so she could no longer sign in.
4. Opened her properties, went to **Member Of**, and **removed her from `Sales-GG`**. She stayed in `All employees`.
5. **Moved the account to the Disabled OU** and confirmed it showed as "Account disabled."
6. Opened the **Nexus 365 admin center → Licenses** and set Sara's license to **None**.
7. In **Active users**, set **mail forwarding** on Sara's mailbox. I first entered the wrong address (Sara's own) instead of `anna@northwind.com`, as the ticket specified. Before marking the ticket resolved, I went back and re-read it, caught the mistake, and corrected it.

## Root Cause

Not a fault, but a planned process. When an employee leaves, every way into company systems has to be closed: the directory account, group-based permissions, cloud licenses, and mail.

## Resolution

All five requested actions were completed: account disabled, removed from `Sales-GG`, moved to the Disabled OU, M365 license revoked, and mail forwarding set to Anna's address.

## Verification

Before closing, I re-read the ticket and checked each item against my work. That's how I caught the forwarding mistake. After the fix, I confirmed `sara` was disabled, in the Disabled OU, not in `Sales-GG`, had no license, and had forwarding set to Anna's address, and that the other users' licenses were unchanged.

## Prevention (recommendations)

- Use a written offboarding checklist so no step gets missed.
- Disable the account first so access stops immediately, then clean up groups, licenses, and mail.
- Re-read the ticket line by line before closing, especially for details like forwarding addresses.
- Also check for things the ticket didn't list: other group memberships, shared mailboxes, VPN access, company devices, and active sessions.
- Decide what happens to her mailbox and files (archive, retention, or handoff) before the account is removed.

## Takeaway

Offboarding is an access-control task. Each step closes a different path into the company, and missing one leaves an open door. It's also the "leaver" part of the joiner/mover/leaver lifecycle that identity and access teams manage.

**Lesson learned:** Forwarding mail to the wrong address (or to the departing user's own mailbox) would have left her messages going nowhere. I caught it by re-reading the ticket and checking my work before closing, and I now do that on every ticket.
