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

## NetMute, abweichend

NetMute wird doppelt vertrieben: im Mac App Store und ab sofort auch direkt
ueber Lemon Squeezy mit Lizenzschluessel. Vier Unterschiede zu den anderen Apps.

| Was | Wert |
| --- | --- |
| Tag | `netmute-v<version>` |
| macOS | `NetMute-<version>.dmg`, Developer ID, notarisiert |
| Windows | gibt es nicht und wird es nicht geben |
| Quelle | privat, `JHBalane/NetMute` |

**Der Direktbuild ist ein anderer Build.** Genau wie bei CellAlert darf die
Lizenz- und Kauffläche niemals in den Store-Build geraten (Apple 3.1.1).
Gesteuert wird das nicht ueber eine Projekteinstellung, sondern ueber eine
Compilerbedingung, die nur das Build-Skript uebergibt:

```
tools/build-direct.sh archive     # setzt SWIFT_ACTIVE_COMPILATION_CONDITIONS=DIRECT_DISTRIBUTION
```

Ein Archiv aus Xcode oder aus einem CI-Lauf ohne dieses Skript enthaelt den
Lizenzcode nicht einmal als Zeichenkette im Binary. Das ist Absicht: eine
Einstellung, die man vergessen kann, waere hier ein Ablehnungsgrund im Review.

**Blocker, Stand 2026-09-10.** Es gibt noch kein Direkt-Release, weil Apple die
Berechtigung `com.apple.developer.networking.networkextension` mit dem Suffix
`-systemextension` fuer die App-ID freischalten und ein passendes
Developer-ID-Provisioning-Profil ausstellen muss. Ohne das schlaegt das
Signieren der Systemerweiterung fehl. `netmute/appcast.xml` ist deshalb ein
leerer, aber gueltiger Kanal.

**Systemerweiterung beim Update.** Ein Update darf das App-Bundle in
`/Applications` nicht ersetzen, waehrend der Filter laeuft — das reisst die
Systemerweiterung ab. Sparkle muss den Filter vorher sauber stoppen. Das ist
der Unterschied zu jeder anderen App in diesem Repository.

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

## OtterSlide, abweichend

OtterSlide ist ein Spiel und kommt in erster Linie aus den Stores (App Store,
Mac App Store, Play Store, Microsoft Store). Der Direkt-Download hier ist die
Zugabe für Leute ohne Store-Konto.

| Was | Wert |
| --- | --- |
| Tag | `otterslide-v<version>` |
| macOS | `OtterSlide-<version>.dmg`, Developer ID, notarisiert |
| Windows | `OtterSlide-<version>-setup.exe` (Inno Setup, **unsigniert**) |
| Quelle | privat, `JHBalane/otterslide_flutter` |

**Kein Sparkle.** Die App aktualisiert sich nicht selbst — der Abschnitt
„Updates" in der README gilt für OtterSlide nicht. Wer die Direkt-Fassung
benutzt, holt sich die nächste Version wieder von Hand. Es gibt daher auch kein
`release.json` und keine ZIP, nur die beiden Installer.

Der Windows-Build kommt aus GitHub Actions (`.github/workflows/windows-msix.yml`
im App-Repo, ausgelöst über die Actions-Registerkarte oder ein `v*`-Tag), weil
Flutter Windows nicht von einem Mac aus bauen kann. Derselbe Lauf erzeugt beides:
die unsignierte MSIX für Partner Center und die `-setup.exe` für hier. Beide
werden als Build-Artefakte abgeholt und von Hand hochgeladen.

Werbung und Käufe sind auf iOS und Android begrenzt. Auf dem Schreibtisch ist
beides aus, die Direkt-Fassung zeigt also keine Abo- oder Kauffläche — was die
Store-Regel aus dem CellAlert-Abschnitt hier von selbst erfüllt.

## Bug-Tracking

**Issues legt nur der Maintainer an.** Meldungen kommen über Zammad
(`support@balane.tech`, Formular auf `balane.app/<lang>/support`), nicht über
GitHub. `.github/ISSUE_TEMPLATE/config.yml` schaltet Blank Issues ab und zeigt
auf „New issue“ nur noch die Support-Links — ein Kanal, kein zweiter Posteingang.
Aus einem Zammad-Ticket wird hier ein Issue, wenn der Bug bestätigt ist; die
Ticket-Nummer gehört in den Issue-Text, damit die Antwort an den Melder
zuordenbar bleibt.

Zusätzlich läuft auf dem Repo ein GitHub *interaction limit*
(`collaborators_only`): Fremde können weder Issues öffnen noch kommentieren,
Lesen bleibt offen. GitHub befristet das auf sechs Monate — **läuft ab am
2027-03-07**, danach neu setzen:

```bash
gh api -X PUT /repos/JHBalane/Balane-Releases/interaction-limits \
  -f limit=collaborators_only -f expiry=six_months
```

Ein Issue pro Bug, Label `app:<slug>` (Pflicht, sonst erscheint es nicht auf der
Status-Seite) plus ein Zustands-Label.

| Label | Bedeutet |
| --- | --- |
| — | gemeldet, noch nicht in Arbeit |
| `in-progress` | wird gerade bearbeitet |
| `awaiting-release` | Fix ist fertig, die Version ist noch nicht draußen |

`awaiting-release` ersetzt `in-progress`, sobald der Fix im Code liegt, und
verschwindet mit dem Release — dann wird das Issue geschlossen.

| Schließen als | Wirkung |
| --- | --- |
| „completed“ | gefixt, bleibt öffentlich gelistet |
| „not planned“ | verschwindet von der Status-Seite |

## Bilder in der README

Die App-Karten unter `.github/assets/` sind die OpenGraph-Bilder der jeweiligen
App-Seite, einmal heruntergeladen statt verlinkt. Ändert sich das Design auf
balane.app, neu ziehen:

```bash
for a in balanedisk cellalert windownote flowvisual chessplosion shipglobal otterslide; do
  curl -sL -o ".github/assets/$a.png" "https://www.balane.app/en/apps/$a/opengraph-image"
done
```
