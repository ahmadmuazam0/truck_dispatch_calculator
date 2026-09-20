# CLEAN REPOSITORY INSTRUCTIONS

Your previous GitHub errors prove the repository is still running the OLD Tauri workflow.

Delete the old repository contents and upload ONLY the contents of this ZIP.

The repository root should contain only items like:

- app/
- main.js
- package.json
- .github/workflows/build-electron-windows.yml
- README.md

It must NOT contain:

- src-tauri/
- Cargo.toml
- Cargo.lock
- any old Tauri workflow YAML file

In GitHub Actions, the workflow name must be:

Build Electron Windows EXE

If you still see a workflow running `npm run tauri`, then the old workflow still exists in the repository.

Expected artifact after success:

Truck-Dispatch-Calculator-Setup-1.0.0.exe
