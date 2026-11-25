---
name: excel-power-bi-automation
description: >
  Automate Excel workbooks and Power BI models through MCP servers — reading,
  writing, and formatting .xlsx files headlessly on any OS, or driving the
  real Excel/Power BI Desktop app on Windows for Power Query, DAX, PivotTables,
  and semantic models. Use when a user wants to automate a spreadsheet,
  reconciliation tracker, open-items list, or Power BI data model, or asks
  which Excel/Power BI MCP tool fits their platform.
version: 1.0.0
author: borghei
category: data-analytics
tags: [excel, power-bi, mcp, automation, reporting]
---

# Excel & Power BI Automation

Three tools cover this space, and they are not interchangeable — picking the wrong one means either hitting a wall (no Excel installed) or missing capability (headless tools can't touch Power Query/DAX). Match the tool to what's actually available and what the task needs.

---

## Picking the Right Tool

| Tool | Runs on | Needs Excel/Power BI installed? | Can do |
|---|---|---|---|
| **Excel MCP Server** (`haris-musa/excel-mcp-server`) | Linux, macOS, Windows | No — pure Python via `openpyxl` | Read/write cells, formulas, formatting, charts, tables; folder-confined, read-only mode, atomic saves |
| **ExcelMcp** (`sbroenne/mcp-server-excel`) | Windows only | Yes — drives the real Excel application | Everything Excel MCP Server does, plus Power Query, DAX, Power Pivot, PivotTables — anything that needs Excel's own engine |
| **Power BI Modeling MCP** (`microsoft/powerbi-modeling-mcp`, Microsoft, public preview) | Windows (Power BI Desktop) or Fabric | Yes — Power BI Desktop or a Fabric workspace | Create/update tables, columns, measures, and relationships in a semantic model from a natural-language description |

**In a headless/cloud environment (no Windows, no Office installed):** only Excel MCP Server works. The other two require the actual desktop applications and cannot run in a Linux container — don't attempt to install them there; tell the user they need their own Windows machine with Excel/Power BI Desktop, used either locally or through a Remote Control / desktop connection.

---

## Excel MCP Server — Setup (works anywhere)

```bash
claude mcp add excel --scope user -- uvx excel-mcp-server stdio --allow-dir /path/to/workbooks
```

Requires [`uv`](https://docs.astral.sh/uv/getting-started/installation/). `--allow-dir` confines the server to one folder — point it at wherever the actual workbooks live, not a throwaway scratch directory, or every `--allow-dir` change means reconfiguring the server. Other useful flags: a read-only mode for "look but don't touch" sessions, and localhost-only HTTP if running it over HTTP transport instead of stdio.

What it's good for: an open-items tracker, a reconciliation workbook, bulk formatting, generating charts from existing data — anything that's really "manipulate this .xlsx file," not "use Excel's calculation engine for something exotic."

What it can't do: Power Query refreshes, DAX measures, Power Pivot models, macros that call into VBA — those need the real application (ExcelMcp) or a BI tool (Power BI Modeling MCP).

---

## ExcelMcp — Setup (Windows only, drives real Excel)

Install per the repo's instructions on a Windows machine with Excel installed. Because it automates the live application, treat it with the same caution as any tool that can change a file a human has open — confirm nothing unsaved is at risk before running bulk operations, and prefer closing the workbook in Excel before letting the MCP server act on it to avoid file-lock conflicts.

---

## Power BI Modeling MCP — Setup (Windows/Fabric, public preview)

Install per Microsoft's repo. Being in public preview, expect occasional rough edges — validate generated measures and relationships against the actual data before trusting a dashboard built from them, especially anything feeding a decision or a report that goes to leadership.

---

## Choosing for a Real Decision

If the task is a recurring open-items tracker or reconciliation file that multiple people touch, first ask whether it should move to a proper shared tool (e.g. a list/database the team already uses) rather than be automated in place — automating a fragile spreadsheet workflow can lock in the fragility. Excel MCP Server is the right tool for "automate this file as it exists today," not a substitute for deciding whether the file should exist in its current form at all.

---

## Common Pitfalls

- **Pointing Excel MCP Server's `--allow-dir` too narrowly or too broadly.** Too narrow and it can't reach the file you need; too broad (e.g. the whole home directory) gives it write access to unrelated files — scope it to the specific workbook folder.
- **Expecting headless tools to do DAX/Power Query.** That's a hard capability boundary, not a configuration issue — if a request needs the calculation engine, the answer is "this needs real Excel/Power BI Desktop," not a workaround.
- **Trusting Power BI Modeling MCP's preview-stage output unchecked.** Spot-check generated relationships and measures against known-good totals before it reaches a report anyone relies on.
