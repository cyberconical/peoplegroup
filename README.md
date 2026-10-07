# Tessera

Share your place with people you have actually met.

Tessera is a local-first prototype for sharing flats only inside real acquaintance. You meet someone in person, connect with two scans, and each contact sees only what their circle (Family, Friends, Met) allows. There is no account and no server, and your data stays on your device.

**Status: MVP.** Everything works between real phones, but without cryptography yet: codes are plain data, not signed or encrypted. The business deck is built into the app.

## How it works

1. **Meet.** One person opens "Meet someone" and shows a QR code. The other taps "Scan their code", picks a circle (Family, Friends, Met) and gets a code to show back. The first person scans that and does the same. Now both have each other, and each has the flats the other shares with their circle.
2. **Stay up to date.** When you change a flat or someone's circle, People marks them with "Update". Open them and send your flats. That works as a QR code or as a text you share through any messenger.
3. **Ask and answer.** Request a stay on someone's flat and send the code. They tap Receive, paste or scan it, and accept or decline. Their answer comes back the same way. Arrival notes travel only in an acceptance.

4. **Through your people.** On each flat you can set terms for "Through my people" and tick which of your circles may pass it on. If Ben is in your Family and you ticked Family, Ben's Family and Friends see that flat, marked "via Ben". It goes one step only, and arrival notes are never passed on.
5. **Intros.** To ask for a stay at a flat you see through someone, ask them for an intro. They send one code to both of you. For 30 days you can ask each other for stays directly. Meet and scan each other's codes in that time to stay connected for good. Otherwise the intro runs out and has to be shared again, which gives another 30 days.

In Stays, people you've met and people you see through someone are listed separately. Family is marked in solid black, friends with an outline, and flats through someone with a dotted line. The map uses the same marks. Under You, "How Tessera works" shows all of this as diagrams.

Every code starts with `TSR1.`. Pasting a whole chat message works, because the app finds the code inside it.

You can only add someone by scanning their code in person (or pasting it inside "Meet someone"). A code from someone you haven't met opens a "Meet first" screen instead.

## Install on Android

1. Install [Obtainium](https://github.com/ImranR98/Obtainium).
2. Add this repository's URL as an app source.
3. Obtainium installs the latest APK from Releases and reports updates.

The APK is signed with this repository's own key. Android may warn that the app comes from an unknown source. That warning is expected for apps outside the Play Store.

You can also open `index.html` directly in a browser. It works offline.

## Practise with test people

Under You, "Test people" adds made-up people to try everything on one phone:

- **Sam**, whom you meet with "Practise meeting" and put in your Friends. Show his code on a second screen and scan it to test the camera, or tap "Simulate scan".
- **Lea**, Sam's sister, in his Family, and **Jonas**, his friend. You see one flat of each through Sam, and Sam can introduce you.
- **Dad**, in your Family, with two flats.

Codes you send them stay on your phone, and they answer within a second. "Let test intros run out" lets you try an expired intro without waiting a month. Test people are never passed on to real contacts, and "Remove test people" deletes them.

## Privacy

- No account, no analytics, no cookies, no tracking.
- The app makes no network requests. Fonts, map data, the QR generator and the QR reader (jsQR, Apache-2.0) are built in.
- Everything is stored in the app's local storage on your device. Codes travel only the way you send them.
- **Codes are not encrypted yet.** Anyone who sees a code can read it. Send each code only to the person it is for, especially accepted requests, which hold your arrival notes.
- The Android app asks for the camera only to scan codes.

## Backup

Under You, tap "Save backup". You get a copy of the app with your data inside (`tessera-backup-<date>.html`). Opening that file in a browser shows your data; "Load backup" in the app restores it. On Android the backup goes through the share menu, so you can save it to Files or Drive.

**Never upload a backup file to this repository as `index.html`.** It contains your personal data. The build refuses to run if it finds data in `index.html`.

Uninstalling the app or clearing its data deletes everything that isn't in a backup.

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
