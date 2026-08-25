# MatrixMD public connector surface

The public endpoint is `https://mcp.matrixmdsoftware.com/mcp/plugin`. It uses MatrixMD OAuth and the signed-in user's existing tenant and clinic-profile grants.

## Available tools

| Tool | Purpose | Change risk |
| --- | --- | --- |
| `tenant_list` | List authorized tenant slugs | Read-only |
| `permissions_list` | Explain operational grants for one tenant | Read-only |
| `clinic_list_read` | Resolve clinic catalogs and tax IDs | Read-only |
| `appointment_type_read` | List or find appointment-type definitions | Read-only |
| `product_read` | List, search, or get products | Read-only |
| `product_write` | Create, update, or archive products | Write; archive requires confirmation |
| `stock_read` | List, search, or get stock state | Read-only |
| `stock_write` | Change stock-management settings or units | Write |
| `medicine_read` | List, search, or get medicine definitions | Read-only |
| `medicine_write` | Create, update, or archive medicine definitions | Write; archive requires confirmation |
| `treatment_read` | List, search, or get treatment templates | Read-only |
| `treatment_write` | Create, update, or archive treatment templates | Write; archive requires confirmation |

Read tools declare `readOnlyHint=true`; write tools declare `destructiveHint=true`. All tools declare `openWorldHint=false` because they access only MatrixMD services.

## Excluded from the public connector

The endpoint does not advertise or accept patient, patient-treatment, appointment, availability, specialist, provider, purchase-order, sale-order, report, admin, or raw-SQL tools. Use the private MatrixMD application for clinical or personal-data workflows.

## Error handling

- `permission_denied`: do not retry; explain the required permission and use `permissions_list` if clarification is useful.
- Unknown `tenantslug`: call `tenant_list` and use only an exact returned value.
- Missing archive confirmation: identify the record and ask for explicit confirmation before retrying with `confirm: true`.
- Invalid input: state which required value is missing or invalid; never fabricate it.
