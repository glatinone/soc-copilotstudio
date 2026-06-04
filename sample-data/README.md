# Sample Data

These JSON files simulate real Microsoft API responses. Use them to test your agents in Copilot Studio without waiting for live security events.

---

## Files

| File | Simulates | Key scenario |
|------|-----------|--------------|
| `risky-signins-sample.json` | Graph API `/auditLogs/signIns` response | Impossible travel: Singapore to Nigeria in 82 minutes, MFA not enforced |
| `phishing-alerts-sample.json` | Graph API `/security/alerts` response | BEC targeting CFO + malicious Excel attachment to Finance team |
| `audit-logs-sample.json` | Graph API `/auditLogs/directoryAudits` response | Global Admin role assigned at 2am, Conditional Access policy modified from external IP |

---

## How to use for testing

### Option A: Paste into Copilot Studio test panel

1. Open any agent in Copilot Studio
2. Click **Test** to open the test panel
3. Paste the JSON content and ask the agent to analyze it

Example prompt for Identity Threat Agent:
```
Analyze these risky sign-ins detected from Entra ID:

[paste contents of risky-signins-sample.json here]
```

### Option B: Use to test your flows

If you want to test flow logic without live data, temporarily replace the Graph API HTTP step in any flow with a "Compose" step containing the sample JSON, then continue the flow from there.

---

## Scenarios covered

**Impossible travel** (risky-signins-sample.json)
- john.doe logs in from Singapore at 08:14 UTC
- Same account logs in from Lagos, Nigeria at 09:36 UTC (82 minutes later, 11,000+ km apart)
- Second login has no MFA — single factor only
- Expected agent output: CRITICAL, impossible travel, immediate account disable

**BEC targeting executive** (phishing-alerts-sample.json)
- Phishing email sent to CFO impersonating Microsoft
- Typosquat domain: `micros0ft-verify-account.com`
- Expected agent output: HIGH, credential harvesting, BEC risk

**Suspicious admin activity** (audit-logs-sample.json)
- Global Admin role assigned at 2:15am
- Conditional Access policy modified from external IP at 1:48am
- CFO password reset at 3:22am
- Expected agent output: CRITICAL, unauthorized after-hours changes
