# Bora Circle — Android + Web starter

A responsive installable web app/PWA with a Firebase-ready backend scaffold. The app runs immediately in **local demo mode** and stores demo progress in the browser. Connect Firebase to enable real accounts and cloud persistence.

## Run locally
Use a local static server (ES modules and service workers do not run reliably from `file://`):
```bash
python -m http.server 8080
```
Open `http://localhost:8080`.

## Connect Firebase (required for real multi-user production)
1. Create a Firebase project at https://console.firebase.google.com.
2. Add a Web app and copy its config into `firebase-config.js`.
3. Enable Authentication providers: Google and Email/Password. Configure authorized domains.
4. Create Firestore and Storage; deploy `firestore.rules` and `storage.rules`.
5. Install Firebase CLI, sign in, select your project, then deploy:
```bash
npm install -g firebase-tools
firebase login
firebase use --add
cd functions && npm install && cd ..
firebase deploy
```
6. Set the verified owner email:
```bash
firebase functions:config:set app.owner_email="..." # legacy config only; alternatively set OWNER_EMAIL parameter in deployment
```
For production, set the `OWNER_EMAIL` function parameter to `ezekieljidim@gmail.com` (or your replacement email), deploy functions, then sign in using that verified Google account. The backend grants owner claim only to the exact verified email. Do not put secrets in client code.
7. Configure App Check, billing plan as required, backup/export, abuse controls, and domain before public launch.

**Security note:** Owner access is a server-issued custom claim, never a client-editable profile field. Review rules and test with Firebase Emulator before launch. Configure owner email as a deployment parameter, not by trusting a submitted email.

## Android
This project is a PWA and can be installed from Chrome's browser menu once hosted over HTTPS. For a native Android wrapper:
```bash
npm install @capacitor/core @capacitor/cli
npx cap init "Bora Circle" "com.boracircle.app" --web-dir .
npm install @capacitor/android
npx cap add android
npx cap sync android
npx cap open android
```
Build/sign the APK or AAB in Android Studio. Push notifications on native Android require Firebase Cloud Messaging setup and Android platform configuration.

## Included in this build
- Premium purple responsive shell, home dashboard, profile editor, feed composer, reactions, comments, report/block actions, search, quiz, tier ladder, points shop, leaderboard, ARMY map aggregation view, notifications panel, admin tools, AI fan-helper panel.
- 28-level XP threshold table and progress calculations.
- Local demo persistence for fast preview.
- Firebase Google/email auth integration points, Firestore profile sync, Storage uploads, security rules, Cloud Functions for serial ARMY IDs, owner provisioning, XP ledger, and moderation.

## Important scope boundary
This is a runnable first build and backend scaffold, not a deployed production service. Live translation, video transcoding, push delivery, global map tiles, real-time chat, AI model access, moderation review operations, payments-free shop inventory synchronization, and competition voting need configured provider credentials and additional production testing. The UI labels these integrations honestly; do not launch publicly until those services are completed and tested.
