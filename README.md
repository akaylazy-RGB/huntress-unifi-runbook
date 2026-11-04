# Huntress SIEM + UniFi Syslog Runbook (Write-up)

> **Note:** This repository documents a runbook I wrote and use at a managed services provider. Client names, IP addresses, and tenant details are not published. This write-up covers the problem, the finding, and how the runbook is structured.

## The problem

Firewall and gateway logs are some of the most useful evidence during an incident, and some of the most often missing. Our UniFi gateways weren't feeding the SIEM, so network activity at client sites was invisible to the people triaging alerts.

## What I did

- Integrated **Huntress SIEM** with a **UniFi UDM Pro Max** using remote syslog over **UDP 514**, first in a test organization before rolling it out.
- Found that **current UniFi OS groups syslog by application** (UniFi OS, Protect, Network) instead of the flat legacy categories that most existing guides describe. Configs written against the old layout silently miss streams.
- Wrote a **reusable, per-organization runbook** that covers both the modern application-grouped configuration and the legacy supplemental streams, so a technician can onboard a new site without rediscovering this.

## How the runbook is structured

1. **Pre-checks:** confirm gateway model and UniFi OS version, and confirm the Huntress SIEM collector for that organization.
2. **Modern configuration:** enable remote syslog per application group and point it at the collector.
3. **Legacy supplemental streams:** additional settings for older firmware or categories the modern layout doesn't cover.
4. **Validation:** confirm events are arriving in the SIEM for each enabled stream before closing the change.
5. **Rollback:** how to disable forwarding cleanly if something misbehaves.

## Why it matters

Getting network telemetry into the SIEM closes a common blind spot in MSP incident response: an endpoint alert can now be correlated with what the firewall saw.

## Skills demonstrated

SIEM integration · Syslog · UniFi OS / UDM Pro · Huntress · Network security monitoring · Runbook and SOP writing
