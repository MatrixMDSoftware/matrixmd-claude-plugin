# MatrixMD for Claude

MatrixMD for Claude combines the public MatrixMD operations MCP connector with a skill that guides safe catalog and inventory workflows.

- Connector: `https://mcp.matrixmdsoftware.com/mcp/plugin`
- Public guide: <https://matrixmdsoftware.com/claude/>
- Privacy: <https://matrixmdsoftware.com/privacy>
- Terms: <https://matrixmdsoftware.com/terms>
- Support: <https://matrixmdsoftware.com/contact>

The connector is available now as a custom Claude connector. This repository is the public source for the plugin submitted to Anthropic's community plugin directory. Directory availability is pending Anthropic review.

## What it can manage

The public connector exposes 12 tools for:

- authorized clinic and permission discovery;
- clinic reference catalogs and appointment-type definitions;
- products and stock;
- medicine definitions; and
- treatment templates.

Every action is limited to clinics and operations already authorized for the signed-in MatrixMD account. Write tools preserve the connector's existing permission checks, and archive operations require an exact record plus explicit confirmation.

## Privacy boundary

The public connector does not expose or accept patient records, appointments, availability, specialists, providers, purchases, sales, financial reports, clinical encounters, diagnoses, government identifiers, administrative tools, or raw SQL.

Do not paste or upload excluded health or identity data into an AI conversation. Use the approved private MatrixMD application or contact MatrixMD support for those workflows.

## Connect in Claude

1. Open **Settings → Connectors** in Claude.
2. Choose **Add custom connector**.
3. Enter `https://mcp.matrixmdsoftware.com/mcp/plugin`.
4. Complete the MatrixMD OAuth login and consent flow.
5. Ask Claude which MatrixMD clinics you can manage.

MatrixMD uses OAuth 2.0 dynamic client registration, authorization code flow, S256 PKCE, and rotating refresh tokens. Hosted Claude surfaces use `https://claude.ai/api/mcp/auth_callback`; Claude Code uses an ephemeral loopback callback.

## Test the plugin locally

Prerequisite: install and authenticate [Claude Code](https://code.claude.com/docs/en/overview).

```bash
git clone https://github.com/MatrixMDSoftware/matrixmd-claude-plugin.git
claude --plugin-dir ./matrixmd-claude-plugin
```

Claude will discover the bundled `matrixmd` MCP server and prompt for OAuth when the connector is first used. The skill is available as `matrixmd:manage-matrixmd-operations` and can also be invoked automatically when its description matches the request.

Validate the package before contributing:

```bash
npx -y @anthropic-ai/claude-code@2.1.243 plugin validate . --strict
```

See [docs/TESTING.md](docs/TESTING.md) for the connector and privacy test matrix.

## Repository structure

```text
.claude-plugin/plugin.json
.mcp.json
skills/manage-matrixmd-operations/SKILL.md
skills/manage-matrixmd-operations/references/public-surface.md
evals/evals.json
docs/TESTING.md
```

## Security and support

Never commit MatrixMD credentials, OAuth tokens, reviewer accounts, or production data. Follow [SECURITY.md](SECURITY.md) for private vulnerability reporting and [SUPPORT.md](SUPPORT.md) for product help.

## License

[MIT](LICENSE)
