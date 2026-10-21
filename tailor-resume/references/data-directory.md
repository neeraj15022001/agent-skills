# Data Directory and First Use

Resolve `DATA_DIR` in this order: an explicit user-selected data directory; an existing `.proficiently/` in the current project; an existing `~/.proficiently/`; otherwise a new `.proficiently/` in the current persistent workspace. Preserve existing data. If the workspace is temporary, request a persistent destination before promising durable storage; drafting can proceed from supplied materials meanwhile.

The `.proficiently` name is a backward-compatible data folder, not a required plugin. Initialize missing directories inline as needed under the session's filesystem permissions. No separate setup command is required. Never move data into the installed skill directory.

- `resume/`: user-provided originals; do not overwrite originals with tailored versions.
- `preferences.md`: confirmed target roles, location/remote constraints, salary amount/currency/period, must-haves, nice-to-haves, and dealbreakers. Unknown preferences stay unknown.
- `profile.md`: optional additional work history and factual corrections, attributed to the appropriate employer and dates.
- `linkedin-contacts.csv`: user-supplied contacts export, only needed for network scans.
- `job-history.md`, `network-scan-history.md`: append-only search/scan records when those workflows run.
- `company-careers.json`: careers URL cache for network scans.
- `application-data.md`: confirmed application answers; collect only fields needed for the requested application.
- `jobs/`: per-posting artifacts.

Reuse a job folder only when its saved canonical posting URL or requisition ID matches. For new jobs use `jobs/[company-slug]-[role-slug]-[job-id-or-short-url-hash]/`; do not key solely by company/date because multiple roles can collide. Reuse matching legacy folders. Store source URL or supplied-text provenance and retrieval date in `posting.md`. For text without a URL, use a short content hash. Preserve earlier drafts when making a materially different revision.

User-provided attachments or pasted resume text are valid inputs. Use an available PDF/documents skill for file extraction or export when needed. Do not require a PDF upload when usable resume text is already supplied. Render and inspect requested final documents; never rename Markdown to .pdf or .docx.
