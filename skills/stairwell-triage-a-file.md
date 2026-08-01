---
name: Triage a suspicious file with Stairwell
description: Pull AI + static analysis, sandbox detonation, sightings, and variants for a file hash to decide if it is malicious and where it has been seen.
api: https://app.stairwell.com/v1
operations: [GetObjectMetadata, SummarizeFile, TriggerObjectDetonation, GetObjectDetonation, ListObjectSightings, ListObjectVariants, CreateObjectOpinion]
source: https://docs.stairwell.com/reference
---

# Triage a suspicious file with Stairwell

Use this to turn a single file hash (SHA-256) into a verdict and blast-radius.

## Auth
All calls go to `https://app.stairwell.com/v1` with header
`Authorization: Bearer <API token>` (Settings > Auth tokens, type API/CLI).

## Steps
1. **GetObjectMetadata** — fetch hashes, the MalEval verdict, network indicators, and YARA matches for the object.
2. **SummarizeFile** — get an AI-generated summary of the file's behavior and intent.
3. If dynamic analysis is needed: **TriggerObjectDetonation** to submit the object to the sandbox (returns pending), then poll **GetObjectDetonation** for behavioral artifacts, MITRE ATT&CK TTPs, and dropped files.
4. **ListObjectSightings** — enumerate which assets in your environments saw the file, when, and at what path (scopes the incident).
5. **ListObjectVariants** — pivot to structurally similar files (TLSH/imphash) to catch polymorphic variants.
6. **CreateObjectOpinion** — record the analyst verdict so it propagates to your team.

## Conventions
- List calls paginate with `page_size` / `page_token`; follow `next_page_token`.
- Detonation is asynchronous: submit, then poll — do not block.
- Errors follow the google.rpc.Status model; 401 = bad token, 403 = wrong environment scope. See errors/.
