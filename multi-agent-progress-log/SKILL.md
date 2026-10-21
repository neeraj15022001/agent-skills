---
name: multi-agent-progress-log
description: Maintain a shared, chronological progress log (PROGRESS_LOG.md) in a coding project so work can hand off seamlessly between different coding agents/assistants (Claude, Gemini, ChatGPT/Codex, etc.) — for example when one agent's usage limit or context runs out mid-task and another picks up where it left off. Use this skill at the START of any coding task in a repository to check for and read an existing PROGRESS_LOG.md before doing anything else, and use it again after finishing each meaningful step of the work to append a new log entry. Trigger this whenever the user mentions switching agents, resuming work, continuing where another AI left off, running out of tokens/messages/usage, multi-agent handoff, or working across Claude/Gemini/ChatGPT/Codex on the same codebase — and also proactively, on every coding task, to check whether the project already has one of these logs in place.
---

# Multi-Agent Progress Log

A lightweight, agent-agnostic handoff protocol. A single Markdown file, `PROGRESS_LOG.md`, lives in the project root and acts as shared short-term memory across whichever coding agent (Claude, Gemini, ChatGPT/Codex, or others) is currently working on the codebase. Because different agents don't share context or memory, this file is the only thing that does.

Follow this protocol any time you (the agent) work on a coding task inside a project directory.

## 1. At the start of every session — check and read the log

Before writing any code or making a plan:

1. Look in the project root for `PROGRESS_LOG.md`.
2. **If it exists:** read the whole file, but pay closest attention to:
   - The **most recent entries** (what was just done, by whom).
   - The **"Next step"** field of the latest entry — this is your starting point.
   - Any **key decisions** logged earlier that constrain how you should proceed (architecture choices, naming conventions, libraries picked, things explicitly rejected and why).
   - If there's an `ARCHIVE_INDEX` section at the top (see §4), skim the summaries there too — don't skip it just because it's old.
3. **If it doesn't exist yet:** this is either a brand-new project or the first time this protocol is being used on it. Create the file (template in §3) before you make your first change, and say briefly that you're setting it up.
4. If you were already working on this project earlier in *this same conversation* and nothing suggests another agent has touched it since, you don't need to re-read the whole file again — but always re-read it if there's any chance work happened elsewhere (new session, user mentions using another tool, time has clearly passed).

Never assume you know the current state of the project from your own prior turns alone — another agent may have changed things since. The log is the source of truth, not your memory of the conversation.

## 2. After each meaningful step — append an entry

Log an entry after completing a meaningful unit of work — not after every trivial edit. A "meaningful step" is roughly one of:

- A subtask or feature is completed (or a clear sub-part of one).
- A non-trivial decision was made (a library choice, an architectural pattern, a naming/schema decision, a trade-off) — especially if you rejected an alternative.
- You hit a blocker, error, or open question that the next agent (possibly you, later) needs to know about.
- You're about to run out of room to continue (context getting long, task naturally pausing, handing off).

Don't log: single-line fixes, formatting-only changes, or exploratory reads that didn't change anything.

Each entry MUST capture all four of:
1. **What was done** — concrete summary, not vague ("implemented X" not "made progress").
2. **Files touched** — paths, one per line or comma-separated.
3. **Key decisions & reasoning** — any choice made and *why*; if none, write "None."
4. **Next step** — the concrete thing whoever reads this next should do first. This is the most important field — never leave it empty or vague.

Append entries to the **bottom** of the log (chronological, oldest first), using the template in §3. Never edit or delete a previous agent's entry — the log is append-only history. If something an earlier entry said turns out to be wrong, add a new entry noting the correction; don't rewrite the old one.

## 3. Entry format

Use this exact structure for the file and for each entry:

```markdown
# Progress Log

> Shared cross-agent progress log. Read the latest entries before starting work; append a new entry after each meaningful step. Do not edit or delete past entries.

---

## 2026-09-28 14:32 UTC — Claude
**Did:** Implemented the CSV export endpoint and wired it into the reports page.
**Files:** src/api/export.ts, src/pages/Reports.tsx
**Decisions:** Used streaming response instead of buffering the whole CSV in memory, since export files can be large (>50k rows). Rejected the `csv-writer` package in favor of a hand-rolled streamer to avoid adding a dependency for something this small.
**Next:** Add a loading/progress indicator on the Reports page while the export streams; not started yet.

---

## 2026-09-28 15:10 UTC — Gemini
**Did:** Added the loading indicator for CSV export from the previous entry's next step.
**Files:** src/pages/Reports.tsx
**Decisions:** None.
**Next:** Write tests for src/api/export.ts — none exist yet, and the streaming logic is easy to get wrong at row boundaries.

---
```

Rules for filling this in:
- **Timestamp:** use `YYYY-MM-DD HH:MM UTC` (24-hour clock, UTC so it's unambiguous across agents/timezones). If you can't determine the real current time, use the most precise time you have available and note it's approximate rather than omitting it.
- **Agent name:** use a clear, consistent name — `Claude`, `Gemini`, `ChatGPT` (or `Codex` if that's the specific tool). If you know your specific model version and it's relevant (e.g. two different Claude models worked on this), you may add it in parentheses, e.g. `Claude (Sonnet)`.
- Keep each field to 1-4 sentences — detailed enough to be useful, not a full transcript.
- Separate entries with a `---` horizontal rule for readability.

## 4. Keeping the log manageable (archiving)

Long-running projects will grow this file large enough that reading it fully becomes expensive. Once `PROGRESS_LOG.md` exceeds roughly **40 entries or ~300 lines**, archive the older half:

1. Create (or append to) `progress-log-archive/` in the project root if it doesn't exist.
2. Move the oldest entries (everything except roughly the most recent 15-20) into a new dated file there, e.g. `progress-log-archive/2026-09-01_to_2026-09-20.md`, verbatim.
3. At the top of the now-trimmed `PROGRESS_LOG.md`, maintain a running `## Archive Index` section (above the entries) with one line per archive file summarizing the phase of work it covers, e.g.:
   ```markdown
   ## Archive Index
   - `progress-log-archive/2026-09-01_to_2026-09-20.md` — initial scaffolding, auth, and DB schema design.
   ```
4. Keep the most recent entries in the live file so a resuming agent gets full detail on recent work without opening archives, but can still find older context if needed.

Do this archiving as its own logged step (log that you did it, and why, per the normal entry format) so it's visible in history.

## 5. Practical notes

- The log lives in the project root as plain Markdown so it's human-readable, diffable in git, and easy for any agent's file tools to open with no special parsing.
- If the project already has some other convention for this (e.g. a `CHANGELOG.md` used the same way, or the user says they already track this differently), don't create a second, competing file — ask, or adapt to what's there.
- This log is about *work continuity*, not a replacement for git commit messages or code comments — keep entries focused on what a cold-starting agent needs to pick up the thread, not a full diff narration.
- If you're mid-step and about to run out of context/usage, prioritize writing a good "Next step" over finishing the step yourself — a clear handoff beats a half-finished silent edit.
