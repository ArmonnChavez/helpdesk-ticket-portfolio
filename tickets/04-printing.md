# Ticket #1007: Unable to Print

**Category:** Endpoint / Services
**Difficulty:** Easy
**Priority:** Medium (25-minute SLA)
**Environment:** Windows domain (northwind.local), server DC01

## Issue

Cecilia Holm (Finance) reported that no one on the finance team could print. The printers were powered on and looked fine, but nothing came out, and the print queues seemed stuck.

## Troubleshooting Steps

1. Opened Event Viewer on DC01 and checked the **System** log.
2. Found an **Error** from the Service Control Manager (Event ID 7036): the **Print Spooler service terminated**.
3. Opened the Services console and located **Print Spooler**. Its status was **Stopped**, with the startup type set to Automatic.
4. Started the service.

## Root Cause

The **Print Spooler service had stopped**. The spooler holds print jobs in memory until they're sent to the printer, so with it down, jobs stacked up in the queues and nothing printed for anyone. The printers themselves were fine.

## Resolution

Started the Print Spooler service from the Services console, which let the queues process again.

## Verification

The scenario's check is `sc query` showing Print Spooler = RUNNING. After the restart, the finance team should be able to print, and I'd ask Cecilia to confirm with a test page.

## Prevention (recommendations)

- Configure the service's **Recovery** options to restart automatically after a failure.
- Monitor the Print Spooler service and alert when it stops.
- If it keeps stopping, investigate the cause, such as a corrupted print job or a faulty printer driver, instead of restarting it each time.

## Takeaway

When a whole team can't print but the printers look healthy, the problem is usually the service behind them. The System log pointed straight to the failed service.
