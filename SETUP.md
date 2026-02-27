# FocusFlow — Setup Guide

## Quick Start (No Sync)

Just open `index.html` in any browser. The app works fully offline with localStorage — no setup needed.

---

## Deploy to GitHub Pages (Free Hosting)

This makes FocusFlow accessible from any device via a URL.

### Step 1: Create a GitHub Repository

1. Go to [github.com/new](https://github.com/new)
2. Name it `focusflow` (or anything you like)
3. Set it to **Public** (required for free GitHub Pages)
4. Click **Create repository**

### Step 2: Upload Files

Upload all files from this folder to the repo:

```
focusflow/
├── index.html
├── manifest.json
├── sw.js
├── icon-192.png
├── icon-512.png
└── SETUP.md
```

You can drag-and-drop them into the GitHub web interface, or use git:

```bash
cd focusflow
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/focusflow.git
git push -u origin main
```

### Step 3: Enable GitHub Pages

1. Go to your repo → **Settings** → **Pages**
2. Under "Source", select **Deploy from a branch**
3. Select **main** branch and **/ (root)** folder
4. Click **Save**
5. Wait 1-2 minutes, then visit: `https://YOUR_USERNAME.github.io/focusflow/`

Your app is now live! You can install it as a PWA on any device.

---

## Set Up Cloud Sync (Optional)

Cloud sync lets your sessions and goals stay in sync across all your devices. It uses Firebase (free tier).

### Step 1: Create a Firebase Project

1. Go to [console.firebase.google.com](https://console.firebase.google.com)
2. Click **Create a project** (or Add project)
3. Name it `focusflow` → Continue
4. Disable Google Analytics (not needed) → Create Project

### Step 2: Enable Authentication

1. In Firebase Console, go to **Authentication** → **Get Started**
2. Click **Sign-in method** tab
3. Enable **Google** provider
4. Set a support email → Save
5. Go to **Settings** → **Authorized domains**
6. Add your GitHub Pages domain: `YOUR_USERNAME.github.io`

### Step 3: Create Firestore Database

1. Go to **Firestore Database** → **Create database**
2. Choose **Start in test mode** (you can add rules later)
3. Select a location closest to you → Enable

### Step 4: Get Your Firebase Config

1. Go to **Project Settings** (gear icon) → **General**
2. Scroll to "Your apps" → Click the web icon (`</>`)
3. Register app with nickname "FocusFlow" (no hosting needed)
4. Copy the `firebaseConfig` object — it looks like this:

```json
{
  "apiKey": "AIzaSy...",
  "authDomain": "focusflow-xxxxx.firebaseapp.com",
  "projectId": "focusflow-xxxxx",
  "storageBucket": "focusflow-xxxxx.appspot.com",
  "messagingSenderId": "123456789",
  "appId": "1:123456789:web:abcdef"
}
```

### Step 5: Add Config to FocusFlow

1. Open FocusFlow in your browser
2. Go to **Settings** → **Firebase Setup**
3. Paste the JSON config
4. Click **Save Config**
5. Reload the page
6. Click **Sign In** — sign in with your Google account

Your data now syncs automatically across all devices where you're signed in!

### Step 6: Secure Your Database (Recommended)

In Firestore Console → **Rules**, replace the default rules with:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

This ensures each user can only read/write their own data.

---

## Installing as a PWA

Once deployed to GitHub Pages, you can install FocusFlow like a native app:

- **Chrome/Edge (Desktop):** Click the install icon in the address bar, or the install banner in the app
- **Safari (iOS):** Tap Share → Add to Home Screen
- **Chrome (Android):** Tap the install banner, or Menu → Add to Home Screen

The app works offline after installation!

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Sign-in popup blocked | Allow popups for your GitHub Pages domain |
| "Auth domain not authorized" | Add your domain in Firebase → Authentication → Settings → Authorized domains |
| Data not syncing | Check browser console for errors; verify Firestore is in test mode or rules are correct |
| PWA not installable | Must be served over HTTPS (GitHub Pages does this automatically) |
| Service worker not updating | Hard refresh (Ctrl+Shift+R) or clear site data in DevTools |
