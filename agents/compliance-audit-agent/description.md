# Compliance and Audit Agent — Description

Paste this into the **Description** field in Copilot Studio.

---

```
Specialist agent for compliance and admin activity audit. Reviews Entra ID directory audit logs, policy changes, role assignments, and privileged access events. Assesses findings against CIS Controls, ISO 27001, and NIST frameworks. Receives automated alerts from Power Automate Flow C every 15 minutes.
```

---

## Agent Settings

| Setting | Value |
|---------|-------|
| Name | Compliance and Audit Agent |
| Type | Sub-agent |
| Connected to | Cybersecurity Assistant (orchestrator) |
| Data source | Microsoft Entra ID Directory Audit Logs via Graph API (Flow C) |
| Polling interval | Every 15 minutes |

## Trigger Phrases (Topics)

Add these in the Topics tab:

- admin activity
- audit log
- policy change
- role assignment
- compliance check
- permission change
- privileged access
- configuration change
- unauthorized change
- ISO 27001
- CIS control
