# CPC Quality Admin — Release Contract

## Purpose

This repository is the public update channel for CPC Quality Admin. Source code does not need to live here.

## Versioning

Use semantic release tags:

`vMAJOR.MINOR.PATCH`

Example: `v0.1.0`.

The value in `update.json.version` must match the release version without the leading `v`.

## Release asset

Canonical asset name:

`CPC-Quality-Admin.zip`

The ZIP contains only files required to run the released application. The external updater is responsible for replacing application binaries after the main process exits.

## Local-state exclusion — mandatory

Never package, publish, overwrite, delete, or migrate these user-local paths as part of a normal application update:

- `.secrets/`
- `Cache/`
- `Logs/`
- `Backups/`

If an update package contains `.secrets/`, the client/updater should reject the package rather than extract it.

## Manifest contract

`update.json` fields:

- `schemaVersion`: manifest schema version.
- `appId`: must equal `cpc-quality-admin`.
- `version`: available application version.
- `releaseTag`: GitHub Release tag, e.g. `v0.1.0`.
- `assetName`: expected Release asset name.
- `sha256`: SHA-256 of the exact ZIP asset, lowercase or uppercase hex accepted by the client after normalization.
- `mandatory`: whether the UI may offer "Để sau".
- `notes`: short user-facing release summary.

The client must not install an update when the manifest is malformed, the app ID is wrong, the requested release/asset cannot be resolved, or SHA-256 verification fails.

## Intended client journey

1. CPC Quality Admin starts normally.
2. It checks the manifest without blocking normal use if the update service is unavailable.
3. If `remote version > local version`, it shows an update notification.
4. User chooses `Cập nhật ngay`.
5. Package is downloaded and SHA-256 verified.
6. Main app launches `CPC Quality Updater.exe` with enough information to locate the staged package and current process.
7. Main app exits.
8. Updater waits for the main process to exit, preserves protected local state, replaces only release-managed files, then restarts CPC Quality Admin.

## First publication

`update.json` currently uses version `0.0.0`; this means the channel is initialized but no production update has been published. Do not point production clients at a fake downloadable release.
