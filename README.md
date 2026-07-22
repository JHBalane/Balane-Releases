# Balane Releases

Öffentliche Installer der Balane-Apps. Dieses Repository enthält keinen
Quellcode — die Dateien liegen ausschließlich als Release-Assets.

**Downloads: [Releases](https://github.com/JHBalane/Balane-Releases/releases)**

Die Produktseiten mit Beschreibung, Screenshots und Preisen stehen auf
[balane.app](https://www.balane.app).

## Aufbau

Ein Release pro App und Version, Tag `<app>-v<version>`:

| Tag | App | Assets |
| --- | --- | --- |
| `flowvisual-v1.0.1` | FlowVisual | `FlowVisual-1.0.1.dmg` (macOS), `FlowVisual-Setup-1.0.1.exe` (Windows) |
| `flowvisual-v1.0.0` | FlowVisual | `FlowVisual-1.0.0.dmg` (macOS), `FlowVisual-Setup-1.0.0.exe` (Windows) |
| `windownote-v0.9.0` | WindowNote | `WindowNote-0.9.0.dmg` (macOS), `WindowNote-windows.zip` (Windows) |

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

## Prüfsummen

Zu jedem Release stehen die SHA-256-Summen in den Release-Notes. Nach dem
Download vergleichen:

```
shasum -a 256 FlowVisual-1.0.1.dmg                      # macOS
certutil -hashfile FlowVisual-Setup-1.0.1.exe SHA256   # Windows
```
