# CPC Quality Admin Releases

Public update channel for **CPC Quality Admin**.

This repository is reserved for application update metadata and GitHub Releases. It is not a storage location for user-local secrets or runtime state.

## Update channel

- Manifest: `update.json`
- Release tags: `vMAJOR.MINOR.PATCH`
- Release asset: `CPC-Quality-Admin.zip`
- Client flow: app checks `update.json` -> notifies user -> downloads release asset -> launches external updater -> app exits -> updater replaces binaries -> app restarts.

## Protected local state

The update package must never contain or overwrite:

- `.secrets/`
- `Cache/`
- `Logs/`
- `Backups/`

See `RELEASE_CONTRACT.md` for the release/update contract.
