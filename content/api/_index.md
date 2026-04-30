---
title: "fanta.bio Web API"
linkTitle: "API"
description: "REST API for programmatic access to fanta.bio's functional genome annotations — CREs, genes, SNPs, expression, and ChIP-Atlas TF binding."
weight: 2
---

The fanta.bio REST API gives programmatic access to the same functional genome annotations powering the website — CREs, genes, SNPs, expression data, and ChIP-Atlas transcription factor binding. It is the foundation for tools, notebooks, and the [MCP server](/mcp/).

{{< cards >}}
  {{< card link="https://api.fanta.bio/docs" title="Interactive API explorer" subtitle="Scalar UI — try every endpoint in your browser" icon="external-link" >}}
  {{< card link="https://api.fanta.bio/openapi.json" title="OpenAPI 3.1 spec" subtitle="Machine-readable JSON — generate clients in any language" icon="code" >}}
{{< /cards >}}

## Base URL

```
https://api.fanta.bio
```

## What's available

The API exposes four resource families plus a unified search endpoint:

| Resource | Purpose | Example |
|----------|---------|---------|
| **CREs** | Cis-regulatory elements with coordinates, expression, and TF binding | `/v0/cres/FCHS_1` |
| **Genes** | Gene records resolved by Ensembl ID, symbol, or synonym | `/v0/genes/TP53` |
| **SNPs** | GWAS-associated variants with trait annotations | `/v0/snps/rs10001548` |
| **Analysis** | Cross-database aggregations (shared regulators, regulatory summaries) | `POST /v0/analysis/shared-regulators` |
| **Unified search** | One query fans out to genes + SNPs + CREs in parallel | `POST /v0/search` |

For the full endpoint list and request/response shapes, browse the [interactive docs at api.fanta.bio/docs](https://api.fanta.bio/docs).

## Quick examples

### Search for a gene

```bash
curl "https://api.fanta.bio/v0/genes?query=TP53"
```

The gene resolver accepts Ensembl IDs (`ENSG00000141510.20`), official symbols (`TP53`), or synonyms (`p53`) — all three resolve to the same canonical record.

### Find CREs near a SNP

```bash
curl "https://api.fanta.bio/v0/snps/rs10001548/cres"
```

### Unified cross-entity search

```bash
curl -X POST "https://api.fanta.bio/v0/search" \
  -H "Content-Type: application/json" \
  -d '{"query": "TP53", "limit": 10}'
```

The endpoint auto-detects entity-shaped queries: `rs10001548` → SNPs only, `ENSG00000141510.20` → genes only, free text → all three in parallel.

### Cross-database analysis

```bash
curl -X POST "https://api.fanta.bio/v0/analysis/gene-regulatory-summary" \
  -H "Content-Type: application/json" \
  -d '{"gene_id": "TP53", "cre_limit": 50}'
```

Returns the gene record, associated CREs sorted by distance from TSS, and the dominant transcription factors binding those CREs — in a single request.

## Response format

All responses follow the same envelope:

```json
{
  "success": true,
  "data": [...],
  "meta": {
    "version": "v0",
    "count": 20,
    "limit": 20,
    "offset": 0,
    "hasMore": true
  }
}
```

Errors return `{success: false, error: {code, message, details?}}` with HTTP status codes that match the error class (404, 400, 429, etc.).

## Features at a glance

- **FTS5 full-text search** with weighted BM25 and exact-match boost — `TP53` ranks ahead of `TP53TG3`
- **Synonym resolution** — `p53`, `IFI15`, official symbols, and Ensembl IDs all work
- **Wildcard filters** — `*` (any chars), `?` (single char) on CRE filter endpoints
- **Rate limiting** with per-endpoint hourly limits (returns `RATE_LIMIT_EXCEEDED` with `retryAfter`)
- **Response caching** for idempotent reads
- **OpenAPI 3.1** for type-safe client generation in any language
