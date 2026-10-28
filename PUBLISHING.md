# PropTech Backlog — GitHub Pages deployment

1. On the GitHub account `ahmedjamil0504-bot`, create a NEW PUBLIC repository named `proptech-backlog`. Do not use your existing pilot repo.
2. Upload `index.html`, `config.js` and optionally `README.md` from this archive to the repository root; do NOT upload internal bank backlog exports. No secret key is included.
3. Go to Settings > Pages > Build and deployment > Deploy from a branch > `main` > `/ (root)` > Save.
4. Visit `https://ahmedjamil0504-bot.github.io/proptech-backlog/` after Pages reports a successful deployment.
5. In Supabase > Authentication > Settings, disable public sign-ups. Under Authentication > URL Configuration, set Site URL and Redirect URLs to the deployed GitHub Pages URL.
6. In Supabase > Authentication > Users, invite each verified user and find their User UUID. Add them to `public.members` with an approved role using SQL Editor (this is an admin operation):
   `insert into public.members(user_id,role) values ('USER_UUID_HERE','admin');`
7. Test authorization with one approved user, one nonmember, plus read/write attempts. Do not add real bank data until the security tests and any required approval are complete.

The website itself is publicly loadable; data is protected by authentication + database RLS. This prototype uses one JSON workspace document, so concurrent editors may overwrite each other's changes. Back up regularly, and migrate to per-item rows with concurrency control before team production use.