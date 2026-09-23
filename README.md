# grok-connector-plugins-prod

Production team marketplace for Grok connector plugins. It is the production
counterpart of
[`grok-connector-plugins-staging`](https://github.com/minupalaniappan/grok-connector-plugins-staging)
and exists so a Cursor team can install a Finance plugin that dials the
**production** xAI connectors gateway from a team-scoped marketplace, without
depending on the public `cursor-public` listing.

This is not a distribution channel. End users install `finance` from
`cursor-public`.

## Plugins

| Plugin | Upstream | Upstream commit |
|---|---|---|
| `finance` | [`cursor/plugins` `third_party/finance`](https://github.com/cursor/plugins/tree/main/third_party/finance) (v1.1.1) | cdc46b3 |

`finance` is a verbatim copy of upstream `third_party/finance` (same plugin
name, display name and description). The MCP server URL is the same production
gateway URL upstream uses:
`https://connectors-gateway.grok.com/gateway/v1/finance/mcp`. Re-copy from
upstream when the upstream plugin changes.

## Using it

1. Cursor dashboard → Plugins → Marketplaces → import from repository, URL
   `https://github.com/minupalaniappan/grok-connector-plugins-prod`.
   Registering it for a team requires team admin (or the team's
   "allow third-party plugin imports" setting).
2. Set the team marketplace policy for `finance` (Required, Default or
   Optional) or install it per user from Cursor Settings → Plugins.
3. `finance` shows up only in Grok Bot (`cursor: "never"`); Cursor never
   lists it.

## What installing it creates

Installing `finance` creates a Cursor-dialed HTTP MCP row at the gateway
URL above. That row authenticates through the Cursor backend's registered
OAuth client for `connectors-gateway.grok.com`; until that client id is mounted
on the backend the row reports that it needs authorization. It is distinct from
the Grok-served `user-Finance-xai` row that the backend synthesizes for users
whose Grok account has the Finance connector, which does not need this plugin.
The gateway URL must also be admitted by the team's MCP policy for any member
governed by an MCP allowlist.

## Validation

```
npm install --no-save ajv ajv-formats
node scripts/validate-plugins.mjs
```
