# Network Engineering Study PWA — Supabase version

## Before uploading to GitHub Pages

1. Open `config.js`.
2. Replace `YOUR_PROJECT_REF` with your Supabase Project URL host, e.g. `https://abc123.supabase.co`.
3. Replace `sb_publishable_YOUR_KEY_HERE` with the **Publishable key** from Supabase.
4. Do **not** use a `sb_secret_...` key, `service_role`, or database password.
5. In Supabase Authentication → URL Configuration, Site URL and Redirect URL should be:
   `https://razaan16ali.github.io/network-engineering-webapp/`

## Database requirement

The app uses `study_progress` with:

- `id` — UUID primary key
- `user_id` — UUID
- `task_id` — text
- `completed` — boolean
- `updated_at` — timestamptz

The app uses an upsert on `(user_id, task_id)`, so this unique constraint must exist:

```sql
create unique index if not exists study_progress_user_task_unique
on public.study_progress (user_id, task_id);
```

RLS policies must restrict each row to `auth.uid() = user_id` as configured in Supabase.

## GitHub Pages files

Upload these files/folders to the repository root:

- `index.html`
- `config.js`
- `manifest.json`
- `service-worker.js`
- `icons/icon-192.png`
- `icons/icon-512.png`

The app keeps a local cache and also syncs checklist progress to Supabase after login. If the old localStorage checklist has progress and the account has no cloud rows yet, the app migrates that progress to the account.
