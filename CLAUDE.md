# wonderpush-expo-plugin

Expo config plugin / module for WonderPush, used together with `react-native-wonderpush`. Source of truth for the release steps: Trello card https://trello.com/c/kUGcEvnV/139-react-native-wonderpush (the "wonderpush-expo-plugin" section; Wiki board, Deployment list). Keep this file and the card in sync when the procedure changes.

Always confirm with the user before any outward-facing step (push, tag push, `npm publish`, GitHub release, ReadMe changelog).

## Native SDK upgrades

This plugin does **not** pin the native Android/iOS WonderPush SDK versions: those come from `react-native-wonderpush`. When the core mobile SDKs are released, no change is needed here (checked 2026-11-20 for Android 4.7.2 / iOS 4.5.2). Only release this plugin when its own code changes.

## Developing

```bash
# Under wonderpush-expo-plugin/: edit the plugin, then
npm run prepare        # also done by `npm install` in the Expo app
# Under your-expo-app/:
npx expo prebuild
npm run android
npm run ios
```

## Publishing a new version

1. Edit the version in `android/build.gradle` (`version = '...'` at line 4, and `versionName` in `android.defaultConfig`, ~line 38), then `git add android/build.gradle`.
2. `npm run clean`, check `git status`.
3. `npm version --force patch` (or minor/major): updates `package.json`, commits and tags `vX.Y.Z`.
4. `git push origin master --follow-tags`.
5. `npm publish`: needs 2FA (browser authentication or OTP), so the user must run it in their own terminal after `npm login`. Verify with `curl -s -o /dev/null -w '%{http_code}' https://registry.npmjs.org/wonderpush-expo-plugin/$VERSION` (404 for a few minutes after publishing).
6. GitHub release for tag `vX.Y.Z` (`gh release create vX.Y.Z --verify-tag`) with a changelog.
7. ReadMe changelog: see "Announce on ReadMe" in `../wonderpush-sdks/CLAUDE.md` (REST API with `~/.readme-api-key`), title `[SDK][Expo] Release vX.Y.Z`.

Current version: 1.0.0 (latest git tag `v1.0.0`).
