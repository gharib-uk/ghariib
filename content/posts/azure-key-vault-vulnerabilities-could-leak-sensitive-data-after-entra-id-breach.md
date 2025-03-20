---
title: "Azure Key Vault Vulnerabilities Could Leak Sensitive Data After Entra ID Breach"
date: 2025-01-29
---

A detailed walkthrough demonstrates how attackers can manipulate Azure Key Vault’s access policies after compromising Entra ID (formerly Azure AD) credentials.

According to Faran Siddiqui, a penetration tester report, a “Key Vault 06 – Abuse Decryption Key,” shed light on this critical vulnerability and the technique involving AzureAD CLI and Microsoft Graph API.

The breakdown stems from “Key Vault 06 – Abuse Decryption Key,” a security lab scenario focused on helping red team testers and penetration testers understand potential threats in the Azure Key Vault ecosystem.

By leveraging compromised credentials and exploiting gaps in access policies, attackers can retrieve sensitive information stored in Azure Key Vault, including encryption keys and secrets. This walkthrough shares how such an attack unfolds and how Burp Suite can be used to observe interactions with Azure endpoints.

**`Collect Threat Intelligence with TI Lookup to improve your company’s security - Get 50 Free Request`**

## **How Does the Attack Works**

The attack scenario begins with an attacker gaining access to compromised Azure credentials. Using PowerShell commands, the attacker logs into the Azure environment, establishes connections to Microsoft Graph API via tokens, and enumerates accessible resources.

**Access to Azure Resources:**

The attacker initiates the compromise by running the following commands:

```
   $passwd = ConvertTo-SecureString "<password>" -AsPlainText -Force
   $creds = New-Object System.Management.Automation.PSCredential("<user_email>", $passwd)
   Connect-AzAccount -Credential $creds
   $Token = (Get-AzAccessToken -ResourceTypeName MSGraph).Token
   Connect-MgGraph -AccessToken ($Token | ConvertTo-SecureString -AsPlainText -Force)
```

These steps authenticate the attacker and prepare them to enumerate Azure resources.

**Discovering the Key Vault:**

The `Get-AzResource` Command reveals a list of resources accessible to the compromised account.

Among them is an Azure Key Vault. Using Burp Suite, it was observed that a GET request to the management endpoint retrieves the data in JSON format:

```
   https://management.azure.com/subscriptions/<subscription_id>/resources?api-version=2021-04-01
```

This response identifies the Key Vault’s name (e.g., `aszlytkq347`).

**Testing Permission Restrictions:**

Attempting to list secrets using `Get-AzKeyVaultSecret` confirms that the compromised account lacks the necessary permissions. Similar restrictions apply when trying to fetch secrets using REST API calls:

```
   https://<vaultname>.vault.azure.net/secrets?api-version=7.0
```

**Enumerating Available Keys:**

The attacker shifts focus to enumerating cryptographic keys stored in the Key Vault using:

```
   Get-AzKeyVaultKey -VaultName <vault_name>
```

This command retrieves information about stored keys, revealing their algorithms, types, and versions. Requests to endpoints such as `/keys` and `/keys/<keyname>` expose metadata about the keys.

**Decrypting Sensitive Data:**

With access to at least one RSA key and its associated version, the attacker leverages the decryption feature by combining the compromised information with the provided decryption key (given at the start of the lab). Using the `Invoke-AzKeyVaultKeyOperation` command, they decrypt sensitive data as follows:

```
   $encryptedBytes = [Convert]::FromBase64String('<Base64 string>')
   $DecryptedData = Invoke-AzKeyVaultKeyOperation -Operation Decrypt -Algorithm RSA1_5 -ByteArrayValue $encryptedBytes -VaultName <vault_name> -Name <key_name>
   [system.Text.Encoding]::UTF8.GetString($DecryptedData.RawResult)
```

This converts encrypted Base64 strings into readable UTF-8 text, thereby exposing sensitive contents.

**Burp Suite Observations:**

Burp Suite logs reveal a POST request to the endpoint:

```
   /keys/<keyname>/<version>/decrypt?api-version=7.5-preview.1
```

The request body includes the encrypted content (Base64) sent for decryption, with the Key Vault responding with the plain-text result.

The blog highlights critical security oversights in access policy configurations and the reliance on compromised credentials. Attackers abusing key decryption processes underlines the importance of enforcing robust Role-Based Access Control (RBAC) permissions and auditing Azure activity logs for suspicious behavior.

## **Mitigation Steps**

Enterprise Security recommends the following measures to safeguard Azure Key Vaults from such attacks:

1. **Restrict Key Vault Permissions:** Ensure that limited users or service principals are granted Key Vault access, particularly for decryption or key management operations.

4. **Enable Managed Identities:** Use Managed Identities for an extra layer of security instead of static credentials.

7. **Audit Logs and Alerts:** Monitor logs for unusual activity in the Azure Key Vault, particularly POST requests to `/decrypt` endpoints.

10. **Enable Conditional Access Policies:** Implement Conditional Access to restrict login attempts from suspicious IPs or regions.

13. **Regular Key Vault Reviews:** Periodically review and rotate keys, ensuring no stale or unauthorized keys exist.

This insightful walkthrough highlights ongoing challenges in securing cloud environments, particularly Azure. The alarming ease with which access policies can be exploited stresses the need for organizations to treat cloud security as a continuous process.

****Integrating Application Security into Your CI/CD Workflows Using Jenkins & Jira -> Free Webinar****

The post Azure Key Vault Vulnerabilities Could Leak Sensitive Data After Entra ID Breach appeared first on Cyber Security News.

Go to Source
