# Aangan — Releases

Public download host for the **Aangan** Android app. Source code lives in a
separate private repository; this repo only carries release binaries.

## Download

**[Download the latest APK](https://github.com/Martial-Gautam/aangan-release/releases/latest/download/app-release.apk)**

That link always resolves to the newest published release, so it never needs
updating. The Aangan website links to it through its own `/download` route.

## Installing

Android blocks installs from outside the Play Store by default. After the
download finishes, open the file and allow installs from your browser when
prompted.

## Publishing a new version

The APK is ~104 MB, which is over GitHub's 100 MB limit for files committed to
git. It must be attached as a **release asset**, never committed:

```bash
cd ~/Aangan_app
flutter build apk --release
gh release create v1.0.1 \
  build/app/outputs/flutter-apk/app-release.apk \
  --repo Martial-Gautam/aangan-release \
  --title "Aangan v1.0.1"
```

Keep the asset filename as `app-release.apk` — the website's download link
depends on it.
