# Lab 02 — Evidence Screenshots

This folder contains screenshots captured during **Lab 02 — Group-Based Access Control**.

The screenshots provide visual evidence of the implementation, validation, auditing, and access-control testing performed in the OsCorp Microsoft Entra ID laboratory environment.

---

## Evidence Index

| Screenshot | Evidence | Purpose |
|---|---|---|
| `01-security-groups.png` | Group Creation | Demonstrates creation of the OsCorp departmental security groups. |
| `02-engineering-group-membership.png` | Engineering Membership | Demonstrates Peter Parker's membership in `SG-Engineering-Users`. |
| `03-oscorp-group-membership.png` | Group Membership | Demonstrates the relationship between OsCorp users and their departmental security groups. |
| `04-group-audit-log.png` | Audit Evidence | Demonstrates group creation and/or membership changes recorded in Microsoft Entra audit logs. |
| `05-access-granted.png` | Access Granted | Demonstrates Peter Parker receiving access to the OsCorp Engineering resource through group-based authorization. |
| `06-access-denied.png` | Access Denied | Demonstrates a user outside the Engineering group being denied access to the Engineering resource. |

---

## Evidence Sequence

The screenshots follow the sequence of the lab:

```text
Group Creation
      ↓
Group Membership
      ↓
Membership Validation
      ↓
Audit Evidence
      ↓
Access Granted
      ↓
Access Denied
```
