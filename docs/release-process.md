---
Status: canonical
Source of truth: implementation
Last verified: 2026-04-18
---

# Manual Release Workflow Guide 🚀

This document defines the standard procedure for publishing installer-based releases of Ranger from the private source repository to the public `equireel-ranger-releases` repository.

## Prerequisites
- Authenticated `git` and `gh` (GitHub CLI) access.
- **Inno Setup** installed for building the installer.
- A clean working directory in the `main` branch.

---

## Step 1: Version Bumping
Before starting the release, update the version number in the following locations:
1.  **`.specify/memory/constitution.md`**: Update the `Version` line and the `Release Roadmap`.
2.  **`README.md`**: Ensure any version-specific installation notes are updated.
3.  **`app/ranger.py`** (if version constant exists): Ensure the internal version matches.

Commit these changes:
```bash
git add .
git commit -m "chore: bump version to vX.Y.Z"
```

---

## Step 2: Tagging
Create a signed git tag for the release:
```bash
git tag -a vX.Y.Z -m "Ranger Release X.Y.Z"
git push origin vX.Y.Z
```

---

## Step 3: Building the Installer
1.  **Compile**: Run the build script to generate the bundled application folder:
    ```cmd
    build.bat
    ```
    This creates `dist/Ranger/`.
2.  **Pack**: Open `installer/ranger-setup.iss` in Inno Setup and compile it.
    - Output: `dist/Ranger-Setup.exe`

---

## Step 4: Generating Checksums
Generate a SHA256 hash for the installer binary to ensure integrity:
```powershell
# Windows PowerShell
CertUtil -hashfile dist/Ranger-Setup.exe SHA256 > dist/Ranger-Setup.exe.sha256
```
Clean up the checksum file to contain *only* the hex hash.

---

## Step 5: Updating Release Metadata (Public Repo)
1.  Navigate to the cloned public repository: `cd ../equireel-ranger-releases`.
2.  **Manifest**: Update `manifest.json` with the new release details:
    ```json
    {
      "version": "X.Y.Z",
      "installer_url": "https://github.com/ahgarawani/equireel-ranger-releases/releases/download/vX.Y.Z/Ranger-Setup.exe",
      "sha256": "NEW_HASH_HERE",
      "release_notes_url": "https://github.com/ahgarawani/equireel-ranger-releases/releases/tag/vX.Y.Z",
      "minimum_supported_version": "1.0.0"
    }
    ```
3.  **Checksum File**: Copy the new `.sha256` file to the `checksums/` directory.
4.  **Push Metadata**:
    ```bash
    git add manifest.json checksums/
    git commit -m "feat: release vX.Y.Z metadata"
    git push origin main
    ```

---

## Step 6: Creating the GitHub Release
Create the release on the public repository and upload the installer:
```bash
gh release create vX.Y.Z ../Equireels-Ranger/dist/Ranger-Setup.exe \
    --repo ahgarawani/equireel-ranger-releases \
    --title "Ranger X.Y.Z" \
    --notes "Release notes for version X.Y.Z..."
```

---

## Step 7: Post-Release Verification
1.  Verify the download link in `manifest.json` works.
2.  Verify the installer runs and installs to `%PROGRAMFILES%\Ranger\`.
3.  Check that no Python runtime is required on the target machine.
