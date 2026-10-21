# ATS Interaction Notes

Identify the actual host, page, and controls before acting. URL patterns suggest a platform; they do not guarantee a fixed form or tool capability.

- **Greenhouse**: Often hosted on `boards.greenhouse.io` or `job-boards.greenhouse.io`, sometimes embedded in an employer page. Use the current tool's supported frame operations if available. If an embedded form is inaccessible, follow the observed employer/ATS link or iframe source using supported inspection. Preserve the specific job identity and required URL parameters; do not invent an embed URL.
- **Lever**: Often hosted on `jobs.lever.co`. Follow the observed application link. Inspect custom questions, combobox suggestions, required attachments, and consent controls; do not presume all fields are native inputs.
- **Workday**: Often hosted on a `myworkdayjobs.com` subdomain. Authentication and wizard layout vary by employer. Use an existing authorized session or request the necessary sign-in action. Inspect every wizard page, including conditional fields and review. Do not assume a fixed page count or prefilled country is correct.
- **Other ATS**: Use observed labels and live validation. Do not transfer assumptions about required fields, consent, or upload limitations from another platform.

Use supported element/label operations first, then documented visual interaction when necessary. Refresh state after selection/navigation. Read validation without intentionally submitting blank forms or declarations to discover fields. A failed or incomplete scan is a limitation to report, not permission to guess.
