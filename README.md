# PropTech Backlog v6 – Shared workspace (free-tier build)

## What is included
- Existing Swedbank-inspired fictional-data prototype: hierarchy, Kanban, editable tasks, weekly rank, dashboards.
- Supabase password-based sign-in with invited verified accounts.
- Postgres Row Level Security (RLS): users must be in `members` to read, and `admin/editor` to update.
- Cloud-saved shared workspace (including weekly lists and board order), with realtime refresh for other members.
- GitHub Pages compatible: no build step.
- With blank `config.js`, runs in **local demo mode only** using fictional data.

## Setup (no paid subscription required for a small demo)
1. Create a **Supabase Free** project. Open SQL Editor and run `setup.sql`.
2. In Supabase Authentication settings **disable public user signups**. Invite individual users via Authentication > Users. Make sure invited accounts are verified and have a password. The app does **not** use official Swedbank SSO, and `@swedbank.se` alone does not grant access.
3. Find each user UUID in Authentication > Users. In SQL Editor add membership, e.g. `insert into public.members(user_id,role) values ('THE-UUID','admin');`. Do this for every approved person.
4. In project settings/API, copy **Project URL** and the **publishable/anon public key** into `config.js`. Never put the Supabase service role key or secrets in public files.
5. In Supabase Authentication URL Configuration, set Site URL and add redirect allowlist for your full GitHub Pages URL (`https://YOURNAME.github.io/REPO/`).
6. Create a public GitHub repository containing `index.html` and `config.js` only (and optionally this README), then Settings > Pages > Deploy from branch > main / root. **Do not publish internal backlog or credentials** as repository files. `setup.sql` contains no credentials but does not need to be hosted.
7. Sign in as a listed admin. The database starts empty intentionally. Create sample services and tasks and verify on a second permitted account. Test a user NOT on the allowlist and verify that all database requests fail.

## Security notes
- GitHub Pages serves public HTML/JS. Your workspace rows are protected by Supabase Auth + RLS. A login overlay alone is not a data-security boundary.
- Frontend keys are public by design; protection must be enforced in Postgres RLS.
- No access based solely on an email suffix; invite and explicitly add UUID in the `members` table.
- Supabase Free does not provide bank-managed enterprise SSO. External bank data still needs to be permitted for external storage.
- This version uses a **single JSON workspace document**. Concurrent writes from two editors can overwrite recent edits (last write wins). Suitable for a low-traffic prototype, **not a production-grade collaborative system** until the data model uses row-level records, version checks, and tested conflict handling.
- Changes save asynchronously: verify the message before navigating away.
- Free projects may pause after a period of inactivity and do not include managed daily backups. Export your JSON regularly.
- Test access controls and data behavior before using anything other than fictional data.