# Updates & releases

## Where downloads live

Every version of every Balane desktop app is published here as a GitHub
Release — one release per app and version, tagged `<app>-v<version>`
(e.g. `windownote-v1.0`). The installers are attached to the release as files:
a `.dmg` for macOS, a `-setup.exe` for Windows.

**You never need to dig through this repository.** These links always hand you
the newest build:

| Link | What you get |
| --- | --- |
| `balane.app/download/<app>` | newest macOS version |
| `balane.app/download/<app>-win` | newest Windows version |

For example: [balane.app/download/windownote](https://www.balane.app/download/windownote).

## How updates work

**macOS:** the apps update themselves. They check this repository for a newer
version (via Sparkle) and offer the update inside the app — download, one
click, done. No need to come back here.

**Windows:** download the newest installer via the links above and run it; it
replaces the installed version.

## Release notes

Each release carries its notes — what was fixed and what is new — directly on
the [releases page](https://github.com/JHBalane/Balane-Releases/releases).
Fixed bugs are also visible on the [status page](https://www.balane.app/en/status).

## Older versions

All previous versions stay available. Open the
[releases list](https://github.com/JHBalane/Balane-Releases/releases) and
filter by app name, e.g. `windownote`.

## Signing

macOS builds are Developer-ID-signed and notarized by Apple; updates are
additionally verified by the app itself (EdDSA signature) before installing.
