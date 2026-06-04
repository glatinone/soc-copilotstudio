# Flow B: Defender Phishing Alert Puller

Polls Microsoft Defender for Office 365 every 5 minutes for active phishing alerts and routes each one to the Phishing Investigation Agent for AI analysis.

## What it does

1. Authenticates to Microsoft Graph API using client credentials
2. Pulls security alerts filtered by `category eq 'Phishing'` and `status ne 'resolved'` (top 20)
3. For each alert: skips already-resolved ones, builds a detailed investigation prompt, sends it to the Phishing Investigation Agent
4. If no alerts found: logs a normal status message
5. Always writes an execution summary at the end

## Graph API endpoint used

```
GET https://graph.microsoft.com/v1.0/security/alerts
  ?$filter=category eq 'Phishing' and status ne 'resolved'
  &$orderby=createdDateTime desc
  &$top=20
```

## Data fields captured per alert

- Alert ID, title, severity, and status
- Detection timestamp
- Category and description
- Affected user states (who was targeted)
- Suspicious network connections (URLs)
- Suspicious file states (attachments)

## Placeholders to replace

| Placeholder | Description |
|-------------|-------------|
| `YOUR_TENANT_ID` | Your Microsoft 365 tenant ID |
| `YOUR_CLIENT_ID` | App Registration client ID |
| `YOUR_CLIENT_SECRET` | App Registration client secret |
| `YOUR_AGENT_ENDPOINT` | Direct Line endpoint from Copilot Studio |
| `YOUR_AGENT_API_KEY` | Direct Line secret key from Copilot Studio |

## Required permissions

`SecurityEvents.Read.All`, `SecurityAlert.Read.All`, `SecurityIncident.Read.All`

These require admin consent. See [entra-app-registration.md](../../docs/entra-app-registration.md).

## Schedule

Runs every **5 minutes**. Adjust in the Recurrence trigger if needed.
