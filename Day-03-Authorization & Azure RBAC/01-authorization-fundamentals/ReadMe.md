# Lab 01 — Authorization Fundamentals

## Objective

Understand and demonstrate the foundations of Azure authorization and Azure Role-Based Access Control (Azure RBAC).

This lab builds on the authentication work from Day 02 and answers:

> **What is an authenticated identity allowed to do?**

The lab will use the modern Microsoft Azure portal / Microsoft Entra admin center and the Az PowerShell module.

---

## Learning Objectives

By the end of this lab, I will be able to:

- Explain authentication vs authorization.
- Explain Azure RBAC.
- Identify a security principal.
- Identify a role definition.
- Identify a role assignment.
- Explain Azure RBAC scope.
- Explain inherited permissions.
- Distinguish Azure RBAC from Microsoft Entra directory roles.
- Apply least-privilege thinking to Azure access.

---

## Environment

| Item | Value |
|---|---|
| Organization | OsCorp |
| Azure subscription | Azure subscription 1 |
| Identity platform | Microsoft Entra ID |
| PowerShell | PowerShell 7.6.5 |
| Azure PowerShell | Az 16.3.0 |
| Legacy AzureAD module | Not used |

---

## 1. Authentication vs Authorization

Authentication answers:

> Who are you?

Authorization answers:

> What are you allowed to do?

```text
Identity
   ↓
Authentication
   ↓
Authenticated identity
   ↓
Authorization
   ↓
Role + Scope + Action
   ↓
Azure resource
```

A successful authentication does not automatically provide access to an Azure resource.

---

## 2. Azure RBAC Model

Azure RBAC can be understood as:

```text
Security Principal
        +
Role Definition
        +
Scope
        =
Role Assignment
```

Example:

```text
Peter Parker
      +
Reader
      +
Resource Group
      =
Read access within that scope
```

The actual result must be validated in the OsCorp environment.

---

## 3. Security Principals

Possible Azure RBAC security principals include:

- User
- Group
- Service principal
- Managed identity

OsCorp identities include:

| Identity | Department | Group |
|---|---|---|
| Peter Parker | Engineering | `SG-Engineering-Users` |
| Tony Stark | Technology | `SG-Technology-Users` |
| Natasha Romanoff | Security | `SG-Security-Users` |
| Steve Rogers | Operations | `SG-Operations-Users` |

---

## 4. Role Definitions

A role definition describes permitted Azure actions.

Common built-in roles include:

| Role | General purpose |
|---|---|
| Reader | View Azure resources |
| Contributor | Manage Azure resources without managing RBAC access |
| Owner | Full resource management including access management |

Do not assume the most powerful role is the correct role.

---

## 5. Scope

Azure RBAC can be assigned at different scopes:

```text
Management Group
      ↓
Subscription
      ↓
Resource Group
      ↓
Resource
```

Higher-level assignments can be inherited by resources below them.

The smallest practical scope should be preferred.

---

## 6. Practical Investigation

### Portal

Using the Azure portal:

1. Open **Subscriptions**.
2. Select **Azure subscription 1**.
3. Open **Access control (IAM)**.
4. Review **Role assignments**.
5. Inspect the assigned principal, role and scope.
6. Do not modify assignments yet unless the lab step explicitly requires it.

### PowerShell

Verify the current context:

```powershell
Get-AzContext
```

List role assignments:

```powershell
Get-AzRoleAssignment
```

Inspect role definitions:

```powershell
Get-AzRoleDefinition
```

Inspect a specific role:

```powershell
Get-AzRoleDefinition -Name "Reader"
```

Record the actual output.

---

## 7. Investigation Questions

Answer these from the actual environment:

1. Which identities currently have Azure RBAC assignments?
2. Which roles are assigned?
3. At what scope?
4. Are any permissions inherited?
5. Are any assignments broader than necessary?
6. Are any group-based assignments present?
7. Is the current Owner access being used only for setup?

---

## 8. Evidence

### Screenshot 01 — Azure RBAC baseline

**Filename:**

```text
01-azure-rbac-baseline.png
```

**Purpose:** Show the starting authorization state.

```markdown
![Azure RBAC baseline](./screenshots/01-azure-rbac-baseline.png)
```

### Screenshot 02 — Role assignment details

**Filename:**

```text
02-role-assignment-details.png
```

**Purpose:** Show a role, principal and scope.

```markdown
![Role assignment details](./screenshots/02-role-assignment-details.png)
```

### Screenshot 03 — PowerShell RBAC query

**Filename:**

```text
03-powershell-rbac-query.png
```

**Purpose:** Show actual Az PowerShell role-assignment output.

```markdown
![PowerShell RBAC query](./screenshots/03-powershell-rbac-query.png)
```

---

## 9. Evidence Log

| Evidence | Status | Notes |
|---|---|---|
| Azure RBAC baseline | ⬜ Pending | |
| Role assignment details | ⬜ Pending | |
| PowerShell RBAC query | ⬜ Pending | |

---

## 10. Security Risks

- Do not grant Owner by default.
- Do not grant Global Administrator to solve an Azure RBAC problem.
- Do not change the subscription directory as a troubleshooting shortcut.
- Do not ignore inherited access.
- Do not publish secrets or authentication information.
- Do not document expected output as actual evidence.

---

## 11. Troubleshooting

If role assignments cannot be viewed:

1. Verify the Azure PowerShell context.
2. Confirm the correct subscription is selected.
3. Check whether the account has sufficient Azure permissions to read role assignments.
4. Use the Azure portal to cross-check the result.
5. Record the actual error rather than assuming the cause.

---

## 12. Lessons Learned

**What did I learn?**

- 

**What surprised me?**

- 

**What security decision did I make?**

- 

**What would I do differently in production?**

- 

---

## 13. Outcome

**Status:** ⬜ Not yet completed

The lab is complete when the Azure RBAC model has been investigated and the evidence has been captured from the actual OsCorp environment.

---

## Related Documentation

- [Day 03 — Authorization & Azure RBAC](../README.md)
- [Day 02 — Authentication](../Day-02-Authentication/README.md)
- [Day 01 — IAM Fundamentals](../Day-01-IAM-Fundamentals/README.md)
- [OsCorp Environment Setup](../00-Environment-Setup/README.md)
