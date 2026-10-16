# Ticket #6: Computer Asking for BitLocker Recovery Key

**Category:** Endpoint / Encryption
**Difficulty:** Medium
**Priority:** High (20-minute SLA)
**Environment:** Windows 11 managed device (PC-ANNA), domain northwind.local

## Issue

Anna Lind (Sales) reported that after an update, her computer booted to a blue screen asking for a BitLocker recovery key, and she couldn't get in.

## Troubleshooting Steps

1. Opened Event Viewer on DC01 and checked the **Security** log.
2. Found a **BitLocker warning (Event ID 24650)**: "PC-ANNA: recovery key required (firmware change)."
3. Opened **Nexus Endpoint Manager** and selected PC-ANNA. The device was flagged **At Risk (1)**. BitLocker encryption showed **Recovery mode (locked)**, while Windows Update showed up to date and storage was normal.
4. Used the **Unlock (recovery)** action to recover the key and unlock the drive.

## Root Cause

A **firmware change** on the machine caused BitLocker to detect a change to the boot environment and lock the drive in recovery mode. This is a built-in protection, so BitLocker asks for the recovery key to make sure the person booting the device is authorized.

## Resolution

Unlocked the drive using the recovery option in Nexus Endpoint Manager, which recovered the key and returned BitLocker to an unlocked state.

## Verification

The scenario's check is BitLocker = **Unlocked**. After the recovery, the "At Risk" flag should clear, and I'd ask Anna to confirm that the computer boots normally and she can sign in.

## Prevention (recommendations)

- Make sure recovery keys are backed up to Active Directory (or the organization's key store) so the help desk can retrieve them quickly.
- Suspend BitLocker before planned firmware or BIOS updates so the drive doesn't lock afterward.
- Verify the user's identity before giving out a recovery key, since the key unlocks the whole drive.
- Never paste recovery keys into tickets, chats, or documentation.

## Takeaway

A BitLocker recovery prompt after an update isn't a failure. It's the encryption doing its job after a hardware or firmware change. The event log gave the reason, and the endpoint manager gave the fix.
