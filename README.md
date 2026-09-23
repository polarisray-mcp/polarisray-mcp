# PolarisRay MCP

Remote [Model Context Protocol](https://modelcontextprotocol.io/) servers for **public U.S. FDA and NIH datasets**. Free beta — no API key for public sources.

**Start here:** [`https://mcp.polarisray.com/federated`](https://mcp.polarisray.com/federated)

**Docs:** [polarisray.com/docs](https://polarisray.com/docs/) · **Site:** [polarisray.com](https://polarisray.com) · **Catalog:** [polarisray.com/sources](https://polarisray.com/sources)

> Discovery-grade access for assistants. Confirm anything important against the primary agency record (returned `url` when available). **Not medical advice.**

---

## What this is

PolarisRay exposes each live dataset as an MCP server over HTTP on `mcp.polarisray.com`. Point Claude, Cursor, VS Code, or any remote-MCP client at an endpoint URL.

| Endpoint | Corpus |
| --- | --- |
| [`/federated`](https://mcp.polarisray.com/federated) | **Recommended.** One call across MAUDE, ClinicalTrials.gov, DailyMed, openFDA SPL, and FDA recalls |
| [`/maude`](https://mcp.polarisray.com/maude) | FDA device adverse-event reports (MAUDE) |
| [`/faers`](https://mcp.polarisray.com/faers) | FDA drug adverse-event cases (FAERS) — **separate** from federated |
| [`/recalls`](https://mcp.polarisray.com/recalls) | FDA enforcement / recall reports |
| [`/clinical_trials`](https://mcp.polarisray.com/clinical_trials) | ClinicalTrials.gov (via CTTI AACT) |
| [`/dailymed`](https://mcp.polarisray.com/dailymed) | NLM DailyMed labels |
| [`/spl`](https://mcp.polarisray.com/spl) | openFDA Structured Product Labels |

FAERS is **not** included in `/federated`. Conference search is planned (`/conferences`), not live.

Hits include an official source `url` when a public detail page exists (MAUDE, trials, DailyMed / openFDA labels, iRES recalls). FAERS has no per-case FDA web page; links point at the quarterly extract.

---

## Install

### Quick add (`npx add-mcp`)

Federated first (streamable HTTP):

```bash
npx add-mcp https://mcp.polarisray.com/federated -n polarisray-federated
```

Optional: target a specific agent, e.g. Cursor or Claude Code:

```bash
npx add-mcp https://mcp.polarisray.com/federated -n polarisray-federated -a cursor -y
npx add-mcp https://mcp.polarisray.com/federated -n polarisray-federated -a claude-code -y
```

Other live paths work the same way (replace the URL), e.g. FAERS:

```bash
npx add-mcp https://mcp.polarisray.com/faers -n polarisray-faers
```

### Claude Desktop / Claude Code (JSON)

Add to MCP config (`claude_desktop_config.json`, or `claude mcp add`):

```json
{
  "mcpServers": {
    "polarisray-federated": {
      "url": "https://mcp.polarisray.com/federated"
    }
  }
}
```

Full endpoint set (optional):

```json
{
  "mcpServers": {
    "polarisray-maude": { "url": "https://mcp.polarisray.com/maude" },
    "polarisray-faers": { "url": "https://mcp.polarisray.com/faers" },
    "polarisray-recalls": { "url": "https://mcp.polarisray.com/recalls" },
    "polarisray-clinical-trials": { "url": "https://mcp.polarisray.com/clinical_trials" },
    "polarisray-dailymed": { "url": "https://mcp.polarisray.com/dailymed" },
    "polarisray-spl": { "url": "https://mcp.polarisray.com/spl" },
    "polarisray-federated": { "url": "https://mcp.polarisray.com/federated" }
  }
}
```

### Cursor / other remote MCP clients

Same shape — supply the HTTP URL for the server you want. Public sources need **no credentials**.

---

## Federated tools overview

On [`/federated`](https://mcp.polarisray.com/federated):

| Tool | Use when |
| --- | --- |
| `federated_search` | Keyword + shared facets across MAUDE, trials, DailyMed, openFDA SPL, recalls; optional `source`, dates, filters |
| `semantic_search` | Meaning-based / hybrid search when wording differs from indexed text |
| `correlate` | One facet (e.g. disease or manufacturer): AE volume vs registered trials + co-occurring terms |
| `resolve` | Normalize lay terms → canonical facet labels before precise search/correlate |

Shared facets include `disease`, `device_type`, `substance`, `anatomy`, and `manufacturer`. Responses include `meta` (pagination, filters, completeness). Federated `meta.authoritative` is **false** — discovery-grade, not a substitute for official registry APIs.

Per-source servers expose their own search / get tools; see each page under [Sources](https://polarisray.com/sources).

---

## Caveats (read before relying on results)

- **Public data only** — public-domain U.S. Food and Drug Administration and U.S. National Institutes of Health corpora; free beta; endpoints may change.
- **Discovery-grade** — useful in an assistant; confirm via returned `url` or the agency’s own tools before decisions.
- **Not medical advice** — informational / research use only; not diagnosis, treatment, or clinical decision support.
- **Passive surveillance limits** — MAUDE / FAERS reports are unverified, may be incomplete or duplicated, and do **not** prove causation; counts are not rates.
- **FAERS ≠ federated** — drug-case search lives at `/faers` only.
- **No PHI** — do not send protected health information in queries; treat the host like any third-party API.
- **Not an FDA/NIH product** — PolarisRay is an access layer; data sources are attributed, not partnered.

More detail: [Legal](https://polarisray.com/legal) · source disclaimers on each [Sources](https://polarisray.com/sources) page.

---

## Links

- Docs: https://polarisray.com/docs/
- Federated endpoint: https://mcp.polarisray.com/federated
- Source catalog: https://polarisray.com/sources
- Why PolarisRay: https://polarisray.com/why
- Status: https://polarisray.com/status

---

## License / attribution

Underlying datasets are from the **U.S. Food and Drug Administration** and the **U.S. National Institutes of Health** (ClinicalTrials.gov via CTTI AACT; DailyMed via NLM — as documented on each source page). This README describes the PolarisRay MCP access layer; it is not an official FDA or NIH publication.
