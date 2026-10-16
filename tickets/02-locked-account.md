# Ticket #2: Anna Cannot Log In (Locked Account)

**Category:** Identity / Authentication
**Difficulty:** Easy
**Priority:** High (15-minute SLA)
**Environment:** Windows domain (northwind.local), domain controller DC01

## Issue

Anna Lind (Sales) reported that she couldn't log in and was seeing an "account is locked" message. She said she had mistyped her password a few times the day before.

## Troubleshooting Steps

1. Opened Event Viewer on DC01 and checked the **Security** log.
2. Found an **Event ID 4740 (Account Lockout)** entry: account `anna` was locked after 6 failed attempts, coming from PC-ANNA.
3. Opened Active Directory Users and Computers and located Anna's account in the Sales OU. It showed a lock icon and the status "Account is locked out."

## Root Cause

Repeated incorrect password attempts triggered the account lockout policy. This matched what Anna reported, and the logs showed the failed attempts came from her own machine.

## Resolution

Unlocked Anna's account from Active Directory so she could try logging in again.

## Verification

Confirmed the account no longer showed as locked out, and the ticket verified as resolved.

## Prevention (recommendations)

- Remind users that a locked account clears after the lockout window, or can be unlocked by the help desk.
- If the same account locks repeatedly, look for a stale saved password on a phone, mapped drive, or service, instead of just unlocking it each time.
- Check that failed attempts come from the user's own device. Attempts from unknown machines could point to something more serious.

## Takeaway

Lockout tickets are the most common request on a help desk. Checking the Security log first confirms the cause and where the attempts came from before touching the account.
