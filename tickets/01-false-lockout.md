# Ticket #1241: "Account is locked" Errors, but No Accounts Locked

**Category:** Identity / Authentication
**Difficulty:** Hard
**Priority:** Critical (30-minute SLA)
**Environment:** Windows domain (northwind.local), domain controller DC01

## Issue

IT reported that half the office was seeing "account is locked" and couldn't log in since the morning. The service desk had already unlocked and re-enabled several accounts, and it didn't help. One user, Anna, also appeared to have lost access to her Sales group.

## Troubleshooting Steps

1. Opened Event Viewer on DC01 and checked the Application log for anything irregular.
2. Found a **Time-Service error**: the clock was off by 6 min 42 s from the domain controller, past Kerberos's 5-minute tolerance.
3. Found a **Kerberos error** (`KRB_AP_ERR_SKEW`): the clock difference was too large, so ticket requests were rejected. This was being logged in a way that looked like failed logins.
4. Found a **warning** that user `anna` had been removed from the `Sales-GG` group.
5. Opened Directory, located Anna's account, and added her back to `Sales-GG` in her account properties.
6. Opened Services and scanned for anything not showing "Running." Found that **Time Sync** and **Background Intelligent Transfer** were both stopped.

## Root Cause

- The **Time Sync service had stopped**, so clocks drifted beyond Kerberos's 5-minute tolerance. Authentication failed for many users, and it looked like account lockouts. No accounts were actually locked, which is why unlocking them didn't help.
- Separately, **Anna had been removed from Sales-GG**. The logs show the removal but not what caused it.

## Resolution

- Restarted Time Sync and Background Intelligent Transfer.
- Re-added Anna to Sales-GG.

## Verification

Confirmed Time Sync was running and Anna was a member of Sales-GG. The ticket verified as resolved.

## Prevention (recommendations)

- Monitor the Time Sync service and alert when it stops.
- When many users report login failures at once, check time sync and Kerberos errors before unlocking accounts one by one.
- Alert on changes to group membership so unexpected removals get noticed quickly.

## Takeaway

Widespread "locked" accounts usually point to an infrastructure problem, not individual accounts. The logs showed the real cause, and the symptoms alone didn't.
