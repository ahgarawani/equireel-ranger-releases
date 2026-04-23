# Equireel-Ranger-Releases

Public distribution repository for Ranger release assets and update metadata.

> [!CAUTION]
> This repository is public for update delivery only. Ranger source code remains private.

## Purpose
- Host installer release assets (`Ranger-Setup.exe`)
- Publish updater metadata (`manifest.json` at repository root)
- Publish installer checksums (`checksums/Ranger-Setup.exe.sha256`)

## Contract by Example
- Release tag: `v2.1.1`
- Release title: `Ranger 2.1.1`
- Release asset: `Ranger-Setup.exe`
- Manifest URL: `https://raw.githubusercontent.com/ahgarawani/equireel-ranger-releases/main/manifest.json`
- Installer URL in manifest: `https://github.com/ahgarawani/equireel-ranger-releases/releases/download/v2.1.1/Ranger-Setup.exe`

## Repository Scope
- Allowed: release metadata, installer checksum files, release-process docs
- Not allowed: application source code or private credentials

## License
Proprietary. All rights reserved.
