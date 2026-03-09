# 🎒 Lost & Found Gallery — Setup Guide

## Files in this repo

| File | Purpose |
|------|---------|
| `index.html` | Public gallery — parents browse this |
| `admin.html` | Staff upload page — protected by Google login |
| `photos.json` | Auto-managed item database (don't edit manually) |
| `images/` | Auto-created folder where photos are stored |

---

## Step 1 — Create your GitHub repo

1. Create a new **public** repo on GitHub (e.g. `lost-and-found`)
2. Upload `index.html`, `admin.html`, and `photos.json` to the root
3. Go to **Settings → Pages → Source → Deploy from branch → main / root**
4. Your gallery will be live at: `https://YOUR_USERNAME.github.io/lost-and-found/`

---

## Step 2 — Update index.html

Open `index.html` and find these two lines near the bottom and update them:

```js
const REPO_OWNER = 'YOUR_GITHUB_USERNAME';
const REPO_NAME  = 'YOUR_REPO_NAME';
```

---

## Step 3 — Set up Google OAuth

This lets only people with a Google account log into the admin page (and logs who uploaded each item).

1. Go to https://console.cloud.google.com/
2. Create a new project (e.g. "lost-and-found")
3. Go to **APIs & Services → OAuth consent screen**
   - User type: External
   - Fill in app name and your email
4. Go to **APIs & Services → Credentials → Create Credentials → OAuth 2.0 Client ID**
   - Application type: **Web application**
   - Authorized JavaScript origins: `https://YOUR_USERNAME.github.io`
5. Copy the **Client ID** (looks like `123456789.apps.googleusercontent.com`)
6. Open `admin.html` and paste it here:
   ```js
   const GOOGLE_CLIENT_ID = 'YOUR_GOOGLE_CLIENT_ID.apps.googleusercontent.com';
   ```

---

## Step 4 — Create a GitHub Personal Access Token

This allows the admin page to upload photos directly to your repo.

1. Go to GitHub → **Settings → Developer settings → Personal access tokens → Fine-grained tokens**
2. Click **Generate new token**
   - Name: "Lost and Found Admin"
   - Expiration: 1 year (you'll need to renew)
   - Repository access: Only select **this repo**
   - Permissions → Contents: **Read and write**
3. Copy the token (starts with `ghp_`)
4. In the admin page, go to **Settings** and paste it in the **GitHub Personal Access Token** field
5. Also fill in **GitHub Repo** as `yourusername/your-repo-name`

> ⚠️ This token is stored in the browser's localStorage — it never leaves the device of the person logged in. Do not share it publicly.

---

## Step 5 — AI Categorization (optional, pending privacy review)

> **Do not enable this until your school has reviewed and approved it.**

When enabled, each photo is sent to Anthropic's Claude API with this exact prompt:
> *"This is a lost and found item photo. Classify it as exactly one of: Jacket / Hoodie, Shirt / Pants, Water Bottle, Backpack / Bag, Other. Reply with just the category name."*

No names, no personal information, no metadata is sent. Anthropic does not store images beyond the API call.

To enable when approved:
1. Get an API key at https://console.anthropic.com/
2. Paste it in the **Claude API Key** field in the admin Settings card
3. Toggle **AI Auto-Categorization** on

---

## Day-to-day usage (for staff)

1. Go to `https://YOUR_USERNAME.github.io/lost-and-found/admin.html`
2. Sign in with Google
3. Tap the upload area, take a photo of the item
4. Select (or confirm AI-selected) category
5. Tap **Upload Item** — done! Live within ~30 seconds

To clean up old items: use the **Purge Old Items** card, select a time range, and purge.

---

## Sharing with parents

Send them just the gallery URL:
```
https://YOUR_USERNAME.github.io/lost-and-found/
```

Works great in a newsletter, school app, or QR code posted near the lost & found bin.
