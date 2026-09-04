# Maintainer-Notizen

Interne Konventionen für dieses Release-Repository. Nutzer brauchen das nicht,
für die reicht die [README](README.md).

## Grundregel

Ein Release pro App und Version, Tag `<app>-v<version>` (z. B.
`windownote-v0.9.1`), die Installer als Release-Assets: `.dmg` für macOS,
`-setup.exe` für Windows.

balane.app liest dieses Repository über die GitHub-API und leitet die stabilen
Links auf das jeweils jüngste Release mit dem passenden Präfix um. Ein Release
ist damit ohne Deploy der Website sofort live:

| Link | Ziel |
| --- | --- |
| `balane.app/download/<app>` | neueste macOS-Datei |
| `balane.app/download/<app>-win` | neueste Windows-Datei |
| `balane.app/appcast/<app>` | Sparkle-Feed |

Manche Releases führen zusätzlich `<App>-<version>.zip` und `release.json`.
Beide gehören dem Sparkle-Updater: die ZIP ist das, was er installiert, und
`release.json` trägt Build-Nummer und Signatur, die ein GitHub-Release selbst
nicht kennt. Für einen manuellen Download sind sie nicht gedacht, dafür ist die
`.dmg` da.

> **Achtung bei Download-Zahlen.** `release.json` wird von jedem laufenden
> Sparkle-Client bei jedem Update-Check abgerufen und zählt in GitHubs
> Download-Statistik mit. Der Anteil ist groß und wächst mit dem Alter eines
> Releases. Wer echte Installer-Zahlen will, zählt nur `.dmg`, `.exe`, `.msix`
> und die Windows-ZIPs.

## CellAlert, abweichend

CellAlert ist keine reine Desktop-App, sondern die Desktop-Fläche eines
Web-Abos (Fristen aus Excel). Zwei Unterschiede:

| Was | Wert |
| --- | --- |
| Tag | `cellalert-v<version>` |
| macOS | `CellAlert-<version>.dmg`, Developer ID, notarisiert |
| Windows | `CellAlert-<version>-windows-portable.zip` (noch kein `-setup.exe`) |
| Quelle | privat, `JHBalane/cellalert-app` |

Der Windows-Build kommt aus GitHub Actions (`.github/workflows/windows-build.yml`
im App-Repo) und wird bei einem Tag automatisch hierher hochgeladen. Solange es
keinen Installer gibt, ist das Windows-Asset ein portables ZIP. Der Link
`balane.app/download/cellalert-win` muss also auch darauf zeigen können.

Die Store-Varianten (Mac App Store, Microsoft Store, App Store, Play Store)
laufen getrennt davon. Nur die hier verlinkten Direkt-Downloads dürfen die
Abo- und Checkout-Fläche zeigen, die Store-Builds nicht (Apple 3.1.1). Gesteuert
wird das im App-Repo über `--dart-define=DIRECT_SALE=true`.

## ship global, abweichend

`ship global` ist eine Tauri-App und benutzt nicht Sparkle, sondern Tauris eigenen
Updater. Der Unterschied ist nicht kosmetisch: Sparkle liest einen Feed, Tauri fragt
eine Adresse mit seiner laufenden Version und bekommt entweder 204 oder ein JSON.

| Was | Wert |
| --- | --- |
| Tag | `shipglobal-v<version>` |
| Assets | `ShipGlobal-<version>-<arch>.dmg`, `ShipGlobal-<version>-<arch>.app.tar.gz` + `.sig` |
| Windows | `ShipGlobal-Setup-<version>.exe`, `.nsis.zip` + `.sig` |
| Download-Seite | https://www.shipglobal.dev/en/download |
| Update-Endpunkt | `https://www.shipglobal.dev/api/updates/{{target}}/{{arch}}/{{current_version}}` |

**Kein `latest.json` als Asset.** In einem Repo mit mehreren Apps ist GitHubs
`releases/latest` das jüngste Release *irgendeiner* App. Ein BalaneDisk-Release würde
ship-global-Installationen dessen JSON unterschieben. Der Endpunkt oben filtert nach
dem Tag-Präfix und liefert nur ship global. Aus demselben Grund lesen auch die
Download-Knöpfe auf shipglobal.dev über die GitHub-API statt über `latest/download`.

## Bug-Tracking

Ein Issue pro Bug, Label `app:<slug>` (Pflicht, sonst erscheint es nicht auf der
Status-Seite) plus optional `in-progress`.

| Schließen als | Wirkung |
| --- | --- |
| „completed“ | gefixt, bleibt öffentlich gelistet |
| „not planned“ | verschwindet von der Status-Seite |

## Bilder in der README

Die App-Karten unter `.github/assets/` sind die OpenGraph-Bilder der jeweiligen
App-Seite, einmal heruntergeladen statt verlinkt. Ändert sich das Design auf
balane.app, neu ziehen:

```bash
for a in balanedisk cellalert windownote flowvisual chessplosion shipglobal; do
  curl -sL -o ".github/assets/$a.png" "https://www.balane.app/en/apps/$a/opengraph-image"
done
```
