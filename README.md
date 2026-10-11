# Aashraya (ResQMesh)

Aashraya is an Android app. The project requires Android 8.0 (API 26) or later.

## Download and install

For the latest build, open the repository's **Actions** tab, select the latest successful **Build Android APK** run on `main`, and download the `Aashraya-debug-apk` artifact. Extract the APK, open it on your Android device, and follow the install prompt. Android may ask you to allow your browser or file manager to install apps from that source.

Debug APKs are signed with a temporary GitHub Actions debug key. To install a newer debug build over an older one, both must have the same signing key; otherwise uninstall the old app before installing the new APK. For stable app updates without uninstalling, use the signed APK attached to a GitHub Release.

GitHub Releases are created from `v*` tags. The repository owner must configure the Android signing secrets described below before publishing the first release.

This APK is for Android only; it cannot be installed on iPhone, iPad, or desktop operating systems.

## Publishing a release

The GitHub Actions workflow builds and attaches a signed APK whenever a tag beginning with `v` is pushed. Before the first release, configure these repository secrets under **Settings > Secrets and variables > Actions**:

- `ANDROID_KEYSTORE_BASE64`: Base64-encoded Android signing keystore.
- `ANDROID_KEY_ALIAS`: Alias of the signing key.
- `ANDROID_STORE_PASSWORD`: Keystore password.
- `ANDROID_KEY_PASSWORD`: Signing key password.

Create and securely back up a keystore outside the repository. For example, in PowerShell:

```powershell
keytool -genkeypair -v -keystore "$env:USERPROFILE\resqmesh-upload.jks" -keyalg RSA -keysize 2048 -validity 10000 -alias resqmesh
[Convert]::ToBase64String([IO.File]::ReadAllBytes("$env:USERPROFILE\resqmesh-upload.jks")) | Set-Clipboard
```

Paste the copied value into the `ANDROID_KEYSTORE_BASE64` secret, and add the alias and passwords as the other secrets. Never commit or share the keystore or its passwords. Keep the same keystore for every release so existing users can install updates.

After pushing the project to GitHub and configuring the secrets, create a version tag to publish:

```text
git tag v1.0.0
git push origin v1.0.0
```

The build appears under the repository's **Actions** tab. When it succeeds, the APK is attached to the GitHub Release for that tag. For later releases, increment `versionCode` and `versionName` in `app/build.gradle.kts` before creating the next tag.

## Build locally

Install JDK 17 and Android SDK Platform 34, then run:

```text
gradlew.bat assembleDebug
```

The debug APK is written to `app/build/outputs/apk/debug/app-debug.apk`. It is intended for testing; use the signed APK attached to a GitHub Release for distribution.
