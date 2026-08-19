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

This becomes increasingly important as organisations adopt cloud services, automation, APIs, and machine-to-machine communication.

## OsCorp Lab Environment

The practical exercises in this journey will be performed in a fictional organisation called **OsCorp**.

The environment will progressively evolve from a basic identity directory into a more realistic enterprise IAM environment.

The lab will eventually include:

Users
Security groups
Applications
Service identities
Access policies
Roles
Privileged identities
Authentication controls
Identity governance
Hybrid identity
Automation
IAM troubleshooting scenarios

## Fictional Lab Identities

The identities used throughout this laboratory are fictional.

Names are inspired by characters and organisations from the Marvel / MCU universe and are used purely for educational and demonstration purposes.

No real individuals are represented by these accounts.

All accounts, applications, resources, and data created within the environment are intended for laboratory use only.

Naming Standards

To keep the OsCorp environment consistent and scalable, naming conventions were established before creating identity objects.

## Users

User Principal Names (UPNs) follow:

firstname.lastname

Examples:

- `peter.parker`
- `tony.stark`
- `steve.rogers`
- `natasha.romanoff`
  
## Security Groups

Security groups use the following naming convention:

SG-<Department>-Users

Where:

## SG = Security Group

Examples:

SG-Finance-Users
SG-IT-Users
SG-HR-Users
SG-Sales-Users

The SG prefix is a naming convention used by this laboratory. It does not represent an Entra group type, scope, or permission level.

As the journey progresses into traditional Active Directory, additional group concepts such as Global, Domain Local, and Universal groups will be introduced separately.

## Lab Resources

Lab resources will use descriptive names that identify their purpose and environment.

Naming conventions may evolve as the OsCorp environment becomes more complex and additional technologies are introduced.

## Practical Labs

The practical exercises are designed to demonstrate how the concepts above are implemented and operated in an identity environment.

Each lab will document, where applicable:

Objective
Scenario
Requirements
Design
Implementation
Validation
Evidence
Audit / logging
Troubleshooting
Remediation
Lessons learned

The objective is not simply to configure a service, but to understand why it is configured, how to validate it, and how to troubleshoot it when something goes wrong.

## Day 01 Labs

## Lab 01 — User Provisioning

Provision fictional OsCorp identities in Microsoft Entra ID using the established naming standards.

Topics include:

User creation
Identity attributes
Account status
Naming standards
User validation
Audit evidence

## Lab 02 — Group-Based Access

Use security groups to organise identities and build the foundation for group-based access control.

Topics include:

Security groups
Group membership
Access management
Authorization
Least privilege

## Lab 03 — Authentication

Demonstrate authentication using an OsCorp test identity.

Topics include:

User sign-in
Authentication
Authentication evidence
Authentication failures
Troubleshooting

## Visual Notes

![Day 01 — IAM Fundamentals](./day-01-iam-fundamentals.png)

## What's Next?

Day 2 — Authentication Fundamentals

Authentication vs authorization
Password authentication
MFA
Passwordless authentication
Authentication factors
Identity providers
Authentication protocols

---

**Day 01 complete. 🔐**

Next: Authentication fundamentals
