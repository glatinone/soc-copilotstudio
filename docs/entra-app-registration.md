# Entra ID App Registration Setup

Before building any flows, you need to register an application in Microsoft Entra ID. This gives your Power Automate flows permission to read security data via the Graph API.

---

## Step 1 — Create the App Registration

1. Go to [portal.azure.com](https://portal.azure.com)
2. Navigate to **Microsoft Entra ID** (formerly Azure Active Directory)
3. Click **App registrations** in the left sidebar
4. Click **+ New registration**
5. Configure:
   - Name: `SOC Copilot Studio Connector` (or any name you prefer)
   - Supported account types: **Accounts in this organizational directory only**
   - Redirect URI: leave blank
6. Click **Register**
7. Copy and save your **Application (client) ID** and **Directory (tenant) ID** from the Overview page

---

## Step 2 — Create a Client Secret

1. Inside your app registration, click **Certificates and secrets**
2. Click **+ New client secret**
3. Description: `SOC flows secret`
4. Expiry: choose based on your security policy (recommended: 12 months)
5. Click **Add**
6. **Copy the secret Value immediately** — it will not be shown again
7. Store it securely (use a password manager or Azure Key Vault)

---

## Step 3 — Add API Permissions

1. Click **API permissions** in the left sidebar
2. Click **+ Add a permission**
3. Select **Microsoft Graph**
4. Select **Application permissions** (not Delegated)
5. Add each of these permissions:

| Permission | Used by |
|------------|---------|
| `AuditLog.Read.All` | Flow A, Flow C |
| `Directory.Read.All` | Flow A, Flow C |
| `IdentityRiskyUser.Read.All` | Flow A |
| `IdentityRiskEvents.Read.All` | Flow A |
| `User.Read.All` | Flow A |
| `SecurityEvents.Read.All` | Flow B |
| `SecurityAlert.Read.All` | Flow B |
| `SecurityIncident.Read.All` | Flow B |

6. Click **Add permissions**

---

## Step 4 — Grant Admin Consent

> This step requires a **Global Administrator** or **Privileged Role Administrator** account.

1. After adding all permissions, you will see a warning: "Not granted for [tenant]"
2. Click **Grant admin consent for [your tenant name]**
3. Click **Yes** to confirm
4. All permissions should now show a green checkmark: "Granted for [tenant]"

If you do not have admin rights, send this message to your IT admin:

```
Hi, I need admin consent granted for the following permissions 
on the app registration named "SOC Copilot Studio Connector":

- AuditLog.Read.All
- Directory.Read.All
- IdentityRiskyUser.Read.All
- IdentityRiskEvents.Read.All
- User.Read.All
- SecurityEvents.Read.All
- SecurityAlert.Read.All
- SecurityIncident.Read.All

Path: Entra ID > App registrations > SOC Copilot Studio Connector > 
API permissions > Grant admin consent

Thank you
```

---

## Step 5 — Save Your Credentials

You now have three values. Store them securely.

| Value | Where to find it |
|-------|-----------------|
| Tenant ID | App registration > Overview > Directory (tenant) ID |
| Client ID | App registration > Overview > Application (client) ID |
| Client Secret | The value you copied in Step 2 |

These replace the placeholders in all three flows:
- `YOUR_TENANT_ID`
- `YOUR_CLIENT_ID`
- `YOUR_CLIENT_SECRET`

---

## Security Notes

- Never commit these values to source control
- Rotate the client secret before its expiry date
- Use Azure Key Vault for production deployments
- Limit the app registration to the minimum permissions required for your use case
- Review and audit app registration activity periodically via Entra ID audit logs
