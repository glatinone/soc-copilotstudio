# Identity Threat Agent — Description

Paste this into the **Description** field in Copilot Studio.

---

```
Specialist agent for identity and authentication threat investigation. Analyzes risky sign-ins, impossible travel, MFA anomalies, and credential attacks from Microsoft Entra ID logs. Receives automated alerts from Power Automate Flow A every 5 minutes.
```

---

## Agent Settings

| Setting | Value |
|---------|-------|
| Name | Identity Threat Agent |
| Type | Sub-agent |
| Connected to | Cybersecurity Assistant (orchestrator) |
| Data source | Microsoft Entra ID via Graph API (Flow A) |
| Polling interval | Every 5 minutes |

## Trigger Phrases (Topics)

Add these in the Topics tab:

- suspicious login
- risky sign-in
- impossible travel
- MFA bypass
- credential attack
- brute force
- account compromise
- unusual login
- sign-in alert
- identity threat
