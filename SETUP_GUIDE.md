# 🔥 Firebase Setup Guide — Bachelorette Party App

Follow these steps **in order** to get the app working with live data.

---

## Step 1 — Create a Firebase Project

1. Go to [https://console.firebase.google.com](https://console.firebase.google.com)
2. Click **"Add project"**
3. Enter a project name (e.g. `sofia-bachelorette`)
4. Disable Google Analytics if you don't need it (optional)
5. Click **"Create project"** and wait for it to be ready

---

## Step 2 — Enable Firestore (Database)

1. In your Firebase project, click **"Firestore Database"** in the left sidebar
2. Click **"Create database"**
3. Choose **"Start in test mode"** (you can tighten security later)
   - Test mode allows read/write for 30 days without authentication
4. Choose a region (e.g. `eur3` for Europe, `us-central` for US)
5. Click **"Enable"**

Firestore will automatically create the following collections when the app first runs:
- `rsvp_list` — stores RSVP form submissions
- `gallery_images` — stores URLs of uploaded photos

---

## Step 3 — Enable Firebase Storage (for Photo Gallery)

1. In your Firebase project, click **"Storage"** in the left sidebar
2. Click **"Get started"**
3. Choose **"Start in test mode"**
4. Click **"Next"** → then **"Done"**

### ✏️ Update Storage Rules (Important!)
After enabling Storage, click the **"Rules"** tab and replace the existing rules with:

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if true;
    }
  }
}
```

Click **"Publish"**. (This allows public uploads — fine for a private party app.)

---

## Step 4 — Register a Web App & Get Your Config

1. In your Firebase project, click the **gear icon (⚙️)** → **"Project settings"**
2. Scroll down to **"Your apps"** section
3. Click the **web icon `</>`**
4. Enter an app nickname (e.g. `bachelorette-web`)
5. **Do NOT** check "Firebase Hosting" for now
6. Click **"Register app"**
7. You will see a block of code like this:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project-id",
  storageBucket: "your-project-id.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef123456"
};
```

8. **Copy this entire object**

---

## Step 5 — Paste Config Into the App

1. Open the file `js/firebase-config.js` in your code editor
2. Find the placeholder `firebaseConfig` object:

```javascript
const firebaseConfig = {
  apiKey:            "YOUR_API_KEY",
  authDomain:        "YOUR_PROJECT_ID.firebaseapp.com",
  ...
};
```

3. **Replace it** with your copied config from Step 4
4. Save the file

---

## Step 6 — Customize App Content

Open these files and look for `✏️ EDIT` comments to personalize:

| File | What to customize |
|---|---|
| `index.html` | Bride's name, event date |
| `location.html` | Venue name, address, GPS coordinates, Waze/Maps links |
| `payments.html` | Amount, deadline, phone number, Bit/Paybox links |
| `schedule.html` | Timeline events and times |
| `quiz.html` | Quiz questions, options, and answers about your bride |

---

## Step 7 — Deploy the App (Choose One)

### Option A — Firebase Hosting (Recommended, Free)

```bash
# Install Firebase CLI
npm install -g firebase-tools

# Login
firebase login

# Initialize (in your project folder)
firebase init

# Choose: Hosting
# Public directory: . (just a dot, the current folder)
# Single page app: No

# Deploy!
firebase deploy
```

Your app will be live at `https://your-project-id.web.app` 🎉

---

### Option B — Netlify (Drag & Drop, Free)

1. Go to [https://netlify.com](https://netlify.com) and sign up
2. Drag your entire project folder onto the Netlify dashboard
3. Done! You'll get a live URL instantly.

---

### Option C — GitHub Pages

1. Push your project to a GitHub repository
2. Go to repo **Settings → Pages**
3. Set Source to `main` branch, root folder
4. Click Save — your app is live at `https://username.github.io/repo-name`

---

## ✅ Checklist Before Sharing the Link

- [ ] Firebase config pasted into `js/firebase-config.js`
- [ ] Firestore enabled in test mode
- [ ] Storage enabled in test mode with correct rules
- [ ] `location.html` — address and GPS coordinates updated
- [ ] `payments.html` — amount, deadline, and payment links updated
- [ ] `schedule.html` — timeline events customized
- [ ] `quiz.html` — all 10 questions updated for your bride
- [ ] App deployed and tested on mobile

---

## 🔐 Security Note

The current setup uses **test mode** which allows open read/write access for 30 days. This is fine for a small, private party app. After your event, you can delete the project or update the security rules to lock it down.

---

## 📁 Project File Structure

```
bachelorette-app/
├── index.html          ← Home screen (6-block grid menu)
├── location.html       ← Directions & Maps
├── rsvp.html           ← RSVP form → Firestore
├── payments.html       ← Payment links & info
├── schedule.html       ← Day timeline
├── gallery.html        ← Photo upload → Storage + Firestore
├── quiz.html           ← Bride trivia quiz
├── css/
│   ├── global.css      ← Shared styles, theme, components
│   ├── index.css       ← Home screen grid styles
│   ├── location.css    ← Location page styles
│   ├── rsvp.css        ← RSVP form styles
│   ├── payments.css    ← Payments page styles
│   ├── schedule.css    ← Timeline styles
│   ├── gallery.css     ← Gallery & lightbox styles
│   └── quiz.css        ← Quiz game styles
└── js/
    └── firebase-config.js  ← 🔥 YOUR CONFIG GOES HERE
```

---

Made with 💗 for the party of the year!
