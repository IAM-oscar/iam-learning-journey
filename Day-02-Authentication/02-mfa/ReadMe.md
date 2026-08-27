# Lab 02 — Multifactor Authentication (MFA) 🔐

## Lab Status

**Status:** 🔄 In Progress

**Platform:** Microsoft Entra ID

**Organisation:** OsCorp

**Environment:** Azure / Microsoft Entra ID Free

---

## 1. Objective

The objective of this lab is to develop a practical understanding of Multifactor Authentication (MFA) and how additional authentication factors can strengthen the security of user identities.

The lab will use the fictional **OsCorp** environment created during the previous IAM labs.

The practical exercise will demonstrate:

- MFA registration
- Authentication methods
- MFA authentication
- MFA-related sign-in activity
- Authentication details
- Sign-in log investigation
- MFA troubleshooting
- Security evidence and auditability

---

## 2. What is Multifactor Authentication?

Multifactor Authentication (MFA) requires an identity to provide additional verification beyond the primary authentication factor.

A simplified authentication flow is:

```text
Username
   ↓
Password
   ↓
Additional Authentication Factor
   ↓
Authentication Result
   ↓
Application / Resource

```
## 3. Authentication Factors

Authentication factors can be grouped into three common categories:

Factor	Examples
Something you know	Password, PIN
Something you have	Authenticator app, security key
Something you are	Fingerprint, facial recognition

MFA combines multiple authentication factors or authentication methods to increase confidence that the person requesting access is the legitimate identity owner.

## 4. MFA in the OsCorp Environment

The OsCorp environment uses Microsoft Entra ID as its identity provider.

The tenant has Microsoft Entra security defaults enabled.

Security defaults provide baseline identity protection and can require users to register for MFA and use MFA during authentication.

The lab will therefore work with the existing security configuration rather than weakening the tenant security posture.

## 5. Identity Used in This Lab

The primary identity used for the MFA exercise is:

User	Department	Purpose
Peter Parker	Engineering	MFA registration and authentication testing

Peter Parker was previously used in Lab 01 to demonstrate a successful authentication.

This allows the MFA exercise to build on the authentication evidence already captured.

## 6. MFA Registration

Before an identity can complete an MFA challenge using a particular authentication method, the required method must be registered.

The registration process establishes an authentication method associated with the user's identity.

The practical exercise will investigate the authentication methods available to Peter Parker.

The administrator will review the user's authentication configuration in Microsoft Entra ID.

## 7. Authentication Methods

Authentication methods provide different mechanisms for verifying an identity.

Examples include:

Password
Microsoft Authenticator
Passkeys
FIDO2 security keys
Certificate-based authentication
Temporary Access Pass
Other supported authentication methods

The available methods depend on the Microsoft Entra tenant configuration and licensing.

The lab will document the authentication methods actually available in the OsCorp environment rather than assuming that every method is available.

## 8. Practical Exercise — MFA Registration

The MFA exercise will use Peter Parker as the test identity.

Objective

Demonstrate that:

Peter Parker has an active Microsoft Entra identity.
MFA is required by the tenant's security baseline.
Peter Parker can register an authentication method.
The authentication method is associated with the identity.
Peter Parker can complete an MFA-protected authentication.
Microsoft Entra records the authentication activity.

## 9. Implementation

The administrator will review Peter Parker's authentication configuration.

The process will be:

Locate Peter Parker in Microsoft Entra ID.
Open the user's authentication methods.
Review the currently registered authentication methods.
Identify the method used for MFA.
Complete MFA registration if required.
Authenticate as Peter Parker.
Complete the MFA challenge.
Confirm successful authentication.
Review the resulting sign-in activity.

## 10. MFA Authentication Flow

The expected authentication flow is:
```text
Peter Parker
      ↓
Username
      ↓
Password
      ↓
Microsoft Entra ID
      ↓
MFA Requirement
      ↓
Additional Authentication Method
      ↓
MFA Verification
      ↓
Authentication Successful
      ↓
Application / Resource
```
The actual authentication method and sign-in behaviour will be validated using Microsoft Entra authentication and sign-in information.

## 11. Authentication Method Validation

After MFA registration, the authentication configuration should be reviewed.

| Check | Expected Result |
|---|---|
| User exists | Yes |
| Account enabled | Yes |
| Authentication method registered | Yes |
| Authentication method associated with Peter Parker | Yes |
| MFA authentication performed | Yes |
| Authentication successful | Yes |
| Sign-in activity recorded | Yes |

## 12. MFA Sign-In Test

Peter Parker will be used to perform an MFA-protected authentication.

The test will demonstrate:

Password Authentication
        ↓
MFA Challenge
        ↓
MFA Verification
        ↓
Successful Authentication

The successful authentication should result in an authenticated Microsoft session.

The authentication result will then be correlated with Microsoft Entra sign-in logs.

## 13. Sign-In Log Investigation

After the MFA authentication attempt, the administrator will review the associated sign-in event.

Navigate to:

Microsoft Entra ID → Monitoring & health → Sign-in logs

The sign-in event should be investigated for:

User
Date and time
Application
Authentication requirement
Authentication method
Authentication result
IP address
Location
Authentication details
Additional authentication information
Failure reason, if applicable

The objective is to understand what Microsoft Entra actually recorded rather than relying only on the user's experience during sign-in.

## 14. MFA Evidence Correlation

The MFA process should be validated across multiple sources of evidence.

The expected relationship is:
```text
User Identity
     ↓
Authentication Method
     ↓
MFA Challenge
     ↓
Successful Authentication
     ↓
Sign-In Event
     ↓
Authentication Details
```
This provides an auditable chain showing how the identity was authenticated.

## 15. MFA Troubleshooting Scenario
Scenario

Peter Parker reports that he is unable to complete MFA authentication.

The IAM administrator must investigate the issue.

Investigation Process

The troubleshooting process should follow:
```text
User reports MFA problem
          ↓
Identify the user
          ↓
Check account status
          ↓
Review authentication methods
          ↓
Review sign-in logs
          ↓
Review authentication details
          ↓
Identify failure reason
          ↓
Remediate
          ↓
Retest authentication
          ↓
Validate sign-in logs
```
The objective is to demonstrate a structured IAM troubleshooting process rather than simply resetting the user's authentication configuration.

## 16. Troubleshooting Considerations

When investigating an MFA issue, consider:

- Is the user account enabled?
- Is the user able to authenticate with the primary credential?
- Is an MFA method registered?
- Is the registered authentication method available?
- Was the MFA challenge presented?
- Did the MFA verification succeed?
- Did the sign-in fail before or after MFA?
- What does the sign-in log report?
- What does the authentication details section report?
- Is there evidence of an authentication policy or security baseline affecting the request?

The investigation should be based on observable evidence.

## 17. Evidence

Screenshots captured during the lab will demonstrate MFA configuration, authentication, validation, and troubleshooting activities.

Screenshot 01 — Peter Parker Authentication Methods

Filename: 01-peter-parker-auth-methods.png

Purpose: Demonstrate the authentication methods currently associated with Peter Parker.

Screenshot 02 — MFA Registration

Filename: 02-mfa-registration.png

Purpose: Demonstrate the MFA registration or authentication-method configuration process for Peter Parker.

Screenshot 03 — MFA Authentication Result

Filename: 03-mfa-authentication-result.png

Purpose: Demonstrate that Peter Parker successfully completed an MFA-protected authentication.

Screenshot 04 — MFA Sign-In Log

Filename: 04-mfa-sign-in-log.png

Purpose: Demonstrate that the MFA-related authentication activity was recorded in Microsoft Entra sign-in logs.

Screenshot 05 — MFA Authentication Details

Filename: 05-mfa-authentication-details.png

Purpose: Demonstrate the authentication details recorded by Microsoft Entra for the MFA-related sign-in.

Screenshot 06 — MFA Troubleshooting Evidence

Filename: 06-mfa-troubleshooting.png

Purpose: Demonstrate the investigation and validation of an MFA authentication issue.

## 18. Lessons Learned

This section will be completed after the practical exercise.

Key areas to reflect on:

Why MFA provides stronger protection than password-only authentication
How authentication factors differ
How authentication methods are registered
How Microsoft Entra evaluates MFA requirements
How MFA activity appears in sign-in logs
How authentication details can be used during investigations
How to troubleshoot MFA failures
Why authentication evidence is important for IAM operations
How security baselines affect authentication behaviour

## 19. Lab Outcome

The lab is complete when:

 - Peter Parker's authentication methods reviewed
 - MFA registration completed or validated
 - MFA authentication performed
 - Successful MFA authentication validated
 - Sign-in logs reviewed
 - Authentication details reviewed
 - MFA troubleshooting scenario investigated
 - Evidence screenshots captured
 - Lessons learned completed
 - README updated with final results
 
## Related Documentation
Day 02 — Authentication Fundamentals
Lab 01 — Authentication Fundamentals
Day 01 — IAM Fundamentals
Lab 01 — User Provisioning
Lab 02 — Group-Based Access Control
Lab 03 — Identity Lifecycle
OsCorp Environment Setup
