# Flutter Joystick Example

Demo app for the [flutter_joystick](https://pub.dev/packages/flutter_joystick) package.
Live demo and store builds are produced from this repository.

## Run

```bash
flutter pub get
flutter run
```

## Release

Signing material is never stored in this repo.

- **Android:** create `android/key.properties` (git-ignored) with `storeFile`, `storePassword`, `keyAlias`, `keyPassword`; keep the keystore outside the repo.
- **CI:** provide the keystore and Play service account through CI secrets, and write them to disk at build time.
- **Version:** bump `version:` in `pubspec.yaml`; the plugin version is the `flutter_joystick` constraint.

## Public repository

Nothing secret belongs in this repo or its history. Keystores, service account keys and tokens are GitHub secrets in the `production` environment (protect it with required reviewers and restrict it to protected branches/tags). Store releases run via fastlane (`bundle exec fastlane android release`) from `.github/workflows/release-android.yml` on `v*` tags; the preflight rejects a build number that is not higher than every version code already on Play.
