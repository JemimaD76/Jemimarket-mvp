# JemimaMarket MVP (Seller Analytics + Search + Infinite Scroll)

This repo is a working MVP for a modern marketplace marketplace with:
- Auth (email/password)
- Listings + image uploads (S3/R2 or MinIO)
- Favorites
- Messaging
- Search + facets + infinite scroll
- Relevance ranking + typo tolerance
- Admin reports/takedown
- **Seller Analytics Dashboard** (views/favorites/enquiries + per-listing interested users)

## The iPad reality check (important)
An iPad **cannot run** Docker/Postgres/Node locally the way a laptop can. To *run and test a functioning app from an iPad*, use one of these:

### Option A — GitHub Codespaces (recommended for iPad)
1. Upload this folder to a GitHub repo (from Files app or GitHub web UI).
2. In GitHub: **Code → Codespaces → Create codespace**.
3. In the Codespaces terminal:
   - `npm install`
   - `docker compose up -d`
   - `npx prisma migrate dev`
   - `npm run dev`
4. Open the forwarded port (Codespaces shows a “Ports” tab) → you’ll get a working URL.

### Option B — Deploy (Vercel + managed Postgres)
- Put this repo on GitHub
- Create a Postgres DB on Neon/Supabase
- Set env vars in Vercel
- Run `prisma migrate deploy` during build (already compatible)

## Local (laptop) quickstart
```bash
docker compose up -d
npm install
npx prisma migrate dev
npm run dev
```
Open http://localhost:3000

## MinIO bucket setup (local uploads)
```bash
docker compose up -d
docker run --rm --network host minio/mc sh -c '
  mc alias set local http://localhost:9000 minio minio12345 &&
  mc mb -p local/test || true &&
  mc anonymous set download local/test
'
```

Use `.env.example` to create your `.env`.

---

## Seller Analytics
- Seller dashboard: `/seller/dashboard`
- Per listing analytics: `/seller/listings/[listingId]`

Views are tracked when a listing page loads via `/api/listings/[id]/view`.
Favorites and enquiries update counters automatically.

