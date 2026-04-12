# FitForge v6 — Firebase Cloud Setup Guide

## Overview of what this guide covers
1. **Firebase Console** — create your free cloud database
2. **index.html** — paste your config (2 minutes)
3. **GitHub** — deploy to the web
4. **Phone** — install as an app
5. **Migrating your old 5 days of data** — zero data loss

---

# PART 1 — GOOGLE FIREBASE SETUP

## Step 1 — Create a Firebase Account & Project

1. Open your browser and go to:
   ```
   https://console.firebase.google.com
   ```

2. Sign in with your **Google account** (Gmail).

3. Click the big **"Create a project"** button.

4. Enter a project name — example: `fitforge-myname`
   - It doesn't matter what you call it

5. On the next screen — **Google Analytics**: toggle it **OFF** (not needed)

6. Click **"Create project"**

7. Wait about 10 seconds for it to provision, then click **"Continue"**

---

## Step 2 — Register a Web App

1. On your project's home page, look for the icons in the "Get started" section
2. Click the **Web icon** — it looks like `</>`
3. In the "App nickname" field, type: `FitForge`
4. **Do NOT** check "Firebase Hosting" — you're using GitHub Pages
5. Click **"Register app"**
6. You'll now see a screen showing your **config object** — it looks like this:

```javascript
const firebaseConfig = {
  apiKey:            "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain:        "fitforge-myname.firebaseapp.com",
  projectId:         "fitforge-myname",
  storageBucket:     "fitforge-myname.appspot.com",
  messagingSenderId: "123456789012",
  appId:             "1:123456789012:web:abcdef1234567890"
};
```

7. **COPY THIS ENTIRE BLOCK** — you'll need it in Part 2

8. Click **"Continue to console"**

---

## Step 3 — Enable Anonymous Authentication

This lets the app sign each browser in silently — no username/password needed.

1. In the left sidebar, click **"Build"** → **"Authentication"**
2. Click **"Get started"**
3. Click the **"Sign-in method"** tab
4. Find **"Anonymous"** in the list and click it
5. Toggle the **"Enable"** switch to ON (blue)
6. Click **"Save"**

✅ Done — your users will get a silent, automatic login.

---

## Step 4 — Create the Firestore Database

This is the actual cloud database that stores your workout data.

1. In the left sidebar, click **"Build"** → **"Firestore Database"**
2. Click **"Create database"**
3. On the next screen — choose **"Start in test mode"**
   - This allows read/write without login checks for 30 days (fine for personal use)
   - You'll update the rules in Step 5
4. Choose your **Cloud Firestore location** — pick the one closest to you:
   - India/Asia: `asia-south1` (Mumbai)
   - Europe: `eur3`
   - US: `nam5`
5. Click **"Enable"**
6. Wait a few seconds for the database to provision

---

## Step 5 — Set Firestore Security Rules

This step locks your database so only your own anonymous session can read/write your data.

1. In Firestore, click the **"Rules"** tab
2. Delete everything that's there and paste this:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/data/{document} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

3. Click **"Publish"**

✅ This means: only the logged-in anonymous user can access their own data path.

---

# PART 2 — UPDATE YOUR index.html

## Step 6 — Paste Your Firebase Config

1. Open `index.html` in any text editor (Notepad, VS Code, TextEdit, etc.)

2. Search for this block near the top of the file:
```javascript
const FIREBASE_CONFIG = {
  apiKey:            "PASTE_YOUR_API_KEY_HERE",
  authDomain:        "PASTE_YOUR_AUTH_DOMAIN_HERE",
  projectId:         "PASTE_YOUR_PROJECT_ID_HERE",
  storageBucket:     "PASTE_YOUR_STORAGE_BUCKET_HERE",
  messagingSenderId: "PASTE_YOUR_MESSAGING_SENDER_ID_HERE",
  appId:             "PASTE_YOUR_APP_ID_HERE"
};
```

3. Replace the placeholder values with YOUR actual values from Step 2.

Example — your file should look like this after pasting:
```javascript
const FIREBASE_CONFIG = {
  apiKey:            "AIzaSyAbc123XYZexampleKeyHere",
  authDomain:        "fitforge-myname.firebaseapp.com",
  projectId:         "fitforge-myname",
  storageBucket:     "fitforge-myname.appspot.com",
  messagingSenderId: "123456789012",
  appId:             "1:123456789012:web:abcdef1234567890"
};
```

4. **Save the file**

---

# PART 3 — GITHUB DEPLOYMENT

## Step 7 — Create a New GitHub Repository

1. Go to **https://github.com** — sign in or create an account

2. Click **"+"** (top right) → **"New repository"**

3. Fill in:
   - **Repository name**: `fitforge` (or anything you like)
   - **Visibility**: Public
   - **DO NOT** check "Add a README file"
   - **DO NOT** add .gitignore or license

4. Click **"Create repository"**

---

## Step 8 — Upload Your Files

1. On the empty repository page, click **"uploading an existing file"**

2. Drag and drop ALL 4 files at once:
   ```
   index.html
   manifest.json
   sw.js
   README.md
   ```

3. In the "Commit changes" section at the bottom:
   - Message: `FitForge v6 — Firebase cloud sync`

4. Click **"Commit changes"**

---

## Step 9 — Enable GitHub Pages

1. Click the **"Settings"** tab (top of your repo)

2. In the left sidebar, scroll down and click **"Pages"**

3. Under **"Source"**:
   - Select: **"Deploy from a branch"**

4. Under **"Branch"**:
   - Branch: **main**
   - Folder: **/ (root)**

5. Click **"Save"**

6. Wait **2–3 minutes** for deployment

7. Refresh the Pages settings — your URL will appear:
   ```
   https://YOUR-USERNAME.github.io/fitforge/
   ```

---

## Step 10 — Add Your GitHub URL to Firebase (CORS fix)

To allow your GitHub Pages site to talk to Firebase:

1. Go back to Firebase Console → **Authentication**
2. Click the **"Settings"** tab
3. Scroll to **"Authorized domains"**
4. Click **"Add domain"**
5. Enter your GitHub Pages domain:
   ```
   YOUR-USERNAME.github.io
   ```
6. Click **"Add"**

✅ Firebase will now accept requests from your GitHub Pages URL.

---

# PART 4 — INSTALL ON PHONE

## Android (Chrome)

1. Open `https://YOUR-USERNAME.github.io/fitforge/` in **Chrome**
2. Wait for the app to load fully (first load may take 5 seconds)
3. Tap the **⋮** (three-dot menu) in the top-right
4. Tap **"Add to Home Screen"**
5. Tap **"Add"**
6. FitForge icon appears on your home screen ✅

## iOS (Safari)

1. Open the URL in **Safari** (must be Safari, not Chrome, for iOS install)
2. Wait for the app to fully load
3. Tap the **Share button** (rectangle with arrow pointing up)
4. Scroll down and tap **"Add to Home Screen"**
5. Tap **"Add"**
6. FitForge icon appears ✅

---

# PART 5 — MIGRATE YOUR EXISTING 5 DAYS OF DATA

Your old app used `localStorage` on the old GitHub URL. Here is how to carry that data over with zero loss.

## Method A — Automatic Migration (if old URL still works)

1. Open your **OLD GitHub Pages URL** in the browser (if it still loads the app)
2. Click the **"⬇ Export"** button in the top bar
3. A file named `fitforge_backup_YYYY-MM-DD.json` will download
4. Now open your **NEW GitHub Pages URL**
5. Click **"⬆ Import"** in the top bar
6. Select the downloaded `.json` file
7. The app will load all your data AND sync it to Firebase cloud ✅

## Method B — Browser Console (if old URL is inaccessible)

If your old GitHub repo URL is gone:

1. Open Chrome on the device where you originally used the app
2. Go to the **OLD URL** (even if it shows a 404, the localStorage might still be there)
   - Or open `chrome://settings/content/all` → find the old site → open it
3. Press **F12** (or right-click → Inspect) to open DevTools
4. Click **"Application"** tab → **"Local Storage"** → click the old URL
5. Find the key `ff_v5`
6. Right-click the value → **Copy value**
7. Create a file called `fitforge_backup.json` and paste this structure:

```json
{
  "version": "ff_v5",
  "exportedAt": "2025-01-01T00:00:00.000Z",
  "state": PASTE_YOUR_COPIED_VALUE_HERE
}
```

8. Save the file and import it into the new app

## Method C — Manual Entry (last resort)

If none of the above work, open the app fresh and re-enter your 5 days:
- Go to each completed day in the Workout screen
- Enter your weights and reps
- Check all sets as done
- Save — the recommendation engine will learn from this data going forward

---

# PART 6 — UPDATING THE APP IN FUTURE

## Option A: GitHub Web Interface (easiest)

1. Go to your GitHub repo
2. Click `index.html`
3. Click the **pencil icon** (Edit this file)
4. Select all → delete → paste new code
5. Click **"Commit changes"**
6. Wait 2 minutes → hard refresh on your phone

## Option B: Git Command Line

```bash
# First time — clone your repo
git clone https://github.com/YOUR-USERNAME/fitforge.git
cd fitforge

# Make your changes to index.html, then:
git add .
git commit -m "Updated: [describe what changed]"
git push origin main

# GitHub Pages auto-deploys in ~2 minutes
```

---

# TROUBLESHOOTING

## App shows "Connecting to cloud…" forever
- Your Firebase config values are wrong — re-check Step 6
- Make sure you added your GitHub Pages domain in Firebase Authorized Domains (Step 10)

## "Permission denied" error in console
- Your Firestore security rules aren't set — redo Step 5

## Data not syncing between devices
- Each anonymous session is tied to one browser
- Use **Export on Device A → Import on Device B** to transfer data
- Both devices will then sync independently to the cloud

## Changes on GitHub not showing on phone
- Hard refresh: hold the reload button → "Hard Reload"
- Or clear site data: Settings → Privacy → Clear browsing data

## "PASTE_YOUR_API_KEY_HERE" still visible
- You forgot to paste your Firebase config — redo Step 6

---

# HOW THE SYNC WORKS

```
┌─────────────┐         ┌──────────────────┐         ┌─────────────┐
│  Your Phone  │ ──────► │  Firebase Cloud   │ ◄────── │  Your Laptop │
│  (Chrome)    │         │  Firestore DB     │         │  (any browser)│
└─────────────┘         └──────────────────┘         └─────────────┘
       │                        │
       └── localStorage ────────┘ (offline backup, always written too)
```

- Every set tick → instantly saves to localStorage (instant, offline-safe)
- Every "Save & Sync" button → pushes full state to Firestore cloud
- On app open → loads from cloud first, falls back to local if offline
- Old data from previous localStorage sessions → auto-migrated on first cloud login

---

*FitForge v6 — Adaptive coach, cloud-synced, zero data loss*
