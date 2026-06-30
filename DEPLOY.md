# Deploying meliorist.com on GitHub Pages (free)

This folder is a complete static site. Hosting it on GitHub Pages is free, including a custom domain. You only pay for the domain name itself (which you already own).

## What's in this folder

```
index.html        → the one-page website
CNAME             → tells GitHub Pages to serve the site at meliorist.com
.nojekyll         → stops GitHub from processing/hiding files (important for the plugins folder)
plugins/          → publicly accessible at https://meliorist.com/plugins/
  index.html      → a simple listing page
  manifest.json   → example WordPress update manifest (edit with your real plugins)
  (drop your plugin .zip files here)
```

## One-time setup

1. **Create a GitHub account** (free) at https://github.com if you don't have one.

2. **Create a new repository.**
   - Click **New repository**.
   - Name it anything (e.g. `meliorist-site`). It can be Public or Private — Pages works with both on free accounts now, but Public is simplest.
   - Don't add a README; you'll upload these files.

3. **Upload the files.**
   - On the repo page, click **Add file → Upload files**.
   - Drag in everything from this folder: `index.html`, `CNAME`, `.nojekyll`, and the `plugins` folder.
   - If the upload UI hides `.nojekyll` (some browsers hide dotfiles), don't worry — you can recreate it later via **Add file → Create new file**, name it `.nojekyll`, leave it empty, and commit.
   - Commit the changes.

4. **Turn on GitHub Pages.**
   - Go to **Settings → Pages**.
   - Under **Source**, choose **Deploy from a branch**.
   - Branch: `main`, folder: `/ (root)`. Save.
   - After a minute, the site will be live at `https://<your-username>.github.io/meliorist-site/`.

5. **Connect the meliorist.com domain.**
   - Still in **Settings → Pages**, the **Custom domain** field should already show `meliorist.com` (because of the CNAME file). If not, type it and save.
   - At your domain registrar (where you bought meliorist.com), add these DNS records:

     **Apex domain (meliorist.com)** — add four A records pointing to GitHub:
     ```
     185.199.108.153
     185.199.109.153
     185.199.110.153
     185.199.111.153
     ```
     (Optionally add the matching AAAA/IPv6 records from GitHub's docs.)

     **www subdomain** — add a CNAME record:
     ```
     www  →  <your-username>.github.io
     ```
   - Back in Settings → Pages, tick **Enforce HTTPS** once it becomes available (can take up to a few hours after DNS propagates).

That's it. `https://meliorist.com` serves the site, and `https://meliorist.com/plugins/` serves your plugin files.

## Updating WordPress plugins later

Each time you release a new plugin version:

1. Build your plugin `.zip` (e.g. `meliorist-myplugin-1.2.0.zip`).
2. Upload it into the `plugins/` folder of the repo (**Add file → Upload files**).
3. Edit `plugins/manifest.json` — bump the `version` and update the `download_url` to point at the new zip.
4. Commit. The new files are live within a minute at `https://meliorist.com/plugins/...`.

Your installed WordPress plugins' updater code should fetch `https://meliorist.com/plugins/manifest.json`, compare versions, and download the zip from `download_url` when an update is available. (The exact updater code lives in each plugin; this folder just hosts the files it reads.)

## Notes
- Keep `.nojekyll` in place — without it, GitHub may ignore files/folders that start with certain characters and can interfere with serving raw assets.
- GitHub Pages has soft limits (~1 GB repo, ~100 GB/month bandwidth) — plenty for plugin distribution, but not meant for huge binaries or heavy traffic.
- The contact email on the site is set to `contact@meliorist.com`. If you want a different address, edit the two `mailto:`/text spots in `index.html`.
