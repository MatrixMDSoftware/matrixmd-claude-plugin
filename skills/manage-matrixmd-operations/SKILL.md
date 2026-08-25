---
name: manage-matrixmd-operations
description: Manage authorized MatrixMD clinic catalogs and inventory through the public MatrixMD MCP connector. Use this skill whenever a user asks to list, find, create, update, archive, or reconcile MatrixMD products, stock, medicine definitions, treatment templates, appointment types, or supporting clinic catalogs. Do not use the public connector for patients, appointments, encounters, diagnoses, government identifiers, or other personal health information.
---

# Manage MatrixMD operations

Use the MatrixMD MCP connector to manage operational records for clinics the signed-in user is authorized to access.

## Workflow

1. Call `tenant_list` before any tenant-scoped tool. Never invent or guess a `tenantslug`.
2. If more than one tenant is available and the request does not identify one, ask which tenant to use.
3. Read the current record before changing it. Use the narrowest query available and identify the exact record ID.
4. Preserve fields the user did not ask to change. Do not infer prices, tax IDs, quantities, barcodes, or catalog IDs.
5. Use `clinic_list_read` to resolve tax IDs instead of guessing.
6. Before an archive operation, identify the exact record, explain that it will be deactivated, and obtain explicit confirmation. Only then send `confirm: true`.
7. After a write, read the affected record again and summarize the resulting state.

## Permission handling

- Treat `permission_denied` as final for that tool and tenant. Do not retry it.
- Call `permissions_list` only to explain which public operational tools the signed-in MatrixMD profile can use.
- Never try another tenant to bypass a denied operation.

## Public privacy boundary

The public connector intentionally excludes patient records, patient treatments, appointments, appointment availability, specialists, providers, purchases, sales, reports, raw SQL, diagnoses, encounters, and government identifiers.

If a user asks for excluded data or a clinical workflow:

1. Do not ask the user to paste or upload the excluded data into any AI conversation.
2. Explain that the public MatrixMD connector cannot process personal health information or government identifiers.
3. Direct the user to the approved private MatrixMD application or MatrixMD support.

Read [references/public-surface.md](references/public-surface.md) when you need the exact tool boundary or write-safety mapping.
