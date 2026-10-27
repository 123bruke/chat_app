# ChatFlow

A modern real-time messaging app inspired by Telegram, built with React, TypeScript, and Firebase.

ChatFlow lets users send instant one-to-one messages, see online presence, manage profiles, switch themes, and collaborate in a clean, responsive chat experience.

![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-7-3178C6?logo=typescript&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-8-646CFF?logo=vite&logoColor=white)
![Tailwind](https://img.shields.io/badge/Tailwind%20CSS-4-38B2AC?logo=tailwindcss&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-12-FFCA28?logo=firebase&logoColor=white)

## Overview

ChatFlow is a browser-based chat application focused on speed, clarity, and usability. It uses Firebase for authentication and real-time Firestore data syncing, while the UI is powered by React and a responsive component system.

This project includes:

- real-time one-to-one messaging
- Google sign-in and email/password auth
- user discovery and conversation creation
- online/offline presence tracking
- read receipts and conversation status indicators
- theme and wallpaper customization
- mobile-friendly layout for desktop and smaller screens

## Features

### Messaging

- Real-time 1-on-1 conversation threads
- Instant delivery with Firestore listeners
- Message read state and presence indicators
- Rich messages with text, media, and file attachments
- Unread message counts and active conversation previews

### Authentication and accounts

- Google sign-in with popup fallback to redirect flow
- Email/password sign-up and sign-in
- Demo accounts for quick testing across tabs
- User search and profile browsing
- Editable profile details such as avatar and bio

### Experience

- Dark mode, light mode, and system theme support
- Custom chat wallpapers
- Notifications and send-on-enter behavior
- Responsive layout optimized for different screen sizes

## Tech stack

| Layer | Technology |
| --- | --- |
| Frontend | React 19 + TypeScript |
| Build tool | Vite 8 |
| Styling | Tailwind CSS 4 |
| Backend/Auth | Firebase Authentication |
| Database | Firestore |
| Icons | Lucide React |
| Animation | Motion |

## Project structure

```text
chat_app/
├── src/
│   ├── App.tsx
│   ├── main.tsx
│   ├── components/
│   │   ├── auth/
│   │   ├── sidebar/
│   │   ├── chat/
│   │   ├── profile/
│   │   └── settings/
│   ├── context/
│   │   ├── AuthContext.tsx
│   │   └── SettingsContext.tsx
│   ├── firebase/
│   │   ├── config.ts
│   │   └── firestoreService.ts
│   ├── types/
│   │   └── chat.ts
│   └── utils/
├── config/
├── docs/
├── firestore-data/
├── .env.example
├── index.html
├── package.json
├── tsconfig.json
├── vite.config.ts
├── README.md
└── LICENSE
```

## Getting started

### Prerequisites

- Node.js 18+
- A Firebase project with Authentication and Firestore enabled

### 1) Install dependencies

```bash
npm install
```

### 2) Configure environment variables

Copy the example file and provide your Firebase client configuration:

```bash
cp .env.example .env.local
```

Example:

```env
VITE_FIREBASE_API_KEY=your-api-key
VITE_FIREBASE_AUTH_DOMAIN=your-project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your-project-id
VITE_FIREBASE_STORAGE_BUCKET=your-project.firebasestorage.app
VITE_FIREBASE_MESSAGING_SENDER_ID=your-sender-id
VITE_FIREBASE_APP_ID=your-app-id
```

The app also includes a default Firebase config in `config/firebase-applet-config.json`, and environment variables override it when present.

### 3) Run the app

```bash
npm run dev
```

Open the local URL shown by Vite, typically:

- http://localhost:3000

### 4) Production build

```bash
npm run build
npm run preview
```

## Firebase setup

### Enable authentication

In the Firebase Console:

1. Go to Authentication
2. Enable Google sign-in
3. Enable Email/Password sign-in
4. Add your local and production domains under Authorized domains

### Firestore rules

The project includes Firestore security rules in `firestore-data/firestore.rules`.

These rules protect:

- user profile access
- conversation visibility
- message write permissions
- private-by-default access control

Deploy them with:

```bash
firebase deploy --only firestore:rules
```

## Demo accounts

For quick testing, you can use the built-in demo accounts:

- Alice — `alice.chatflow@example.com`
- Bob — `bob.chatflow@example.com`

Or use the quick test account buttons in the login screen to simulate a two-user conversation in separate tabs.

## Available scripts

```bash
npm run dev       # start the Vite dev server on port 3000
npm run build     # run type-check and create the production build
npm run preview   # preview the production build locally
npm run lint      # TypeScript type-check
```

## Troubleshooting

### Google sign-in popup blocked

This may happen when browser popup restrictions are active. The app automatically falls back to the redirect sign-in flow. If that still fails:

- ensure the domain is in Firebase Authorized domains
- enable Google sign-in in Firebase Authentication
- check whether the browser is blocking third-party cookies or popups

### auth/operation-not-allowed

This usually means the sign-in provider is disabled in Firebase. Enable it in Authentication → Sign-in method.

### Firestore connection warnings

The app reconnects automatically when the network is restored. If you see offline warnings, check the Firebase project configuration and Firestore rules.

## License

This project does not currently declare a public license. If you are planning to reuse or redistribute the code, contact the maintainer first.

---

Made with ❤️ by Biruk Ahmye
