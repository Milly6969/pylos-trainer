# Pylos Trainer — GitHub Pages + Firebase deploy package

This repository contains the **latest Pylos Trainer v25 game** (wooden board, AI, local multiplayer, Analysis Board and Opening Explorer) repackaged as a smaller website with installable Android home-screen icon. The **second approved PYLOS icon** is used. No changes to core gameplay were intended.

## Files

- `docs/index.html`: game page (GitHub Pages site entry).
- `docs/assets/`: board and wooden sphere images split out of the HTML for faster caching.
- `docs/icons/`: app icon sizes, including Android maskable and Apple touch icons.
- `docs/manifest.webmanifest`, `docs/sw.js`: installable progressive web app (PWA) and offline asset cache.
- `docs/firebase-config.js`: paste your **public Firebase Web app configuration here once** before uploading (or paste into the existing Settings field on each device).
- `database.rules.json`: starter Firebase Realtime Database rules.
- `firebase.json`: optional **Firebase Hosting** config, pointing at the same `docs/` website.

## A. Upload to GitHub Pages

1. Make a GitHub repository (for example `pylos-trainer`). It can be public if using free GitHub Pages; the **web app code and Firebase Web app configuration will be visible** to anyone visiting the repository or site.
2. Upload the **contents of this package**, preserving folders. Commit files to the `main` branch.
3. Go to the GitHub repository's **Settings → Pages → Build and deployment → Source: Deploy from a branch**.
4. Select **main** and **/docs**, then save. GitHub provides a URL such as `https://YOUR_USERNAME.github.io/pylos-trainer/`.
5. Open that website in mobile Chrome. Install via browser menu **Add to Home screen** or **Install app** (wording differs). The selected icon should appear.

## B. Configure Firebase private multiplayer rooms

1. Visit `https://console.firebase.google.com/` and create a project.
2. Under **Authentication → Sign-in method**, turn on **Anonymous** sign-in.
3. Under **Authentication → Settings → Authorized domains**, add the hosted app domain, e.g. `YOUR_USERNAME.github.io`. If you later use Firebase Hosting too, also authorize its domain. Firebase may enforce extra domain requirements; follow the console's messages.
4. Under **Build → Realtime Database**, create a database in **Locked mode** initially. Do not leave world-readable/test rules enabled.
5. Open its **Rules** tab and publish the contents of `database.rules.json`. These are family-use starter rules for private rooms, not proof that all submitted moves are legal.
6. Under **Project settings → Your apps**, register a **Web app**; copy the client-side `firebaseConfig` values into `docs/firebase-config.js` including the exact **databaseURL** of your Realtime Database.
7. Commit the edited `docs/firebase-config.js` to GitHub and use the GitHub Pages URL. Firebase Web app config is *not* a private secret. **Never upload service-account JSON or private credentials.**
8. On both phones open the same hosted URL. Select **Opponent → Online multiplayer**. The first player taps **Create room** (Ivory), then sends the invitation link/code; the second player taps **Join room** (Onyx). Moves synchronize when each full turn—including reclaims—is completed.

**Limitation:** This project uses anonymous identity and an eight-character unlisted room code. It is intended for a private family game, not a hardened public multiplayer service. Firebase rules prevent casual unauthorized writes but do not validate the complete legality of every Pylos move. A malicious player running modified client code could still submit an invalid position. Real two-device play must be tested after you connect your Firebase project.

## C. Firebase Hosting alternative (optional)

GitHub Pages is sufficient to host the website; Firebase still supplies the real-time database. You do **not** need to deploy the site to both GitHub Pages and Firebase Hosting.

If you prefer to host the website on Firebase as well, install the Firebase CLI, sign in, and from the repository root run:

```bash
firebase login
firebase use --add
firebase deploy --only hosting,database
```

This uses the included `firebase.json` and publishes the `docs/` folder to Firebase Hosting along with the database rules. The Firebase project must be created first. Configure your hosted domain in Firebase Auth.

## D. Updating the game

- Replace game files under `docs/`, not `database.rules.json` unless rules change.
- When updating cached app files, change the `CACHE` version at the top of `docs/sw.js` and redeploy so installed apps refresh.
- The existing browser-local save key is retained. **Moving from a `file://` download to a hosted website creates a different browser origin**, so an old local saved game will not automatically appear on the website. The same applies to switching from GitHub Pages to a Firebase Hosting domain.
- Offline AI and local two-player modes work after the PWA assets are cached; live multiplayer always requires an internet connection.

## Verification checklist

- [ ] GitHub Pages site opens at HTTPS
- [ ] Wood default and Simple alternative work
- [ ] AI move, reclaim and Best move work
- [ ] Analysis Board and Opening Explorer open and return safely
- [ ] Add-to-Home-screen installs with the selected icon
- [ ] Firebase Anonymous Auth and Realtime Database rules are enabled
- [ ] A private room opens on phone 1 and phone 2 can join it
- [ ] A complete turn with a reclaim appears on the other phone

## Documentation

- [GitHub Pages branch publishing](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)
- [Firebase Realtime Database web setup](https://firebase.google.com/docs/database/web/start)
- [Firebase Anonymous Authentication](https://firebase.google.com/docs/auth/web/anonymous-auth)
- [Firebase Realtime Database security](https://firebase.google.com/docs/database/security)
