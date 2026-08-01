---
name: Hunt the historical corpus with a YARA rule
description: Evaluate a YARA rule ad-hoc against Stairwell's historical object corpus, then persist and manage it if it is useful.
api: https://app.stairwell.com/v1
operations: [ScanYaraRule, CreateYaraRule, QueryYaraRuleMatches, ListYaraRules, UpdateYaraRule, DeleteYaraRule]
source: https://docs.stairwell.com/reference
---

# Hunt the historical corpus with a YARA rule

Use this to test a detection idea against history before operationalizing it.

## Auth
`https://app.stairwell.com/v1`, header `Authorization: Bearer <API token>`.

## Steps
1. **ScanYaraRule** — run the rule body ad-hoc against the historical corpus. Combines candidate-file lookup (partial binary indexes) with variant discovery for any SHA256/SHA1/MD5 literals in the rule. Returns matches synchronously (up to `max_matches`). The rule is NOT persisted; `environments` controls scope (empty = every readable environment).
2. If the rule is valuable, **CreateYaraRule** to persist it in an environment for continuous, retroactive detection.
3. **QueryYaraRuleMatches** — list objects matching a saved rule.
4. **ListYaraRules** / **UpdateYaraRule** / **DeleteYaraRule** — manage the rule set and states.

## Conventions
- `ScanYaraRule` is evaluation-only and synchronous; `CreateYaraRule` is what enables ongoing detection + webhook notifications (yara_rule_match; see asyncapi/).
- Paginate match lists with `page_size` / `page_token`.
- Errors follow google.rpc.Status (see errors/).
