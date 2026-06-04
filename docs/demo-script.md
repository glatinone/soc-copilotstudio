# Demo Script

A 5-minute walkthrough of the Microsoft Copilot SOC using sample data. Use this for stakeholder demos or to verify your setup is working correctly.

---

## Setup before the demo

1. Open Copilot Studio
2. Navigate to the **Cybersecurity Assistant** agent
3. Click **Test** to open the test panel
4. Have the sample data files open in a text editor: `sample-data/risky-signins-sample.json`

---

## Scene 1: Impossible Travel Alert (2 minutes)

**What you are showing:** The orchestrator receiving a sign-in alert and routing it to the Identity Threat Agent for analysis.

**Step 1** — In the test panel, type:

```
Analyze these risky sign-ins detected from Entra ID in the last 5 minutes:
```

Then paste the contents of `sample-data/risky-signins-sample.json`.

**What to expect:**
- Orchestrator acknowledges the alert and states it is routing to Identity Threat Agent
- Identity Threat Agent produces a full IDENTITY THREAT REPORT
- Risk Level: CRITICAL
- Attack Type: Impossible Travel
- Flags the Singapore to Nigeria login (82 minutes, 11,000+ km)
- Flags missing MFA on the second login
- Recommends immediate account disable and session revocation

**Talking point:** "Microsoft already detected this as risky. The agent turns that raw signal into an actionable investigation report in seconds — no analyst had to go look for it."

---

## Scene 2: Phishing Alert (1.5 minutes)

**Step 1** — In the test panel, type:

```
Phishing alert from Defender for Office 365:
```

Then paste the contents of `sample-data/phishing-alerts-sample.json`.

**What to expect:**
- Routes to Phishing Investigation Agent
- Identifies typosquat domain (`micros0ft-verify-account.com`)
- Flags CFO targeting as BEC risk
- Recommends quarantine and org-wide sender block

**Talking point:** "Defender flagged it. The agent explains exactly why it is dangerous and what to do about it — in language both the SOC analyst and the manager can understand."

---

## Scene 3: Executive Summary (1.5 minutes)

**Step 1** — In the test panel, type:

```
Generate an executive summary of the security incidents detected today.
Include the impossible travel incident for john.doe and the phishing attempt targeting the CFO.
```

**What to expect:**
- Routes to SOC Summary Agent
- Produces a structured executive summary
- Plain language, no jargon
- Business impact section
- Outstanding action items
- Ready to forward to management

**Talking point:** "The same investigation that took minutes to run — the summary is ready for the CEO in one more message. No one had to write a report."

---

## Closing talking points

- Everything runs on Microsoft infrastructure you already have
- No new vendor, no additional security tool to manage
- Flows poll Entra and Defender automatically every 5 minutes
- The architecture is modular — new threat domains can be added as new agents
- This is a starting point, not a finished product — the community can extend it
