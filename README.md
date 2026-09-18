<div align="center">

# 💬 ChatFlow

**A modern, Telegram-inspired real-time messaging app built with React, TypeScript & Firebase.**

ChatFlow brings instant 1-on-1 messaging to the browser with live presence, search, profiles, themes and a fully responsive UI.

![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-7-3178C6?logo=typescript&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-8-646CFF?logo=vite&logoColor=white)
![Tailwind](https://img.shields.io/badge/Tailwind%20CSS-4-38B2AC?logo=tailwindcss&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-12-FFCA28?logo=firebase&logoColor=white)

</div>

---

## ✨ Features

### Messaging
- **Real-time 1-on-1 chats** — instant delivery via Firestore live listeners
- **Message read receipts & presence** — online/offline status with last-seen timestamps
- **Rich messages** — text, image uploads and file attachments
- **Unread counters** — per-conversation badges that reset when you open the chat

### Accounts & Users
- **Multiple sign-in methods**:
  - Google Sign-In (popup with automatic **redirect fallback** when popups are blocked)
  - Email & password (login + registration)
  - **1-click demo accounts** (Alice & Bob) for instant multi-tab testing
- **User search** by name or username, with a "New Chat" modal
- **Profiles** — avatars, bios, and a profile viewer for other users

### Experience
- **Dark / Light / System themes**
- **Chat wallpapers** — default, pattern, midnight, sage, sunset
- **Settings** — enter-to-send, sound notifications, and more
- **Fully responsive** — mobile-first layout with adaptive sidebar/chat switching

---

## 🧰 Tech Stack

| Layer      | Technology                                   |
| ---------- | -------------------------------------------- |
| Frontend   | [React 19](https://react.dev/) + TypeScript  |
| Build      | [Vite 8](https://vitejs.dev/)                |
| Styling    | [Tailwind CSS 4](https://tailwindcss.com/)   |
| Backend    | [Firebase](https://firebase.google.com/) (Auth + Firestore) |
| Icons      | [Lucide React](https://lucide.dev/)          |
| Animations | [Motion](https://motion.dev/)                |

---

## 🚀 Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) 18+ (npm is bundled)
- A [Firebase](https://firebase.google.com/) project (for auth + Firestore)

### 1. Install dependencies

```bash
npm install
```

### 2. Configure environment variables

Copy `.env.example` to `.env.local` and fill in your Firebase client config:

```bash
VITE_FIREBASE_API_KEY=your-api-key
VITE_FIREBASE_AUTH_DOMAIN=your-project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your-project-id
VITE_FIREBASE_STORAGE_BUCKET=your-project.firebasestorage.app
VITE_FIREBASE_MESSAGING_SENDER_ID=your-sender-id
VITE_FIREBASE_APP_ID=your-app-id
```

> The app also ships with a default Firebase config in
> [`config/firebase-applet-config.json`](./config/firebase-applet-config.json). Any `VITE_FIREBASE_*`
> environment variables **override** it, so you can run against your own project without
> touching the config file.

### 3. Run the dev server

```bash
npm run dev
```

Open http://localhost:3000 (or the URL Vite prints).

### 4. Production build

```bash
npm run build   # outputs to dist/
npm run preview # serve the production build locally
```

---

## 🔥 Firebase Setup

### Enable Authentication

1. Go to your Firebase console → **Authentication → Sign-in method**.
2. Enable **Google** and **Email/Password**.
3. Under **Settings → Authorized domains**, add the domains where the app runs
   (e.g. `localhost`, your preview/production domain). Missing domains cause
   `auth/unauthorized-domain` errors.

### Security rules

The project ships hardened Firestore rules in [`firestore.rules`](./firestore.rules)
covering:

- **Users** — read for any signed-in user, write/update only for the owner
- **Conversations** — accessible only to participants
- **Messages** — participants can read and create; only the sender can edit their own
  messages while recipients can update delivery status
- **Default deny-all** catch-all rule

Deploy them to your Firebase project (or your emulator) before going live:

```bash
firebase deploy --only firestore:rules
```

### Data model

```
users/{uid}                          → UserProfile
conversations/{conversationId}       → Conversation (participants + preview)
conversations/{id}/messages/{msgId}  → Message
```

> `config/firebase-applet-config.json` also declares a **namespace Firestore database**
> (`firestoreDatabaseId`). The app explicitly passes it to `getFirestore()` in
> [`src/firebase/config.ts`](./src/firebase/config.ts).

---

## 🧭 Troubleshooting

### `Firebase: Error (auth/popup-blocked).`

This happens when the browser (or an embedded preview iframe) blocks the Google
sign-in popup. ChatFlow handles it automatically:

1. It tries `signInWithPopup` first.
2. If the popup is blocked, it falls back to `signInWithRedirect` (a full-page redirect).
3. On return, `getRedirectResult` completes the sign-in.

If you still can't sign in with Google, check:

- The domain is listed under Firebase **Authorized domains**.
- **Google** is enabled in Authentication → Sign-in method.
- The browser isn't aggressively blocking third-party popups/cookies.

### `auth/operation-not-allowed`

The requested sign-in provider is not enabled in your Firebase project. Enable it under
**Authentication → Sign-in method**.

### "Client is offline" / connection warnings

Firestore may not have warm connections yet or network restrictions apply. Messages
resume automatically once connected.

---

## 👥 Demo Accounts

Skip setup and chat with yourself across two browser tabs:

- **Alice** — `alice.chatflow@example.com`
- **Bob** — `bob.chatflow@example.com`

Or just use the **Quick Test Accounts** buttons right on the login screen —
no credentials needed. Open the app in two tabs, log in as Alice in one and Bob in
the other, and watch messages arrive in real time.

---

## 🗂️ Project Structure

```
src/
├── main.tsx                  # App entry point
├── App.tsx                   # Root layout, modals, conversation wiring
├── components/
│   ├── auth/AuthScreen.tsx   # Login / register / Google / demo login
│   ├── sidebar/              # Conversation list, user search modal
│   ├── chat/                 # Chat window, bubbles, composer, media preview
│   ├── profile/              # Edit-profile & view-profile modals
│   └── settings/             # Theme, wallpaper & notification settings
├── context/
│   ├── AuthContext.tsx       # Firebase auth + redirect/popup flow
│   └── SettingsContext.tsx   # Persisted user preferences
├── firebase/
│   ├── config.ts             # Firebase init, Firestore helpers, errors
│   └── firestoreService.ts   # All Firestore CRUD + realtime listeners
├── types/chat.ts             # Shared types (UserProfile, Conversation, Message)
└── utils/                    # date & audio helpers
```

---

## 📜 Available Scripts

| Command             | Description                          |
| ------------------- | ------------------------------------ |
| `npm run dev`       | Start the Vite dev server on :3000  |
| `npm run build`     | Type-check (via lint) + production build |
| `npm run lint`      | Type-check with `tsc --noEmit`      |
| `npm run preview`   | Serve the production build locally   |

---

## 📄 License

Private / internal project. No license is specified — contact the maintainer before
reusing the code.

---

<div align="center">
  Made with ❤️ by   Biruk Ahmye
</div>
