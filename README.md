# Microsoft Copilot SOC

**An AI-powered Security Operations Center built entirely on Microsoft infrastructure.**

No Sentinel. No third-party SIEM. No custom backend code.

Just Copilot Studio, Power Automate, and the Microsoft Graph API — tools you likely already have.

---

## What this does

Five AI agents work together to monitor your Microsoft 365 environment around the clock:

| Agent | What it watches | Data source |
|-------|----------------|-------------|
| Identity Threat Agent | Risky sign-ins, impossible travel, MFA bypass | Entra ID via Graph API |
| Phishing Investigation Agent | Phishing emails, suspicious senders, malicious URLs | Defender for Office 365 |
| Compliance and Audit Agent | Admin activity, policy changes, permission escalations | Entra ID directory audit logs |
| SOC Summary Agent | Synthesizes findings into executive and analyst reports | All agents |
| Cybersecurity Assistant | Orchestrates all of the above | Entry point for analysts |

Power Automate flows poll Microsoft's security APIs every 5-15 minutes and push alerts directly to the agents for immediate AI analysis.

---

## Architecture

```
Microsoft Entra ID + Defender for Office 365
              |
       Power Automate Flows
       (every 5-15 minutes)
              |
    Cybersecurity Assistant   <-- Orchestrator
              |
    __________|__________________________
    |          |           |            |
Identity   Phishing   Compliance    SOC
Threat     Invest.    Audit         Summary
Agent      Agent      Agent         Agent
```

Full architecture details: [docs/architecture.md](docs/architecture.md)

---

## What's included

```
microsoft-copilot-soc/
├── agents/
│   ├── orchestrator/              # Cybersecurity Assistant instructions
│   ├── identity-threat-agent/     # Identity threat investigation prompts
│   ├── phishing-investigation-agent/  # Phishing analysis prompts + tool config
│   ├── compliance-audit-agent/    # Compliance audit prompts
│   └── soc-summary-agent/         # Report generation prompts
├── flows/
│   ├── flow-a-risky-signins/      # Power Automate flow definition (JSON)
│   ├── flow-b-defender-phishing/  # Power Automate flow definition (JSON)
│   └── flow-c-audit-logs/         # Power Automate flow definition (JSON)
└── docs/
    ├── setup-guide.md             # Full step-by-step setup
    ├── architecture.md            # Design decisions and extension points
    └── entra-app-registration.md  # App registration and permissions guide
```

---

## Prerequisites

- Microsoft 365 tenant
- Copilot Studio license (per-user or per-tenant)
- Microsoft Defender for Office 365 Plan 1 or Plan 2
- Power Automate (included with most M365 plans)
- VirusTotal API key (free tier works)
- Entra ID admin access (or an admin who can grant consent)

---

## Getting started

**Total setup time: 2-3 hours**

1. [Create the Entra ID App Registration](docs/entra-app-registration.md) — 30 min
2. [Import and configure the three Power Automate flows](flows/README.md) — 45 min
3. [Build the five Copilot Studio agents](docs/setup-guide.md) — 60-90 min
4. Connect flows to agents via Direct Line endpoint
5. Test end to end

Full walkthrough: [docs/setup-guide.md](docs/setup-guide.md)

---

## How the flows work

Each flow follows the same pattern:

1. Authenticate to Microsoft Graph API using OAuth2 client credentials
2. Pull security events from the last polling interval
3. If threats found: format the data and POST to the appropriate Copilot Studio agent
4. Agent analyzes and produces a structured investigation report
5. If nothing found: log a normal status

The Bearer token expression that works reliably in Power Automate:
```
concat('Bearer ', body('Parse_Access_Token')?['access_token'])
```

---

## Real output example

When Flow A detects a risky sign-in, the Identity Threat Agent produces a report like this:

```
IDENTITY THREAT REPORT
======================
Risk Level: CRITICAL

Attack Type: Impossible Travel

Affected Accounts:
- john.doe@company.com

Suspicious Indicators:
- Login from Singapore at 08:14 UTC
- Successful login from Nigeria at 09:36 UTC (82 minutes later)
- Travel distance: ~11,000 km — physically impossible
- MFA not enforced on second login
- Unrecognized device used in Nigeria login

Timeline:
- 08:14 UTC — Login from Singapore (known device, normal location)
- 09:36 UTC — Login from Lagos, Nigeria (unknown device, no MFA)

Recommended Actions:
[IMMEDIATE] Disable account, revoke all active sessions
[INVESTIGATE] Check for data access or exfiltration since 09:36 UTC
[ESCALATE] Notify IT security team and account owner immediately

Summary:
A user account shows a login pattern that is physically impossible —
successful authentications from two countries 82 minutes apart with
no MFA challenge on the second login. This indicates likely account
compromise and requires immediate containment.
```

---

## Known limitations

- Scheduled polling (not real-time): 5-minute gap for identity and phishing alerts, 15 minutes for audit logs
- Copilot Studio publish license required to get the Direct Line endpoint for flow-to-agent connection
- Client secret requires periodic rotation (set a calendar reminder before expiry)
- This is a detection and investigation tool — it does not take automated remediation actions (by design)

---

## Extending this system

The architecture is modular. Some directions you can take it:

- Add an Endpoint Threat Agent using Defender for Endpoint alerts
- Add Teams notifications in each flow for real-time analyst pings
- Connect to ServiceNow or Jira for automatic incident ticket creation
- Upgrade to near-real-time with Microsoft Graph webhooks
- Add Microsoft Sentinel integration for organizations with that license

See [docs/architecture.md](docs/architecture.md) for more extension ideas.

---

## Contributing

Pull requests welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).

---

## License

MIT — see [LICENSE](LICENSE)

---

## Background

This system was built as an internal project at a mid-size enterprise running Microsoft 365, with the goal of getting AI-assisted security operations without adding another vendor. The full story and lessons learned are documented in a Medium article linked in the project background.

The core insight: Microsoft already detects risky logins, flags phishing emails, and logs every admin action. The data is all there. The gap is that it sits in separate portals waiting for a human to go look. These agents do the looking instead.

## Repository topics

If you find this useful, consider adding a GitHub star. It helps others find the project.

Suggested search topics: `microsoft-copilot-studio` `power-automate` `security-operations-center` `microsoft-graph-api` `entra-id` `defender-office-365` `soc-automation` `ai-security`
