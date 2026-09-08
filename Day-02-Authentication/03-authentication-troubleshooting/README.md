# Lab 03 --- Authentication Troubleshooting & Sign-In Investigation

## Objective

This lab investigates an authentication failure in Microsoft Entra ID
and demonstrates an evidence-based troubleshooting process.

The investigation covers:

-   Azure PowerShell authentication
-   Microsoft Entra sign-in logs
-   Device Code authentication
-   Security Defaults
-   Error `AADSTS530035`
-   Web Account Manager (WAM) authentication
-   Azure subscription context validation
-   Authentication and authorization boundaries
-   Least-privilege security considerations

The goal is not simply to make authentication work. The goal is to
determine **why an authentication attempt failed, identify the security
control responsible, apply the least disruptive remediation, and
validate the result with evidence**.

------------------------------------------------------------------------

## Environment

  Item                              Configuration
  --------------------------------- ------------------------------
  Organization                      OsCorp
  Identity platform                 Microsoft Entra ID
  Azure subscription                Azure subscription 1
  Azure PowerShell module           Az 16.3.0
  PowerShell                        PowerShell 7.6.5
  Authentication account            `oscorp.iam.labs`
  Entra tenant                      OsCorp
  Security baseline                 Security Defaults enabled
  Azure PowerShell authentication   Interactive/WAM
  Legacy AzureAD module             Not used

> **Security note:** Subscription identifiers, tenant identifiers, IP
> addresses, request IDs, correlation IDs, session IDs, and other
> diagnostic identifiers should be treated carefully when publishing
> screenshots to a public repository. Crop or redact unnecessary values
> before committing evidence to GitHub.

------------------------------------------------------------------------

# 1. Why Authentication Troubleshooting Matters

A successful IAM implementation is not only about creating users,
groups, and authentication policies.

A production IAM administrator must also be able to answer:

-   Why did authentication fail?
-   Was the problem identity, authentication, authorization, or policy?
-   Which security control blocked the request?
-   Was the authentication method supported?
-   Did the user actually authenticate?
-   Did the user receive an Azure resource-plane authorization context?
-   Was the remediation secure, or did it weaken the tenant?

This lab demonstrates that troubleshooting process.

------------------------------------------------------------------------

# 2. Authentication Architecture

The investigation can be viewed as the following sequence:

``` text
User / Administrator
        |
        v
Authentication Request
        |
        v
Microsoft Entra ID
        |
        +---- Security Defaults evaluation
        |
        v
Authentication Flow
        |
        +---- Device Code Flow
        |          |
        |          +---- Blocked
        |               AADSTS530035
        |
        +---- Interactive/WAM
                   |
                   +---- Successful authentication
                              |
                              v
                       Azure PowerShell
                              |
                              v
                       Azure subscription
                       resource-plane context
```

The important distinction is that **authentication and authorization are
separate controls**.

A user can authenticate successfully while still lacking authorization
to perform a particular Azure resource operation.

------------------------------------------------------------------------

# 3. PowerShell Tooling

## 3.1 PowerShell Version

The lab uses PowerShell 7.6.5.

The version was verified with:

``` powershell
$PSVersionTable.PSVersion
```

The lab deliberately uses modern PowerShell rather than relying on
legacy Windows PowerShell tooling.

------------------------------------------------------------------------

## 3.2 Az PowerShell Module

The Azure PowerShell module was installed for the current user:

``` powershell
Install-Module -Name Az -Repository PSGallery -Scope CurrentUser -Force
```

The installed module was verified as:

``` text
Az
16.3.0
```

The **Az PowerShell module** is used here for Azure resource-plane
operations and Azure subscription context management.

> **Architecture note:** Microsoft Entra directory and sign-in
> information should not be assumed to be exposed through Az PowerShell
> cmdlets. This lab uses the modern Microsoft Entra admin center for
> sign-in investigation and Az PowerShell for Azure resource-plane
> authentication/context validation.

------------------------------------------------------------------------

# 4. Initial Authentication Problem

The first Azure PowerShell authentication attempt encountered a local
token-cache authentication problem:

``` text
SharedTokenCacheCredential authentication failed
```

A device-code authentication attempt was then tested.

The command used was:

``` powershell
Connect-AzAccount -UseDeviceAuthentication
```

The device-code flow prompted the administrator to authenticate through
the Microsoft device login page.

However, the authentication request was subsequently blocked by
Microsoft Entra Security Defaults.

------------------------------------------------------------------------

# 5. Device Code Authentication Failure

## 5.1 Sign-In Event

The Microsoft Entra sign-in logs were investigated using the modern
**Sign-in events** experience.

The relevant event occurred at:

``` text
2026-09-08T02:00:17Z
```

The application was:

``` text
Microsoft Azure PowerShell
```

The event showed:

``` text
Status: Failure
Authentication requirement: Single-factor authentication
Sign-in error code: 530035
Failure reason: Access has been blocked by security defaults.
```

This provided direct evidence of the root cause.

### Evidence

![Device Code authentication blocked by Security
Defaults](./screenshots/01-device-code-authentication-failure.png)

------------------------------------------------------------------------

## 5.2 Error Interpretation

The error code was:

``` text
530035
```

The failure reason reported by Entra was:

``` text
Access has been blocked by security defaults.
```

This means the request was blocked by the tenant's security baseline
rather than failing because of an incorrect password or an Azure
subscription authorization problem.

The investigation therefore identified a
**security-policy/authentication-flow issue**.

------------------------------------------------------------------------

# 6. Why Authentication Details Were Not Available

When the failed `02:00:17Z` event was opened, the **Basic info** view
contained the complete failure information.

The Authentication Details view did not provide a normal authentication
event for this request.

This is consistent with the observed behavior: the request was blocked
by Security Defaults before a normal authentication event was triggered.

Therefore, this lab does **not** claim that an authentication method was
successfully evaluated for the blocked request.

The evidence supports the narrower conclusion:

> The Microsoft Azure PowerShell request was blocked by Security
> Defaults and returned error `530035`.

This distinction is important when documenting authentication incidents.

------------------------------------------------------------------------

# 7. Security Investigation

The failed event established the following:

  Investigation item           Observed result
  ---------------------------- ----------------------------------------------
  User                         `oscorp.iam.labs`
  Application                  Microsoft Azure PowerShell
  Event time                   `2026-09-08T02:00:17Z`
  Status                       Failure
  Authentication requirement   Single-factor authentication
  Sign-in error code           `530035`
  Failure reason               Access has been blocked by security defaults
  Authentication details       No normal authentication event available

The evidence indicates that the authentication request was stopped by
the tenant security baseline.

------------------------------------------------------------------------

# 8. Remediation Strategy

The security control was **not disabled**.

Security Defaults remained enabled.

Instead, the authentication method was changed to an interactive
authentication approach supported by the environment.

Azure PowerShell was configured to use **Web Account Manager (WAM)**:

``` powershell
Update-AzConfig -EnableLoginByWam $true
```

The configuration returned:

``` text
EnableLoginByWam : True
Applies To       : Az
Scope            : CurrentUser
```

This established WAM as the default interactive login experience for the
current user.

------------------------------------------------------------------------

# 9. Successful Interactive Authentication

After WAM was enabled, Azure PowerShell was connected using the OsCorp
tenant and Azure subscription:

``` powershell
Connect-AzAccount `
    -Tenant "<OSCORP-TENANT-ID>" `
    -Subscription "<AZURE-SUBSCRIPTION-ID>"
```

The account selection displayed the OsCorp Azure subscription:

``` text
Azure subscription 1
```

The tenant was:

``` text
OsCorp
```

The interactive authentication completed successfully.

------------------------------------------------------------------------

# 10. Azure PowerShell Context Validation

The authenticated context was validated with:

``` powershell
Get-AzContext
```

The result showed:

``` text
Tenant: 90115781-3bed-422c-9334-062b88e95a24

SubscriptionName : Azure subscription 1
SubscriptionId   : 5----------------------------a
Account          : oscorp.iam.labs
Environment      : AzureCloud
Tenant           : 5----------------------------a
```

The subscription was also independently validated with:

``` powershell
Get-AzSubscription
```

The observed state was:

``` text
Name              : Azure subscription 1
Id                : 5----------------------------a
TenantId          : 5----------------------------a
State             : Enabled
```

This proves that the successful authentication established a usable
Azure PowerShell resource-plane context.

### Evidence

![Authenticated Azure PowerShell
context](./screenshots/02-device-code-sign-in-investigation.png)

------------------------------------------------------------------------

# 11. Successful Entra Sign-In Event

The successful Azure PowerShell sign-in was then located in Microsoft
Entra sign-in events.

The successful event occurred at:

``` text
2026-09-08T02:10:03Z
```

The event showed:

``` text
Application: Microsoft Azure PowerShell
Status: Success
Authentication requirement: Single-factor authentication
```

The Additional Details field reported:

``` text
MFA requirement satisfied by claim in the token
```

### Evidence

![Successful Azure PowerShell
sign-in](./screenshots/03-az-powershell-authenticated-context.png)

------------------------------------------------------------------------

# 12. Important Interpretation of the Successful Event

The successful Entra event should be interpreted carefully.

The event itself does **not** prove that a brand-new MFA prompt occurred
during that sign-in.

Instead, Entra reported:

``` text
MFA requirement satisfied by claim in the token
```

This is consistent with the earlier MFA investigation, where an existing
authentication/token state could satisfy an MFA requirement without
necessarily producing a new MFA challenge.

Therefore, this lab documents the observation rather than claiming a new
MFA challenge occurred.

------------------------------------------------------------------------

# 13. Failed vs Successful Authentication Comparison

The investigation produced the following evidence-based comparison:

  -------------------------------------------------------------------------------------
  Investigation item      Device Code attempt            Interactive/WAM attempt
  ----------------------- ------------------------------ ------------------------------
  Application             Microsoft Azure PowerShell     Microsoft Azure PowerShell

  User                    `oscorp.iam.labs`   `oscorp.iam.labs`

  Result                  Failure                        Success

  Event time              `02:00:17Z`                    `02:10:03Z`

  Error code              `530035`                       None reported

  Failure reason          Access blocked by Security     ---
                          Defaults                       

  Security Defaults       Request blocked                Authentication succeeded

  Azure PowerShell        Not established                Established
  context                                                

  Subscription            Not reached successfully       Azure subscription 1

  `Get-AzContext`         Not usable for this attempt    Valid OsCorp context

  MFA information         No authentication event        MFA requirement satisfied by
                          available                      claim in token
  -------------------------------------------------------------------------------------

This comparison demonstrates why authentication troubleshooting must
distinguish between the **authentication mechanism** and the
**authorization environment**.

------------------------------------------------------------------------

# 14. Root Cause

The root cause identified in this lab was:

> **The Microsoft Azure PowerShell device-code authentication request
> was blocked by Microsoft Entra Security Defaults, producing error
> `AADSTS530035`.**

The evidence does not support attributing the failure to:

-   an incorrect password
-   a missing Azure subscription
-   an incorrect Azure subscription ID
-   insufficient Azure resource authorization
-   disabling or misconfiguration of MFA

The failed sign-in event explicitly identified Security Defaults as the
blocking control.

------------------------------------------------------------------------

# 15. Remediation

The remediation was:

1.  Keep Security Defaults enabled.
2.  Do not weaken the tenant security baseline.
3.  Enable WAM for Azure PowerShell.
4.  Authenticate interactively against the OsCorp tenant.
5.  Target the intended Azure subscription.
6.  Validate the resulting Azure context with `Get-AzContext`.
7.  Validate the subscription with `Get-AzSubscription`.
8.  Correlate the successful authentication with the Entra sign-in
    event.

The remediation successfully established the Azure PowerShell context.

------------------------------------------------------------------------

# 16. Least-Privilege Analysis

The lab account currently has broad Azure subscription permissions
because it is the subscription owner.

This is useful for initial environment setup, but **Owner should not be
treated as the default role for normal operational work**.

Azure RBAC and Microsoft Entra roles are separate authorization systems.

``` text
Microsoft Entra roles
        |
        +---- Directory permissions

Azure RBAC
        |
        +---- Azure resource permissions
```

An Azure subscription Owner role does not automatically make the account
a Microsoft Entra Global Administrator.

For future labs, permissions should be reduced where practical and
assigned at the smallest appropriate scope.

Examples of the least-privilege approach include:

-   Use the minimum Azure RBAC role required for a task.
-   Prefer resource-group or resource-level scope where appropriate.
-   Avoid Owner when Contributor or a narrower role is sufficient.
-   Use a read-only Entra role when only sign-in investigation is
    required.
-   Do not grant Global Administrator merely to simplify a lab.
-   Separate administration from day-to-day user identities where
    possible.

------------------------------------------------------------------------

# 17. Security Risks to Avoid

## 17.1 Do Not Disable Security Defaults

The correct response to a blocked authentication flow is not
automatically to disable the security control.

Security Defaults provide baseline protection for the tenant.

In this investigation, the security control correctly prevented the
attempted authentication flow.

------------------------------------------------------------------------

## 17.2 Do Not Grant Global Administrator

Global Administrator is unnecessary for this authentication
troubleshooting exercise.

Granting it would violate least-privilege principles and would make the
lab less representative of a secure production environment.

------------------------------------------------------------------------

## 17.3 Do Not Change the Subscription Directory

The Azure subscription is associated with the OsCorp Entra tenant.

Changing the subscription's directory would be an unnecessary and
potentially disruptive remediation.

------------------------------------------------------------------------

## 17.4 Do Not Publish Authentication Codes

Device authentication codes are temporary authentication secrets.

They should never be committed to GitHub.

Screenshots containing authentication codes should be deleted, cropped,
or redacted before publication.

------------------------------------------------------------------------

## 17.5 Protect Diagnostic Information

Public screenshots should avoid exposing unnecessary:

-   IP addresses
-   Request IDs
-   Correlation IDs
-   Session IDs
-   User IDs
-   Tenant IDs
-   Subscription IDs
-   Tokens
-   Passwords
-   Secrets

Diagnostic information can be useful during an investigation but does
not necessarily need to be publicly exposed.

------------------------------------------------------------------------

## 17.6 Do Not Use Legacy AzureAD

This lab deliberately uses:

-   Microsoft Entra admin center
-   Az PowerShell
-   Modern authentication

The legacy **AzureAD PowerShell module is not used**.

------------------------------------------------------------------------

# 18. Evidence Collected

The following evidence was captured during the investigation:

  ---------------------------------------------------------------------------------------
  Evidence                                            Purpose
  --------------------------------------------------- -----------------------------------
  `02-device-code-blocked-by-security-defaults.png`   Shows the failed Microsoft Azure
                                                      PowerShell sign-in and `530035`
                                                      Security Defaults block

  `02-az-powershell-authenticated-context.png`        Shows the successfully established
                                                      Azure PowerShell context

  `03-successful-azure-powershell-sign-in.png`        Shows the successful Microsoft
                                                      Azure PowerShell Entra sign-in
  ---------------------------------------------------------------------------------------

The evidence is intentionally based on **actual observed results**,
rather than expected or simulated results.

------------------------------------------------------------------------

# 19. Investigation Timeline

``` text
Initial Azure PowerShell authentication
        |
        v
SharedTokenCacheCredential authentication failed
        |
        v
Device Code authentication attempted
        |
        v
Microsoft Entra sign-in event
2026-09-08T02:00:17Z
        |
        v
AADSTS530035
        |
        v
Access blocked by Security Defaults
        |
        v
Security control investigated
        |
        v
Security Defaults NOT disabled
        |
        v
WAM enabled
        |
        v
Interactive Azure PowerShell authentication
        |
        v
Successful Entra sign-in
2026-09-08T02:10:03Z
        |
        v
Get-AzContext
        |
        v
OsCorp tenant + Azure subscription validated
```

------------------------------------------------------------------------

# 20. Key IAM Lessons

### Lesson 1 --- Authentication failures need evidence

Do not immediately change configuration because a login failed.

First determine:

``` text
Who?
What application?
When?
What authentication method?
What error?
What policy?
What resource?
```

------------------------------------------------------------------------

### Lesson 2 --- Security controls can be the reason authentication fails

A failed authentication does not necessarily mean the credentials are
wrong.

In this lab, Entra explicitly identified Security Defaults as the
blocking control.

------------------------------------------------------------------------

### Lesson 3 --- Do not weaken security to solve a tooling problem

The device-code flow was blocked.

The correct response was to use a supported authentication approach
rather than disable Security Defaults.

------------------------------------------------------------------------

### Lesson 4 --- Authentication and authorization are different

The successful Azure PowerShell login established authentication and an
Azure subscription context.

That does not mean the account automatically has permission to perform
every possible Entra or Azure operation.

------------------------------------------------------------------------

### Lesson 5 --- Token state matters

The successful sign-in event reported:

``` text
MFA requirement satisfied by claim in the token
```

Therefore, the absence of a new MFA prompt should not automatically be
interpreted as MFA being broken or bypassed.

------------------------------------------------------------------------

### Lesson 6 --- Troubleshooting should preserve security posture

A good remediation should solve the operational problem **without
reducing the tenant's security baseline**.

------------------------------------------------------------------------

# 21. Lab Outcome

This lab successfully demonstrated an evidence-based authentication
troubleshooting workflow.

The investigation established that:

-   Azure PowerShell was initially unable to use the local shared token
    cache.
-   Device Code authentication was attempted.
-   Microsoft Entra blocked the device-code request with `AADSTS530035`.
-   The Entra sign-in event explicitly identified Security Defaults as
    the blocking control.
-   Security Defaults were not disabled.
-   WAM was enabled for Az PowerShell.
-   Interactive Azure PowerShell authentication succeeded.
-   `Get-AzContext` confirmed the OsCorp tenant and Azure subscription.
-   `Get-AzSubscription` confirmed the subscription was enabled.
-   The successful Entra event showed the MFA requirement was satisfied
    by a claim in the token.
-   The investigation distinguished authentication, security policy, and
    Azure authorization concerns.

The main architectural lesson is:

> **Troubleshoot the authentication flow before changing authorization
> or weakening security controls.**

------------------------------------------------------------------------

# 22. Related Documentation

-   [Day 02 --- Authentication](../README.md)
-   [Lab 01 --- Authentication
    Fundamentals](../01-authentication-fundamentals/README.md)
-   [Lab 02 --- Multifactor Authentication](../02-mfa/README.md)
-   [Day 01 --- IAM
    Fundamentals](../../Day-01-IAM-Fundamentals/README.md)
-   [OsCorp Environment Setup](../../00-Environment-Setup/README.md)

------------------------------------------------------------------------

# 23. Next Lab

The next stage of the IAM journey will build on the authentication
foundation established in Day 02 and move into deeper identity and
access control scenarios.

The focus will remain on:

-   Evidence-based implementation
-   Least privilege
-   Modern Microsoft Entra administration
-   Azure RBAC
-   Authentication and authorization boundaries
-   Security-focused troubleshooting
-   Reproducible lab documentation
