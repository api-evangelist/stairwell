---
name: Sweep your environment for newly disclosed IOCs
description: Upload or query a threat report's IOCs and find which files/assets in your environment match, for rapid response to a new disclosure.
api: https://app.stairwell.com/v1
operations: [CreateThreatReport, ListThreatReportIocs, QueryThreatReportsMatchingIOCs, ListThreatReportMatches, ListObjectMetadata, GenerateRunToGround]
source: https://docs.stairwell.com/reference
---

# Sweep your environment for newly disclosed IOCs

Use this when a new threat report or IOC set drops and you need to know if you are affected.

## Auth
`https://app.stairwell.com/v1`, header `Authorization: Bearer <API token>`.

## Steps
1. **CreateThreatReport** — upload the report; Stairwell extracts and correlates its IOCs.
2. **ListThreatReportIocs** — review the extracted IOCs (hashes, hostnames, IPs, YARA).
3. **ListThreatReportMatches** — list files in your environment whose metadata matches the report's IOCs (full object metadata per hit).
4. Alternatively, **QueryThreatReportsMatchingIOCs** — given an ad-hoc IOC set, find which threat reports reference them.
5. For any confirmed hit, **GenerateRunToGround** — expand a single IOC into the full incident: all variants and their presence across assets.
6. Use **ListObjectMetadata** with a CEL filter for custom corpus queries.

## Conventions
- CEL filter expressions drive `ListObjectMetadata` (see docs/common-expression-language-cel).
- Retroactive matching means historical files re-match as intel evolves — re-run periodically.
- Paginate with `page_size` / `page_token`.
