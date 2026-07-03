# Resume-Generator
Generate a resume based off provided information.

## Application design

The initial application will use Next.js as the main framework for both the browser experience and the server-side API layer. The application will be divided into the following sections so that user interaction, sensitive operations, document generation, and storage have clear responsibilities.

```text
Next.js application
├── Static information
│   ├── Landing page
│   ├── Privacy and data-use information
│   └── Help and documentation
├── Interactive resume workspace
│   ├── Career-bank editor
│   ├── Job-listing input
│   ├── Tailored-resume review
│   └── Export controls
├── Server-side operations
│   ├── LLM and embedding requests
│   ├── Listing analysis, matching, and selection
│   ├── Resume validation
│   └── Document generation
└── Storage
    ├── Local browser storage for the prototype
    └── Server database and file storage when accounts are introduced
```

### Static information

These pages explain the product, how to use it, and how resume data is handled. They can be rendered as static content because they do not depend on a user's resume data.

### Interactive resume workspace

This is the main client-side experience. Users will create and edit their career bank, paste a job listing, review the selected content, reorder or remove bullets, and request a document export. Fast form validation and temporary local persistence will happen in the browser.

### Server-side operations

Server-side code will protect API keys and handle operations that should not run in the browser. This includes LLM requests, embedding generation, listing analysis, matching, constraint-based selection, truthfulness and parsing checks, and generation of the final document.

### Storage

The prototype will store the career bank and drafts locally in the browser to reduce infrastructure and privacy complexity. A server-side database can be introduced when user accounts, cross-device access, saved application history, or shared data are needed. Exported resume snapshots may require separate file or object storage.

### Optional supporting services

The initial version should remain a single Next.js application. A separate worker or document-rendering service should be added only if long-running imports, batch embedding jobs, page-count measurement, or higher-fidelity document conversion cannot be handled reliably within the main application.

## Technology choices

### Confirmed

- **Next.js** — main full-stack framework for pages, the interactive application, and the server-side API layer
- **TypeScript** — primary language for application code, shared schemas, and domain logic
- **JavaScript** — underlying runtime and ecosystem, with plain JavaScript used only where TypeScript is unnecessary or unsupported
- **Tailwind CSS** — styling and responsive user-interface layout

### To be decided

- **Database** — begin with browser storage for the prototype; select a server database before accounts or cross-device persistence are added
- **Schema validation** — choose a runtime validation library so browser and server code enforce the same resume schema
- **Document generation** — evaluate a TypeScript DOCX library first; introduce a Python renderer only if document fidelity requires it
- **LLM and embedding provider** — select based on structured-output reliability, privacy controls, cost, and latency
- **Authentication and file storage** — defer until server-side accounts and saved exported artifacts are in scope



### Phase 1 — Bare-bones prototype (bank in → tailored .docx out)

- [ ] Define the JSON schema (contact, education, certs, roles, bullets with stable IDs, skills)
- [ ] Build the manual bank-entry UI writing to the schema (doubles as review/edit surface)
- [ ] Listing input → keyword extraction with first-pass importance weighting
- [ ] Semantic matching: embeddings + synonym/skills map, scored against weighted terms
- [ ] Simple selection: top-scoring items within a hard length cap
- [ ] Non-bullet passthrough (contact/education/certs/titles/dates flow untouched)
- [ ] Basic review UI: show selected bullets, allow remove/reorder
- [ ] .docx renderer: python-docx, single column, standard fonts, real text, no load-bearing tables/text-boxes
- [ ] Minimal client-side storage (IndexedDB/local)

### Phase 2 — Correctness and trust

- [ ] Constraint-aware selection: best set, not top-N (page fit, role balance, recency, keyword-repeat cap)
- [ ] Bounded rephrasing (LLM): listing verbiage, hard constraint against new claims; optional conservative→assertive dial
- [ ] Anti-stuffing / readability guard for the human second reader
- [ ] Parse validator (primary): .docx → fields → diff against schema; loop, not pass/fail; frame as "parses cleanly in our reference check"
- [ ] Truthfulness validator (secondary): diff final bullets against bank, flag unsupported claims
- [ ] Edge-case handling (sparse bank, thin bank, over-length, zero matches, repeats, missing sections)
- [ ] Structured LLM output reliability (enforce structure; handle malformed JSON, refusals, retries)

### Phase 3 — Quality and adoption

- [ ] Bank-quality pass: flag missing metrics and vague verbs, prompt to quantify (highest-leverage feature — build early)
- [ ] Gap report in review: listing keywords nothing in the bank covers
- [ ] Parser-assisted import that pre-fills the manual form (bad parse degrades to "fix a few fields")
- [ ] Cold-start onboarding: guided "paste old resumes → extract bullets into bank"
- [ ] Regression/eval set: hand-labeled listing-to-bank pairs, run on every scoring/prompt/model change
- [ ] Per-listing section selection and ordering (skills-forward vs. experience-forward)
- [ ] Summary line generation: listing-specific, from true facts, bounded like bullets

### Phase 4 — Hardening and scale

- [ ] Storage/retention hardening (server-side: encrypt at rest, retention policy)
- [ ] Cost/latency/caching (cache bank embeddings, avoid re-calls on minor edits, batch)
- [ ] Job-title handling (map/annotate vs. leave as-is)
- [ ] Scope boundaries (rule cover letters, LinkedIn summaries, multi-doc in or out of v1)


