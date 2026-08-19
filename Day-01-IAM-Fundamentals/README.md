# Day 01 — IAM Fundamentals 🔐

## Overview

Today marks the beginning of my Identity and Access Management (IAM) learning journey.

The goal of this journey is to build a practical understanding of IAM, moving from fundamental concepts into hands-on labs, troubleshooting scenarios, automation, and real-world identity security use cases.

The practical work will be performed in a fictional organisation called **OsCorp**, using Microsoft Entra ID and, later in the journey, additional Microsoft and identity technologies.

---

## What I Learned

### 1. Identity

An identity represents a person, application, device, service, or other entity that may request access to a resource.

The first question IAM needs to answer is:

**Who or what is requesting access?**

Examples of identities include:

- Human users
- Applications
- Service accounts
- Devices
- Workloads
- Service principals
- Managed identities

---

### 2. Authentication

Authentication verifies the identity making the request.

Common authentication methods include:

- Passwords
- Multi-factor authentication (MFA)
- Certificates
- Biometrics
- Security keys
- Passwordless authentication

The key question is:

**Who are you?**

---

### 3. Authorization

Authorization determines what an authenticated identity is allowed to access or perform.

The key question is:

**What are you allowed to do?**

Authorization decisions can be based on factors such as:

- User identity
- Group membership
- Roles
- Resource permissions
- Policies
- Device state
- Location
- Risk

---

### 4. Access Control

Access control is how authorization decisions are enforced.

Common access-control mechanisms include:

- Roles
- Groups
- Permissions
- Policies
- Conditional Access
- Least privilege

The objective is to provide the access required to perform a task without unnecessarily increasing the identity's privileges.

---

## The IAM Flow

A simplified IAM flow can be represented as:

```text
Identity
   ↓
Authentication
   ↓
Authorization
   ↓
Access Control
   ↓
Resource
```

A useful way to remember it:

**Identify → Verify → Authorize → Control → Access**

## Key Takeaway

IAM is about ensuring that the **right identity gets the right access to the right resource under the right conditions**.

One important concept I took away from Day 1 is that IAM is not limited to human users.

Applications, services, devices, and other non-human identities also require identity, authentication, authorization, and appropriate access controls.

## What's Next?

Day 2 — Authentication Fundamentals

Authentication vs authorization
Password authentication
MFA
Passwordless authentication
Authentication factors
Identity providers
Authentication protocols

## Visual Notes

![Day 01 — IAM Fundamentals](./day-01-iam-fundamentals.png)

---

**Day 01 complete. 🔐**

Next: Authentication fundamentals
