# M-Vave FM-1 Firmware

Notizen zur Firmware des Pocket-Synthesizers **M-Vave FM-1**: wie man vor einem Firmware-Wechsel sichert, wie man zur Original-Firmware zurückkommt, woher alternative Firmware stammt und was dabei schiefgehen kann.

Stand: 2026-11-18. Startpunkt aus einer Websuche, noch nicht am eigenen Gerät geprüft.

## Gerät

| | |
|---|---|
| Hersteller | M-Vave (Cuvave) |
| Modell | FM-1 |
| Chip | JieLi AC791N / WL82 (laut Reverse-Engineering-Projekt, siehe [Risiken](#risiken)) |
| Flash-Chip | vermutlich neu (UC25HQ80, `0xB36014`), siehe [Ursache](#ursache-neuer-flash-chip); in M-Flasher unter Device info prüfen |
| Original-Firmware | V15 (aktuellster offizieller Stand, auf den der Melodee-Installer zurückführt) |
| Installierte Firmware | Blop (Stand 2026-11-18), Version noch eintragen |

## Backup

- **Ganzer Flash-Speicher:** [M-Flasher](https://github.com/jvitkauskas/m-flasher) liest und sichert vor jedem Schreiben den kompletten 1-MiB-Flash über den Download-Modus und kann ihn später zurückspielen. Das ist der einzige bekannte Weg, die installierte Firmware samt Daten zu sichern.
- **Sounds und Einstellungen** sichert der Installer der Community-Firmware [Melodee](https://github.com/keremimo/melodee/wiki) vor dem Flashen. Diese Sicherung vor jedem Wechsel aufbewahren.
- Eigene Patches und Sicherungen gehören **nicht** in dieses Repo, falls sie fremde Firmware-Binärdateien enthalten; Ablageort hier eintragen: _offen_.

## Wiederherstellung

Zurück zur Original-Firmware gibt es zwei Wege:

1. **Offizieller M-Vave-Updater** des Herstellers.
2. **„Return to official V15“** im Installer von Melodee.

Danach die gesicherten Sounds und Einstellungen wieder einspielen.

### Installer meldet „takes no updates“

Meldung beim Versuch, [Blop](https://thefirmwarehub.com/firmware/blop/) zu installieren: *„The firmware on your FM-1 takes no updates. Hold OCT− while switching it on, press Back to official V15, then install again.“* Der Vorgang brach ab, ohne etwas zu schreiben (*„stopped after 0 requests: nothing was written“*).

Am 2026-11-18 war Blop zu dem Zeitpunkt bereits installiert: Das Gerät startet in Blop, und auch mit gehaltenem OCT− kam kein Update-Modus.

### Offizieller Updater erkennt Community-Firmware nicht

M-UPGRADE (offizieller Updater, v3.11.0614) meldet bei laufendem Blop *„A USB MIDI device is present but did not answer“*. Laut Diagnose-Log (`M-UPGRADE/LOG/*.jsonl`) antwortet das Gerät sehr wohl, aber mit einer Kennung, die M-UPGRADE nicht kennt:

| Merkmal | Wert unter Blop |
|---|---|
| USB VID:PID | `1209:0001` (pid.codes, Community-Kennung) |
| MIDI-Identity | `FM-1_94010100` |

M-UPGRADE kann also nicht direkt von Blop zurück auf die Original-Firmware.

### Ursache: neuer Flash-Chip

Laut [FM-1-Rettungsseite des Firmware Hub](https://thefirmwarehub.com/tools/rescue/#chip) haben FM-1, die seit Herbst 2026 verkauft werden, einen anderen Flash-Chip (UC25HQ80, ID `0xB36014`, frühere Geräte: Puya, `0x856014`). Viele Community-Firmwares, darunter Blop 1.1.0, schreiben den Flash nur bei der alten ID. Auf einem neuen Gerät landen Saves dann nur im RAM („SAVED (RAM)“), Updates scheitern, M-UPGRADE lehnt ab und OCT− beim Einschalten hilft nicht. Kaputt ist nichts. Neu installieren hilft nicht.

Vor jeder Community-Firmware auf diesem Gerät prüfen, ob sie den neuen Chip unterstützt (Liste auf der Rettungsseite).

### Rückweg zu V15 mit M-Flasher

[M-Flasher](https://github.com/jvitkauskas/m-flasher) ([Anleitung](https://github.com/jvitkauskas/m-flasher/blob/main/docs/usage.md)) nutzt den Download-Modus im ROM des Chips („WL82 UBOOT1.00“), den keine Firmware überschreiben kann, und kann mit dem neuen Chip umgehen.

1. FM-1 voll laden. Offizielle V15-`.fwsc` von der M-Vave-Seite und M-Flasher (Windows-Zip, nicht signiert) herunterladen. M-UPGRADE, DAW und Browser-Tabs mit MIDI schließen. FM-1 direkt per USB-Datenkabel anschließen, kein Hub.
2. In M-Flasher **DOWNLOAD MODE**: Das Display friert ein, es wird noch nichts geschrieben. Aus- und Einschalten bringt den alten Zustand zurück. Nicht jede Firmware reagiert darauf.
3. M-Flasher liest und sichert den ganzen Flash-Speicher (`Dokumente\M-Flasher\backups`).
4. Angebot „m-bootloader installieren“ ablehnen, damit der Werks-Bootloader bleibt.
5. **Install firmware** mit der V15-`.fwsc`, ungefähr 10 Sekunden. Nicht abziehen.
6. Abstecken, aus- und wieder einschalten.

Reagiert die Firmware nicht auf DOWNLOAD MODE, nennt die Rettungsseite als letzten Ausweg den FM-1 Transporter (Hardware). Vorher dort nachfragen, statt ein Werkzeug nach dem anderen zu probieren.

## Firmware-Quellen

| Quelle | Art | Hinweis |
|---|---|---|
| M-Vave, offizieller Updater | Original | Weg zurück zum Werkszustand |
| [Melodee](https://github.com/keremimo/melodee/wiki) | Community-Firmware | Installer mit Backup und Rückweg zu V15 |
| The Firmware Hub | Sammlung alternativer Firmware | Herkunft je Datei prüfen; installiert: [Blop](https://thefirmwarehub.com/firmware/blop/) |
| [M-Vave Workbench und SDK](https://synthtopia.com/content/2026/10/20/m-vave-officially-opens-fm-1-firmware-to-developers-with-new-workbench-sdk) | Offizielle Entwicklerwerkzeuge (seit Oktober 2026) | Grundlage für eigene Firmware |

## Risiken

- **Abgebrochenes Flashen kann das Gerät unbrauchbar machen.** Ein [Reverse-Engineering-Projekt](https://zeli.app/de/story/49994065) zum Chip JieLi AC791N/WL82 warnt, dass es danach keine bewiesene Rettung gibt.
- Deshalb beim Flashen: Akku voll bzw. stabile Stromversorgung, gutes USB-Kabel direkt am Rechner (kein Hub), Vorgang nicht unterbrechen.
- Firmware nur aus nachvollziehbarer Quelle aufspielen; bei Dateien von Sammelseiten Herkunft und Version notieren.

## Protokoll

| Datum | Von | Nach | Werkzeug | Ergebnis |
|---|---|---|---|---|
| 2026-11-18 | Blop (bereits installiert) | Blop | Blop-Installer | Abbruch ohne Schreibvorgang: „takes no updates“ |
| 2026-11-18 | Blop | – | M-UPGRADE v3.11.0614 | Gerät nicht erkannt (Kennung `1209:0001`, `FM-1_94010100`) |

## Offene Punkte

- Installierte Firmware-Version am Gerät ablesen und oben eintragen.
- Klären, ob die offizielle Workbench ein Auslesen oder eine Rettung nach abgebrochenem Flashen ermöglicht.
- Ablageort für Backups festlegen.
