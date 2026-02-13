# SoundCrate prototype

A simple front-end prototype for a music-focused social app inspired by Letterboxd.

## What this is

This repo is a **static website** (HTML + CSS + JavaScript). That means you can host it on any static host (GitHub Pages, Netlify, Vercel, Cloudflare Pages, etc.) without a backend server.

## Features

- Big 3 musicians and Big 3 songs profile panel.
- Swipe-style song discovery feed from followed users.
- Ability to post newly discovered songs.
- Direct message panel with live message updates.

---

## Run locally

```bash
python -m http.server 8000
```

Then open:

- `http://localhost:8000`

---

## Turn this into a real website (fastest path: GitHub Pages)

### 1) Push this repo to GitHub

```bash
git remote add origin https://github.com/<your-username>/<repo-name>.git
git push -u origin work
```

> If your default branch is `main`, you can merge `work` into `main` first.

### 2) Enable GitHub Pages

1. Go to **Repo → Settings → Pages**.
2. Under **Build and deployment**, choose:
   - **Source**: Deploy from a branch
   - **Branch**: `main` (or `work`) and `/ (root)`
3. Save.

GitHub will publish your site to something like:

- `https://<your-username>.github.io/<repo-name>/`

### 3) If styles/scripts do not load on sub-paths

This project already uses relative paths (`styles.css`, `app.js`), so it should work on GitHub Pages as-is.

---

## One-command alternatives

### Netlify Drop (no CLI)

1. Zip the project files (`index.html`, `styles.css`, `app.js`).
2. Go to [https://app.netlify.com/drop](https://app.netlify.com/drop).
3. Drag and drop the zip.
4. You immediately get a public URL.

### Vercel (CLI)

```bash
npm i -g vercel
vercel --prod
```

(Choose defaults; this static app deploys directly.)

---

## Important next step (to make it a true social app)

Right now, data is in-memory in `app.js`, so refresh resets posts/DMs. To make this production-ready, add:

- Authentication (Clerk/Auth0/Supabase Auth)
- Database (Supabase/Postgres/Firebase)
- Real-time messaging (Supabase Realtime/Pusher/Socket.IO)
- Song metadata integration (Spotify or Apple Music API)
- Media storage for profile images (S3/Cloudinary)

If you want, I can do the next step and scaffold this into a real full-stack app (React + Supabase) with login, follows, persistent Big 3, swipe feed, and real DMs.
