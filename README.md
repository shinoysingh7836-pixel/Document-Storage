# Public Lands Template Library (all-in-GitHub edition)

A clean, searchable web library for approved Word decision-letter templates.
Everything lives in **one GitHub repository** — the website, the template files,
and the list of templates. No external services, no database.

- Staff browse, search, and download. No login.
- An admin unlocks tools with the **`Lanks26`** passcode and uploads / edits /
  removes templates **by drag-and-drop, right on the website**. Each change is
  committed straight to your GitHub repo using a personal access token.

---

## What's in this folder

```
index.html         ← the whole website (one file, drag it into your repo)
documents.json     ← the list of templates (the site updates this for you)
documents/         ← the template files (the site uploads here for you)
README.md          ← this file
```

You only ever edit `index.html` once (three lines). After that, everything is
managed from the website itself.

---

## Setup (about 10 minutes, once)

### 1. Put the files in a GitHub repo
Create a repo (or use an existing one) and drop in `index.html`,
`documents.json`, and the `documents/` folder. Enable **Settings → Pages** so
the site is live, exactly like your other websites.

### 2. Point the website at your repo
Open `index.html`, find the **CONFIG** block near the top of the `<script>`,
and set three values:

```js
const GITHUB_OWNER  = "your-github-username";
const GITHUB_REPO   = "your-repo-name";
const GITHUB_BRANCH = "main";   // or "master", whichever your repo uses
```

These are **not secret** — they just tell the site which repo to read. Commit
the change. The library now loads and anyone can browse and download.

### 3. Create your upload token (the part that enables website uploads)
This is what lets the admin save files from the website back into GitHub.

1. On github.com, go to **Settings → Developer settings → Personal access
   tokens → Fine-grained tokens → Generate new token**.
2. Give it a name (e.g. "Template library uploads") and an expiry.
3. Under **Repository access**, choose **Only select repositories** and pick
   this one repo.
4. Under **Permissions → Repository permissions**, set **Contents** to
   **Read and write**. (Leave everything else as "No access".)
5. Generate, and **copy the token** (starts with `github_pat_`). You won't see
   it again, so keep it somewhere safe.

### 4. Connect and go
On the website, click **Admin**, enter **`Lanks26`**, and you'll be asked to
paste the token. Paste it, tick "Remember on this device," and Connect. That's
it — you can now add templates by drag-and-drop.

---

## Daily use

**Staff:** open the site, search or browse by category, click **Download**.
Files open in Word with the locked legal language intact — they fill only the
editable fields.

**Admin:** click **Admin → enter `Lanks26` →** the button becomes **Add
template**. Click it, **drag a Word file onto the drop zone** (or click to
browse), fill in name / category / tags / description, and **Save template**.
The file is committed to `documents/` and the list updates automatically —
live for everyone within a minute (GitHub Pages takes a moment to refresh).
Edit and Remove on each card work the same way.

---

## About the token (please read once)

- The token is **never stored in the website code**. It lives only in the
  admin's browser (and only if they tick "Remember"). That's why it's safe to
  publish `index.html` publicly.
- The token grants write access to this one repo. Treat it like a password.
  Don't paste it on a shared/public computer with "Remember" ticked.
- If a token is ever exposed or someone leaves, **revoke it** on GitHub
  (Settings → Developer settings → the token → Delete) and generate a new one.
  Nothing else needs to change.
- Set a sensible **expiry** when creating it; GitHub will remind you to renew.

### More than one admin?
Each admin generates **their own** token the same way and pastes it on their
own device. Don't share one token between people — that way access can be
revoked individually. The `Lanks26` passcode is shared and just gates the UI;
the token is what actually authorizes changes.

---

## Good to know
- **It's all in GitHub:** your templates sit in `documents/` in the same repo
  as the site, version-history and all. Every upload/removal is a normal commit
  you can see and undo in GitHub.
- **Changing categories:** edit the `CATEGORIES` list in the CONFIG block of
  `index.html`.
- **Changing the passcode:** edit `ADMIN_CODE` in the same block.
- **GitHub Pages refresh:** after an upload, the live site may take up to a
  minute to show the change because Pages rebuilds. The admin's own view
  updates instantly.
- **File size:** GitHub's API handles files up to ~25 MB easily; Word templates
  are tiny, so no concern.
