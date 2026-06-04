# Contributing

Contributions are welcome. This project is most useful when it covers more Microsoft security scenarios and environments.

---

## What we need most

- **New agent instructions** for other threat domains (endpoint threats, cloud workload threats, insider risk)
- **Flow definitions** for other Microsoft security data sources (Defender for Endpoint, Defender for Cloud, Microsoft Sentinel)
- **Bug reports** when flow definitions fail to import or produce errors in specific environments
- **Documentation improvements** especially for steps that are unclear or outdated
- **Real-world testing feedback** — what worked, what needed adjustment, what broke

---

## How to contribute

1. Fork the repo
2. Create a branch: `git checkout -b feature/your-feature-name`
3. Make your changes
4. Test where possible (especially flow definitions — validate JSON before submitting)
5. Open a pull request with a clear description of what you changed and why

---

## Guidelines

**For agent instructions:**
- Write instructions that are generic enough to work across different organizations
- Remove any organization-specific references before submitting
- Test the instructions in Copilot Studio and confirm they produce sensible output
- Keep instructions within the 8,000 character limit of the Copilot Studio Instructions field

**For flow definitions:**
- All placeholder values must use the standard format: `YOUR_TENANT_ID`, `YOUR_CLIENT_ID`, `YOUR_CLIENT_SECRET`, `YOUR_AGENT_ENDPOINT`, `YOUR_AGENT_API_KEY`
- Validate JSON with `python3 -c "import json; json.load(open('flow-definition.json'))"` before submitting
- Document the API endpoint used and the permissions required

**For documentation:**
- No em dashes
- Plain language — assume the reader is a security professional but not necessarily a developer
- Include screenshots where they would meaningfully reduce setup friction

---

## What we do not accept

- Real credentials, tenant IDs, client secrets, or API keys (even test ones)
- Organization-specific configurations that would not generalize
- Instructions that recommend taking automated remediation actions without explicit human approval

---

## Questions

Open an issue and tag it with `question`.
