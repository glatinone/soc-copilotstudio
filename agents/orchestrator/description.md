# Cybersecurity Assistant — Description

Paste this into the **Description** field in Copilot Studio.

---

```
AI-powered SOC orchestrator. Routes security alerts and analyst queries to specialist agents for identity threats, phishing, compliance audits, and executive reporting. Receives live data from Microsoft Entra ID and Defender for Office 365 via Power Automate flows.
```

---

## Agent Settings

| Setting | Value |
|---------|-------|
| Name | Cybersecurity Assistant |
| Type | Orchestrator (main agent) |
| Sub-agents | Identity Threat Agent, Phishing Investigation Agent, Compliance and Audit Agent, SOC Summary Agent |
| Connected flows | Flow A (risky sign-ins), Flow B (phishing alerts), Flow C (audit logs) |

## Routing Table

| Input type | Routes to |
|------------|-----------|
| Sign-in logs, MFA events, risky users | Identity Threat Agent |
| Suspicious emails, phishing alerts | Phishing Investigation Agent |
| Admin activity, policy changes, audit logs | Compliance and Audit Agent |
| Summary requests, executive reports | SOC Summary Agent |
