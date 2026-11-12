# FreshBite Kenya (Next.js, ready for Vercel)

JavaScript version of the PHP FreshBite site: same pages, same look, same database tables.

## 1. Database (MySQL-compatible)
Vercel has no MySQL, so use a hosted one. TiDB Cloud (free tier) speaks MySQL, so your SQL works unchanged.
1. Create a free cluster, create a database (e.g. `freshbite`), copy its connection details.
2. Copy `.env.example` to `.env.local` and fill in `DATABASE_URL`, `DB_SSL=true` and a long random `SESSION_SECRET`.

## 2. Load your data
- **Keep your real data** (recommended): in XAMPP phpMyAdmin choose your database > Export > Go, then run  
  `npm run db:import -- path/to/your-export.sql`  
  Customer and admin passwords keep working (same bcrypt hashes).
- **Start fresh** with sample meals: `npm run db:import` and then  
  `npm run admin:create -- myusername "a-long-password"`

## 3. Run it on your computer
```
npm install
npm run dev
```
Open http://localhost:3000 (admin: http://localhost:3000/admin/login).

## 4. Deploy to Vercel
1. Put this folder in a GitHub repository (the `.gitignore` keeps secrets out).
2. In Vercel: Add New > Project > import the repository.
3. Settings > Environment Variables: add everything from `.env.example` (at least `DATABASE_URL`, `DB_SSL`, `SESSION_SECRET`).
4. Storage tab > Create > **Blob** > connect it to the project (this adds `BLOB_READ_WRITE_TOKEN` for meal photo uploads).
5. Deploy. Then set `MPESA_CALLBACK_BASE` to your live URL and redeploy if you use M-Pesa.

## What changed from PHP
| PHP | Next.js |
|---|---|
| `*.php` pages | `app/` folder (pages are server components, same SQL) |
| PHP sessions | signed cookies (login), cookie cart |
| CSRF tokens | built into Next.js server actions |
| `uploads/meals/` on disk | old photos in `public/uploads/meals`, new uploads go to Vercel Blob |
| `setup/create_admin.php` | `npm run admin:create` |
| `mpesa_callback.php` | `/api/mpesa/callback` (works directly on your Vercel URL, no ngrok needed) |
