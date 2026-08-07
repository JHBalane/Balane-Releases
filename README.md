<div align="center">

# Balane Releases

**Official downloads for Balane desktop apps.** No source code here — every
release carries the signed installers as files. Built in Munich by
[Balane](https://www.balane.app).

[![Total downloads](https://img.shields.io/github/downloads/JHBalane/Balane-Releases/total?style=for-the-badge&label=DOWNLOADS&labelColor=000000&color=00ffff)](https://github.com/JHBalane/Balane-Releases/releases)
[![Status](https://img.shields.io/badge/STATUS-BUGS_%26_FIXES-00ffff?style=for-the-badge&labelColor=000000)](https://www.balane.app/en/status)
[![Support](https://img.shields.io/badge/SUPPORT-GET_HELP-00ffff?style=for-the-badge&labelColor=000000)](https://www.balane.app/en/support)
[![Updates](https://img.shields.io/badge/DOCS-UPDATES-00ffff?style=for-the-badge&labelColor=000000)](https://github.com/JHBalane/Balane-Releases/wiki/Updates-and-Releases)

</div>

## Get the apps

These links always hand you the newest version:

| App | macOS | Windows | All versions |
| --- | --- | --- | --- |
| **[BalaneDisk](https://www.balane.app/en/apps/balanedisk)** | [Download](https://www.balane.app/download/balanedisk) | [Download](https://www.balane.app/download/balanedisk-win) | [Releases](https://github.com/JHBalane/Balane-Releases/releases?q=balanedisk) |
| **[WindowNote](https://www.balane.app/en/apps/windownote)** | [Download](https://www.balane.app/download/windownote) | [Download](https://www.balane.app/download/windownote-win) | [Releases](https://github.com/JHBalane/Balane-Releases/releases?q=windownote) |
| **[FlowVisual](https://www.balane.app/en/apps/flowvisual)** | [Download](https://www.balane.app/download/flowvisual) | [Download](https://www.balane.app/download/flowvisual-win) | [Releases](https://github.com/JHBalane/Balane-Releases/releases?q=flowvisual) |
| **[Chessplosion](https://www.balane.app/en/apps/chessplosion)** | [Download](https://www.balane.app/download/chessplosion) | — | [Releases](https://github.com/JHBalane/Balane-Releases/releases?q=chessplosion) |

macOS apps update themselves after that (see
[Updates & Releases](https://github.com/JHBalane/Balane-Releases/wiki/Updates-and-Releases)).

## Something broken?

Every reported bug is tracked in the open — in this repository's
[issues](https://github.com/JHBalane/Balane-Releases/issues) and on the public
[status page](https://www.balane.app/en/status), from *reported* through
*in progress* to *fixed*. How to report one: [Getting Help](https://github.com/JHBalane/Balane-Releases/wiki/Getting-Help).

---

## Maintainer-Notizen

Ein Release pro App und Version, Tag `<app>-v<version>`
(z. B. `windownote-v0.9.1`), die Installer als Release-Assets:
`.dmg` für macOS, `-setup.exe` für Windows.

balane.app liest dieses Repository über die GitHub-API und leitet die stabilen
Links auf das jeweils jüngste Release mit dem passenden Präfix um. Ein Release
ist damit ohne Deploy der Website sofort live:

| Link | Ziel |
| --- | --- |
| `balane.app/download/<app>` | neueste macOS-Datei |
| `balane.app/download/<app>-win` | neueste Windows-Datei |
| `balane.app/appcast/<app>` | Sparkle-Feed |

Manche Releases führen zusätzlich `<App>-<version>.zip` und `release.json`.
Beide gehören dem Sparkle-Updater — die ZIP ist das, was er installiert, und
`release.json` trägt Build-Nummer und Signatur, die ein GitHub-Release selbst
nicht kennt. Für einen manuellen Download sind sie nicht gedacht: dafür ist die
`.dmg` da.

Bug-Tracking: ein Issue pro Bug, Label `app:<slug>` (Pflicht, sonst erscheint
es nicht auf der Status-Seite) plus optional `in-progress`. Schließen als
„completed" = gefixt und bleibt öffentlich gelistet; „not planned" =
verschwindet von der Status-Seite.

## Prüfsummen

Zu jedem Release stehen die SHA-256-Summen in den Release-Notes. Nach dem
Download vergleichen:

```
shasum -a 256 FlowVisual-1.0.1.dmg                      # macOS
certutil -hashfile FlowVisual-Setup-1.0.1.exe SHA256   # Windows
```
