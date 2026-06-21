# [AGENTS.md](http://AGENTS.md)

## Ross Tax Pro Masterfile Platform — Agent Operating Guide

This file defines how AI agents, developers, automation workers, documentation assistants, and implementation tools should operate inside the Ross Tax Pro Masterfile Platform repository.

The platform includes Mintlify documentation, static HTML pages, backend API services, MongoDB sync scripts, client masterfile records, tax-year records, transcript workflow design, reconciliation logic, notice response documentation, ERO gateway workflows, and universal print packet generation.

---

## 1. Project Purpose

Ross Tax Pro Masterfile Platform is designed to support controlled tax-practice operations for:

- Client masterfile management
- Tax-year masterfile tracking
- MongoDB sync
- Client import workflows
- Transcript pull job design
- ERO gateway transmission metadata
- Reconciliation workflows
- Withholding and math-error workpapers
- IRS notice / letter tracking
- Transaction code glossary
- Recommendation generation
- Universal print packet preparation
- Audit trace and correlation logging
- Mintlify documentation

This repository may include documentation, static UI files, backend API code, MongoDB scripts, importer scripts, worker skeletons, schemas, and deployment files.

---

## 2. Critical Security Boundary

Agents must never place the following into static files, documentation pages, commits, examples, screenshots, logs, generated output, or public code:

- Plaintext SSNs
- Full taxpayer TINs
- Bank account numbers
- Taxpayer documents
- Raw client spreadsheets
- IRS credentials
- TDS client secrets
- TDS access tokens
- OAuth tokens
- Private keys
- Production MongoDB connection strings
- Passwords
- API keys
- Protected taxpayer transcripts
- Unmasked personally identifiable information

Use placeholders, masked values, synthetic examples, hashed identifiers, or documented backend-only workflows.

Correct example:

```text
maskedTin: "***-**-1234"
```

Incorrect example:

```text
ssn: "123-45-6789"
```

---

## 3. Protected Workflow Rule

Static pages and Mintlify documentation are for presentation, training, and operational guidance only.

Protected workflows must run through secured backend services, including:

- Client imports
- MongoDB writes
- TDS transcript pulls
- Taxpayer transcript normalization
- IRS notice response preparation
- Workpaper generation using client data
- Universal print packet generation using protected records
- Audit event persistence
- Any workflow involving taxpayer identifiers or taxpayer documents

Agents must not wire protected taxpayer workflows directly into client-side HTML.

---

## 4. Repository Structure

Expected project layout:

```text
.
├── logo/
│   ├── light.svg
│   └── dark.svg
├── .mintignore
├── AGENTS.md
├── docs.json
├── favicon.svg
├── index.mdx
├── quickstart.mdx
├── README.md
├── LICENSE
├── architecture.mdx
├── security.mdx
├── deployment.mdx
├── client-masterfile.mdx
├── tax-year-masterfile.mdx
├── mongo-sync.mdx
├── query-catalog.mdx
├── api-reference.mdx
├── ero-gateway.mdx
├── tds-worker.mdx
├── reconciliation.mdx
├── notice-index.mdx
├── tc-glossary.mdx
├── print-packets.mdx
├── mongo/
│   ├── createCollections.js
│   ├── indexes.js
│   ├── queries.js
│   └── aggregations.js
├── server/
│   ├── package.json
│   ├── .env.example
│   └── src/
│       ├── server.js
│       ├── db.js
│       ├── audit.js
│       ├── importers/
│       │   └── xlsxClientImporter.js
│       └── workers/
│           └── tdsPullWorker.js
└── static/
    ├── index.html
    ├── app.html
    ├── clients.html
    ├── tax-years.html
    ├── mongo-sync.html
    ├── tds-worker.html
    ├── queries.html
    ├── deployment.html
    ├── security.html
    └── metadata.html
```

Agents should preserve this structure unless specifically instructed to reorganize the project.

---

## 5. Agent Roles

### 5.1 Documentation Agent

The Documentation Agent maintains Mintlify files.

Responsible for:

- `index.mdx`
- `quickstart.mdx`
- `architecture.mdx`
- `security.mdx`
- `deployment.mdx`
- `client-masterfile.mdx`
- `tax-year-masterfile.mdx`
- `mongo-sync.mdx`
- `query-catalog.mdx`
- `api-reference.mdx`
- `ero-gateway.mdx`
- `tds-worker.mdx`
- `reconciliation.mdx`
- `notice-index.mdx`
- `tc-glossary.mdx`
- `print-packets.mdx`

Rules:

- Use clear headings.
- Keep examples synthetic.
- Do not include secrets.
- Do not include real taxpayer data.
- Do not claim the platform can force IRS account changes.
- Use warnings for protected workflows.
- Keep instructions production-oriented.

---

### 5.2 Mintlify Navigation Agent

The Mintlify Navigation Agent maintains `docs.json`.

Responsible for:

- Site name
- Theme colors
- Logo paths
- Favicon path
- Navigation groups
- Page order
- Topbar links
- Metadata

Rules:

- Every page listed in navigation must exist.
- Every MDX file must have frontmatter.
- Navigation names should be concise.
- Do not place credentials or private URLs in `docs.json`.

---

### 5.3 Static UI Agent

The Static UI Agent maintains `static/`.

Responsible for:

- Static landing pages
- One-file app shell
- Metadata display
- Collection registry display
- Query catalog display
- Deployment guidance pages
- Security guidance pages

Rules:

- Static files must not connect directly to MongoDB.
- Static files must not call TDS.
- Static files must not contain secrets.
- Static files may show synthetic examples only.
- Static files may display masked identifiers only.
- Static forms are demonstration interfaces unless wired to a secure backend.

---

### 5.4 Backend API Agent

The Backend API Agent maintains `server/`.

Responsible for:

- API routes
- Health checks
- Client masterfile read endpoints
- Tax-year endpoints
- Sync job endpoints
- TDS pull job queue endpoint
- Reconciliation query endpoint
- Audit trace endpoint

Rules:

- Validate all request bodies.
- Require tenant scoping.
- Require correlation IDs for protected actions.
- Do not log plaintext PII.
- Do not expose protected records without authorization checks.
- Use environment variables or vault-managed secrets.
- Keep `.env` out of Git.

---

### 5.5 MongoDB Agent

The MongoDB Agent maintains `mongo/`.

Responsible for:

- Collection bootstrap scripts
- Index scripts
- Query catalog
- Aggregation pipelines
- Data model documentation

Rules:

- Collections must be tenant-scoped.
- Indexes must support client, tax-year, notice, transcript, reconciliation, and audit lookups.
- Protected identifiers must be hashed or encrypted.
- Use masked identifiers for display.
- Avoid unbounded queries in production examples.

---

### 5.6 Import Agent

The Import Agent maintains client import workflows.

Responsible for:

- XLSX import logic
- CSV import logic
- Import batch metadata
- Client normalization
- Tax-year record creation
- Identifier hashing
- Audit event writing

Rules:

- Imports must run server-side.
- Never write plaintext SSNs to `client_master`.
- Use `client_identifiers` for hashed or encrypted identifiers.
- Store masked TIN only where display is needed.
- Write `import_batches`.
- Write `audit_events`.
- Skip incomplete rows safely.
- Report inserted, updated, skipped, and total counts.

---

### 5.7 TDS Worker Agent

The TDS Worker Agent maintains transcript pull worker design.

Responsible for:

- Transcript pull job queue logic
- Authorization checks
- TDS worker skeleton
- Transcript normalization
- TC code extraction
- Recommendation generation
- Audit event writing

Rules:

- TDS access must be backend-only.
- TDS credentials must be stored in environment variables or vault.
- Do not embed TDS client IDs or secrets in frontend code.
- Confirm taxpayer authorization before any pull.
- Store transcript metadata and transaction rows securely.
- Write audit events for every pull attempt.
- Mark failed jobs as retryable or terminal.

---

### 5.8 Reconciliation Agent

The Reconciliation Agent maintains workpaper and variance logic.

Responsible for:

- Return-to-transcript comparison
- Income matching
- Withholding matching
- Credit comparison
- Payment comparison
- Refund event comparison
- Offset review
- Notice amount comparison
- Variance classification
- Reviewer disposition

Rules:

- Every variance must include source amount, return amount, variance amount, category, materiality, and review note.
- Do not auto-close material variances.
- Do not fabricate source documents.
- Preserve evidence references.
- Require human review for material differences.

---

### 5.9 Notice Response Agent

The Notice Response Agent maintains CP/LTR response workflows.

Responsible for:

- Notice code index
- Letter index
- Response deadlines
- Required evidence lists
- Forms and schedules checklist
- Response packet structure

Rules:

- Do not generate unsupported IRS responses.
- Follow the notice instructions.
- Calendar deadlines.
- Require reviewer approval before final packet.
- Preserve copy of notice, response, evidence, and delivery proof.

---

### 5.10 Transaction Code Glossary Agent

The TC Glossary Agent maintains transaction code explanations.

Responsible for:

- TC code index
- Literal meaning
- Plain-language meaning
- Operational lane
- Recommended evidence
- Safe next step
- Related forms
- Related notices
- Review warnings

Rules:

- TC explanations must be operational guidance, not guarantees.
- TC 570 and TC 810 must not be described as automatically releasable.
- TC 571, 572, and 811 are IRS-posted resolution indicators.
- Always require transcript context, notice context, and taxpayer authorization.
- For uncommon codes, note that exact meaning depends on module, action code, and current IRS references.

---

### 5.11 Print Packet Agent

The Print Packet Agent maintains universal print packet structure.

Responsible for:

- Cover page
- Client summary
- Tax-year summary
- Authorization summary
- Transcript summary
- TC code analysis
- Notice index
- Workpaper index
- Withholding calculation
- Reconciliation variance report
- Recommendation summary
- Evidence index
- Audit trace
- Reviewer approval block

Rules:

- Print packets must be review-ready.
- Do not include unnecessary PII.
- Mask identifiers where possible.
- Include source references.
- Include reviewer sign-off.
- Include packet generation timestamp.

---

## 6. Required Commands

Common development commands:

```bash
npm install
npm run dev
npm run validate
npm run manifest
```

Backend commands:

```bash
cd server
npm install
npm start
npm run mongo:create
npm run mongo:indexes
npm run import:clients
npm run worker:tds
```

Mintlify command:

```bash
mintlify dev
```

Static preview command:

```bash
python -m http.server 8080
```

---

## 7. Environment Rules

Required backend variables:

```bash
NODE_ENV=production
PORT=8787
MONGODB_URI=...
MONGODB_DB=ross_tax_pro
TENANT_ID=rtpsc
PII_HASH_SALT=...
JWT_ISSUER=...
JWT_AUDIENCE=...
TDS_CLIENT_ID=...
TDS_CLIENT_SECRET=...
TDS_REDIRECT_URI=...
ALLOW_DEMO_MODE=false
```

Rules:

- `.env` must not be committed.
- `.env.example` may be committed with placeholders.
- Use vault or environment variables for production secrets.
- Do not print secrets in logs.
- Do not expose secrets in static files.

---

## 8. MongoDB Collection Standards

Core collections:

```text
tenants
users
roles
client_master
client_identifiers
client_tax_years
authorizations
transcript_pull_jobs
transcripts
transcript_transactions
return_records
return_line_items
withholding_records
notice_records
forms_schedule_index
workpapers
reconciliation_runs
reconciliation_variances
refund_events
gateway_transmissions
sync_jobs
import_batches
recommendations
print_packets
document_manifest
audit_events
settings
```

Every protected collection should include:

```text
tenantId
createdAt
updatedAt
correlationId where applicable
```

Every protected write should create an `audit_events` record.

---

## 9. Query Rules

Queries must be tenant-scoped.

Correct:

```js
{ tenantId, clientId, taxYear }
```

Incorrect:

```js
{ clientId }
```

Use indexes for:

- Tenant ID
- Client ID
- Tax year
- Status
- Notice due date
- TC code
- Correlation ID
- Import batch ID
- Sync job status

Avoid unbounded production queries.

---

## 10. Import Rules

When importing client data:

1. Read file server-side.
2. Normalize row fields.
3. Hash SSN/TIN values.
4. Store masked TIN only.
5. Upsert `client_master`.
6. Upsert `client_identifiers`.
7. Upsert `client_tax_years`.
8. Create `import_batches`.
9. Create `audit_events`.
10. Return inserted, updated, skipped, and total counts.

Never embed imported spreadsheet rows into documentation or static HTML.

---

## 11. Transcript Rules

For transcript workflows:

1. Confirm authorization.
2. Queue `transcript_pull_jobs`.
3. Run backend-only worker.
4. Pull approved transcript type.
5. Normalize transcript metadata.
6. Normalize transaction code rows.
7. Store into `transcripts` and `transcript_transactions`.
8. Generate recommendations.
9. Write audit event.
10. Update job status.

Static HTML must not call TDS directly.

---

## 12. ERO Gateway Rules

Gateway records may include:

- ERO status
- EFIN status
- ETIN or transmitter status
- API client status
- Transmission ID
- Client ID
- Tax year
- Submission status
- Error code
- Rejection code
- Correlation ID
- Human review flag

Important boundary:

Active ERO, EFIN, ETIN, or API client status does not mean the platform can directly alter IRS accounts, remove transcript codes, release freezes, or guarantee refund dates.

---

## 13. TC 570 and TC 810 Rules

### TC 570

TC 570 generally means additional account action is pending.

Agent response should recommend:

- Pull authorized account transcript.
- Review TC sequence.
- Check for TC 971 notice marker.
- Identify CP or LTR notice.
- Classify issue lane.
- Gather requested evidence.
- Respond through official notice channel.
- Monitor for TC 571, TC 572, TC 846, adjustments, or new notices.

Do not say:

```text
We can release TC 570 directly.
```

### TC 810

TC 810 generally means refund freeze.

Agent response should recommend:

- Pull authorized account transcript.
- Identify TC 810 date.
- Check for TC 811.
- Review action codes and notice trail.
- Determine underlying issue.
- Verify identity or substantiate records if requested.
- Respond through official IRS channel.
- Monitor for TC 811, TC 846, offsets, or adjustments.

Do not say:

```text
We can force release TC 810.
```

---

## 14. Notice and Letter Rules

For CP/LTR responses:

- Identify notice number.
- Identify tax year.
- Identify response deadline.
- Identify requested documents.
- Compare notice to return and transcript.
- Generate evidence checklist.
- Draft response only with support.
- Require reviewer approval.
- Record delivery proof.
- Store audit event.

Do not ignore deadlines.

Do not send unsupported documents.

---

## 15. Recommendation Rules

Recommendations must include:

- Issue summary
- Transcript codes involved
- Notice or letter involved
- Tax year
- Recommended evidence
- Required forms
- Safe next action
- Deadline if known
- Reviewer warning
- Audit trace ID

Recommendations must not include:

- Guaranteed refund release dates
- Unsupported legal conclusions
- Fabricated IRS responses
- Unverified taxpayer claims
- Embedded private data

---

## 16. Universal Print Packet Rules

A print packet should include:

```text
Cover Page
Client Summary
Tax-Year Summary
Authorization Summary
Transcript Summary
TC Code Analysis
Notice / Letter Index
Forms and Schedules Checklist
Withholding Calculation
Reconciliation Variance Report
Recommendation Summary
Evidence Index
Audit Trace
Reviewer Approval Block
```

Print packet settings:

```text
Paper: Letter
Orientation: Portrait
Margins: Default or Narrow
Background Graphics: Enabled for branded packet
Headers/Footers: Off for clean filing copies
PDF Export: Enabled
```

---

## 17. Coding Standards

Use:

- Clear file names
- Explicit functions
- Environment variables for secrets
- Server-side validation
- Tenant-scoped queries
- Audit logging
- Correlation IDs
- Structured errors
- Safe defaults

Avoid:

- Hardcoded secrets
- Unmasked PII
- Unbounded database reads
- Direct frontend database connections
- Direct frontend TDS calls
- Silent failures
- Fake success states

---

## 18. Documentation Standards

Every MDX page should include:

```mdx
---
title: Page Title
description: Short page description.
---
```

Use:

- Clear headings
- Tables for structured data
- Steps for procedures
- Warnings for protected workflows
- Notes for context
- Synthetic data examples
- Masked identifiers

Avoid:

- Real taxpayer examples
- Real SSNs
- Real bank details
- Live credentials
- Unsupported IRS claims

---

## 19. Build Validation

Before finalizing changes:

- Confirm `docs.json` navigation paths exist.
- Confirm every MDX page has frontmatter.
- Confirm no `.env` file is committed.
- Search for obvious secret strings.
- Search for unmasked SSN patterns.
- Confirm static pages load.
- Confirm API health route works.
- Confirm Mongo scripts do not hardcode credentials.
- Confirm importer does not log plaintext identifiers.

Suggested checks:

```bash
grep -R "mongodb+srv://" . --exclude=".env.example"
grep -R "TDS_CLIENT_SECRET" . --exclude=".env.example"
grep -R "[0-9][0-9][0-9]-[0-9][0-9]-[0-9][0-9][0-9][0-9]" .
```

---

## 20. Deployment Rules

Recommended deployment split:

| Layer | Target |
| --- | --- |
| Mintlify Docs | Mintlify |
| Static App | S3, CloudFront, Vercel, Netlify, or Cloudflare Pages |
| Backend API | Render, Railway, Fly.io, ECS, App Runner, or secure Node host |
| MongoDB | MongoDB Atlas |
| Secrets | Vault, cloud secrets manager, or environment variables |
| Workers | Backend worker process or scheduled job runtime |

Deployment must include:

- HTTPS
- Restricted MongoDB access
- Least-privilege database user
- Environment-managed secrets
- Logging
- Monitoring
- Rollback process
- Incident response plan

---

## 21. Agent Completion Checklist

Before an agent marks work complete:

- Files are in the correct location.
- No secrets were added.
- No unmasked taxpayer identifiers were added.
- Documentation pages have frontmatter.
- Static files contain no protected data.
- Backend code uses environment variables.
- Mongo queries are tenant-scoped.
- Import logic masks or hashes identifiers.
- TDS logic is backend-only.
- TC 570 and TC 810 are described safely.
- Recommendations require verified sources.
- Print packets include reviewer approval.
- Audit events are documented.
- README and Quickstart remain accurate.

---

## 22. Final Operating Rule

Agents may document, scaffold, validate, and organize the Ross Tax Pro Masterfile Platform.

Agents must not expose taxpayer data, embed secrets, bypass authorization, fabricate IRS outcomes, claim direct IRS account control, or promise refund release.

Protected taxpayer workflows require backend services, taxpayer authorization, secure credentials, audit logging, human review, and verified source records.