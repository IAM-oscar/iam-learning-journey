# Lab 03 — Identity Lifecycle Management 🔄

## Lab Status

**Status:** 🔄 In Progress

**Platform:** Microsoft Entra ID

**Organisation:** OsCorp

**Environment:** Azure / Microsoft Entra ID Free

---

## 1. Objective

The objective of this lab is to demonstrate the identity lifecycle management process within Microsoft Entra ID.

The lab focuses on three common stages of an identity lifecycle:

```text
Joiner
   ↓
Mover
   ↓
Leaver
```
The goal is to understand how identity changes should be managed throughout an employee's lifecycle while maintaining appropriate access and applying the principle of least privilege.

## 2. Scenario

OsCorp needs a consistent process for managing identities as employees join the organisation, change roles or departments, and eventually leave the organisation.

**The IAM administrator is responsible for ensuring that:**

- New users receive the appropriate identity and access
- Users changing roles receive updated access
- Previous access is removed when no longer required
- Departing users are disabled appropriately
- Identity changes are auditable

The lifecycle model used in this lab is:

```text
Joiner
  ↓
Identity Provisioning
  ↓
Access Assignment
  ↓
Mover
  ↓
Access Review
  ↓
Leaver
  ↓
Access Removal / Account Disablement
```

## 3. Identity Lifecycle Model

**Joiner**

A Joiner is a new employee or identity entering the organisation.

**Typical IAM activities include:**

- Creating the identity
- Setting required attributes
- Assigning the appropriate department
- Adding the user to required security groups
- Validating account status
- Recording the provisioning activity

**Mover**

A Mover is an existing employee whose role, department, or responsibilities change.

**Typical IAM activities include:**

- Reviewing the user's existing access
- Updating identity attributes
- Removing access that is no longer required
- Adding access required for the new role
- Validating the resulting group membership
- Reviewing audit logs

**The key principle is:**

Access should follow the user's current business requirement, not their previous role.

**Leaver**

A Leaver is an employee who is leaving the organisation.

**Typical IAM activities include:**

- Disabling the user account
- Removing unnecessary access
- Preventing further authentication
- Reviewing the user's memberships
- Recording the administrative action
- Reviewing audit evidence

The objective is to reduce the risk of continued access after the user's employment or business relationship ends.

## 4. OsCorp Identity Used in This Lab

The lifecycle scenario will use the fictional OsCorp identity:

Peter Parker

Initial state:

|Attribute|	Value|
|User|	Peter Parker|
|Department|	Engineering|
|Security Group|	SG-Engineering-Users|
|Account Status|	Enabled|

Peter Parker will then progress through the lifecycle scenarios.

## 5. Joiner Scenario

**Scenario**

Peter Parker joins OsCorp as an Engineering employee.

The IAM administrator must provision the identity and provide the access required for the Engineering role.

**Expected Configuration**
```text
Peter Parker
      ↓
Department: Engineering
      ↓
SG-Engineering-Users
      ↓
Engineering Access
```
**Validation**

Verify:

- Peter Parker exists
- Account is enabled
- Department is Engineering
- User Principal Name is correct
- Peter belongs to SG-Engineering-Users
- No unnecessary departmental groups are assigned

The resulting configuration should follow the principle of least privilege.

## 6. Mover Scenario

Scenario

Peter Parker changes departments and moves from Engineering to Technology.

The IAM administrator must update the identity and adjust group membership accordingly.

Initial State
```text
Peter Parker
      ↓
Engineering
      ↓
SG-Engineering-Users
```
Required Change

Peter's department changes to:  Technology

His group membership must therefore be updated.

Expected Result
```text
Peter Parker
      ↓
Technology
      ↓
SG-Technology-Users
```
The previous Engineering group membership should be removed unless there is a documented business requirement for Peter to retain Engineering access.

**Mover Validation**

Verify:

- Department changed from Engineering to Technology
- SG-Engineering-Users membership removed
- SG-Technology-Users membership added
- Account remains enabled
- No unnecessary group memberships remain
- Audit logs record the relevant changes

## 7. Least Privilege During a Move

A department change should not automatically result in additional access without reviewing the user's existing permissions.

The IAM administrator should consider:
```text
Previous Role
     ↓
Review Existing Access
     ↓
Remove Unnecessary Access
     ↓
Apply New Role Access
     ↓
Validate
```
This helps prevent privilege accumulation, where users retain permissions from previous roles.

## 8. Leaver Scenario
Scenario

Peter Parker leaves OsCorp.

The IAM administrator must prevent the identity from continuing to authenticate and access organisational resources.

Expected Action

Disable Peter Parker's account.

The account should no longer be available for normal authentication.

Leaver Process
```text
Employee Leaves
       ↓
Identify User
       ↓
Disable Account
       ↓
Review Group Membership
       ↓
Remove Unnecessary Access
       ↓
Review Audit Logs
       ↓
Validate Account Status
```
Validation

Verify:

- Peter Parker's account is disabled
- The account can no longer be used for normal authentication
- Relevant group memberships have been reviewed
- Unnecessary access has been removed
- The administrative action appears in the audit logs

## 9. Implementation

Phase 1 — Joiner

The Joiner state is represented by Peter Parker's original Engineering identity.

Validate the configuration created during Lab 01:
```text
Peter Parker
      ↓
Engineering
      ↓
SG-Engineering-Users
```

Phase 2 — Mover

Update Peter Parker's department from:

Engineering

to:

Technology

Remove:

SG-Engineering-Users

Add:

SG-Technology-Users

Save the changes.

Phase 3 — Leaver

After completing the Mover validation, disable Peter Parker's account.

Navigate to Peter Parker's user account and change the account status to disabled.

Save the change.

## 10. Validation

The lifecycle state should be validated after each phase.

### Joiner Validation

| Check | Expected Result |
|---|---|
| Account exists | Yes |
| Account enabled | Yes |
| Department | Engineering |
| Engineering group membership | Present |
| Unnecessary groups | None |

### Mover Validation

| Check | Expected Result |
|---|---|
| Account exists | Yes |
| Account enabled | Yes |
| Department | Technology |
| Engineering group membership | Removed |
| Technology group membership | Present |
| Unnecessary groups | None |

### Leaver Validation

| Check | Expected Result |
|---|---|
| Account exists | Yes |
| Account enabled | No |
| Authentication | Disabled |
| Access reviewed | Yes |
| Audit activity reviewed | Yes |

## 11. Audit Evidence

Identity lifecycle changes should be auditable.

Microsoft Entra audit logs can be used to investigate identity lifecycle activity including:

- User creation
- User attribute changes
- Group membership changes
- Account disablement
- Administrative actions

Navigate to:

**Microsoft Entra ID → Monitoring & health → Audit logs**

Review the relevant events after each lifecycle transition.

The audit evidence should demonstrate:

- Who performed the action
- What action was performed
- Which identity was affected
- When the action occurred
- Whether the operation succeeded

## 12. Troubleshooting Scenario

Scenario

Peter Parker has moved from Engineering to Technology, but still appears to have Engineering group membership.

The IAM administrator must investigate why the old access remains.

Investigation
```text
User reports incorrect access
          ↓
Review user attributes
          ↓
Review group memberships
          ↓
Compare against current department
          ↓
Identify outdated membership
          ↓
Remove unnecessary access
          ↓
Revalidate
          ↓
Review audit logs
```
Investigation Checks

Verify:

- Peter Parker's current department
- Current security group memberships
- Previous Engineering membership
- Technology membership
- Relevant audit events
- Whether the access change was successfully applied
- Expected Resolution

Remove:

SG-Engineering-Users

if Engineering access is no longer required.

Retain:

SG-Technology-Users

as the appropriate group for Peter's new department.

## 13. Evidence

Screenshots captured during the lab will demonstrate the identity lifecycle transitions and validation activities.

Screenshot 01 — Joiner State

Filename: 01-joiner-state.png

Purpose: Demonstrate Peter Parker's initial Engineering identity and group membership.

Screenshot 02 — Mover: Department Change

Filename: 02-mover-department-change.png

Purpose: Demonstrate Peter Parker's department changing from Engineering to Technology.

Screenshot 03 — Mover: Group Membership

Filename: 03-mover-group-membership.png

Purpose: Demonstrate removal from SG-Engineering-Users and membership in SG-Technology-Users.

Screenshot 04 — Leaver: Account Disabled

Filename: 04-leaver-account-disabled.png

Purpose: Demonstrate that Peter Parker's account has been disabled as part of the leaver process.

Screenshot 05 — Lifecycle Audit Evidence

Filename: 05-lifecycle-audit-log.png

Purpose: Demonstrate that identity lifecycle changes were recorded in Microsoft Entra audit logs.

## 14. Lessons Learned

This lab demonstrates that IAM is not limited to creating identities.

Identity management continues throughout the identity lifecycle.

**Key observations:**

- Joiners require appropriate provisioning and access assignment.
- Movers require access review when their role or department changes.
- Previous access should not automatically remain after a role change.
- Leavers require timely account disablement and access removal.
- Group-based access simplifies lifecycle management.
- Least privilege should be maintained throughout the identity lifecycle.
- Audit logs provide evidence of administrative identity changes.
- Identity lifecycle processes should be repeatable and auditable.

## 15. Lab Outcome

The lab is complete when:

 - Joiner state validated
 - Peter Parker's Engineering identity validated
 - Mover scenario completed
 - Department changed to Technology
 - Engineering group membership removed
 - Technology group membership added
 - Mover state validated
 - Leaver scenario completed
 - Peter Parker's account disabled
 - Group membership reviewed
 - Audit logs reviewed
 - Troubleshooting scenario completed
 - Evidence captured
 - Lessons learned documented

## 16. Final Result

This lab demonstrates the identity lifecycle:
```text
Joiner
  ↓
Provision
  ↓
Assign Access
  ↓
Mover
  ↓
Review & Modify Access
  ↓
Leaver
  ↓
Disable & Remove Access
```
**The key IAM principle demonstrated is:**

Access should reflect the user's current business requirement throughout the identity lifecycle.

This lifecycle approach helps reduce unnecessary access, supports least privilege, and provides an auditable process for managing identities.

## Related Documentation
Day 01 — IAM Fundamentals
Lab 01 — User Provisioning
Lab 02 — Group-Based Access Control
OsCorp Environment Setup
