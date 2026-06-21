# Ross Tax Pro Masterfile \+ MongoDB Sync \+ Mintlify Production Package

Build Status: Production-ready shell Company: Ross Tax Pro Software Co. Platform Scope: Client masterfile, tax-year records, MongoDB sync, transcript workflow, reconciliation, notices, workpapers, gateway services, and documentation.

## Overview

This package provides a complete production workspace for Ross Tax Pro Software Co. It includes static pages, a one-file HTML application, Mintlify documentation, MongoDB collection design, query catalog, backend API structure, XLSX client import logic, TDS transcript worker design, ERO gateway transmittal workflow, and audit-trace metadata.

The platform is designed to organize internal client masterfile data by tax year and support tax operations such as reconciliation, notice response, withholding review, transcript analysis, refund-status tracking, and universal print packet generation.

## Package Contents

```text
.
├── logo/
│   ├── light.svg
│   └── dark.svg
├── docs.json
├── favicon.svg
├── index.mdx
├── quickstart.mdx
├── README.md
├── LICENSE
├── AGENTS.md
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
├── scripts/
│   ├── seed-docs.mjs
│   ├── validate-docs.mjs
│   └── build-manifest.mjs
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

## Core Data Collections

The MongoDB model includes:

- `tenants`
- `users`
- `roles`
- `client_master`
- `client_identifiers`
- `client_tax_years`
- `authorizations`
- `transcript_pull_jobs`
- `transcripts`
- `transcript_transactions`
- `return_records`
- `return_line_items`
- `withholding_records`
- `notice_records`
- `forms_schedule_index`
- `workpapers`
- `reconciliation_runs`
- `reconciliation_variances`
- `refund_events`
- `gateway_transmissions`
- `sync_jobs`
- `import_batches`
- `recommendations`
- `print_packets`
- `document_manifest`
- `audit_events`
- `settings`

## Security and PII Boundary

This package must not embed or expose:

- Plaintext SSNs
- Bank account data
- Taxpayer documents
- IRS credentials
- TDS client secrets
- OAuth access tokens
- Private keys
- Production MongoDB credentials
- Protected taxpayer records

Client spreadsheet imports must be processed server-side only. Identifiers must be hashed, encrypted, or masked before storage. Static files must never contain protected taxpayer data.

## Environment Variables

Create a `.env` file from `.env.example`:

```bash
cp server/.env.example server/.env
```

Required variables:

```bash
NODE_ENV=production
PORT=8787
MONGODB_URI=mongodb+srv://<user>:<password>@<cluster>/<database>
MONGODB_DB=ross_tax_pro
TENANT_ID=rtpsc
PII_HASH_SALT=replace-with-secure-random-value
JWT_ISSUER=https://auth.example.com/
JWT_AUDIENCE=ross-tax-pro-api
TDS_CLIENT_ID=store-in-vault-not-static-files
TDS_CLIENT_SECRET=store-in-vault-not-static-files
TDS_REDIRECT_URI=https://example.com/oauth/callback
ALLOW_DEMO_MODE=false
```

## Install

```bash
cd server
npm install
```

## Create MongoDB Collections

```bash
npm run mongo:create
```

## Create MongoDB Indexes

```bash
npm run mongo:indexes
```

## Start API

```bash
npm start
```

## Import Client Spreadsheet

```bash
INPUT_XLSX="/secure/path/client-file.xlsx" npm run import:clients
```

The importer must:

1. Read the spreadsheet server-side.
2. Hash SSN/TIN values.
3. Store masked TIN only.
4. Create client master records.
5. Create tax-year records.
6. Write import batch metadata.
7. Write audit events.

## TDS Worker

TDS transcript pulls must run backend-only.

Required controls:

- Valid taxpayer authorization
- Form 2848, Form 8821, or taxpayer-authenticated access
- Vault-managed credentials
- Audit logging
- Correlation ID
- Retry handling
- Transcript normalization
- Recommendation generation

Static HTML cannot pull TDS transcripts directly.

## ERO Gateway

The ERO gateway layer tracks:

- ERO status
- EFIN status
- ETIN or transmitter status
- API client readiness
- Gateway transmissions
- Rejections
- Error codes
- Correlation IDs
- Audit events

Active ERO, EFIN, ETIN, or API status does not mean the platform can directly change IRS records or force refund release. Official IRS workflows, taxpayer authorization, and notice instructions control.

## Reconciliation

The reconciliation engine compares:

- Filed return values
- Transcript values
- Wage and income records
- Withholding records
- Notice amounts
- Refund events
- Payments
- Credits
- Adjustments

Each variance should include:

- Source amount
- Return amount
- Difference
- Materiality status
- Workpaper explanation
- Evidence reference
- Reviewer disposition

## Universal Print Packet

Print packets should include:

- Client cover page
- Tax-year summary
- Transcript summary
- TC code analysis
- Notice / letter index
- Forms and schedules checklist
- Withholding calculation
- Reconciliation variance report
- Recommendation summary
- Evidence index
- Audit trace

## Mintlify Documentation

Run locally:

```bash
npm run dev
```

Validate:

```bash
npm run validate
```

Generate manifest:

```bash
npm run manifest
```

## Deployment

Recommended deployment pattern:

1. Deploy static pages to S3, CloudFront, Vercel, Netlify, or similar static host.
2. Deploy Mintlify docs separately.
3. Deploy backend API to a Node.js production host.
4. Store secrets in environment variables or a vault.
5. Configure MongoDB Atlas IP allowlist or Private Endpoint.
6. Run collection bootstrap.
7. Run indexes.
8. Import clients through backend importer.
9. Queue authorized TDS jobs through backend API.
10. Monitor audit events and correlation IDs.

## Production Readiness Checklist

- No secrets in frontend
- No plaintext SSNs in docs
- MongoDB collections created
- MongoDB indexes created
- API health endpoint working
- Client importer tested
- TDS worker isolated backend-only
- Audit events enabled
- Correlation IDs enabled
- Role-based access model documented
- Universal print packet defined
- Incident and rollback process documented

## License

Proprietary. All rights reserved.

Copyright © 2021–2026 Ross Tax Pro Software Co.