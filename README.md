# Balane Releases

Öffentliche Installer der Balane-Apps. Dieses Repository enthält keinen
Quellcode — die Dateien liegen ausschließlich als Release-Assets.

**Downloads: [Releases](https://github.com/JHBalane/Balane-Releases/releases)**

Die Produktseiten mit Beschreibung, Screenshots und Preisen stehen auf
[balane.app](https://www.balane.app).

## Aufbau

Ein Release pro App und Version, Tag mit App-Präfix:

| Tag | App | Assets |
| --- | --- | --- |
| `flowvisual-v1.0.0` | FlowVisual | `FlowVisual-1.0.0.dmg` (macOS), `FlowVisual-Setup-1.0.0.exe` (Windows) |

## Prüfsummen

Zu jedem Release stehen die SHA-256-Summen in den Release-Notes. Nach dem
Download vergleichen:

```
shasum -a 256 FlowVisual-1.0.0.dmg          # macOS
certutil -hashfile FlowVisual-Setup-1.0.0.exe SHA256   # Windows
```
