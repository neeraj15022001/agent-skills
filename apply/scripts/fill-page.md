# Fill One Application Page

Read the containing skill's `references/browser-setup.md`. This is a local procedure, not an executable or mandatory agent. Receive the existing tab, field mapping `{label, value, observed_target}`, relevant authorization, and exact resume/letter paths.

1. Refresh page state, locate each field by observed label/context, and preserve unrelated prefilled values. If prefilled facts conflict with confirmed answers, resolve the discrepancy before changing them.
2. Fill known answers with supported browser operations. Select actual options for dropdowns/comboboxes and inspect conditional fields revealed by selection. Scroll as needed. Do not assume any platform hides radio buttons or supports a particular tool method.
3. Upload the exact approved PDF/DOCX when supported. Check file type, displayed filename, and completion. Otherwise report the field and absolute path for manual upload.
4. Verify displayed values and validation state. Reinspect stale targets and try a supported alternative once; then report the failure. Never guess a required answer, legal declaration, or consent.
5. Return filled fields, failures with reasons, missing answers, uploaded files, manual uploads, current page title, and whether a review page is visible. Use structured JSON if the caller needs machine-readable results.

The parent workflow owns page advancement and submission. This procedure does not click Next, Save and Continue, Submit, or Send. Do not persist new answers or delegate control of this tab concurrently.
