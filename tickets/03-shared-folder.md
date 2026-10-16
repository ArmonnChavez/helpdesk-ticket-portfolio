# Ticket #3: User Cannot Open Shared Folder

**Category:** Permissions / File Shares
**Difficulty:** Easy
**Priority:** Low (20-minute SLA)
**Environment:** Windows domain (northwind.local), file server share `\\fileserver\Sales`

## Issue

David Stål (Sales) couldn't open `\\fileserver\Sales`. He was new to the `Sales-GG` group, but the share wasn't allowing that group.

## Troubleshooting Steps

1. Opened Event Viewer on DC01 and checked the **Security** log.
2. Found an **access denied** entry: user `david` attempted to reach `\\fileserver\Sales`, and the log pointed to incorrect share permissions.
3. Opened Shared Folders (Shares) and selected the **Sales** share. Its properties showed `Sales-GG` with **Access Denied**.
4. Confirmed that David's account was already in `Sales-GG`, so the problem was on the share, not on his account.

## Root Cause

The `Sales` share had not been granted access for the `Sales-GG` group. David's group membership was correct, but the share didn't allow that group, so access was denied.

## Resolution

Granted `Sales-GG` access to the Sales share through the Shares panel.

## Verification

Checked the Shares panel for the Sales share and confirmed access now showed as allowed for the group. The ticket verified as resolved.

## Prevention (recommendations)

- Assign share permissions to **groups**, not individual users, so new team members get access automatically when they're added to the group.
- Include share access in the new-hire and team-transfer checklist.
- When a user can't open a share, check group membership and share permissions before looking at the user's computer.

## Takeaway

When access is denied and the user's group membership looks right, the problem is often on the resource, not on the user. The event log showed where to look, and the Shares panel showed the cause.
