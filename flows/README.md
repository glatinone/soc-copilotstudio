# Power Automate Flow Definitions

This folder contains the three core flows that power the Microsoft Copilot SOC system. Each flow pulls live data from Microsoft services and routes it to the appropriate Copilot Studio agent for AI analysis.

---

## Flow Overview

| Flow | Schedule | Data Source | Target Agent |
|------|----------|-------------|--------------|
| [Flow A](./flow-a-risky-signins/) | Every 5 minutes | Entra ID Sign-in Logs (Graph API) | Identity Threat Agent |
| [Flow B](./flow-b-defender-phishing/) | Every 5 minutes | Defender for Office 365 Alerts | Phishing Investigation Agent |
| [Flow C](./flow-c-audit-logs/) | Every 15 minutes | Entra ID Directory Audit Logs (Graph API) | Compliance and Audit Agent |

---

## Before You Import

You will need the following values ready. All are obtained from your Azure App Registration and Copilot Studio agent.

| Placeholder | Where to find it |
|-------------|-----------------|
| `YOUR_TENANT_ID` | Azure Portal > Entra ID > Overview |
| `YOUR_CLIENT_ID` | Azure Portal > App Registrations > your app > Overview |
| `YOUR_CLIENT_SECRET` | Azure Portal > App Registrations > your app > Certificates and Secrets |
| `YOUR_AGENT_ENDPOINT` | Copilot Studio > your agent > Settings > Channels > Direct Line |
| `YOUR_AGENT_API_KEY` | Copilot Studio > your agent > Settings > Channels > Direct Line > Secret Key |

> The App Registration setup guide is in [/docs/entra-app-registration.md](../docs/entra-app-registration.md)

---

## How to Import a Flow

1. Go to [make.powerautomate.com](https://make.powerautomate.com) and select your environment
2. Click **+ Create** then choose **Scheduled cloud flow**
3. Once inside the flow editor, click the **three dots (...)** menu in the top right
4. Select **Code view** (or **Edit in advanced mode**)
5. Replace all existing content with the contents of `flow-definition.json`
6. Click **OK** or **Apply**
7. Find and replace all placeholder values (search for `YOUR_`) with your actual credentials
8. Click **Save**
9. Test with **Test > Manually** before enabling the schedule

---

## Required App Registration Permissions

Your Azure App Registration needs the following API permissions with admin consent granted:

| Permission | Type | Used by |
|------------|------|---------|
| `AuditLog.Read.All` | Application | Flow A, Flow C |
| `Directory.Read.All` | Application | Flow A, Flow C |
| `IdentityRiskyUser.Read.All` | Application | Flow A |
| `IdentityRiskEvents.Read.All` | Application | Flow A |
| `User.Read.All` | Application | Flow A |
| `SecurityEvents.Read.All` | Application | Flow B |
| `SecurityAlert.Read.All` | Application | Flow B |
| `SecurityIncident.Read.All` | Application | Flow B |

---

## Important Notes

- **Never commit real credentials** to this repo. The placeholders (`YOUR_TENANT_ID` etc.) must always be replaced locally after import.
- The Bearer token expression for Authorization headers must use the expression editor in Power Automate: `concat('Bearer ', body('Parse_Access_Token')?['access_token'])`
- URI query strings with spaces must be URL-encoded as `%20` to pass Power Automate's validator
- JSON schemas in Parse JSON steps are best entered using **"Use sample payload to generate schema"** to avoid Monaco editor rendering issues
