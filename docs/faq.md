# FAQ

Common questions and issues when setting up the Microsoft Copilot SOC.

---

## App Registration

**Q: Do I need a Global Administrator to set this up?**

For the App Registration itself, no — any user can create an app registration. But granting admin consent for Application permissions requires a Global Administrator or Privileged Role Administrator. You can create the registration and add permissions yourself, then ask your admin to grant consent.

**Q: The client secret I saved stopped working. What happened?**

Client secrets expire. Check the expiry date in Entra ID > App registrations > your app > Certificates and secrets. If expired, create a new secret, copy the value immediately, and update it in all three flows.

**Q: Can I use a managed identity instead of a client secret?**

Yes, for Power Automate Premium flows in Azure you can use a managed identity. For standard Power Automate cloud flows, client credentials (client ID + secret) is the most straightforward approach.

---

## Power Automate Flows

**Q: The flow fails at "Get Graph API Access Token" with a 400 error.**

Check the request body in the HTTP step. The most common cause is a typo in the placeholder values. Verify `YOUR_TENANT_ID`, `YOUR_CLIENT_ID`, and `YOUR_CLIENT_SECRET` are all correct and have no extra spaces.

**Q: The flow gets a 403 Forbidden from the Graph API.**

Admin consent has not been granted for one or more permissions. Go to Entra ID > App registrations > your app > API permissions and check that all permissions show a green "Granted" status.

**Q: The Parse JSON step fails with a schema error.**

The API response structure changed or an optional field is missing. Use "Generate from sample payload" in the Parse JSON step with a real API response to regenerate the schema.

**Q: The Authorization header is not working. I get 401 Unauthorized.**

The Bearer token must be built using the expression editor, not the standard field. Use: `concat('Bearer ', body('Parse_Access_Token')?['access_token'])`. Do not combine static text and dynamic content in the standard input field.

**Q: My URI with spaces fails validation.**

URL-encode spaces as `%20`. Power Automate's validator does not accept unencoded spaces in URIs.

**Q: The flow runs but no data is sent to the agent.**

Check the condition step. If no risky sign-ins, phishing alerts, or audit events exist in the polling window, the flow correctly takes the FALSE branch and does not send anything. This is expected behavior. To test, use the sample data files in `/sample-data/`.

---

## Copilot Studio Agents

**Q: The Instructions field says I've exceeded the character limit.**

The Copilot Studio Instructions field has an 8,000 character limit. If you hit this, trim the instructions by removing examples or condensing the rules section. The core behavior instructions are more important than examples.

**Q: I cannot find the Agents tab or Tools tab.**

These are hidden in the overflow menu. Click the **+7** (or similar number) dropdown in the tab bar at the top of the agent editor. This reveals hidden tabs including Agents, Tools, Topics, Activity, and Channels.

**Q: The orchestrator is not routing to the right sub-agent.**

The routing depends on the Instructions field. Check that the sub-agent names in the instructions exactly match the actual agent names in Copilot Studio. Also verify the agents are connected in the Agents tab.

**Q: I cannot publish my agent to get the Direct Line endpoint.**

Publishing requires a Copilot Studio license. If you do not have one, you can still test using the built-in Test panel in Copilot Studio. The Test panel works without a publish license.

**Q: The agent is not calling the VirusTotal / DMARC tool.**

Tool calling requires the tool description in the Tools tab to be clear and specific. The description must tell the agent exactly when to call it. See the example tool description in `agents/phishing-investigation-agent/description.md`.

---

## General

**Q: How much does this cost to run?**

The main cost driver is Copilot Studio messages. Each time a flow sends data to an agent, it consumes messages. At 5-minute polling intervals with typical Microsoft 365 tenant activity, expect roughly 200-400 agent messages per day across all flows. Check your Copilot Studio license for included message limits.

**Q: Can I use this with Microsoft Sentinel instead of the Graph API?**

Yes. Replace the Graph API HTTP steps in the flows with Sentinel API calls. The agent instructions and architecture remain the same. The Sentinel Incidents API endpoint is: `https://management.azure.com/subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.OperationalInsights/workspaces/{workspace}/providers/Microsoft.SecurityInsights/incidents`.

**Q: Is this production-ready?**

The architecture is sound and the flows have been tested with real Microsoft 365 tenant data. For production use, additionally consider: rotating secrets into Azure Key Vault, adding error handling and retry logic to flows, setting up flow run alerts, and adding a Teams notification step for critical alerts.
