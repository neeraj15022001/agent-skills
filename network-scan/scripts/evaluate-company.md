# Evaluate Current Openings at Companies

Input: company names/careers URLs, resume summary, preferences, and relevant contact attribution. Read the containing skill's `references/browser-setup.md` and `references/fit-scoring.md`.

For each company, inspect current listings with available web tools or a browser when interaction is required. Use relevant keyword variants and observed filters. Inspect promising postings for requirements, open status, location/remote eligibility, compensation, title, and canonical URL. Record how much of the listing/pagination was inspected; partial coverage cannot prove no matches.

Return one record per company: `company`, `careers_url`, `scan_status` (complete/partial/failed), `total_roles_seen` (null when unknown), `matching_roles`, `coverage`, and optional `error`. Each matching role includes title, location, URL, fit, supporting evidence, unknowns, and contact attribution. Include High/Medium fits; distinguish a completed scan with zero matches from failed access.

Use one controlled browser tab per sequential workflow. Do not fill applications or contact anyone. The caller owns history/cache writes. If the caller explicitly delegates independent read-only batches, each worker must have isolated tool state and return results without mutating shared files.
