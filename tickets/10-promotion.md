# Ticket #10: Promotion, Erik Karlsson Moving from Sales to IT

**Category:** User Lifecycle / Identity and Access
**Difficulty:** Hard
**Priority:** Low (25-minute SLA)
**Environment:** Windows domain (northwind.local) and Nexus 365 admin center

## Issue

Björn (IT) reported that Erik Karlsson had been promoted from Sales to IT and asked for his access to be updated:

- Remove him from `Sales-GG`
- Add him to `IT-Admins`
- Move his account to the IT OU
- Upgrade his M365 license from E1 to E3

## Troubleshooting Steps

1. Opened Active Directory Users and Computers and located Erik's account in the Sales OU.
2. Opened his properties and went to **Member Of**. He was in `Sales-GG` and `All employees`.
3. **Removed him from `Sales-GG`**, then **added him to `IT-Admins`**. He stayed in `All employees`.
4. Used **Move** to **move his account from the Sales OU to the IT OU**.
5. Opened the **Nexus 365 admin center** and **upgraded his license to E3** so it matched his coworkers in IT.

## Root Cause

Not a fault, but a role change. When someone moves departments, their access has to change with them, or they keep permissions from their old job and miss the ones they need for the new one.

## Resolution

All four requested changes were completed: removed from `Sales-GG`, added to `IT-Admins`, moved to the IT OU, and license upgraded to E3.

## Verification

The scenario's check is that `erik` is in the IT OU, in `IT-Admins`, not in `Sales-GG`, and has an E3 license. I confirmed each item in the Directory and Nexus 365 consoles.

## Prevention (recommendations)

- Treat a transfer as **remove old access, then add new access**, so nothing is left behind (privilege creep).
- Confirm the request is approved by HR or the manager before adding someone to a privileged group like `IT-Admins`.
- Give new access based on the role, and match it to coworkers in the same job.
- Review group memberships periodically to catch leftover access from past role changes.

## Takeaway

Role changes are the "mover" part of the joiner/mover/leaver lifecycle. The risky mistake is adding the new access and forgetting to remove the old. Together with offboarding, this ticket shows access following a person's current job.
