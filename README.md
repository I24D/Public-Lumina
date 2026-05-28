# Lumina OpenClaw Desktop

Official public distribution repository for Lumina OpenClaw Desktop.

This repository is intentionally small. It is the public download and release hub for installers, runtime bundles, update manifests, and user-facing documentation. Product source code, private credentials, and internal build systems are not stored here.

## Current Stable Release

**Latest version:** `1.0.10`
**Release tag:** `lumina-desktop-v1.0.10`
**Release page:** https://github.com/I24D/Public-Lumina/releases/latest

For normal installation, download:

`Lumina.OpenClaw_1.0.10_x64-setup.exe`

The other release files are runtime/update assets used by Lumina Desktop and the release pipeline. Most users do not need to download them manually.

## What Lumina OpenClaw Is

Lumina OpenClaw Desktop is the public desktop build of the Lumina assistant environment. It packages the OpenClaw runtime with Lumina-specific defaults and integrations so a user can install the app and open the chat without manually assembling the runtime.

The current public build focuses on:

- A Windows x64 installer for Lumina OpenClaw Desktop.
- Bundled OpenClaw runtime assets.
- Lumina Code integration for development-oriented agent workflows.
- Runtime manifests for verification and future update automation.
- A clean public release surface for users, testers, and investors.

## Install

1. Open the latest release:
   https://github.com/I24D/Public-Lumina/releases/latest
2. Download `Lumina.OpenClaw_1.0.10_x64-setup.exe`.
3. Run the installer.
4. Open Lumina OpenClaw from the Start Menu or desktop shortcut.
5. Start a chat session.

## Release Assets

| Asset | Purpose | Typical user |
| --- | --- | --- |
| `Lumina.OpenClaw_1.0.10_x64-setup.exe` | Windows installer | End users |
| `lumina-release-1.0.10-win32-x64.manifest.json` | Release metadata and checksums | Updater/build validation |
| `lumina-openclaw-runtime-bundle-1.0.10-win32-x64.tar.gz` | Runtime payload | Desktop updater/runtime deployment |
| `lumina-openclaw-runtime-bundle-1.0.10-win32-x64.manifest.json` | Runtime bundle manifest | Verification |
| `lumina-bootstrapper-1.0.10-win32-x64.tar.gz` | Bootstrapper runtime artifact | Internal/update tooling |

See [Release Assets](docs/RELEASE_ASSETS.md) for more detail.

## Repository Role

This repository is public on purpose. It should contain only:

- Current public release assets.
- User-facing documentation.
- Support and security instructions.
- Release notes and changelogs.

It should not contain:

- Private source code.
- API keys, tokens, `.env` files, or credentials.
- Old unsupported installers.
- Local build output that is not part of an official release.

## Verification

GitHub provides SHA256 digests for release assets. On Windows, you can verify a downloaded installer with:

```powershell
Get-FileHash .\Lumina.OpenClaw_1.0.10_x64-setup.exe -Algorithm SHA256
```

Compare the result with the digest shown on the release asset.

## Project Status

`v1.0.10` is the only supported public release currently listed in this repository. Older public release tags were removed to avoid confusion while the project is being prepared for a cleaner public/investor-facing presentation.

## Support

For installation or runtime issues, see [Support](SUPPORT.md).

For security-sensitive reports, see [Security](SECURITY.md).
