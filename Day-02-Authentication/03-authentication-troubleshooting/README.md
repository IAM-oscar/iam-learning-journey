# Lab 03 — Authentication Troubleshooting & Sign-In Investigation

## Objective

Investigate Azure and Microsoft Entra authentication behaviour using modern Azure PowerShell and the Microsoft Entra admin center.

This lab focuses on:

- Establishing a modern Azure PowerShell environment
- Authenticating to the OsCorp/Oslabs Azure environment
- Understanding authentication context
- Investigating Microsoft Entra sign-in activity
- Understanding successful and interrupted authentication events
- Investigating authentication details
- Understanding token-based authentication behaviour
- Applying the principle of least privilege during troubleshooting

---

## Environment

| Component | Configuration |
|---|---|
| Organization | OsCorp |
| Identity Platform | Microsoft Entra ID |
| Entra License | Microsoft Entra ID Free |
| PowerShell | PowerShell 7.6.5 |
| Azure PowerShell | Az 16.3.0 |
| Test Identity | Peter Parker |
| Test Department | Engineering |
| Authentication Baseline | Security Defaults enabled |
| Azure Environment | AzureCloud |

---

## Security Architecture Approach

This lab follows the **principle of least privilege**.

Authentication troubleshooting should be performed without unnecessarily increasing administrative privileges or weakening existing security controls.

The following principles apply:

- Use the minimum permissions required for the task.
- Do not grant Global Administrator simply to resolve a permissions problem.
- Do not disable Security Defaults to force an authentication result.
- Do not remove authentication methods simply to generate test evidence.
- Do not deliberately weaken authentication controls for troubleshooting.
- Do not expose passwords, tokens, client secrets, or private keys.
- Do not publish unnecessary personal network information.

---

# Part 1 — PowerShell Environment

## Step 1 — Verify PowerShell Version

PowerShell 7.6.5 was used for this lab.

The PowerShell version was verified with:

```powershell
$PSVersionTable.PSVersion
```

The environment returned:

```text
Major    : 7
Minor    : 6
Build    : 5
Revision : 0
```

PowerShell 7 was used instead of Windows PowerShell 5.1 to provide the modern PowerShell environment used for the Azure lab.

---

## Step 2 — Check for Az PowerShell

The installed Azure PowerShell modules were checked using:

```powershell
Get-Module -Name Az -ListAvailable
```

Initially, no Az module was returned.

This confirmed that the modern Az PowerShell module needed to be installed.

---

## Step 3 — Install Az PowerShell

The Az PowerShell module was installed for the current Windows user:

```powershell
Install-Module -Name Az -Repository PSGallery -Scope CurrentUser -Force
```

The installation was then verified:

```powershell
Get-Module -Name Az -ListAvailable
```

The installed module was:

```text
Name       : Az
Version    : 16.3.0
PSEdition  : Core, Desk
```

### Security Consideration

The module was installed using:

```text
-Scope CurrentUser
```

This avoids unnecessarily installing the module system-wide or elevating the PowerShell session solely for module installation.

---

# Part 2 — Azure PowerShell Authentication

## Step 4 — Initial Authentication Attempt

The initial Azure authentication attempt used:

```powershell
Connect-AzAccount
```

The authentication process returned:

```text
Connect-AzAccount: SharedTokenCacheCredential authentication failed
```

### Investigation

The computer running the lab is signed into Windows using a separate work account.

That work identity is not the OsCorp/Oslabs Azure lab identity.

The initial authentication attempt therefore encountered a cached Microsoft identity that was not appropriate for the intended lab tenant.

---

## Step 5 — Use Device Authentication

Rather than modifying the Windows work account or removing existing authentication configuration, device authentication was used:

```powershell
Connect-AzAccount -UseDeviceAuthentication
```

A Microsoft device authentication flow was presented.

The OsCorp/Oslabs lab account was then used to complete the authentication process.

The authentication completed successfully.

### Security Consideration

Device authentication allowed the intended OsCorp/Oslabs identity to be explicitly selected without changing the Windows work account configured on the computer.

The Windows work identity and the OsCorp/Oslabs lab identity remain separate.

---

# Authentication Architecture

The authentication flow used in this lab can be represented as:

```text
Local Lab Computer
       |
       | PowerShell 7
       |
       v
Azure PowerShell (Az)
       |
       | Connect-AzAccount
       |
       v
Microsoft Entra ID
       |
       v
Azure Subscription
       |
       v
Azure Resource Manager
       |
       v
Azure Resources
```

The important distinction is that **Azure PowerShell authentication establishes an Azure management context**.

It does not mean that Az PowerShell is itself an Entra directory administration interface.

Azure resource management and Microsoft Entra identity administration should be treated as related but distinct management planes.

---

# Part 3 — Azure Authentication Context

After successful authentication, the Azure PowerShell context should be verified using:

```powershell
Get-AzContext
```

The context should be reviewed for:

- Authenticated account
- Subscription
- Tenant
- Azure environment

The expected Azure environment is:

```text
AzureCloud
```

The Azure subscription context can also be reviewed using:

```powershell
Get-AzSubscription
```

### Evidence

Capture the authenticated Azure PowerShell context as:

```text
screenshots/01-az-authentication-context.png
```

Capture the subscription context as:

```text
screenshots/02-az-subscription-context.png
```

> These screenshots should only be added after the corresponding commands have been executed and the results verified.

---

# Part 4 — Microsoft Entra Sign-In Investigation

## Step 6 — Open Sign-In Logs

The modern Microsoft Entra admin center is used to investigate authentication activity.

Navigate to:

```text
Microsoft Entra admin center
    |
    v
Entra ID
    |
    v
Monitoring & health
    |
    v
Sign-in logs
```

Select the interactive user sign-in logs.

---

## Step 7 — Filter for Peter Parker

The sign-in logs are filtered for the OsCorp identity:

```text
Peter Parker
```

The existing authentication activity contains successful and interrupted interactive sign-in events.

No artificial authentication failure is required for this investigation.

Using existing authentication activity allows the troubleshooting exercise to be performed without deliberately weakening or disrupting the authentication configuration.

---

# Part 5 — Authentication Details Investigation

## Step 8 — Investigate a Successful Sign-In

A successful Peter Parker sign-in event is selected.

The following information is reviewed:

- User
- Date and time
- Application
- Resource
- Sign-in status
- Authentication details
- Authentication method
- Authentication requirement
- Result detail

---

## Step 9 — Investigate Authentication Method

The authentication details for the investigated event showed:

```text
Authentication method:
Previously satisfied
```

The authentication was successful.

The full result detail was:

```text
First factor requirement satisfied by claim in token
```

### Interpretation

The authentication requirement did not require Peter Parker to repeat the first authentication factor during this sign-in.

The existing authentication token contained a claim indicating that the first-factor requirement had already been satisfied.

This explains why a subsequent authentication attempt did not necessarily display a new authentication prompt.

### Important Security Interpretation

The absence of a new authentication prompt should not automatically be interpreted as an authentication or MFA failure.

Authentication requirements can be satisfied through information contained in an existing authentication token.

Authentication troubleshooting should therefore examine the sign-in event and authentication details before concluding that an authentication control has failed.

---

# Troubleshooting Finding

During MFA testing, a subsequent authentication attempt did not display a new MFA prompt.

Rather than assuming that MFA was malfunctioning, the corresponding sign-in event was investigated.

The event showed:

```text
Authentication method:
Previously satisfied
```

The full result detail was:

```text
First factor requirement satisfied by claim in token
```

The sign-in itself succeeded.

### Finding

The available evidence indicates that the authentication requirement was already satisfied through information contained in the existing authentication token.

This demonstrates why the absence of a new authentication prompt does not necessarily indicate that authentication or MFA is broken.

---

# Evidence

## Screenshot 01 — Azure PowerShell Authentication Context

```text
screenshots/01-az-authentication-context.png
```

Shows the authenticated Azure PowerShell context after connecting to the OsCorp/Oslabs Azure environment.

---

## Screenshot 02 — Azure Subscription Context

```text
screenshots/02-az-subscription-context.png
```

Shows the Azure subscription context available to the authenticated lab identity.

---

## Screenshot 03 — Authentication Details Investigation

```text
screenshots/03-authentication-details-investigation.png
```

Shows authentication details for a successful Peter Parker sign-in.

Key evidence includes:

- Authentication method: Previously satisfied
- Authentication succeeded
- First factor requirement satisfied by claim in token

---

# Security Risks to Avoid

## 1. Do Not Disable Security Defaults

Security Defaults should not be disabled simply to force an MFA prompt or create a particular troubleshooting result.

The existing security baseline should remain enabled during the lab.

---

## 2. Do Not Grant Excessive Privileges

Do not assign:

```text
Global Administrator
```

or:

```text
Owner
```

simply because an operation requires additional permissions.

Identify the required permission and use the least-privileged role capable of performing the task.

---

## 3. Do Not Delete Authentication Methods

Do not remove Peter Parker's registered Microsoft Authenticator method merely to generate a different MFA test result.

The existing authentication configuration provides legitimate evidence for the lab.

---

## 4. Do Not Expose Credentials

Never commit the following to GitHub:

- Passwords
- Access tokens
- Refresh tokens
- Client secrets
- Private keys
- Session cookies

---

## 5. Protect Network Information

Sign-in logs may contain network and location information.

Before publishing screenshots publicly:

- Crop personal public IP addresses where appropriate.
- Remove unnecessary personal device information.
- Remove unrelated work-account information.
- Retain fictional OsCorp identity information needed to demonstrate the lab.

---

# Lessons Learned

1. PowerShell 7 provides the modern PowerShell environment used for Azure administration.

2. The Az PowerShell module provides the modern Azure management experience used in this lab.

3. Az PowerShell should be used instead of the deprecated AzureAD PowerShell module.

4. Installing modules with `-Scope CurrentUser` can avoid unnecessary administrative elevation.

5. A Windows work account does not need to be the identity used for Azure administration.

6. Device authentication can be used when cached Microsoft authentication causes the default Az authentication flow to select the wrong identity.

7. Azure PowerShell authentication establishes an Azure management context.

8. Microsoft Entra sign-in logs provide valuable evidence for authentication troubleshooting.

9. Authentication details provide additional information beyond the basic sign-in status.

10. A missing authentication prompt does not automatically indicate that authentication or MFA is broken.

11. Authentication requirements can be satisfied through claims contained in an existing authentication token.

12. Authentication troubleshooting should be evidence-driven rather than assumption-driven.

---

# Lab Outcome

This lab establishes the foundation for investigating Azure and Microsoft Entra authentication behaviour.

The completed work demonstrates the ability to:

- Use PowerShell 7 for Azure administration.
- Install and verify the modern Az PowerShell module.
- Authenticate to an Azure lab environment.
- Resolve an authentication issue caused by cached identity selection.
- Use device authentication to explicitly authenticate the intended lab identity.
- Understand the Azure PowerShell authentication context.
- Investigate Microsoft Entra sign-in activity.
- Interpret authentication details.
- Understand token-based satisfaction of authentication requirements.
- Apply least-privilege principles during authentication troubleshooting.
- Preserve existing security controls while investigating authentication behaviour.

---

# Related Documentation

- [Day 02 — Authentication](../README.md)
- [Lab 01 — Authentication Fundamentals](../01-authentication-fundamentals/README.md)
- [Lab 02 — Multifactor Authentication](../02-mfa/README.md)
- [Day 01 — IAM Fundamentals](../../Day-01-IAM-Fundamentals/README.md)
- [OsCorp Environment Setup](../../00-Environment-Setup/README.md)
