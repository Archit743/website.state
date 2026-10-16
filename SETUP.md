# Pradeep Sales OMS — Firebase Setup Guide

Your orders are now saved to Firebase Firestore — a free cloud database.
Every device that opens the app will see the same live data.

---

## Step 1 — Create a Firebase Project (free)

1. Go to https://console.firebase.google.com
2. Click **Add project**
3. Name it: `pradeep-sales-oms`
4. Disable Google Analytics (not needed) → **Create project**

---

## Step 2 — Create Firestore Database

1. In the left sidebar → **Build → Firestore Database**
2. Click **Create database**
3. Choose **Start in test mode** → Next
4. Select region: `asia-south1 (Mumbai)` → **Enable**

---

## Step 3 — Register your Web App & get config

1. In Firebase console → click the **</>** (Web) icon on the home screen
2. App nickname: `oms-web` → click **Register app**
3. You will see a code block like this:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "pradeep-sales-oms.firebaseapp.com",
  projectId: "pradeep-sales-oms",
  storageBucket: "pradeep-sales-oms.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

4. Copy all these values.

---

## Step 4 — Paste config into app.js

Open `app.js` and find the top section (lines 1–12):

```js
const FIREBASE_CONFIG = {
  apiKey:            "PASTE_YOUR_API_KEY_HERE",
  authDomain:        "PASTE_YOUR_PROJECT_ID.firebaseapp.com",
  projectId:         "PASTE_YOUR_PROJECT_ID",
  ...
};
```

Replace each `"PASTE_..."` value with the values from Step 3.

---

## Step 5 — Deploy to Vercel

Push all three files to your GitHub repo:
- `index.html`
- `style.css`
- `app.js`

Vercel will auto-deploy. Done.

---

## What happens now

- Every order created is saved to Firestore instantly
- Every device that opens the app gets a **live real-time feed**
- If someone updates an order on one PC, all other open tabs update automatically within 1 second
- Data is **permanent** — it does not reset on page load

## Firestore free tier limits (more than enough)

| What | Free limit |
|---|---|
| Reads | 50,000 / day |
| Writes | 20,000 / day |
| Storage | 1 GB |

For a 5-person team handling 100 orders/day, you will not come close to these limits.

---

## Troubleshooting

**Red error bar at the top of the app**
→ Your Firebase config in app.js is still set to placeholder values. Paste the real config.

**"Missing or insufficient permissions"**
→ Your Firestore rules are blocking reads/writes. Go to Firebase → Firestore → Rules and make sure it says:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```
(This is fine for internal team use. Add authentication later if needed.)

**Orders not showing on other PC**
→ Make sure both devices are using the same Vercel URL, not opening the HTML file locally from disk.
