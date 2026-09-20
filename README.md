# Apney — Releases

Public download host for the **Apney** Android app (formerly Aangan). Source code lives in a
separate private repository; this repo only carries release binaries.

## Download

**[Download the latest APK](https://github.com/Martial-Gautam/aangan-release/releases/latest/download/app-release.apk)**

That link always resolves to the newest published release, so it never needs
updating. The Apney website links to it through its own `/download` route.

## Installing

Android blocks installs from outside the Play Store by default. After the
download finishes, open the file and allow installs from your browser when
prompted.

## Publishing a new version

The APK must be attached as a **release asset**, never committed:

1. In `~/Aangan_app/pubspec.yaml`, raise `version:` — both parts, e.g.
   `1.0.1+2` → `1.0.2+3`. Android only installs an update whose build
   number (after the `+`) is higher than the one on the phone.
2. `./release.sh` in `~/Aangan_app` — it builds with `android/key.properties`
   (the release key) and `.release.env` (the crash-reporting DSN), and refuses
   to build without the key. An APK built without it
   carries the debug key and will not install over what people have.
   Three files come out; publish **`app-arm64-v8a-release.apk`** (~40 MB,
   every phone since ~2017), renamed to `app-release.apk`. The unsplit
   `flutter build apk` bundles all three architectures into 112 MB that every
   phone downloads and two-thirds of which none can run.
3. Edit `latest.json` in this repo: `version` and `build` to match pubspec
   (`build` is the number after the `+`), and a line of `notes`. Commit it.
   The app reads this file to know an update exists and shows an *Update*
   card on Home; a `build` that does not go up means nobody is told.
4. Attach **both** the renamed `app-release.apk` and
   `latest.json` to a new GitHub Release: **Releases → Draft a new release →
   tag `v1.0.2` → drag the two files in → Publish.** Or, with the `gh` CLI:

```bash
cp ~/Aangan_app/build/app/outputs/flutter-apk/app-arm64-v8a-release.apk \
   ~/aangan-release/app-release.apk
gh release create v1.0.2 \
  ~/aangan-release/app-release.apk \
  ~/aangan-release/latest.json \
  --repo Martial-Gautam/aangan-release \
  --title "Apney v1.0.2"
```

Keep the asset filenames as `app-release.apk` and `latest.json` — the
website's download link and the app's update check depend on them.

The release key (`apney-release.jks`) and its password are the one thing that
cannot be recreated: lose them and no future version can install over the
app. Keep a copy off the laptop.
