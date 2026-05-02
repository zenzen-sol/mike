# Mike

Open-source release containing the Mike frontend and backend.

## Contents

- `frontend/` - Next.js application
- `backend/` - Express API, Supabase access, document processing, and migrations
- `backend/migrations/000_one_shot_schema.sql` - one-shot Supabase schema for fresh databases

## Setup

Install dependencies:

```bash
npm install --prefix backend
npm install --prefix frontend
```

Create local env files from the examples:

```bash
cp backend/.env.example backend/.env
cp frontend/.env.local.example frontend/.env.local
```

Run `backend/migrations/000_one_shot_schema.sql` in the Supabase SQL editor for a fresh database.

Start the backend:

```bash
npm run dev --prefix backend
```

Start the frontend:

```bash
npm run dev --prefix frontend
```

Open `http://localhost:3000`.

## Required Services

- Supabase Auth and Postgres
- S3-compatible object storage (see [Object Storage](#object-storage))
- At least one supported model provider key, depending on which models you enable
- LibreOffice for DOC/DOCX to PDF conversion

## Object Storage

The backend uses the AWS S3 SDK and works with any S3-compatible provider.
Configure these env vars in `backend/.env`:

| Var | Description |
| --- | --- |
| `S3_ENDPOINT_URL` | Provider endpoint URL |
| `S3_REGION` | Region. Defaults to `auto` (correct for R2). Required for Supabase Storage and AWS S3. |
| `S3_ACCESS_KEY_ID` | Access key |
| `S3_SECRET_ACCESS_KEY` | Secret key |
| `S3_BUCKET_NAME` | Bucket name (default: `mike`) |

Examples:

**Cloudflare R2**
```
S3_ENDPOINT_URL=https://<account-id>.r2.cloudflarestorage.com
S3_REGION=auto
S3_ACCESS_KEY_ID=<r2-access-key>
S3_SECRET_ACCESS_KEY=<r2-secret-key>
S3_BUCKET_NAME=mike
```

**Supabase Storage** (S3-compatible endpoint; create the bucket and an S3 access key under Storage → S3 Connection in the Supabase dashboard)
```
S3_ENDPOINT_URL=https://<project-ref>.supabase.co/storage/v1/s3
S3_REGION=<project-region, e.g. us-east-1>
S3_ACCESS_KEY_ID=<supabase-s3-access-key>
S3_SECRET_ACCESS_KEY=<supabase-s3-secret-key>
S3_BUCKET_NAME=mike
```

The legacy `R2_*` env var names from earlier releases are still accepted as a
fallback, so existing deployments do not need to change anything.

## Checks

```bash
npm run build --prefix backend
npm run build --prefix frontend
npm run lint --prefix frontend
```

## License

AGPL-3.0-only. See `LICENSE`.
