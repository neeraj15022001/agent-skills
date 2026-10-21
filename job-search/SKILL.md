---
name: job-search
description: Find and evaluate current job openings against a supplied resume and preferences; save matches and search history.
---

# Job Search

Read `references/priority-hierarchy.md`, `references/data-directory.md`, and `references/prerequisites.md` for runtime conventions and inline setup. Accept search terms from the user's message; otherwise use target roles in preferences. This is a single search run. Schedule recurring searches only when requested, using available Codex automation tools.

1. Load candidate facts, preferences, optional contacts, and existing history. Keep a snapshot of prior job URLs/IDs for deduplication before appending this run.
2. Follow `references/browser-setup.md` to search current openings. Honor specified sites; otherwise use employer careers sites and job boards, including Hiring.cafe when useful. Apply role, location, and recency filters. Extract bounded listing batches rather than whole-page dumps.
3. Resolve employer/ATS posting links and verify role status. Record title, company, location, remote eligibility, compensation with currency/period, posting date if available, source URL, and retrieval date. Mark unknown fields explicitly. If direct links or full postings cannot be verified, identify that limitation rather than inventing a URL or presenting a closed role as open.
4. Evaluate using `references/fit-scoring.md`; `scripts/evaluate-jobs.md` gives an optional structured evaluation format. Apply confirmed dealbreakers before scoring. Separate unknown requirements from actual mismatches.
5. Append evaluated jobs to `DATA_DIR/job-history.md` using `assets/templates/job-entry.md`. Use `assets/templates/job-history-init.md` only for a new history file. Save High-fit postings in collision-resistant job folders per the data-directory reference. Reuse matching folders and preserve earlier artifacts.
6. Present new High/Medium fits with direct posting links, evidence for fit, important gaps, salary/location, and optional matched network contacts. Distinguish no matches from inaccessible or unsearched sources. Do not silently treat an unverified company alias as a confirmed network connection.
7. Complete any already-requested tailoring or application work via the installed `tailor-resume`, `cover-letter`, or `apply` skills. Otherwise suggest a useful next action without doing external outreach. Persist preference changes only when the user supplies them.

Use `assets/templates/matching-rules.md` only as an optional scaffold for `DATA_DIR/preferences.md`; do not adopt its sample constraints.
