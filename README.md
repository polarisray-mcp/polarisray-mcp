# PolarisRay MCP

Remote [Model Context Protocol](https://modelcontextprotocol.io/) servers for **public healthcare & life-sciences data** — FDA MAUDE, FAERS and recalls, ClinicalTrials.gov and global trial registries, DailyMed and openFDA SPL labels, ChEMBL, PubChem and PDB, plus Analytics for ready-made answers. Free beta — no account or API key for public sources.

**Start here:** [`https://mcp.polarisray.com/federated`](https://mcp.polarisray.com/federated)

**Docs:** [polarisray.com/docs](https://polarisray.com/docs/) · **Site:** [polarisray.com](https://polarisray.com) · **Catalog:** [polarisray.com/sources](https://polarisray.com/sources)

> Built for healthcare & life-sciences teams — research, safety, and regulatory. Discovery-grade access for assistants: confirm important findings against the primary agency record (returned `url` when available). **Not medical advice.**

---

## What this is

PolarisRay turns hard-to-search public datasets into MCP servers your AI assistant can query in plain language. Each live dataset is an MCP server over HTTP on `mcp.polarisray.com`. Point Claude, Cursor, VS Code, or any remote-MCP client at an endpoint URL.

| Endpoint | Corpus |
| --- | --- |
| [`/federated`](https://mcp.polarisray.com/federated) | **Recommended.** One call across all ten data sources below, plus the main Analytics tools |
| [`/maude`](https://mcp.polarisray.com/maude) | FDA device adverse-event reports (MAUDE) |
| [`/faers`](https://mcp.polarisray.com/faers) | FDA drug adverse-event cases (FAERS) — also included in federated |
| [`/recalls`](https://mcp.polarisray.com/recalls) | FDA enforcement / recall reports |
| [`/clinical_trials`](https://mcp.polarisray.com/clinical_trials) | ClinicalTrials.gov (via CTTI AACT) |
| [`/clinical_trials_intl`](https://mcp.polarisray.com/clinical_trials_intl) | Global trial registries: trials registered outside ClinicalTrials.gov (China, India, Japan, the EU and more) — beta |
| [`/dailymed`](https://mcp.polarisray.com/dailymed) | NLM DailyMed labels |
| [`/spl`](https://mcp.polarisray.com/spl) | openFDA Structured Product Labels |
| [`/chembl`](https://mcp.polarisray.com/chembl) | ChEMBL bioactive molecules: targets, mechanisms, indications (EMBL-EBI) |
| [`/pubchem`](https://mcp.polarisray.com/pubchem) | PubChem: drug substances to chemical identity (CID, InChIKey, formula) |
| [`/pdb`](https://mcp.polarisray.com/pdb) | RCSB PDB: experimentally determined 3D structures |
| [`/analytics`](https://mcp.polarisray.com/analytics) | Answers, not records: drug and company dossiers, label gaps, class comparisons, loss of exclusivity, market share |

`/federated` searches all ten data sources in one call; each is also available as its own server. `/analytics` computes answers across them (its main tools are also on `/federated`). Browse the [catalog](https://polarisray.com/sources) for tools, examples, and disclaimers.

Hits include an official source `url` when a public detail page exists (MAUDE, ClinicalTrials.gov and the national trial registries, DailyMed / openFDA labels, iRES recalls). FAERS has no per-case FDA web page; links point at the quarterly extract.

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
    "polarisray-clinical-trials-intl": { "url": "https://mcp.polarisray.com/clinical_trials_intl" },
    "polarisray-chembl": { "url": "https://mcp.polarisray.com/chembl" },
    "polarisray-pubchem": { "url": "https://mcp.polarisray.com/pubchem" },
    "polarisray-pdb": { "url": "https://mcp.polarisray.com/pdb" },
    "polarisray-analytics": { "url": "https://mcp.polarisray.com/analytics" },
    "polarisray-federated": { "url": "https://mcp.polarisray.com/federated" }
  }
}
```

### Cursor / other remote MCP clients

Any client that supports remote MCP over HTTP works the same way — supply the endpoint URL. Public datasets need **no credentials**. Sources that wrap rate-limited or licensed data document an API-key flow on their individual pages.

---

## Federated tools overview

On [`/federated`](https://mcp.polarisray.com/federated):

| Tool | Use when |
| --- | --- |
| `federated_search` | Keyword + shared facets across all ten sources; optional `source`, dates, filters |
| `semantic_search` | Meaning-based / hybrid search when wording differs from indexed text |
| `correlate` | One facet (e.g. disease or manufacturer): AE volume vs registered trials + co-occurring terms |
| `resolve` | Normalize lay terms → canonical facet labels before precise search/correlate |
| `asset_dossier` | Everything public about one drug: trial pipeline, FAERS trend, label changes, recalls, timeline |
| `company_dossier` | One company across sources: pipeline, portfolio, safety, recalls |
| `label_gap` | FAERS signals for a drug that no current label names |
| `class_compare` | Every drug in a class side by side on FAERS reactions |
| `find_classes` / `find_companies` | Look up a drug class or a company (spellings and subsidiaries grouped) |

The analysis tools come from [`/analytics`](https://mcp.polarisray.com/analytics), which has the full set (signal scans, label checks, exclusivity, Medicare spending, market share, company financials).

Shared facets include `disease`, `device_type`, `substance`, `anatomy`, and `manufacturer`. Responses include `meta` (pagination, filters, completeness). Federated `meta.authoritative` is **false** — discovery-grade, not a substitute for official registry APIs.

Per-source servers expose their own search / get tools; see each page under [Sources](https://polarisray.com/sources).

---

## Caveats (read before relying on results)

- **Public data / free beta** — public datasets from U.S. agencies (FDA, NIH, CMS, SEC), EMBL-EBI ChEMBL, RCSB PDB and national clinical-trial registries; free early beta (no ads, paywalls, or accounts for public sources); datasets, tools, and endpoints may change.
- **Discovery-grade** — useful in an assistant; confirm via returned `url` or the agency’s own tools before decisions. PolarisRay does not guarantee accuracy, completeness, or timeliness.
- **Not professional / medical advice** — informational / research use only; not diagnosis, treatment, clinical decision support, or other professional advice.
- **Passive surveillance limits** — MAUDE / FAERS reports are unverified, may be incomplete or duplicated, and do **not** prove causation; counts are not rates.
- **FAERS in federated and standalone** — drug-case search is in `/federated` and also at `/faers`.
- **No PHI** — do not send protected health information in queries; treat the host like any third-party API. MCP endpoints process queries only to return results and operate the service — not for advertising, and not sold. (See [Legal](https://polarisray.com/legal).)
- **Not an official product of any data provider** — PolarisRay is an access layer; data sources are attributed, not partnered. Datasets remain under their authorities’ terms.

More detail: [Legal](https://polarisray.com/legal) · [About](https://polarisray.com/about) · source disclaimers on each [Sources](https://polarisray.com/sources) page.

---

## Links

- Docs: https://polarisray.com/docs/
- Federated endpoint: https://mcp.polarisray.com/federated
- Source catalog: https://polarisray.com/sources
- Why PolarisRay: https://polarisray.com/why
- About: https://polarisray.com/about
- Status: https://polarisray.com/status
- Legal: https://polarisray.com/legal

---

## License / attribution

Underlying datasets, as documented on each source page:

- **U.S. Food and Drug Administration** — MAUDE, FAERS, Enforcement Reports (recalls), openFDA SPL, Orange Book, Purple Book, warning letters
- **U.S. National Institutes of Health** — ClinicalTrials.gov (via CTTI AACT), DailyMed (NLM), PubChem (NCBI)
- **CMS** (Medicare / Medicaid spending, NADAC, Open Payments) and **SEC EDGAR** — used by Analytics
- **EMBL-EBI ChEMBL** — CC BY-SA 3.0
- **RCSB PDB** — CC0
- **Global trial registries** — data via WHO ICTRP, not endorsed by WHO; free use only; the current processing date is on the [source page](https://polarisray.com/sources/clinical_trials_intl/)

Datasets remain the property of their respective authorities. This README describes the PolarisRay MCP access layer; it is not an official publication of any data provider.
