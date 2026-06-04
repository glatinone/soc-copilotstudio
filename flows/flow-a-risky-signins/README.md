# Flow A: Risky Sign-ins Puller

Polls Microsoft Entra ID every 5 minutes for risky sign-in events and routes them to the Identity Threat Agent for AI analysis.

## What it does

1. Authenticates to Microsoft Graph API using client credentials
2. Pulls sign-in logs filtered by `riskLevelDuringSignIn ne 'none'` (top 50 most recent)
3. If risky sign-ins are found: formats the data and sends a structured investigation prompt to the Identity Threat Agent
4. If nothing found: logs a normal status message
5. Always writes an execution summary at the end

## Graph API endpoint used

```
GET https://graph.microsoft.com/v1.0/auditLogs/signIns
  ?$filter=riskLevelDuringSignIn ne 'none'
  &$orderby=createdDateTime desc
  &$top=50
```

## Data fields captured per sign-in

- User display name and email (UPN)
- IP address
- Location (city and country)
- Risk level during sign-in
- Risk state
- Authentication requirement (MFA or not)
- Device detail

## Placeholders to replace

| Placeholder | Description |
|-------------|-------------|
| `YOUR_TENANT_ID` | Your Microsoft 365 tenant ID |
| `YOUR_CLIENT_ID` | App Registration client ID |
| `YOUR_CLIENT_SECRET` | App Registration client secret |
| `YOUR_AGENT_ENDPOINT` | Direct Line endpoint from Copilot Studio |
| `YOUR_AGENT_API_KEY` | Direct Line secret key from Copilot Studio |

## Schedule

Runs every **5 minutes**. Adjust in the Recurrence trigger if needed.
