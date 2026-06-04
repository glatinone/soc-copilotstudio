# Flow C: Audit Logs Puller

Polls Microsoft Entra ID every 15 minutes for admin activity and directory audit logs, then routes suspicious activity to the Compliance and Audit Agent for AI analysis.

## What it does

1. Authenticates to Microsoft Graph API using client credentials
2. Pulls directory audit logs from the last 15 minutes (top 50)
3. Filters for high-risk activities: Add, Delete, Update, Reset, Disable, Enable, role changes, and policy changes
4. If activity found: builds a structured compliance investigation prompt and sends to the Compliance and Audit Agent
5. If nothing found: logs a normal compliance status
6. Always writes an execution summary at the end

## Graph API endpoint used

```
GET https://graph.microsoft.com/v1.0/auditLogs/directoryAudits
  ?$filter=activityDateTime ge {15 minutes ago}
  &$orderby=activityDateTime desc
  &$top=50
```

## Data fields captured per audit event

- Activity name and timestamp
- Category
- Result (success or failure)
- Initiated by (user display name, UPN, IP address)
- Target resources affected

## High-risk activity filter

The flow filters for events containing these keywords in the activity name:
`Add`, `Delete`, `Update`, `Reset`, `Disable`, `Enable`, `role`, `policy`

This catches the most security-relevant admin actions while skipping routine read operations.

## Placeholders to replace

| Placeholder | Description |
|-------------|-------------|
| `YOUR_TENANT_ID` | Your Microsoft 365 tenant ID |
| `YOUR_CLIENT_ID` | App Registration client ID |
| `YOUR_CLIENT_SECRET` | App Registration client secret |
| `YOUR_AGENT_ENDPOINT` | Direct Line endpoint from Copilot Studio |
| `YOUR_AGENT_API_KEY` | Direct Line secret key from Copilot Studio |

## Required permissions

`AuditLog.Read.All`, `Directory.Read.All`

These require admin consent. See [entra-app-registration.md](../../docs/entra-app-registration.md).

## Schedule

Runs every **15 minutes**. Audit logs are less time-sensitive than sign-in events, so a 15-minute interval is appropriate and reduces API call volume.
