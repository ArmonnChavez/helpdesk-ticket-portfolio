# Ticket #8: David Missing Teams and Outlook

**Category:** Cloud / M365 Licensing
**Difficulty:** Easy
**Priority:** Medium (20-minute SLA)
**Environment:** Windows domain (northwind.local) and Nexus 365 admin center

## Issue

David Stål (Sales) reported that he could log into the cloud, but Teams and Outlook said "no license assigned."

## Troubleshooting Steps

1. Checked David's account properties in Active Directory (account status, group membership). Nothing was irregular, so the problem wasn't with his sign-in.
2. Opened the **Nexus 365 admin center → Active users**. David's mailbox was active, but his **license was set to None**, while his coworkers Anna and Cecilia each had an E3 license.
3. Assigned David an **E3 license** from the License dropdown, matching his coworkers in the same department.

## Root Cause

David's account had **no Microsoft 365 license assigned**. He could sign in, because his identity was fine, but without a license he had no access to apps such as Teams and Outlook.

## Resolution

Assigned David an E3 license in the Nexus 365 admin center, matching the license his Sales coworkers already had.

## Verification

The scenario's check is that David has a license in Cloud Admin. After assigning it, I confirmed the License column no longer showed "None" for his account, and I'd ask David to sign out, sign back in, and confirm that Teams and Outlook open.

## Prevention (recommendations)

- Include license assignment in the new-hire checklist so it isn't forgotten.
- Consider group-based licensing so access is assigned automatically based on department.
- Review unlicensed active accounts regularly to catch gaps before users report them.
- Compare a user's setup against a coworker in the same role when something is missing. It's a quick way to spot what's different.

## Takeaway

When a user can log in but the apps say "no license," the problem is licensing, not authentication. Ruling out the directory account first saved time and pointed me to the right place.
