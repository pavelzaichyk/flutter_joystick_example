# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Public repository

This repo is (or will be) **public**. Anything committed, including git history, is world-readable:
- Never commit keystores, `key.properties`, Play/App Store service account JSON, `.p12`, provisioning profiles, `.env` files or tokens. These live only in GitHub secrets (environment `production`) and are written to disk at build time.
- Do not paste secrets into commit messages, workflow files, issues or logs. If one leaks, rotate it; deleting the commit is not enough.
- Workflows must stay safe for forks: no `pull_request_target`, no secrets on PR-triggered jobs, release jobs only on `v*` tags / manual dispatch. Interpolate untrusted values via `env:`, not `${{ }}` inside `run:`.
- Store listing text and screenshots under `fastlane/metadata/` are public too.

## Overview

Demo app for the [flutter_joystick](https://pub.dev/packages/flutter_joystick) package (version pinned by the `flutter_joystick` constraint in `pubspec.yaml`). The whole app lives in a single file, `lib/main.dart`: `MainPage` lists demo screens (`JoystickExample`, `JoystickCustomizationExample`, `JoystickAreaExample`, `SquareJoystickExample`) plus small shared widgets (`JoystickModeDropdown`, `Button`, `Ball`). There are no tests.

## Commands

```bash
flutter pub get
flutter run                          # run on a device/emulator
flutter analyze                      # lint (flutter_lints; build/, android/, ios/, web/ are excluded)
flutter build web --release --no-web-resources-cdn
dart run flutter_launcher_icons      # regenerate icons from assets/icon/icon.png
```

## Deployment

- **Web:** `.github/workflows/deploy-web.yml` runs on push to `main`: `flutter analyze`, `flutter build web`, then `wrangler deploy`. `wrangler.jsonc` serves `build/web` as Cloudflare Workers static assets (SPA fallback) on `joystick.pavelzaichyk.com`. The Flutter version is pinned in the workflow; actions are pinned by commit SHA.
- **Android release:** signing needs a git-ignored `android/key.properties` (`storeFile`, `storePassword`, `keyAlias`, `keyPassword`) with the keystore kept outside the repo. Never commit signing material.
- **Versioning:** bump `version:` in `pubspec.yaml`.
- **Play release (fastlane):** `bundle exec fastlane android release` builds the AAB and publishes to production; `android metadata` pushes only the listing from `fastlane/metadata/android`. CI: `.github/workflows/release-android.yml` (on `v*` tags or manual). Needs secrets `ANDROID_KEYSTORE_BASE64`, `ANDROID_STORE_PASSWORD`, `ANDROID_KEY_ALIAS`, `ANDROID_KEY_PASSWORD`, `PLAY_JSON_KEY`.
