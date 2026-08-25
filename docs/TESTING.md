# Testing MatrixMD for Claude

Run these checks before a release or Anthropic submission.

## Plugin structure

```bash
npx -y @anthropic-ai/claude-code@2.1.243 plugin validate . --strict
```

Expected: `Validation passed` with no warnings.

## Connector discovery and authentication

1. Add `https://mcp.matrixmdsoftware.com/mcp/plugin` as a custom connector in Claude.
2. Confirm the initial unauthenticated request returns HTTP `401` with a `WWW-Authenticate` header pointing to the plugin-specific protected-resource metadata.
3. Complete OAuth dynamic client registration and S256 PKCE using the hosted Claude callback.
4. Repeat from Claude Code with an ephemeral `127.0.0.1` or `localhost` callback.
5. Confirm an expired access token refreshes successfully, the refresh token rotates, and reusing the consumed token returns `invalid_grant`.

Never put test credentials or tokens in this repository or a public issue.

## Tool inventory

The connector must expose exactly the 12 tools documented in the skill reference. Each tool must have:

- a human-readable `title`;
- `readOnlyHint` or `destructiveHint` matching its behavior;
- `idempotentHint`; and
- `openWorldHint=false`.

Test each tool with valid input and at least one invalid or unauthorized input. Errors should name the missing or denied value and give a safe next step.

## Privacy regression

Verify `tools/list` does not contain patients, patient treatments, appointments, availability, specialists, providers, purchase orders, sale orders, reports, admin tools, or raw SQL. Then ask Claude for a patient by government identifier and verify it does not request, accept, or process the data.

## Skill cases

The source prompts and expected behavior live in [`evals/evals.json`](../evals/evals.json). Review both tool choices and the user-facing answer. A passing run must preserve tenant boundaries, read-before-write behavior, exact user-supplied values, archive confirmation, and the privacy refusal.
