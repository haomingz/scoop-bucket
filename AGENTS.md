# Agent Instructions

This document provides context for AI coding assistants when working on this repository.

## Project Overview

This is a **Scoop bucket** – a repository of app manifests for [Scoop](https://scoop.sh), the Windows command-line installer. It hosts apps not found in the official Scoop buckets.

## Repository Structure

- `bucket/` – JSON manifest files for each app (e.g. `bucket/gws.json`)
- `README.md` – Usage instructions and Apps table (must stay in sync with `bucket/`)
- `.markdownlint.yaml` – Markdown lint rules

## Adding a New App

1. **Create a manifest** at `bucket/<app-name>.json`. The filename becomes the install name (e.g. `gws.json` → `scoop install haomingz/gws`).

2. **Manifest fields** (required):
   - `version` – Semantic version string
   - `description` – Short summary
   - `homepage` – Project URL
   - `license` – e.g. MIT, Apache-2.0
   - `architecture` – `64bit`, `arm64`, and/or `32bit` with `url` and `hash`
   - `bin` – Executable name(s) exposed to PATH
   - `checkver` – Usually `"github"` for auto version detection
   - `autoupdate` – URL template using `$version`

3. **Optional fields**: `extract_dir` (for archives), `pre_install`, `post_install`, etc.

4. **Get the hash**:

   ```powershell
   scoop hash path/to/downloaded/file
   ```

5. **Test**:

   ```powershell
   scoop install path/to/bucket/app-name.json
   ```

6. **Update README** – Add a row to the Apps table in alphabetical order by app name:

   ```markdown
   | [app-name](./bucket/app-name.json) | Description | [homepage](url) |
   ```

7. **Commit message**: `feat(manifest): add app-name vX.Y.Z`

## Updating Existing Apps

When updating a manifest (new version or autoupdate fix), **always verify**:

### 1. File extension / URL format

- Check the latest release assets: the download URL suffix may have changed (e.g. `.tar.gz` → `.zip`, `.7z`, `.msi`).
- Update both the main `url` and the `autoupdate` URL template to match the current format.
- If the format changed, Excavator/checkver may fail until the manifest is corrected.

### 2. Archive directory structure

- Different archive formats or release processes can change the internal layout.
- **Verify the structure** before setting or keeping `extract_dir`:

  ```powershell
  # Download and inspect zip contents
  $zip = "$env:TEMP\app-check.zip"
  Invoke-WebRequest -Uri "<release-url>" -OutFile $zip -UseBasicParsing
  Add-Type -AssemblyName System.IO.Compression.FileSystem
  [System.IO.Compression.ZipFile]::OpenRead($zip).Entries | ForEach-Object { $_.FullName }
  Remove-Item $zip -Force
  ```

- If files are at the **root** (e.g. `gws.exe`), **omit** `extract_dir`.
- If files are in a **subfolder** (e.g. `gws-x86_64-pc-windows-msvc/gws.exe`), set `extract_dir` to that folder name.
- Never assume structure is unchanged between versions; confirm before committing.

## Conventions

- Use lowercase, hyphen-separated names for manifests (e.g. `kubernetes-mcp-server.json`)
- For GitHub releases, use `checkver: "github"` and `v$version` in autoupdate URLs
- Prefer `.exe` or `.zip` URLs; support `64bit` and `arm64` when available
- If Excavator fails to detect new versions or install/update fails, check: **file extension changed?** and **extract_dir still correct?**
- Keep descriptions concise; full details go in `homepage`

## References

- [Scoop App Manifests](https://github.com/ScoopInstaller/Scoop/wiki/App-Manifests)
- [Creating an App Manifest](https://github.com/ScoopInstaller/Scoop/wiki/Creating-an-app-manifest)
