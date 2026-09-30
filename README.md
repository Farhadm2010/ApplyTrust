# ApplyTrust

**AI-assisted job application workflows with career profiles, resume selection, application queues, and human-reviewed role briefs.**

ApplyTrust is a Chrome extension that organizes the job application process around verified information and human control. It keeps the user's career profile in local browser storage, helps choose the right resume for each role, manages an application queue, fills in standard application forms with verified facts, and keeps a clear record of what it did. The user stays in charge of every consequential step.

> This repository is a portfolio overview. It has documentation only. The extension's source code is private and not published here.

## Business problem

Applying for jobs is repetitive: people retype the same facts, choose between resume versions, and track dozens of applications across different employer sites. General-purpose automation tools can make this faster, but they risk submitting wrong or invented information, exposing personal data, or acting without the applicant's approval.

**Intended users:** job seekers who manage many applications and want help with routine steps without handing control to an automated agent.

## Main capabilities

Implemented in the current development build:

- **Career profile:** the user enters their career facts, which are stored in local browser storage. ApplyTrust does not invent career data.
- **Application documents:** a library of resumes and cover letters, with resume selection for each job.
- **Job intake and application queue:** import job listings, track each application through clear stages, and group the queue by employer.
- **Verified form filling:** on supported application systems (starting with Greenhouse), ApplyTrust detects the application form and fills standard fields from verified profile facts.
- **Role Impact Briefs:** job-specific briefs that the user drafts, edits, and approves, then chooses whether to include with an application. Briefs can be exported to PDF or Word.
- **Action receipts:** a local, append-only record of meaningful actions such as fields filled, documents attached, briefs approved, and confirmations observed.
- **Progress view:** each application shows where it stands, from job selection to confirmation.

Planned: support for additional application systems and broader job discovery, each added only after a policy review.

## My role and contributions

I lead ApplyTrust as **AI Product & Transformation Lead at [Allsaat](https://allsaat.com)**:

- Set product direction, scope, and prioritization.
- Designed the application workflow and the trust and privacy model.
- Defined the human approval gates, submission policy, and sensitive-data rules.
- Coordinated AI-assisted development and ran iterative validation of each release, including checks of the packaged extension.
- Own requirements and acceptance decisions. I use several AI coding tools to support implementation.

## AI capabilities and human-review boundaries

**AI assistance is optional.** If AI is turned off or unavailable, the core product still works using deterministic rules.

AI features inside the product, when enabled:

- Job-description extraction, job-fit assessment, and resume recommendations.
- Optional drafting of answers, cover letters, and Role Impact Briefs for the user to review.
- Every AI output is checked against a defined structure. Invalid output is rejected.
- The user supplies their own key for the configured AI provider (currently OpenAI). The key is held in session storage or memory only for the current browser session, and is never written to persistent local storage.

Privacy boundaries:

- Career data is stored in local browser storage, and ApplyTrust has no cloud backend.
- When AI assistance is turned on, ApplyTrust sends selected, sanitized inputs to the configured AI provider, limited to what each task needs. Those inputs leave the device and are handled by that provider.
- Passwords, one-time codes, API keys, demographic answers, and similar sensitive data are never sent to AI.
- The AI is not allowed to infer sensitive characteristics such as gender, age, ethnicity, or disability.

Human approval:

- **Review before submit** is the default, and in this mode ApplyTrust never clicks the employer's Submit button. The user submits on the employer's website.
- An optional auto-submit mode is implemented in code as a policy-gated path. It is off by default and has not been validated on live applications. The user must opt in explicitly in settings, and even then ApplyTrust submits only when every safety check passes: fit threshold met, correct resume selected, required fields validated, no pending CAPTCHA or sign-in step, and no unresolved work-authorization, sponsorship, salary, or legal questions. If any check fails, the application goes back to the user for review.
- Applications that are already confirmed, or that have an unclear earlier submit attempt, are never resubmitted.
- AI cannot click, navigate, submit, or write arbitrary fields. Free-form AI text never triggers browser actions.
- An application is recorded as submitted only after confirmation is observed, never just because Submit was clicked.

## How it works

```text
Import job → Select resume → Queue → Open employer application → Detect form
→ Fill verified fields → Review exceptions → Submit (by the user, by default) → Confirm → Record receipt
```

1. Add a job to the queue and choose the resume to use.
2. Open the employer's application page. ApplyTrust detects the form.
3. Standard fields are filled from verified profile facts.
4. Anything uncertain is flagged for the user to review.
5. The user reviews and submits on the employer's site. Auto-submit is used only if the user opted in and every check passes.
6. ApplyTrust watches for the confirmation and records an action receipt.

## Validation status

- **Validated in the packaged extension:** the Role Impact Brief flow (import, draft, edit, approve, export, and persistence after reload), and detection of a Greenhouse application form on a live public job page. No application was submitted.
- **Implemented and covered by automated tests; live check pending:** verified-field filling on Greenhouse forms.
- **Implemented in code, not yet validated live:** the optional auto-submit mode. There is no record yet of it being tested on a live application.

## Status

ApplyTrust is under active development and is not published in the Chrome Web Store. This overview makes no claims about users or outcomes.

## Links

- Profile: [github.com/Farhadm2010](https://github.com/Farhadm2010)
- Allsaat: [allsaat.com](https://allsaat.com)
