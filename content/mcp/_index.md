---
title: "fanta.bio MCP"
linkTitle: "MCP"
description: "Use fanta.bio's functional genome annotations directly inside Claude, Cursor, and any other MCP-compatible AI assistant."
weight: 3
---

The fanta.bio MCP (Model Context Protocol) server lets AI assistants like Claude query our functional genome annotations directly in conversation. Ask natural language questions about CREs, as well as their overlapping, neighboring or interacting genes, SNPs, and transcription factor binding. The assistant calls the right tools, fetches the data, and synthesizes an answer.

## Endpoint

```
https://mcp.fanta.bio/mcp
```

The server implements MCP Streamable HTTP (JSON-RPC 2.0) — works with any MCP-compatible client.

## Setup

{{< tabs >}}

  {{< tab name="Claude Desktop" >}}
Add to your Claude Desktop config:

- **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "fanta-bio": {
      "url": "https://mcp.fanta.bio/mcp"
    }
  }
}
```

Restart Claude Desktop to load the MCP server.
  {{< /tab >}}

  {{< tab name="Claude Code" >}}
```bash
claude mcp add fanta-bio https://mcp.fanta.bio/mcp
```
  {{< /tab >}}

  {{< tab name="Claude.ai web" >}}
In a conversation: **Settings → Connectors → Add custom connector**.
Use `https://mcp.fanta.bio/mcp` as the URL.
  {{< /tab >}}

{{< /tabs >}}

## What the assistant can do

The server exposes **19 tools**, **5 prompt templates**, and **2 resources**. Once connected, the assistant lists them automatically — you don't need to remember names. The sections below are a reference for what's available.

### Tools (19)

#### Discovery & search

| Tool | Purpose |
|------|---------|
| `search_genes` | Full-text search by gene ID, symbol, name, or synonym |
| `search_snps` | Full-text search by SNP rs ID or GWAS trait |
| `search_cres` | Filter CREs by ID, name, antigen, or external ID (supports `*`/`?` wildcards) |
| `search_cres_fulltext` | Full-text CRE keyword search across IDs, names, gene symbols, antigens |
| `find_cres_by_region` | Find CREs whose coordinates fall inside a `chr:start-end` window |

#### Single-record lookups

| Tool | Purpose |
|------|---------|
| `get_gene_details` | Full gene record by Ensembl ID, symbol, or synonym |
| `get_snp_details` | Full SNP record by rs ID |
| `get_cre_details` | Full CRE annotation (coordinates, nearest gene, transcripts, SCREEN cCRE links) |

#### Relationships

| Tool | Purpose |
|------|---------|
| `find_cres_by_gene` | All CREs associated with a gene, sorted by distance from TSS |
| `find_cres_by_snp` | All CREs near a SNP, sorted by distance |
| `get_unique_antigens` | Unique TFs binding any of a list of CREs (max 200) |
| `get_cres_batch` | Batch lookup of full CRE annotations by ID (max 200) |

#### Expression & TF binding

| Tool | Purpose |
|------|---------|
| `get_cre_expression` | Expression vector for one CRE across all samples |
| `get_gene_expression` | Expression for the CREs associated with a gene |
| `get_cre_bound_tfs` | ChIP-Atlas TF binding for one CRE with Q-scores per experiment |
| `get_gene_antigens` | Unique TFs binding any of a gene's associated CREs |

#### Cross-database analysis

| Tool | Purpose |
|------|---------|
| `find_shared_regulators` | TFs shared across a set of CREs or genes (max 500 CREs / 50 genes) |
| `get_gene_regulatory_summary` | One-call workflow: gene info + associated CREs + top regulating TFs |
| `get_snp_regulatory_impact` | One-call workflow: SNP info + affected CREs + bound TFs |

{{< callout type="info" >}}
**Identifier resolution:** Tools that take `gene_id` accept Ensembl IDs (`ENSG00000141510.20`), official symbols (`TP53`), or any registered synonym (`p53`, `IFI15`). The API resolves all three to the canonical Ensembl ID.
{{< /callout >}}

### Prompts (5)

Prompts are guided multi-step workflows the assistant can invoke. Pick one from the prompt menu and the assistant runs the right sequence of tool calls automatically.

| Prompt | What it does |
|--------|--------------|
| `find_gene_tfs` | Surface all TFs that regulate a gene, ranked by binding evidence |
| `compare_genes_regulation` | Compare two genes' regulatory landscapes; highlight shared TFs |
| `region_regulatory_analysis` | Analyze every CRE in a genomic region and the TFs active there |
| `disease_snp_analysis` | Find disease-associated SNPs and analyze their regulatory impact |
| `explore_cre_expression` | Explore one CRE's expression pattern, binding TFs, and nearest gene |

### Resources (2)

| Resource URI | Content |
|--------------|---------|
| `organisms://list` | Supported organisms (human/hg38/GRCh38, mouse/mm10/mm39) |
| `schema://database` | Detailed table-by-table description of the underlying D1 schema |

## Example conversations

### Gene-centric: regulatory landscape of TP53

> **You:** What's the regulatory landscape around TP53?

The assistant calls `get_gene_regulatory_summary(gene_id="TP53")` — one tool call returns the gene record, top associated CREs by distance from TSS, and the dominant TFs binding those CREs.

### Comparing two genes

> **You:** Compare TP53 and BRCA1 — do they share regulatory elements or transcription factors?

The assistant runs `get_gene_regulatory_summary` for each gene in parallel, then `find_shared_regulators(gene_ids=["TP53","BRCA1"])`, then synthesizes which TFs are shared and which are unique.

### Disease variant exploration

> **You:** Find SNPs associated with type 2 diabetes and tell me what regulatory elements they could disrupt.

The assistant calls `search_snps(query="type 2 diabetes")`, then for the top hits with `cre_count > 0` calls `get_snp_regulatory_impact(snp_id=...)` to get the SNP info, affected CREs, and bound TFs in one call.

### Synonym resolution

> **You:** What does the IFI15 gene do?

Even though `IFI15` is a synonym (not the official symbol), `get_gene_details(gene_id="IFI15")` resolves to ISG15 and returns its full record.

## Query tips

### Identifiers

- **Genes:** Ensembl IDs, symbols, or synonyms all work
- **SNPs:** rs IDs (`rs10001548`)
- **CREs:** human IDs start with `FCHS_`, mouse with `FCMM_`. CRE *names* often follow `cp<N>@<gene_symbol>` (e.g. `cp1@DDX11L1`)

### Wildcard search (`search_cres` only)

| Pattern | Matches |
|---------|---------|
| `FCHS_1*` | All human CREs whose ID starts with `FCHS_1` |
| `FCMM_1??` | Mouse CREs with exactly 3 digits after `FCMM_1` |
| `cp1@*` | CREs whose name starts with `cp1@` |

### Organism filter

Accepted values everywhere:

- `human`, `hg38`, or `GRCh38` (all map to human)
- `mouse`, `mm10`, or `mm39` (all map to mouse)
- `any` to skip the filter (default)

## Troubleshooting

### Empty results

- Try alternative identifiers (gene symbol vs Ensembl vs synonym)
- Drop the organism filter to `any`
- For wildcard searches, prefix with at least 2 literal characters (`FCHS_*` rather than `*`)

### Rate limits

Per-endpoint hourly limits apply (e.g. 500/hr for ID lookups, 100/hr for full-text search, 50/hr for batch). Hitting one returns `RATE_LIMIT_EXCEEDED` with a `retryAfter` value in seconds. Interactive conversations rarely notice these.
