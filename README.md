# Truck Dispatch Calculator - Windows

This Windows build deliberately uses Electron instead of Tauri.
The calculator itself is unchanged and remains in `app/index.html`.

## Why this version is simpler

There is no Rust toolchain, Cargo project, Tauri build.rs, capability schema,
Windows resource compilation step, or WebView2 bundling configuration to fail.
Electron packages the local HTML directly.

## Build automatically on GitHub

1. Replace the contents of your existing GitHub repository with the contents of this folder.
2. Commit and push to the `main` branch.
3. Open **Actions**.
4. Select **Build Windows EXE**.
5. Click **Run workflow** (or let the push trigger it automatically).
6. When the run succeeds, download the artifact named:
   `Truck-Dispatch-Calculator-Windows`
7. Inside the artifact is the installer:
   `Truck-Dispatch-Calculator-Setup-1.0.0.exe`

No Rust or Visual Studio configuration is needed in your repository.

## Build locally on Windows (optional)

```powershell
npm install
npm start
```

Create installer:

```powershell
npm run dist:win
```

The installer is written to `dist/`.

## Calculator data

The app loads the calculator from a local file and works without an internet
connection. Browser localStorage is retained by Electron, so saved truck
settings remain available between launches.
