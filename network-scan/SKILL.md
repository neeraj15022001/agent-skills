---
name: network-scan
description: Find current job openings at companies represented in a user-supplied LinkedIn contacts export, matched to resume and preferences.
---

# Network Scan

Read `references/priority-hierarchy.md`, `references/data-directory.md`, and `references/prerequisites.md`. Run once unless the user requests scheduling. Use a user-supplied contacts export; do not require LinkedIn scraping.

1. Load candidate facts, preferences, CSV, and history. Accept a positive contact limit or `all`; default to 25. Locate the actual CSV header if export metadata precedes it. Parse available connection dates, sort newest first, keep undated contacts after dated contacts, and report missing required columns rather than guessing.
2. Select contacts, group nonempty company names, and retain names/titles/profile URLs for matching. Normalize punctuation and case conservatively; resolve ambiguous companies using supplied context without merging unrelated employers. Do not include private contact details in public search queries.
3. Load `DATA_DIR/company-careers.json`. Skip `ignored` entries. Reuse careers URLs checked within seven days; resolve stale/missing entries using `scripts/resolve-careers.md`. Fresh URLs are a routing cache, not evidence that cached jobs remain open. Recheck job listings each run.
4. Follow `scripts/evaluate-company.md` and `references/browser-setup.md` to inspect current openings. Work sequentially by default. Record each company's scan status and coverage; failed or partial scans are not zero matches. Evaluate matches using `references/fit-scoring.md`.
5. Merge cache results without discarding previous records. Preserve the existing `careers_url`, `type`, `last_checked`, and `last_found_roles` fields; optional `scan_status`, `last_scan_attempt`, and `error` distinguish partial/failed scans. Update `last_checked` only after successful URL verification or scanning, and role counts only from successful scans.
6. Append companies checked, matches, coverage, and failures to `DATA_DIR/network-scan-history.md`. Deduplicate by canonical posting URL/ID against previous history. Save full High-fit postings under per-job folders using the data-directory reference. Include original source URL, retrieval date, fit evidence, and contact attribution.
7. Present matches with company, role, fit, location/salary when known, posting link, and the relevant contact. Report inaccessible companies separately from confirmed no-match companies. Do not send messages or ask contacts for referrals unless explicitly authorized.

Persist user feedback: set `ignored: true` for skipped companies, correct careers URLs, and update preferences. When tailoring or application work is requested, invoke the relevant installed skill inline.
