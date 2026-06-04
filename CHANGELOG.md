# Changelog

All notable changes to this project will be documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [1.0.0] - 2026-06-04

### Added
- Initial release of Microsoft Copilot SOC
- Cybersecurity Assistant orchestrator with full routing instructions
- Identity Threat Agent for risky sign-in and impossible travel detection
- Phishing Investigation Agent with VirusTotal and DMARC tool integration
- Compliance and Audit Agent mapped to CIS Controls and ISO 27001
- SOC Summary Agent for executive and analyst report generation
- Flow A: Power Automate flow for Graph API risky sign-ins (5-minute schedule)
- Flow B: Power Automate flow for Defender for Office 365 phishing alerts (5-minute schedule)
- Flow C: Power Automate flow for Entra ID directory audit logs (15-minute schedule)
- Full setup guide covering app registration, flow import, and agent configuration
- Architecture documentation with design decisions and extension points
- Entra ID App Registration guide with all required permissions
- MIT License
- Contributing guide

---

## Upcoming

### Planned for v1.1.0
- Sample data files for testing without live Microsoft data
- GitHub Actions workflow to validate flow definition JSON on pull requests
- Demo script for walking through a live investigation scenario
- FAQ document covering common setup issues

### Ideas for v2.0.0
- Endpoint Threat Agent using Defender for Endpoint
- Teams notification step in all flows
- ServiceNow / Jira incident ticketing integration
- Microsoft Graph webhook upgrade for near-real-time detection
- Azure Key Vault integration for secret management
