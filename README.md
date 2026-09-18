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

The APK is ~104 MB, which is over GitHub's 100 MB limit for files committed to
git. It must be attached as a **release asset**, never committed:

1. In `~/Aangan_app/pubspec.yaml`, raise `version:` — both parts, e.g.
   `1.0.1+2` → `1.0.2+3`. Android only installs an update whose build
   number (after the `+`) is higher than the one on the phone.
2. `flutter build apk --release` — with `android/key.properties` in place, so
   the APK carries the Apney release key. An APK built without it carries the
   debug key and will not install over what people have.
3. Attach `build/app/outputs/flutter-apk/app-release.apk` to a new GitHub
   Release: **Releases → Draft a new release → tag `v1.0.2` → drag the file in
   → Publish.** Or, with the `gh` CLI:

```bash
gh release create v1.0.2 \
  ~/Aangan_app/build/app/outputs/flutter-apk/app-release.apk \
  --repo Martial-Gautam/aangan-release \
  --title "Apney v1.0.2"
```

Keep the asset filename as `app-release.apk` — the website's download link
depends on it.

The release key (`apney-release.jks`) and its password are the one thing that
cannot be recreated: lose them and no future version can install over the
app. Keep a copy off the laptop.
