# Inamke Universe Network

A single-page Supabase authentication and member dashboard.

## Features

- Email/password signup and login
- Google OAuth login
- Account suspension checks
- Member points and sharing activity
- Administrator dashboard
- Member status and admin-note management
- GitHub Pages compatible deployment

## Run locally

Because the app uses browser modules and Supabase OAuth redirects, run it through a local HTTP server:

```bash
python3 -m http.server 4173
```

Open `http://localhost:4173`.

## Deploy to GitHub Pages

1. Open the repository **Settings** → **Pages**.
2. Choose **Deploy from a branch**.
3. Select `main` and `/ (root)`.
4. Add the GitHub Pages URL to the Supabase project's allowed redirect URLs.

## Supabase configuration

The app uses one publishable browser key and expects the following tables:

- `profiles`: `id`, `display_name`, `created_at`
- `points_ledger`: `user_id`, `points`
- `sharing_activity`: `user_id`, `provider`, `verified`, `created_at`
- `member_status`: `user_id`, `status`, `admin_note`, `updated_at`

It also calls an `is_admin()` Postgres RPC.

Enable Row Level Security and ensure:

- Users can read and insert their own profile.
- Users can read their own points and sharing activity.
- Users can read their own member status.
- Only verified administrators can call `is_admin()` and read or update all required dashboard data.
- Never put a Supabase `service_role` secret in this static website.

If the app is deployed on another domain, add that domain to Supabase Authentication **Redirect URLs** and enable it as an allowed web origin in the OAuth provider settings.
