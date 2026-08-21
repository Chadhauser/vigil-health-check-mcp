# Vigil Health Check — MCP Server

A **free, read-only bookkeeping diagnostic** for books kept in Xero, exposed as a remote [Model Context Protocol](https://modelcontextprotocol.io) server. Connect it to any MCP-capable AI assistant, supply standard Xero exports as CSV text, and get a health score out of 100 with every finding explained in plain English.

Built on the same diagnostic engine that powers [Vigil Ledger](https://www.vigilledger.com), the continuous compliance-monitoring platform for accountants. Provided by [Insight Professional Partners Ltd](https://insightprofessionalpartners.com), a UK practice led by a CIMA Member in Practice.

**Docs page:** https://www.vigilledger.com/mcp.html

---

## Connect

The server is hosted — nothing to install, no API key, no OAuth.

```
https://www.vigilledger.com/api/mcp
```

**Claude:** Settings → Connectors → Add custom connector → paste the URL above.

**Other MCP clients:**

```json
{
  "mcpServers": {
    "vigil-health-check": {
      "url": "https://www.vigilledger.com/api/mcp"
    }
  }
}
```

Transport: JSON-RPC 2.0 over HTTP POST (streamable HTTP; SSE not required).

## Tools

| Tool | What it does |
| --- | --- |
| `vigil_describe_inputs` | Explains which Xero exports the check can read (Trial Balance, Account Transactions / General Ledger, Aged Receivables, Aged Payables) and how to obtain them — so the assistant knows what to ask the user for. |
| `vigil_run_health_check` | Takes the exports as CSV text plus an optional industry (`none`, `construction`, `retail_wholesale`, `manufacturing`, `software`), runs the diagnostic, and returns a 0–100 score, headline verdict, and each finding with the reasoning for why it matters. |

One file is enough for a result; more files unlock more checks (a second, earlier trial balance enables movement and stock-plug detection). Formats are auto-detected — exports do not need cleaning first.

## What it will never do

- **Read-only, permanently.** There is no tool that posts journals, changes records, or connects to any accounting system. It analyses text the user chooses to provide — nothing else.
- **No financial data is stored.** File contents are processed in memory for the duration of the request and discarded. The only thing ever persisted is an email address, and only if the user volunteers one to be contacted.
- **Diagnosis, not treatment.** The free check shows what is wrong and why it matters. The specific corrections, their order, and a scope to put things right are professional work — that is what [the practice](https://insightprofessionalpartners.com/health-check/) does.

## About this repository

This repository is the public home for the hosted server: documentation and registry metadata (`server.json`). The diagnostic engine itself is proprietary and runs server-side; its findings logic is identical to the [web Health Check](https://insightprofessionalpartners.com/health-check/), which runs the same checks entirely in the visitor's browser.

© Insight Professional Partners Ltd · Company No. 16943160
