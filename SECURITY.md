# Security Policy

## Credential Safety

This repository contains no real credentials. All values in flow definitions and documentation use placeholder format: `YOUR_TENANT_ID`, `YOUR_CLIENT_ID`, `YOUR_CLIENT_SECRET`, `YOUR_AGENT_ENDPOINT`, `YOUR_AGENT_API_KEY`.

If you fork this repo, never commit real credentials. Add them only after importing flows into Power Automate, directly in the browser.

## Reporting a Vulnerability

If you find a security issue in this project (e.g. a flow definition that inadvertently leaks data, or an agent instruction that could be exploited), please open a GitHub Issue tagged `security`.

Do not include sensitive reproduction details in public issues. If the issue is sensitive, contact via GitHub private vulnerability reporting.

## Secrets Management Recommendations

When deploying this system in your environment:

- Store `CLIENT_SECRET` in Azure Key Vault, not in Power Automate plain text fields, for production deployments
- Rotate the client secret before its expiry date — set a calendar reminder
- Use fine-grained Personal Access Tokens with minimal scope and short expiry when automating GitHub operations
- Revoke tokens immediately after use
- Review your Entra ID App Registration permissions periodically and remove any that are no longer needed

## Scope of This Project

This project is a detection and investigation tool only. It does not perform automated remediation actions. All recommended actions in agent outputs require human approval before execution. This is by design.
