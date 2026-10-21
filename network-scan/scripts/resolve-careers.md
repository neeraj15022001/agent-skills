# Resolve Company Careers URLs

Input: company names and any supplied disambiguating company context. Search with available Codex web tools for official careers pages, using company names rather than private contact information. Inspect the employer source or employer-linked ATS to verify association; never promote a remembered URL or search snippet to a verified result without opening it.

Return a JSON object keyed by company name with `careers_url`, `type`, and optional `error`. Types: `direct`, `greenhouse`, `lever`, `workday`, `other_ats`, or `not_found`. Use null for an unresolved URL. Distinguish ambiguity/access failure from a verified absence. Do not resolve ambiguous names by arbitrarily adding 'tech'.

Prefer the verified official careers page; preserve an employer-linked ATS URL when that is the hiring destination. Process remaining companies if one fails. The calling workflow owns cache writes and timestamps.
