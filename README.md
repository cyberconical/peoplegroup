# Tessera made by peoplegroup

Share your place with people you have actually met.

Tessera is a local-first prototype for sharing flats only inside real acquaintance. You meet someone in person, connect with two scans, and each contact sees only what their circle (Family, Friends, Met) allows. There is no account and no server, and your data stays on your device.

**Status: prototype.** The network is simulated. Other people, the QR scan and answers to requests are demo data. Flats, terms, circles, the map and on-device storage are real. The business deck is built into the app.

## Install on Android

1. Install [Obtainium](https://github.com/ImranR98/Obtainium).
2. Add this repository's URL as an app source.
3. Obtainium installs the latest APK from Releases and reports updates.

The APK is signed with this repository's own key. Android may warn that the app comes from an unknown source. That warning is expected for apps outside the Play Store.

You can also open `index.html` directly in a browser. It works offline.

## Privacy

- No account, no analytics, no cookies, no tracking.
- The app makes no network requests. Fonts and map data are built in.
- Everything is stored in the app's local storage on your device.
- **Uninstalling the app or clearing its data deletes everything.** The prototype has no backup or export yet.

## Repository layout

```
index.html                    the whole app: HTML, CSS and JavaScript in one file
.github/workflows/build.yml   builds, signs and attaches the APK to a release
README.md
```

## How the build works

On every published release, GitHub Actions:

1. copies `index.html` into a fresh Capacitor 6 project (Node 20, Java 17),
2. draws the launcher icons and splash,
3. sets `versionCode` to the run number and `versionName` to the release tag,
4. builds, aligns, signs and verifies the APK,
5. attaches `tessera-<tag>.apk` to the release.

A manual run ("Actions" → "Build Android APK" → "Run workflow") builds the same APK and stores it as a workflow artifact instead.

The build stops if `index.html` contains embedded personal data. This guards against uploading a backup file by mistake.

## Publishing a new version from a phone

1. Upload the new `index.html` using "Add file" → "Upload files", then tap "Commit changes". Uploading a file with the same name replaces the old one.
2. Go to "Releases" → "Draft a new release", create a new tag such as `v0.2.0`, add a title and notes, then tap "Publish release".
3. After 5 to 8 minutes the APK is attached to the release, and Obtainium reports the update.

First setup: you cannot upload a folder whose name starts with a dot from a phone. Upload `build.yml` to the repository root, open it in the GitHub editor, and rename it to `.github/workflows/build.yml`.

## Signing

The keystore and its password are embedded in `build.yml`. Anyone can read them in this public repository. That is acceptable for a private app outside the Play Store, but it means anyone could build an APK that Android would treat as an update. To avoid this, move `KS_B64` and `KS_PASS` into repository secrets and reference them as `${{ secrets.KS_B64 }}` and `${{ secrets.KS_PASS }}`. The values must stay the same.

**Never change the key or the `appId` (`app.tessera`).** If either changes, existing installs can no longer be updated, and users have to uninstall, which deletes their data.

## License

[to decide]
