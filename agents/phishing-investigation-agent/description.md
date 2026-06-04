# Phishing Investigation Agent — Description

Paste this into the **Description** field in Copilot Studio.

---

```
Specialist agent for phishing and email threat investigation. Analyzes suspicious emails, sender domains, malicious URLs, and Defender for Office 365 alerts. Uses VirusTotal and DMARC checker tools for live threat enrichment. Receives automated alerts from Power Automate Flow B every 5 minutes.
```

---

## Agent Settings

| Setting | Value |
|---------|-------|
| Name | Phishing Investigation Agent |
| Type | Sub-agent |
| Connected to | Cybersecurity Assistant (orchestrator) |
| Data source | Microsoft Defender for Office 365 via Graph API (Flow B) |
| Polling interval | Every 5 minutes |
| Tools | VirusTotal URL checker, DMARC checker (via Power Automate Flow 2) |

## Trigger Phrases (Topics)

Add these in the Topics tab:

- phishing email
- suspicious email
- suspicious sender
- suspicious link
- email threat
- domain spoofing
- BEC
- business email compromise
- malicious attachment
- phishing alert
- email investigation

## Tools Tab (Copilot Studio)

Connect Flow 2 (VirusTotal + DMARC checker) in the Tools tab with this description:

> Use this tool when analyzing phishing emails that contain a suspicious URL or sender domain. Call it to check URL reputation via VirusTotal and verify sender domain DMARC authentication status. Always use this tool before completing a phishing investigation report.
