# Truck Load Calculator — Cross-Platform Tauri 2 Project

This project uses **one HTML/CSS/JavaScript frontend** for:

- Windows
- macOS
- Linux
- Android
- iOS

The approved V7 calculator is in `web/index.html`.

## 1. Common prerequisites

Install:
- Node.js LTS
- Rust (`rustup`)
- Platform-specific build tools

Then from this folder:

```bash
npm install
```

---

## 2. Windows desktop build

Install:
- Microsoft Visual Studio Build Tools
- "Desktop development with C++"
- WebView2 runtime (normally already present on Windows 10/11)
- Rust MSVC toolchain

Then:

```bash
npm install
npm run desktop:dev
```

Build installer/app:

```bash
npm run desktop:build
```

Tauri places release bundles under:

```text
src-tauri/target/release/bundle/
```

---

## 3. Android APK

Install:
- Android Studio
- Android SDK
- Android SDK Platform Tools
- Android NDK
- Java/JDK as required by your Android Studio/Tauri setup
- Rust Android targets

Add Rust targets:

```bash
rustup target add aarch64-linux-android armv7-linux-androideabi i686-linux-android x86_64-linux-android
```

Initialize Android once:

```bash
npm run android:init
```

Test on a connected device/emulator:

```bash
npm run android:dev
```

Build an APK:

```bash
npm run android:apk
```

For Google Play, build an AAB:

```bash
npm run android:aab
```

For production distribution, configure Android signing/keystore before publishing.

---

## 4. iPhone / iPad

**A Mac is required.**

Install:
- Xcode
- Rust
- CocoaPods
- Node.js

Add iOS Rust targets:

```bash
rustup target add aarch64-apple-ios x86_64-apple-ios aarch64-apple-ios-sim
```

Initialize iOS once:

```bash
npm run ios:init
```

Test:

```bash
npm run ios:dev
```

Build:

```bash
npm run ios:build
```

For App Store/TestFlight distribution you also need:
- Apple Developer account
- App identifier / provisioning
- Signing certificate

You can also open the generated iOS project in Xcode and archive/sign from there.

---

## 5. macOS desktop

On a Mac install Xcode Command Line Tools and Rust:

```bash
xcode-select --install
npm install
npm run desktop:build
```

Signing/notarization is recommended for public distribution.

---

## 6. Linux desktop

Install the Linux dependencies listed in the Tauri prerequisites, Rust, and Node.js.

Then:

```bash
npm install
npm run desktop:build
```

---

## Project structure

```text
truck_load_calculator_cross_platform/
├── web/
│   └── index.html              # Approved calculator UI + logic
├── src-tauri/
│   ├── capabilities/
│   │   └── default.json
│   ├── src/
│   │   ├── lib.rs
│   │   └── main.rs
│   ├── build.rs
│   ├── Cargo.toml
│   └── tauri.conf.json
├── package.json
├── .gitignore
└── README.md
```

## Important

You only edit `web/index.html` when changing calculator UI/logic. Those changes then flow into Android, iOS and desktop builds from the same codebase.


---

## Easiest Windows EXE build — no local Rust / Visual Studio setup

This project now includes:

```text
.github/workflows/build-windows.yml
```

### Build in GitHub

1. Create a new GitHub repository.
2. Upload/push this whole project to the repository.
3. Open the repository on GitHub.
4. Click **Actions**.
5. Select **Build Windows App**.
6. Click **Run workflow**.
7. After the workflow finishes, open that workflow run.
8. Download the generated artifacts from the **Artifacts** section.

The workflow runs on GitHub's `windows-latest` runner and builds:

- NSIS Windows installer (`.exe`)
- WiX/MSI installer (`.msi`)

The first build may also run automatically after the files are pushed to the `main` branch.

### Note about Windows signing

The generated installer will be unsigned unless you later add a Windows code-signing certificate. It can still be used for testing/private installation, but Windows SmartScreen may warn users about an unknown publisher.


### Workflow fix

The Windows workflow intentionally does not enable the `setup-node` npm cache, so a pre-existing `package-lock.json` is not required. The workflow runs `npm install` and builds normally.


---

## Windows CI v2 — NSIS-only build

This package intentionally builds **only the NSIS `.exe` installer first**.

Why:
- NSIS produces the normal Windows `-setup.exe`.
- MSI uses WiX and can fail independently because of Windows VBSCRIPT/WiX validation on hosted runners.
- Once the EXE build is stable, MSI can be added separately.

The GitHub workflow now:
1. installs Node dependencies,
2. prints `tauri info`,
3. runs Tauri directly with `--verbose`,
4. uploads the generated `.exe`,
5. prints the full release file tree if anything fails.

After a successful run:
**Actions → workflow run → Artifacts → Truck-Load-Calculator-Windows-Installer**
