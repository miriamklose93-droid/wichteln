# Wichteln (Secret Santa)

Mobile-first Secret Santa. Next.js + Upstash Redis, free on Vercel. No accounts.

- Draw is generated server-side once. Assignments never leave the server except a person's own match after claiming.
- Claiming uses Redis `HSETNX` (atomic): only the first device gets a name.
- The device keeps a random token in `localStorage` to see its result again.
- Admin: `/admin/<ADMIN_TOKEN>`. Shows claimed yes/no only, never who drew whom.

## Local run

```bash
npm install
cp .env.example .env.local   # set ADMIN_TOKEN; without Redis it uses temporary in-memory storage
npm run dev
npm test                     # 1,000 random draws + edge cases
```

## Deploy (Vercel + Upstash, free)

1. Put this folder on GitHub (new private repo, "uploading an existing file", drag the contents in; do not upload `node_modules` or `.next`).
2. vercel.com, sign in with GitHub, Add New, Project, import the repo. Before clicking Deploy, open "Environment Variables" and add `ADMIN_TOKEN` = a long random string (20+ letters/digits, no spaces). Deploy.
3. In the Vercel project: Storage, Create Database, Upstash (Redis), free plan, connect to the project (all environments). Vercel adds the Redis variables itself.
4. Deployments, latest, three dots, Redeploy (so the app sees the new variables).
5. Open `https://<your-project>.vercel.app/admin/<ADMIN_TOKEN>`, set up the draw, copy the group link, send it to WhatsApp.
