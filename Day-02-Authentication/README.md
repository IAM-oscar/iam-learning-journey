# Day 02 — Authentication Fundamentals 🔐

## Overview

Day 2 focuses on authentication and how Microsoft Entra ID verifies the identity of a user before allowing access to protected resources.

The goal is to move beyond the IAM fundamentals covered on Day 1 and develop a practical understanding of authentication methods, multifactor authentication, authentication protocols, and authentication troubleshooting.

The practical work will continue using the fictional **OsCorp** organisation and the Microsoft Entra ID tenant created during the environment setup.

---

## What I Will Learn

### 1. Authentication

Authentication is the process of verifying the identity of a user, application, device, or other entity.

The key question is:

**Who are you?**

Authentication occurs before authorization.

---

### 2. Authentication vs Authorization

Authentication verifies identity.

Authorization determines what an authenticated identity is allowed to access.

```text
Authentication
      ↓
Who are you?
      ↓
Authorization
      ↓
What are you allowed to access?
```

A successful authentication does not automatically mean that the user should have access to every resource.

### 3. Authentication Factors

Authentication factors can be grouped into categories such as:

- Something you know
- Something you have
- Something you are

Examples include:

| Factor | Examples |
|---|---|
| Something you know | Password, PIN |
| Something you have | Authenticator app, security key |
| Something you are | Fingerprint, facial recognition |

### 4. Password Authentication

Passwords are a traditional authentication mechanism.

The user provides a credential that is verified by the identity provider.

Although passwords remain widely used, passwords alone provide limited protection against attacks such as:

- Password spraying
- Credential stuffing
- Phishing
- Credential theft
- Reuse of compromised passwords

### 5. Multifactor Authentication

Multifactor authentication (MFA) requires additional verification beyond the primary authentication factor.

A simplified MFA flow is:
```text
Username
   ↓
Password
   ↓
Additional verification
   ↓
Authentication successful
```
Microsoft Entra ID supports MFA through authentication methods such as Microsoft Authenticator, passkeys, security keys, and other supported methods.

### 6. Passwordless Authentication

Passwordless authentication removes the traditional password from the authentication process.

Examples include:

- Passkeys
- FIDO2 security keys
- Microsoft Authenticator passwordless authentication
- Windows Hello for Business

Microsoft recommends phishing-resistant methods such as passkeys, FIDO2 security keys, Windows Hello for Business, and certificate-based authentication for stronger authentication scenarios.

### 7. Identity Providers

An Identity Provider (IdP) is responsible for establishing and verifying identities and issuing authentication information that applications can trust.

In this environment:
```text
User
  ↓
OsCorp Microsoft Entra ID
  ↓
Authentication
  ↓
Application / Resource
```
Microsoft Entra ID acts as the identity provider for the OsCorp environment.

### 8. Modern vs Legacy Authentication

Modern authentication uses contemporary identity protocols and supports stronger security mechanisms such as MFA.

Legacy authentication protocols may not support modern authentication controls and can create opportunities for attackers to bypass stronger authentication protections.

Security defaults in Microsoft Entra ID include protection against legacy authentication.

### Authentication Flow

A simplified authentication flow can be represented as:
```text
User
  ↓
Authentication Request
  ↓
Microsoft Entra ID
  ↓
Credential Verification
  ↓
Additional Authentication Factors
  ↓
Authentication Result
  ↓
Token / Session
  ↓
Application or Resource
```

###  OsCorp Authentication Environment

The practical labs will use the existing OsCorp environment.

### Existing Identities

| User | Department | Purpose |
|---|---|---|
| Peter Parker | Engineering | Authentication testing |
| Tony Stark | Technology | Authentication testing |
| Natasha Romanoff | Security | Authentication testing |
| Steve Rogers | Operations | Authentication testing |

### Security Baseline

The OsCorp tenant currently uses Microsoft Entra ID Free.

Because Entra ID Free does not provide Conditional Access, the authentication labs will use the security capabilities available within the Free tier.

Security defaults provide baseline protections including MFA registration and blocking legacy authentication.

### Practical Labs

### Lab 01 — Authentication Fundamentals

Topics:

- Authentication vs authorization
- Authentication factors
- Password authentication
- Identity provider
- Modern authentication

### Lab 02 — Multifactor Authentication

Topics:

- Microsoft Entra MFA
- Microsoft Authenticator
- MFA registration
- MFA sign-in
- MFA troubleshooting
- Authentication evidence
  
### Lab 03 — Authentication Troubleshooting

Topics:

- Sign-in investigation
- Authentication failures
- Authentication method review
- Sign-in logs
- Audit logs
- Troubleshooting methodology
  
### Key IAM Questions

Throughout Day 2, the following questions will be investigated:

### Who is requesting access?

### How is their identity verified?

### Which authentication factors are being used?

### Was authentication successful?

### Was MFA required?

### Which authentication method was used?

### Was the authentication request blocked?

### What evidence exists in the sign-in logs?

### Evidence

Screenshots will be captured throughout the practical labs to demonstrate:

- Authentication configuration
- MFA registration
- Authentication events
- Successful sign-ins
- Failed sign-ins
- Authentication troubleshooting
- Relevant Microsoft Entra logs

### Lessons Learned

This section will be completed after the practical labs.

Key areas to reflect on:

- Difference between authentication and authorization
- Strengths and weaknesses of authentication factors
- Importance of MFA
- Benefits of passwordless authentication
- Risks associated with legacy authentication
- How authentication events can be investigated
- How authentication failures can be troubleshot

### Day 02 Outcome

By the end of Day 2, I should be able to:

- Explain the authentication process
- Distinguish authentication from authorization
- Explain common authentication factors
- Explain MFA
- Understand passwordless authentication
- Identify Microsoft Entra ID as an identity provider
- Understand the difference between modern and legacy authentication
- Investigate authentication events
- Use Microsoft Entra sign-in logs for troubleshooting

Related Documentation
Day 01 — IAM Fundamentals
OsCorp Environment Setup
