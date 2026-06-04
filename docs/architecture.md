# Architecture

## Overview

This system is a multi-agent AI Security Operations Center (SOC) built entirely on Microsoft infrastructure. It requires no third-party SIEM, no additional vendor licensing, and no custom backend — only the tools already available in a standard Microsoft 365 environment.

---

## System Architecture

```
Microsoft Data Sources
        |
        |  (Graph API + Defender API)
        |
Power Automate Flows
  Flow A: Risky Sign-ins     (every 5 min)
  Flow B: Phishing Alerts    (every 5 min)
  Flow C: Audit Logs         (every 15 min)
        |
        |  (HTTP POST via Direct Line)
        |
Cybersecurity Assistant  <-- Orchestrator
        |
        |  (routes to specialist)
        |
   _____|_____________________________
   |          |           |          |
Identity   Phishing   Compliance   SOC
Threat     Invest.    Audit        Summary
Agent      Agent      Agent        Agent
```

---

## Components

### Power Automate Flows

| Flow | Schedule | Source | Target |
|------|----------|--------|--------|
| Flow A | Every 5 min | Entra ID — risky sign-ins | Identity Threat Agent |
| Flow B | Every 5 min | Defender O365 — phishing alerts | Phishing Investigation Agent |
| Flow C | Every 15 min | Entra ID — directory audit logs | Compliance and Audit Agent |

All flows follow the same pattern:
1. Authenticate via OAuth2 client credentials to get a Bearer token
2. Call the Graph API or Security API endpoint
3. Parse the JSON response
4. If data found: format it and POST to the agent endpoint via Direct Line
5. If nothing found: log a normal status

### Agents

| Agent | Role | Connected flows |
|-------|------|----------------|
| Cybersecurity Assistant | Orchestrator, routes all queries | All three flows post here first |
| Identity Threat Agent | Sign-in anomaly and credential threat analysis | Flow A |
| Phishing Investigation Agent | Email threat and phishing investigation | Flow B |
| Compliance and Audit Agent | Admin activity and policy compliance review | Flow C |
| SOC Summary Agent | Executive and analyst report generation | No direct flow — triggered by escalation or manual request |

### Authentication

All flows use OAuth2 client credentials flow:

```
POST https://login.microsoftonline.com/{tenant_id}/oauth2/v2.0/token
  client_id={client_id}
  client_secret={client_secret}
  scope=https://graph.microsoft.com/.default
  grant_type=client_credentials
```

The token is valid for 3600 seconds. Each flow fetches a fresh token at the start of every run.

---

## Graph API Endpoints Used

| Endpoint | Flow | Purpose |
|----------|------|---------|
| `/v1.0/auditLogs/signIns` | Flow A | Risky sign-in events |
| `/v1.0/security/alerts` | Flow B | Defender phishing alerts |
| `/v1.0/auditLogs/directoryAudits` | Flow C | Admin directory changes |

---

## Design Decisions

### Why scheduled polling instead of event triggers?

Power Automate does not support native event-driven triggers for Microsoft Graph security events without additional infrastructure (Event Grid, webhooks, etc.). Scheduled polling at 5-minute intervals is the simplest reliable approach for a no-additional-infrastructure setup.

5 minutes is acceptable for most security operations teams — the gap between detection and analyst response is rarely under 5 minutes in practice.

For real-time response, a future upgrade path exists using Microsoft Graph webhooks or Azure Event Grid, which would reduce latency to under 30 seconds.

### Why Copilot Studio instead of custom backend?

Copilot Studio provides the agent reasoning layer, tool calling, and multi-agent orchestration without requiring custom code. The entire system is buildable through UI configuration alone, which makes it accessible to security teams without dedicated developers.

### Why multi-agent instead of one agent?

Each specialist agent has focused instructions optimized for its threat domain. A single general-purpose agent would require a much larger instruction set and would produce less precise analysis. Specialist agents also make it easier to extend the system — adding a new threat domain means adding a new agent, not rewriting existing instructions.

---

## Extension Points

The architecture is designed to be extended. Some directions the community can take this:

- **Microsoft Sentinel integration**: replace Graph API polling with Sentinel analytics rules and incident API
- **Endpoint threat agent**: add Defender for Endpoint alerts via `/v1.0/security/alerts?$filter=category eq 'Malware'`
- **Teams notification**: add a Teams message step in each flow for real-time analyst alerts
- **ServiceNow or Jira ticketing**: add an HTTP step to create incident tickets from flow alerts
- **Webhook upgrade**: replace scheduled polling with Graph API change notifications for near-real-time detection
- **Third-party EDR**: connect Sophos, CrowdStrike, or SentinelOne via their REST APIs using the same OAuth + HTTP pattern
