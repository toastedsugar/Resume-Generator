# Technical Implementation Checklist

This checklist translates the Resume Generator product roadmap into concrete engineering tasks for the Next.js application. Product goals and phases remain in `README.md`.

## Project foundation

- [x] Initialize the Next.js App Router application in `client/`
- [x] Enable strict TypeScript
- [x] Configure Tailwind CSS and ESLint
- [ ] Choose one package manager and commit its lockfile
- [ ] Define folders for routes, reusable components, domain logic, server code, and tests
- [ ] Add a documented `.env.example` containing placeholder values only
- [ ] Validate required server environment variables at startup
- [ ] Add shared loading, error, and not-found UI

## Resume schema and domain model

- [ ] Define a versioned resume schema and matching TypeScript types
- [ ] Model contact details, education, certifications, roles, bullets, and skills
- [ ] Give every resume entity a stable ID
- [ ] Separate original career-bank content from tailored selections and rewritten variants
- [ ] Record source bullet IDs for imported and generated content
- [ ] Add runtime validation at form, storage, and server boundaries
- [ ] Define schema defaults and a migration strategy for persisted data
- [ ] Create valid, incomplete, and invalid resume fixtures for development and tests

## Application routes and UI

- [ ] Create routes for the landing page, career bank, job listing, resume review, and privacy information
- [ ] Build accessible forms for every supported resume section
- [ ] Support adding, editing, deleting, and reordering resume content
- [ ] Add inline validation without discarding incomplete input
- [ ] Add clear empty, loading, success, and recoverable error states
- [ ] Let users pin, remove, edit, restore, and reorder selected bullets
- [ ] Show why each bullet was selected and which listing requirement it supports
- [ ] Verify keyboard navigation, visible focus states, labels, and screen-reader announcements

## Local storage and persistence

- [ ] Choose an IndexedDB library or direct access pattern
- [ ] Store schema-versioned career-bank data and drafts locally
- [ ] Add debounced autosave and report failed writes to the user
- [ ] Add JSON backup, restore, and full local-data deletion
- [ ] Prevent stale browser tabs from silently overwriting newer data
- [ ] Define the requirements that would trigger server-side persistence
- [ ] Select a server database only after account and access patterns are defined

## Server and AI pipeline

- [ ] Create Node.js Route Handlers for listing analysis, matching, rewriting, validation, and export
- [ ] Keep model credentials and provider calls in server-only modules
- [ ] Validate request size and schema before processing user content
- [ ] Enforce structured model responses
- [ ] Handle malformed output, refusals, timeouts, cancellation, and bounded retries
- [ ] Treat job listings and imported resumes as untrusted input when building prompts
- [ ] Remove unnecessary contact information before external model calls
- [ ] Preserve source bullet IDs for every model-produced variant
- [ ] Add rate limits and user-friendly provider error messages
- [ ] Avoid logging resume text, contact information, credentials, or generated documents

## Listing analysis and selection

- [ ] Normalize job-listing text before analysis
- [ ] Classify required qualifications, preferred qualifications, responsibilities, skills, and seniority signals
- [ ] Implement deterministic importance weighting with unit tests
- [ ] Measure the deterministic baseline before adding semantic similarity
- [ ] Combine relevance, recency, impact, role balance, repetition, and user pins in selection
- [ ] Return score explanations with every selected bullet
- [ ] Handle sparse banks, zero matches, duplicate bullets, unsupported requirements, and over-length results
- [ ] Build a labeled evaluation set before changing scoring rules or prompts

## Document generation and validation

- [ ] Compare a TypeScript DOCX library with `python-docx` using representative fixtures
- [ ] Select the document renderer based on fidelity, deployment complexity, and testability
- [ ] Define one reusable template with explicit fonts, margins, spacing, headings, and bullet styles
- [ ] Generate documents from validated schema data rather than form state or raw model output
- [ ] Return downloads with the correct MIME type, filename, and response headers
- [ ] Extract generated document text and compare it with the selected schema content
- [ ] Test names, dates, links, missing sections, long bullets, and special characters
- [ ] Check actual page count, overflow, split bullets, and orphaned headings
- [ ] Preserve the exact schema data and source IDs used for each frozen export

## Testing and quality controls

- [ ] Add unit tests for schema validation, weighting, matching, selection constraints, and transformations
- [ ] Add component tests for career-bank editing and resume-review interactions
- [ ] Add an end-to-end test from career-bank creation through DOCX download
- [ ] Add regression fixtures for document generation and parsing
- [ ] Run type checking, linting, tests, and a production build in continuous integration
- [ ] Define acceptance criteria for factual accuracy, keyword coverage, page fit, and parser round trips

## Security, privacy, and deployment

- [ ] Document which data stays in the browser and which data is sent to external services
- [ ] Require explicit user review before exporting rewritten content
- [ ] Add input limits, secure response headers, dependency checks, and abuse protection
- [ ] Verify that secrets and local environment files are excluded from version control
- [ ] Configure separate development, preview, and production environments
- [ ] Add privacy-safe error monitoring and health checks
- [ ] Verify hosting limits for request size, execution duration, document binaries, and background work
- [ ] Test the complete resume and export workflow in the production environment
