# Ticket #1216: Computers Getting No IP Addresses (DHCP Service Stopped)

**Category:** Networking / DHCP
**Difficulty:** Easy
**Priority:** Low (15-minute SLA)
**Environment:** Windows domain (northwind.local), server DC01

## Issue

Björn (IT) reported that the entire office network was complaining of "No network connection." Computers were showing **169.254.x.x (APIPA)** addresses, meaning they couldn't reach the DHCP server. The ticket noted that the DHCP service on DC01 was stopped.

## Troubleshooting Steps

1. Opened Event Viewer on DC01 and checked the **System** log.
2. Found an **Error (Event ID 1020, source DHCP)**: "DHCP Server service stopped. Clients will receive APIPA 169.254.x.x addresses."
3. Opened the **DHCP** tool and checked the scope and configuration. Everything looked fine.
4. Checked **DNS** and **Network** to rule out other layers. Both were normal.
5. Opened the **Services** console and found **DHCP Server** (`dhcpserver`) with a status of **Stopped**, even though its startup type was Automatic.
6. Started the service.

## Root Cause

The **DHCP Server service had stopped on DC01**. With no DHCP server answering, client computers couldn't get an IP address lease and assigned themselves APIPA addresses in the 169.254.x.x range. That range only works for local traffic, which is why everyone lost network access at once. The DHCP configuration, DNS, and network were all healthy.

## Resolution

Started the DHCP Server service from the Services console.

## Verification

The scenario's check is `sc query "DHCP Server"` showing State = RUNNING. After starting the service, I confirmed it showed as Running. I'd also ask an affected user to renew their IP address (`ipconfig /renew`) and confirm they get a normal address instead of a 169.254.x.x one.

## Prevention (recommendations)

- Configure the service's **Recovery** options to restart automatically after a failure.
- Monitor the DHCP Server service and alert when it stops, since a stopped DHCP server affects everyone at once.
- Find out why it stopped, such as a crash, a failed update, or a manual stop, so it doesn't repeat. The log shows that it stopped, but not why.
- Consider a secondary DHCP server for redundancy.

## Takeaway

A 169.254.x.x address means a computer couldn't reach a DHCP server. When many users lose connectivity at the same time, check the shared service first, not each computer. Ruling out the DHCP config, DNS, and network confirmed the problem was only the stopped service.
