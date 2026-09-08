# Day 03 --- Authorization & Azure RBAC

## Overview

Day 03 moves from **authentication** to **authorization**.

Day 01 established the identity foundation:

``` text
Identity
   ↓
Users
   ↓
Groups
   ↓
Lifecycle
```

Day 02 established the authentication foundation:

``` text
Identity
   ↓
Authentication
   ↓
MFA
   ↓
Authentication troubleshooting
```

Day 03 answers the next IAM question:

> **What is this identity actually allowed to do?**

The focus is Azure authorization, Azure RBAC, scope, role assignments,
least privilege, authorization troubleshooting, and access auditing.

This day uses the **modern Microsoft Entra admin center** and the **Az
PowerShell module**. The legacy AzureAD PowerShell module is
deliberately not used.

------------------------------------------------------------------------

# 1. Learning Objectives

By the end of Day 03, I will be able to:

-   Explain authentication vs authorization.
-   Explain Azure Role-Based Access Control (Azure RBAC).
-   Explain the relationship between a security principal, role
    definition, and scope.
-   Distinguish Azure RBAC roles from Microsoft Entra directory roles.
-   Identify the difference between subscription, resource-group, and
    resource scope.
-   Read and interpret Azure role assignments.
-   Assign built-in Azure RBAC roles using the least privilege required.
-   Validate effective authorization.
-   Troubleshoot an authenticated user who cannot access an Azure
    resource.
-   Identify excessive permissions.
-   Remove unnecessary role assignments.
-   Investigate Azure authorization changes through activity logs.
-   Use Az PowerShell for Azure resource-plane authorization tasks.
-   Document authorization evidence without inventing results.

------------------------------------------------------------------------

# 2. Authentication vs Authorization

Authentication answers:

> **Who are you?**

Authorization answers:

> **What are you allowed to do?**

The relationship can be represented as:

``` text
                    Authentication
                          |
                          v
                    "Who are you?"
                          |
                          v
                    Authenticated
                       identity
                          |
                          v
                    Authorization
                          |
                          v
                  "What can you do?"
                          |
                          v
                 Role + Scope + Action
                          |
                          v
                    Azure resource
```

A successful sign-in does not automatically grant access to Azure
resources.

Likewise, having an Azure role assignment does not replace
authentication.

Both controls are required.

------------------------------------------------------------------------

# 3. Azure RBAC

Azure Role-Based Access Control provides authorization to Azure
resources.

The core RBAC model can be represented as:

``` text
Security Principal
       +
Role Definition
       +
Scope
       =
Role Assignment
```

For example:

``` text
Peter Parker
      +
Reader
      +
Resource Group
      =
Peter can read resources within that scope
```

The exact permissions depend on the role definition and the resource
scope.

------------------------------------------------------------------------

# 4. Azure RBAC Components

## 4.1 Security Principal

A security principal is the identity receiving permissions.

Examples include:

-   User
-   Group
-   Service principal
-   Managed identity

In this lab environment, fictional OsCorp users and groups will be used.

Existing identities include:

  Identity           Department    Primary Group
  ------------------ ------------- ------------------------
  Peter Parker       Engineering   `SG-Engineering-Users`
  Tony Stark         Technology    `SG-Technology-Users`
  Natasha Romanoff   Security      `SG-Security-Users`
  Steve Rogers       Operations    `SG-Operations-Users`

------------------------------------------------------------------------

## 4.2 Role Definition

A role definition describes what actions an identity is allowed to
perform.

Examples of built-in Azure roles include:

  -----------------------------------------------------------------------
  Role                                General purpose
  ----------------------------------- -----------------------------------
  Owner                               Full Azure resource management
                                      access, including ability to manage
                                      access

  Contributor                         Manage Azure resources but does not
                                      include the ability to manage
                                      access through Azure RBAC

  Reader                              View Azure resources without making
                                      changes
  -----------------------------------------------------------------------

The actual permissions of a role should always be evaluated before
assigning it.

> **Least-privilege principle:** Select the narrowest role that provides
> the required actions.

------------------------------------------------------------------------

## 4.3 Scope

Scope determines **where** the permission applies.

Azure RBAC scopes can include:

``` text
Management Group
      ↓
Subscription
      ↓
Resource Group
      ↓
Resource
```

A role assigned at a higher scope can apply to resources beneath that
scope.

Therefore:

``` text
Subscription scope
```

is generally broader than:

``` text
Resource Group scope
```

which is broader than:

``` text
Individual resource scope
```

when considering inherited Azure RBAC access.

------------------------------------------------------------------------

# 5. Azure RBAC vs Microsoft Entra Roles

These are separate authorization systems.

``` text
Microsoft Entra ID
        |
        +---- Microsoft Entra directory roles
        |
        +---- Directory administration
```

and:

``` text
Azure
        |
        +---- Azure RBAC
        |
        +---- Azure resource authorization
```

Examples:

  Authorization system    Controls
  ----------------------- ---------------------------------------
  Microsoft Entra roles   Directory and identity administration
  Azure RBAC              Azure resource access

An Azure subscription Owner assignment does **not** automatically make
the identity a Microsoft Entra Global Administrator.

This distinction is fundamental to IAM architecture.

------------------------------------------------------------------------

# 6. Current OsCorp Authorization Baseline

The Azure subscription used for the lab is:

``` text
Azure subscription 1
```

The subscription is associated with the OsCorp Microsoft Entra tenant.

The lab administration account currently has broad subscription-level
access because it is the subscription Owner.

This is intentionally treated as an **environment setup condition**, not
the desired steady-state permission model.

The objective of Day 03 is to demonstrate how access can be designed and
validated using narrower permissions.

------------------------------------------------------------------------

# 7. Least Privilege

Least privilege means granting an identity only the permissions required
to perform its intended task.

A useful decision process is:

``` text
What task must the identity perform?
                |
                v
What actions are required?
                |
                v
Which role contains those actions?
                |
                v
What is the smallest appropriate scope?
                |
                v
Assign the role
                |
                v
Validate access
                |
                v
Remove access when no longer required
```

Avoid starting with:

> "Which powerful role can I give this user?"

Instead ask:

> "What exact operation does this user need to perform?"

------------------------------------------------------------------------

# 8. Privilege Hierarchy

For this lab, privilege should be evaluated from both **role breadth**
and **scope breadth**.

For example:

``` text
Broad role + broad scope
        ↓
Highest exposure

Narrower role + broad scope
        ↓
Reduced role privilege

Broad role + narrow scope
        ↓
Reduced blast radius

Narrow role + narrow scope
        ↓
Preferred least-privilege design
```

The best design is not automatically the role with the fewest
permissions.

It is the combination of:

``` text
Minimum required actions
+
Minimum required scope
```

------------------------------------------------------------------------

# 9. Day 03 Lab Structure

Day 03 will contain the following practical labs:

``` text
Day-03-Authorization-RBAC/
│
├── README.md
│
├── 01-authorization-fundamentals/
│   ├── README.md
│   └── screenshots/
│
├── 02-rbac-role-assignments/
│   ├── README.md
│   └── screenshots/
│
├── 03-least-privilege/
│   ├── README.md
│   └── screenshots/
│
└── 04-rbac-troubleshooting/
    ├── README.md
    └── screenshots/
```

Each lab should contain:

-   Objective
-   Architecture
-   Prerequisites
-   Step-by-step implementation
-   PowerShell commands where appropriate
-   Validation
-   Evidence
-   Security considerations
-   Troubleshooting
-   Lessons learned
-   Outcome
-   Related documentation

Actual results will be recorded only after the corresponding
configuration is performed.

------------------------------------------------------------------------

# 10. Lab 01 --- Authorization Fundamentals

## Objective

Establish the conceptual and practical foundation for Azure
authorization.

Topics include:

-   Authentication vs authorization
-   Azure RBAC
-   Role definitions
-   Role assignments
-   Scope
-   Inheritance
-   Azure RBAC vs Microsoft Entra roles
-   Least privilege

The lab will use the Azure portal and Az PowerShell to inspect the
existing authorization environment.

### Key questions

Before assigning anything, answer:

1.  Who is requesting access?
2.  What Azure resource is being accessed?
3.  What operation is required?
4.  Which role provides that operation?
5.  At what scope should the role be assigned?
6.  Is the permission inherited?
7.  Is the permission broader than necessary?

------------------------------------------------------------------------

# 11. Lab 02 --- RBAC Role Assignments

## Objective

Create and investigate Azure RBAC role assignments using an
evidence-first approach.

The lab will demonstrate the relationship:

``` text
Principal
   ↓
Role
   ↓
Scope
   ↓
Access
```

A suitable OsCorp identity or group will be selected for an Azure
resource access scenario.

The role assignment will be created only after determining:

-   Required operation
-   Required role
-   Required scope

### Az PowerShell

The modern Az module can be used to inspect role assignments.

Example:

``` powershell
Get-AzRoleAssignment
```

For a specific principal:

``` powershell
Get-AzRoleAssignment -SignInName "<USER-UPN>"
```

For a specific scope:

``` powershell
Get-AzRoleAssignment -Scope "<RESOURCE-SCOPE>"
```

The exact command used during the lab will be recorded with the observed
result.

### Security requirement

Do not assign Owner simply because it is convenient.

The role must be justified by the actual task.

------------------------------------------------------------------------

# 12. Lab 03 --- Least-Privilege Access

## Objective

Demonstrate why broad access should be reduced when a narrower role is
sufficient.

The exercise will examine:

``` text
Existing privilege
        ↓
Required task
        ↓
Required permissions
        ↓
Narrower role
        ↓
Narrower scope
        ↓
Validation
```

### Example decision model

  -----------------------------------------------------------------------
  Requirement             Candidate role          Scope
  ----------------------- ----------------------- -----------------------
  View resources          Reader                  Resource group

  Manage resources        Contributor             Resource group

  Manage role assignments Appropriate             Smallest required scope
                          access-management role  

  Full administrative     Owner                   Only when genuinely
  control                                         required
  -----------------------------------------------------------------------

These are candidate patterns, not automatic assignments.

The final role will be selected based on the actual task performed in
the lab.

------------------------------------------------------------------------

# 13. Lab 04 --- RBAC Troubleshooting

## Objective

Demonstrate an authorization failure where authentication succeeds but
the user lacks the required Azure permissions.

The investigation will follow:

``` text
User authenticates
        ↓
Authentication succeeds
        ↓
User accesses Azure resource
        ↓
Authorization evaluation
        ↓
Access denied / insufficient permission
        ↓
Investigate role assignments
        ↓
Investigate scope
        ↓
Investigate inheritance
        ↓
Determine missing permission
        ↓
Apply least-privilege remediation
        ↓
Validate access
```

The important lesson is:

> **A successful authentication does not prove authorization.**

------------------------------------------------------------------------

# 14. Azure RBAC PowerShell Investigation

The Az PowerShell module is used for Azure resource-plane authorization
tasks.

## List role assignments

``` powershell
Get-AzRoleAssignment
```

## Filter by user

``` powershell
Get-AzRoleAssignment -SignInName "<USER-UPN>"
```

## Filter by role

``` powershell
Get-AzRoleAssignment -RoleDefinitionName "Reader"
```

## Inspect a role assignment

``` powershell
Get-AzRoleAssignment -SignInName "<USER-UPN>" |
    Format-List *
```

## Inspect available role definitions

``` powershell
Get-AzRoleDefinition
```

For a specific role:

``` powershell
Get-AzRoleDefinition -Name "Reader"
```

These commands are investigation examples. They should be run against
the actual OsCorp environment and the resulting output should be
documented.

------------------------------------------------------------------------

# 15. Role Assignment Lifecycle

Authorization should be treated as a lifecycle, not a one-time
configuration.

``` text
Request
  ↓
Approval
  ↓
Assignment
  ↓
Validation
  ↓
Monitoring
  ↓
Review
  ↓
Removal
```

This aligns with the identity lifecycle work completed on Day 01.

A user moving departments may need different Azure access.

A leaver should not retain unnecessary Azure permissions.

------------------------------------------------------------------------

# 16. Group-Based Authorization

Groups can simplify authorization management.

Instead of assigning the same Azure role individually:

``` text
Peter Parker ─┐
Tony Stark   ─┼──> Security Group ──> Azure RBAC role
Steve Rogers ─┘
```

This can provide:

-   Consistent access
-   Easier onboarding
-   Easier offboarding
-   Reduced administrative effort
-   Better auditability

However, group-based access must still be governed.

A group with a powerful Azure RBAC role can become a significant
privilege concentration point.

Therefore:

> **Group-based access improves administration, but does not remove the
> need for least privilege.**

------------------------------------------------------------------------

# 17. Inheritance

Azure RBAC permissions can be inherited from higher scopes.

For example:

``` text
Subscription
    |
    +---- Resource Group A
    |        |
    |        +---- Resource 1
    |        +---- Resource 2
    |
    +---- Resource Group B
             |
             +---- Resource 3
```

A role assignment at subscription scope can affect resources beneath the
subscription.

A role assignment at Resource Group A can affect resources beneath
Resource Group A.

Therefore, when troubleshooting access, always inspect:

``` text
Direct assignment
        +
Inherited assignment
        +
Group membership
        +
Scope
```

Do not conclude that a user has no access merely because there is no
direct user assignment.

------------------------------------------------------------------------

# 18. Authorization Troubleshooting Checklist

When a user reports:

> "I can sign in, but I can't access the resource."

Use this sequence.

### Identity

-   Is the correct user being investigated?
-   Is the account enabled?
-   Is the user signing in with the expected identity?

### Authentication

-   Did authentication succeed?
-   Is the correct tenant being used?

### Resource

-   Which subscription?
-   Which resource group?
-   Which resource?

### Authorization

-   Does the user have a role assignment?
-   Is the role appropriate?
-   Is the assignment inherited?
-   Is the user a member of a group with the role?
-   Is the role assigned at the correct scope?

### Security

-   Is the role broader than required?
-   Can the role be reduced?
-   Can the scope be reduced?
-   Is there unnecessary persistent privilege?

------------------------------------------------------------------------

# 19. Azure Activity Logs

Authorization changes should be auditable.

Azure Activity Log can help investigate management-plane operations such
as role assignment changes.

The investigation should answer:

``` text
Who made the change?
        ↓
What changed?
        ↓
When?
        ↓
At what scope?
        ↓
What was the previous state?
        ↓
What is the resulting access?
```

For public GitHub evidence, avoid publishing unnecessary identifiers or
sensitive diagnostic information.

------------------------------------------------------------------------

# 20. Security Risks to Avoid

## 20.1 Do Not Use Owner by Default

Owner is highly privileged.

It includes broad resource-management permissions and the ability to
manage access.

Use it only where the task genuinely requires that level of control.

------------------------------------------------------------------------

## 20.2 Do Not Grant Global Administrator for Azure RBAC

Microsoft Entra Global Administrator is not a substitute for Azure RBAC.

Do not grant it simply because a user needs access to an Azure resource.

------------------------------------------------------------------------

## 20.3 Do Not Change Subscription Directory to Fix Authorization

The subscription's Microsoft Entra directory association is an important
architectural boundary.

Do not change it as a troubleshooting shortcut.

------------------------------------------------------------------------

## 20.4 Do Not Assume Authentication Means Authorization

A successful sign-in only establishes identity authentication.

The Azure resource still evaluates authorization.

------------------------------------------------------------------------

## 20.5 Do Not Ignore Inherited Permissions

A user may receive access from:

-   Subscription-level assignment
-   Resource-group assignment
-   Group membership
-   Other inherited assignments

Always investigate the complete access path.

------------------------------------------------------------------------

## 20.6 Do Not Leave Temporary Privilege in Place

Temporary administrative access should be removed after the task is
complete where appropriate.

Unused role assignments increase the attack surface.

------------------------------------------------------------------------

## 20.7 Do Not Publish Sensitive Evidence

Screenshots should not expose:

-   Passwords
-   Tokens
-   Secrets
-   Private keys
-   Authentication codes
-   Unnecessary IP addresses
-   Session IDs
-   Request IDs
-   Correlation IDs
-   Other sensitive diagnostic information

Capture evidence for the technical lesson, then sanitize it before
public publication.

------------------------------------------------------------------------

# 21. Evidence-First Lab Method

Every Day 03 lab should follow this process:

``` text
Plan
  ↓
Configure
  ↓
Validate
  ↓
Investigate
  ↓
Capture evidence
  ↓
Document actual result
  ↓
Review security implications
```

Do not write:

``` text
Expected result = actual result
```

unless the result was actually observed.

Use language such as:

> "The command returned..."

> "The portal displayed..."

> "The sign-in event showed..."

> "The role assignment was observed at..."

This keeps the repository technically credible.

------------------------------------------------------------------------

# 22. Evidence Naming Convention

Screenshots should use descriptive names.

Recommended structure:

``` text
screenshots/
├── 01-authorization-baseline.png
├── 02-role-assignment.png
├── 03-role-assignment-validation.png
├── 04-least-privilege-state.png
└── 05-rbac-troubleshooting.png
```

Use the actual filename that corresponds to the evidence captured.

Embedded screenshots should use relative paths:

``` markdown
![Azure RBAC role assignment](./screenshots/02-role-assignment.png)
```

------------------------------------------------------------------------

# 23. Documentation Standards

Each lab should document:

### Configuration

What was configured?

### Reason

Why was it configured?

### Security decision

Why was this role and scope selected?

### Validation

How was the configuration tested?

### Evidence

What screenshot or command output proves the result?

### Troubleshooting

What problem occurred and how was it investigated?

### Security risks

What should an administrator avoid?

### Outcome

What was actually achieved?

This approach keeps the repository useful as both a learning guide and a
technical portfolio.

------------------------------------------------------------------------

# 24. Day 03 Architecture Model

The complete IAM model developed so far is:

``` text
                         OsCorp IAM
                             |
              +--------------+--------------+
              |                             |
          Identity                     Authentication
              |                             |
        Users / Groups                   Password
              |                           MFA
        Lifecycle                     Security Defaults
              |                             |
              +-------------+---------------+
                            |
                            v
                       Authorization
                            |
                    Azure RBAC / Roles
                            |
                          Scope
                            |
                            v
                     Azure Resources
```

The key control boundaries are:

``` text
Identity
   ↓
Authentication
   ↓
Authorization
   ↓
Resource Access
```

Each boundary must be independently understood and validated.

------------------------------------------------------------------------

# 25. Key IAM Questions

For every authorization request, ask:

1.  Who is requesting access?
2.  Is the identity trusted and authenticated?
3.  What resource is being accessed?
4.  What action is required?
5.  Which role provides that action?
6.  What is the smallest appropriate scope?
7.  Is access direct or inherited?
8.  Is access coming from group membership?
9.  Is the permission temporary or permanent?
10. Who approved the access?
11. How will the access be reviewed?
12. How will the access be removed?

These questions form the basis of practical IAM administration.

------------------------------------------------------------------------

# 26. Day 03 Security Principles

The following principles will guide all practical work:

### Least privilege

Grant only the permissions required.

### Least scope

Apply access at the smallest practical scope.

### Separation of duties

Do not combine unrelated administrative privileges unnecessarily.

### Evidence-based administration

Validate changes rather than assuming they worked.

### Strong authentication

Do not weaken authentication controls to simplify authorization.

### Auditable access

Authorization changes should be traceable.

### Lifecycle management

Access should change when responsibilities change and be removed when no
longer required.

------------------------------------------------------------------------

# 27. Expected Day 03 Outcome

By the end of Day 03, the OsCorp environment should demonstrate a clear
distinction between:

``` text
Who are you?
        ↓
Authentication

What are you allowed to do?
        ↓
Authorization

Where are you allowed to do it?
        ↓
Scope

How much access do you actually need?
        ↓
Least privilege
```

The practical goal is not to maximize permissions.

The goal is to create **controlled, explainable, auditable access**.

------------------------------------------------------------------------

# 28. Lessons to Capture During the Labs

At the completion of each practical lab, record:

-   What was configured?
-   Why was it configured?
-   What worked?
-   What failed?
-   What evidence proves the result?
-   What security decision was made?
-   What would be different in production?
-   What privilege could be reduced?
-   What troubleshooting technique was learned?

These observations will form the final Day 03 lessons learned.

------------------------------------------------------------------------

# 29. Day 03 Completion Checklist

## Conceptual

-   [ ] Authentication vs authorization understood
-   [ ] Azure RBAC understood
-   [ ] Role definitions understood
-   [ ] Role assignments understood
-   [ ] Scope and inheritance understood
-   [ ] Azure RBAC vs Entra roles understood
-   [ ] Least privilege understood

## Practical

-   [ ] Authorization baseline investigated
-   [ ] Azure RBAC role assignment investigated
-   [ ] Role assignment validated
-   [ ] Least-privilege scenario completed
-   [ ] Authorization failure investigated
-   [ ] RBAC access remediated
-   [ ] Activity log evidence investigated

## PowerShell

-   [ ] Az PowerShell verified
-   [ ] `Get-AzContext` verified
-   [ ] `Get-AzRoleAssignment` used
-   [ ] `Get-AzRoleDefinition` used
-   [ ] Role assignment scope investigated
-   [ ] Authorization results validated

## Security

-   [ ] Owner not used unnecessarily
-   [ ] Global Administrator not granted for Azure RBAC
-   [ ] Security Defaults not weakened
-   [ ] Inherited access considered
-   [ ] Temporary privilege removed where appropriate
-   [ ] Evidence sanitized before publication

## Documentation

-   [ ] Screenshots captured
-   [ ] Screenshot filenames descriptive
-   [ ] Markdown image paths validated
-   [ ] Actual results documented
-   [ ] No fabricated evidence
-   [ ] Security risks documented
-   [ ] Lessons learned documented

------------------------------------------------------------------------

# 30. Related Documentation

-   [Day 02 --- Authentication](../Day-02-Authentication/README.md)
-   [Lab 01 --- Authentication
    Fundamentals](../Day-02-Authentication/01-authentication-fundamentals/README.md)
-   [Lab 02 --- Multifactor
    Authentication](../Day-02-Authentication/02-mfa/README.md)
-   [Lab 03 --- Authentication
    Troubleshooting](../Day-02-Authentication/03-authentication-troubleshooting/README.md)
-   [Day 01 --- IAM Fundamentals](../Day-01-IAM-Fundamentals/README.md)
-   [OsCorp Environment Setup](../00-Environment-Setup/README.md)

------------------------------------------------------------------------

# 31. Day 03 Outcome

Day 03 establishes the authorization layer of the OsCorp IAM
environment.

The target architecture is:

``` text
                 IDENTITY
                    |
                    v
              AUTHENTICATION
                    |
                    v
             AUTHORIZATION
                    |
          +---------+---------+
          |                   |
       Azure RBAC        Entra Roles
          |                   |
          v                   v
    Azure Resources     Directory Resources
```

The central principle is:

> **Authenticate strongly, authorize narrowly, scope carefully, validate
> continuously, and remove access when it is no longer required.**

Practical results will be added to the individual Day 03 labs as they
are performed.

