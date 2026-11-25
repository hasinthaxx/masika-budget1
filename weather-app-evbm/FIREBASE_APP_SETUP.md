# Firebase and iPhone setup

## Enable cloud sign-in and sync

1. Create a Firebase project and register a **Web app**. Copy its `apiKey`, `authDomain`, `projectId`, and `appId` into `firebase-config.js`. These web app values identify the Firebase project; they are not server credentials. Never put a service-account key or Apple private key in this app.
2. In Firebase Console, enable **Authentication → Google**. Add the deployed app's HTTPS hostname under **Authentication → Settings → Authorized domains**.
3. If Apple sign-in is wanted, enable **Authentication → Apple** and finish Apple's Services ID, Team ID, Key ID, and private-key setup in Firebase Console. Keep the Apple private key in Firebase, not in this repository. Google and Apple may be linked from the account panel.
4. Create the **Cloud Firestore** database. Publish the contents of `firestore.rules` in Firestore → Rules. These rules allow each signed-in user to read and change only their own transactions.
5. Serve the app from an HTTPS host, deploy it, and test Google/Apple sign-in, a transaction edit, deletion, and sync from a second browser/device. The app asks before uploading existing local transactions to a different account. Currency preferences remain device-local; transaction amounts are not converted.

The same rules can be deployed with the Firebase CLI after selecting the project:

```text
firebase use --add
firebase deploy --only firestore:rules
```

The Firebase SDK is loaded from Google's `gstatic` CDN only when the four web config values are filled in. With the template values empty, the app continues to work as a local-only PWA.

## iPhone and App Store

This repository is currently a static PWA, not an iOS application project. iPhone users can install the HTTPS-hosted PWA from Safari using **Share → Add to Home Screen**. This implementation does not include an Xcode project, Capacitor wrapper, native URL scheme, or signed iOS build.

An App Store release still requires a Mac with Xcode, a native wrapper/build configuration, Apple Developer enrollment, bundle ID and signing assets, App Store Connect setup, provider callback configuration, device testing, privacy disclosures, and review. The in-app account deletion control deletes that Firebase user's transaction documents and then requests Firebase Auth account deletion; Firebase may require a recent sign-in, and a failure is reported rather than silently treated as success. Do not describe this PWA scaffold as App Store-ready.
