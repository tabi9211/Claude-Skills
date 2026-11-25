---
name: sap-documentation-navigator
description: Search official SAP documentation (S/4HANA, BTP, ABAP, CAP) and SAP Community content, and plan safe ABAP development/transport work against ECC and S/4HANA systems. Use when a user asks about SAP S/4HANA, SAP BTP, ABAP, CAP, Fiori, RAP, SAP Community answers, or needs help connecting an MCP server to SAP documentation or an SAP system.
version: 1.0.0
author: borghei
category: engineering-team
tags: [sap, s4hana, abap, btp, erp, mcp]
---

# SAP Documentation Navigator

Guidance for using MCP servers that give Claude read access to SAP's official documentation and, optionally, read/connect access to a real ABAP system — plus the review discipline to apply before anything touches SAP config or code.

---

## Two Different Tools, Two Different Risk Levels

| Tool | What it touches | Risk | Setup |
|---|---|---|---|
| **mcp-sap-docs** (`marianfoo/mcp-sap-docs`) | Public SAP documentation + SAP Community posts, served locally | Low — read-only, no system access, no API key | Clone, `npm run setup` (pulls doc submodules), `npm run build`, run as stdio MCP server |
| **ABAP ADT MCP** (`fr0ster/mcp-abap-adt`) | A real ECC / S/4HANA / BTP ABAP Cloud system via ADT | High — can read (and, depending on config, write) objects in a live SAP landscape | Needs system connection details, a dedicated user, and sign-off from SAP Basis/security before connecting |

**Default to mcp-sap-docs for anything that's really a documentation or "how does X work in SAP" question.** Only reach for an ABAP system connection when the task genuinely requires looking at (or changing) real objects, and only after a human has confirmed:
1. The target is a sandbox or a read-only user — never a production client — unless explicitly told otherwise by someone authorized to say so.
2. SAP Basis and security have approved the connection.
3. The scope is as narrow as possible (one system, one client, least-privilege user).

If any of those three aren't in place, say so and stop — don't configure the connection "to be helpful."

---

## Setting Up mcp-sap-docs

```bash
git clone https://github.com/marianfoo/mcp-sap-docs.git
cd mcp-sap-docs
npm ci
npm run setup   # pulls the SAP documentation submodules — can be a sizeable one-time download
npm run build    # builds the search index (full-text + embeddings)
claude mcp add sap-docs -- node "$(pwd)/dist/src/server.js"
```

No API key or login is required. Optional environment variables:
- `MCP_VARIANT` — `sap-docs` (default) or `abap`, to scope which libraries are indexed.
- `MAX_CONTENT_LENGTH`, `SAP_HELP_TIMEOUT_MS`, `SAP_COMMUNITY_TIMEOUT_MS`, `EMBEDDING_WEIGHT` — tuning knobs, leave at defaults unless you have a reason.

If `npm run setup` or `npm run build` reports some sources as "not found," the corresponding git submodule didn't sync (network restriction or a renamed upstream repo) — that source is simply missing from the index, not a broken install. Re-run `npm run setup` once network access is confirmed, or proceed with the partial index and say so.

---

## Using It

Ask it things like:
- "What does SAP's documentation say about CDS view annotations for analytical queries?"
- "Find the SAP Community discussion on RAP managed vs. unmanaged implementations."
- "What's the recommended approach for extending a standard S/4HANA Fiori app?"

The server returns documentation excerpts and Community thread summaries with source links — treat the content as a starting point, not ground truth for a specific customer's configuration. SAP documentation describes standard behavior; a specific system may be customized away from it.

---

## If an ABAP System Connection Is Actually Needed

1. Confirm the three preconditions above with the user explicitly, in writing (in the conversation), before touching `mcp-abap-adt` configuration.
2. Prefer a read-only ADT user over one with change access, even in a sandbox — most "look at this program" and "explain this class" requests don't need write access.
3. Never generate or run a transport request, activate an object, or change a system setting without the user reviewing the exact change first — treat this the same as any other hard-to-reverse production action.
4. When reporting findings from a live system, be explicit about which system/client they came from, since the same object can differ between ECC, S/4HANA on-prem, and S/4HANA Cloud.

---

## Common Pitfalls

- **Treating Community answers as official.** SAP Community posts are useful context but aren't SAP documentation — flag the distinction when citing one.
- **Assuming one SAP version's behavior applies to another.** ECC, S/4HANA on-prem, and S/4HANA Cloud (ABAP Cloud / RAP) diverge significantly — always check which the question is actually about.
- **Skipping the submodule setup.** Running `npm run build` before `npm run setup` produces an empty or near-empty index with no documents — a 0-result answer from the server usually means setup didn't finish, not that nothing exists.
