# Setup Guide

This guide walks you through building the full Microsoft Copilot SOC system from scratch. Follow the steps in order.

---

## Prerequisites

Before you start, confirm you have access to:

- [ ] Microsoft 365 tenant with Copilot Studio license
- [ ] Microsoft Entra ID (Global Admin or Security Admin access, or an admin who can grant consent)
- [ ] Microsoft Defender for Office 365 Plan 1 or Plan 2
- [ ] Power Automate (included with most Microsoft 365 plans)
- [ ] VirusTotal API key (free tier works — get one at virustotal.com/gui/join-us)

---

## Phase 1 — App Registration (30 minutes)

Follow the full guide: [entra-app-registration.md](./entra-app-registration.md)

At the end of Phase 1 you should have:
- Tenant ID
- Client ID
- Client Secret
- Admin consent granted for all 8 permissions

---

## Phase 2 — Build Power Automate Flows (45-60 minutes)

### How to import a flow definition

1. Go to [make.powerautomate.com](https://make.powerautomate.com)
2. Select your environment (top right)
3. Click **+ Create** > **Scheduled cloud flow**
4. Give it a name, click **Create**
5. In the flow editor, click the **three dots (...)** menu > **Code view** or **Edit in advanced mode**
6. Replace all existing content with the contents of the `flow-definition.json` file
7. Click **OK**
8. Find every `YOUR_` placeholder and replace with your actual values
9. Click **Save**

### Build order

Build in this order. Each flow is independent.

| Step | Flow | File | Time |
|------|------|------|------|
| 1 | Flow A — Risky Sign-ins | [flows/flow-a-risky-signins/flow-definition.json](../flows/flow-a-risky-signins/flow-definition.json) | 15 min |
| 2 | Flow B — Phishing Alerts | [flows/flow-b-defender-phishing/flow-definition.json](../flows/flow-b-defender-phishing/flow-definition.json) | 15 min |
| 3 | Flow C — Audit Logs | [flows/flow-c-audit-logs/flow-definition.json](../flows/flow-c-audit-logs/flow-definition.json) | 15 min |

### Known Power Automate quirks

- **Bearer token expression**: The Authorization header value must be entered in the expression editor as `concat('Bearer ', body('Parse_Access_Token')?['access_token'])` — do not combine static text and dynamic content in the standard field
- **URL encoding**: Spaces in query strings must be `%20` — the validator will reject unencoded spaces
- **Parse JSON schemas**: Use "Use sample payload to generate schema" rather than typing schemas directly. The Monaco editor has rendering issues with curly braces
- **Leave flows off until agents are ready**: Do not enable the schedules until you have the agent endpoint URL

---

## Phase 3 — Build Copilot Studio Agents (60-90 minutes)

### Build order

Build sub-agents first, orchestrator last. The orchestrator needs the sub-agents to exist before you can connect them.

| Step | Agent | Folder |
|------|-------|--------|
| 1 | Identity Threat Agent | [agents/identity-threat-agent/](../agents/identity-threat-agent/) |
| 2 | Phishing Investigation Agent | [agents/phishing-investigation-agent/](../agents/phishing-investigation-agent/) |
| 3 | Compliance and Audit Agent | [agents/compliance-audit-agent/](../agents/compliance-audit-agent/) |
| 4 | SOC Summary Agent | [agents/soc-summary-agent/](../agents/soc-summary-agent/) |
| 5 | Cybersecurity Assistant (orchestrator) | [agents/orchestrator/](../agents/orchestrator/) |

### For each agent

1. Go to [copilotstudio.microsoft.com](https://copilotstudio.microsoft.com)
2. Click **+ Create** > **New agent**
3. Name the agent exactly as specified in the description file
4. Paste the contents of `instructions.md` into the **Instructions** field
5. Paste the description from `description.md` into the **Description** field
6. Add trigger phrases listed in the description file to the **Topics** tab (one by one)
7. Save

### For the orchestrator

After all four sub-agents are created:
1. Create the Cybersecurity Assistant agent
2. Go to the **Agents** tab (use the +7 dropdown to find it)
3. Click **+ Add agent** and connect all four sub-agents
4. Paste the orchestrator instructions

### For the Phishing Investigation Agent (extra step)

After creating the agent, connect Flow 2 (VirusTotal + DMARC checker) in the **Tools** tab:
1. Click the **Tools** tab
2. Click **+ Add tool**
3. Select your Flow 2 from Power Automate
4. In the description field, paste the tool description from the agent's `description.md`

---

## Phase 4 — Connect Flows to Agents (15 minutes)

Once all agents are published:

1. Go to **Cybersecurity Assistant** > **Settings** > **Channels** > **Direct Line**
2. Copy the **Secret Key** and the **Endpoint URL**
3. Go back to Power Automate
4. Open each of Flow A, B, and C
5. Find the `YOUR_AGENT_ENDPOINT` placeholder and replace with the endpoint URL
6. Find the `YOUR_AGENT_API_KEY` placeholder and replace with the secret key
7. Save each flow

---

## Phase 5 — Test End to End

1. Enable Flow A schedule
2. Wait up to 5 minutes for a run
3. Check the flow run history for success
4. Open the Cybersecurity Assistant test panel
5. Check if the alert arrived and was processed by the Identity Threat Agent

For a manual test without waiting:
1. Open the Cybersecurity Assistant test panel
2. Paste a sample sign-in log (see the README for sample data)
3. Confirm the orchestrator routes it to the Identity Threat Agent
4. Confirm the Identity Threat Agent produces a formatted report

---

## Troubleshooting

| Problem | Likely cause | Fix |
|---------|-------------|-----|
| Flow fails at Get Access Token | Wrong Tenant ID, Client ID, or Secret | Double-check credentials in the HTTP step body |
| 403 Forbidden from Graph API | Admin consent not granted | Re-check API permissions in Entra |
| 401 Unauthorized from agent endpoint | Wrong API key or expired token | Regenerate Direct Line secret in Copilot Studio |
| Agent does not route correctly | Instructions not saved properly | Re-paste instructions and save again |
| Parse JSON fails | Schema mismatch | Use "Generate from sample" with a real API response |
