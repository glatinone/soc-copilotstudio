# Agents

This folder contains the instructions and descriptions for all five Copilot Studio agents.

---

## Agent Overview

| Agent | Folder | Role | Connected Flow |
|-------|--------|------|---------------|
| Cybersecurity Assistant | `orchestrator/` | Main entry point, routes all queries to specialist agents | Receives from all flows |
| Identity Threat Agent | `identity-threat-agent/` | Sign-in anomaly and credential threat analysis | Flow A (5 min) |
| Phishing Investigation Agent | `phishing-investigation-agent/` | Email threat and phishing investigation | Flow B (5 min) |
| Compliance and Audit Agent | `compliance-audit-agent/` | Admin activity and policy compliance review | Flow C (15 min) |
| SOC Summary Agent | `soc-summary-agent/` | Executive and analyst report generation | Manual / escalation |

---

## File structure per agent

Each agent folder contains two files:

- `instructions.md` — paste the contents into the **Instructions** field in Copilot Studio
- `description.md` — paste the short description into the **Description** field, plus setup notes and trigger phrases for the Topics tab

---

## Build order

Always build sub-agents before the orchestrator. The orchestrator needs the sub-agents to exist before you can connect them in the Agents tab.

1. Identity Threat Agent
2. Phishing Investigation Agent
3. Compliance and Audit Agent
4. SOC Summary Agent
5. Cybersecurity Assistant (orchestrator) — last

---

## Copilot Studio character limits

The Instructions field has an **8,000 character limit**. Current character counts:

| Agent | Approx. characters |
|-------|--------------------|
| Orchestrator | ~1,800 |
| Identity Threat Agent | ~1,600 |
| Phishing Investigation Agent | ~2,200 |
| Compliance and Audit Agent | ~2,100 |
| SOC Summary Agent | ~1,900 |

All agents are well within the limit. Monitor this if you add custom instructions for your organization.
