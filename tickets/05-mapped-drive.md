# Ticket #1053: Network Drive M: Is Gone

**Category:** Endpoint / Mapped Drives
**Difficulty:** Medium
**Priority:** Medium (20-minute SLA)
**Environment:** Windows 11 managed device (PC-ANNA), domain northwind.local

## Issue

Anna Lind (Sales) reported that drive M: (Sales) was no longer visible in Explorer and she couldn't reach her files.

## Troubleshooting Steps

1. Opened Event Viewer on DC01 and checked the **Security** log.
2. Found a **failed logon (Event ID 4625)** for account `anna`: wrong password supplied, from source address 10.0.1.82. The other entries were normal successful logons.
3. Opened **Nexus Endpoint Manager** and selected PC-ANNA under Configuration. The device was flagged **At Risk (1)**, and the issue was **Mapped drive M: (Sales): Disconnected**.
4. Clicked **Reconnect** on the mapped drive.

## Root Cause

The mapped drive **M: (Sales) had disconnected** on Anna's computer, so it no longer appeared in Explorer. The logs show a wrong-password failure for her account around the same time, which may be related, but they don't confirm why the drive dropped.

## Resolution

Reconnected the mapped drive from Nexus Endpoint Manager.

## Verification

The scenario's check is that M: shows as **Connected**. After the reconnect, the drive status should clear the "At Risk" flag, and I'd ask Anna to confirm she can open her files.

## Prevention (recommendations)

- Map drives through Group Policy or a logon script so they reconnect automatically at sign-in.
- If a drive keeps disconnecting, check for expired or saved credentials and network connectivity before reconnecting it each time.
- Treat repeated failed logons for the same account as a clue worth checking, not just noise.

## Takeaway

A missing drive is often a simple reconnect, but checking the logs first confirms there isn't a bigger authentication problem behind it.
