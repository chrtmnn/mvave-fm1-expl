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
| Installierte Firmware | Original V15 (seit 2026-11-18) |

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

1. FM-1 voll laden. Offizielle V15-Datei [`FM-1_015.fwsc`](https://github.com/m-vave/fm1_open/blob/HEAD/sdk/firmware/FM-1_015.fwsc) aus M-Vaves Repo `m-vave/fm1_open` (m-vave.com bietet nur noch V16 an) und M-Flasher (Windows-Zip, nicht signiert) herunterladen. M-UPGRADE, DAW und Browser-Tabs mit MIDI schließen. FM-1 direkt per USB-Datenkabel anschließen, kein Hub.
2. In M-Flasher **DOWNLOAD MODE**: Das Display friert ein, es wird noch nichts geschrieben. Aus- und Einschalten bringt den alten Zustand zurück. Nicht jede Firmware reagiert darauf.
3. M-Flasher liest und sichert den ganzen Flash-Speicher (`Dokumente\M-Flasher\backups`).
4. Angebot „m-bootloader installieren“ ablehnen, damit der Werks-Bootloader bleibt.
5. **Install firmware** mit der V15-`.fwsc`, ungefähr 10 Sekunden. Nicht abziehen.
6. Abstecken, aus- und wieder einschalten.

Reagiert die Firmware nicht auf DOWNLOAD MODE, nennt die Rettungsseite als letzten Ausweg den FM-1 Transporter (Hardware). Vorher dort nachfragen, statt ein Werkzeug nach dem anderen zu probieren.

## Firmware-Quellen

| Quelle | Art | Hinweis |
|---|---|---|
| M-Vave, offizieller Updater (M-UPGRADE) | Original | [m-vave.com/download](https://www.m-vave.com/download), dort nur V16 (seit 20.10.2026) |
| [m-vave/fm1_open](https://github.com/m-vave/fm1_open) | Original V15 | `sdk/firmware/FM-1_015.fwsc`, Apache-2.0, auch Quelle für „Back to official V15“ im Hub |
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
| 2026-11-18 | Blop | V15 (`FM-1_015.fwsc`) | M-Flasher | Erfolgreich, Flash-Backup in `Dokumente\M-Flasher\backups` |

## Firmware-Empfehlungen

Alle hier genannten speichern laut [Firmware Hub](https://thefirmwarehub.com/tools/rescue/#chip) auf dem neuen Flash-Chip (Stand 2026-11-18). Blop 1.1.0 tut das nicht.

| Firmware | Wofür | Hinweis |
|---|---|---|
| [SLOOP](https://github.com/isod89/sloop-fm1) 2.8.5 | Einstieg für ganze Songs: 4 Spuren (3 Synths, 1 Drums), Song aus Teilen A–D | [Guide](https://github.com/isod89/sloop-fm1/blob/main/GUIDE.md), [Handbuch](https://github.com/isod89/sloop-fm1/blob/main/SLOOP.md), [Installer](https://isod89.github.io/sloop-fm1/) |
| [FM-FAM](https://github.com/davidamomusic/fm-fam) 0.3.1 | DFAM-artig: Kick plus eine Synth-Stimme, Snare/Hats selbst bauen | Beta; [Spickzettel](https://davidamomusic.github.io/fm-fam/fam/docs/FM-FAM-cheatsheet.pdf) |
| [X0X](https://github.com/charlesvestal/fm1-x0x) 1.2.2 | 909/808-Drums, zwei 303 | |
| [Hortator](https://github.com/deadactive/hortator) 1.0.1 | Drumcomputer, 8 Spuren, Grids | |
| [MONO2K](https://github.com/InigoHernandez/mono2k) 1.12 | Monomachine-artig, 4 Drum-Synths | |
| [Ravel](https://github.com/De-Pattern/Ravel_FM1) 1.42.0 | 33 Drum-Maschinen nach Machinedrum | |

### SLOOP: erster Song

1. Spur 4 (Drums) per ALGORITHM, Kit mit PRESETS (z. B. 808, BOOMBAP).
2. REC, frei spielen, REC auf der „1“ nach dem letzten Takt schließt die Schleife und setzt das Tempo. REC während des Abspielens nimmt obendrauf auf.
3. Spur 1: Bassline aufnehmen. SEL halten + Grundton setzt die Tonart.
4. Spur 2: SEL halten, Knopf 1 auf 7TH: jede weiße Taste spielt einen Akkord.
5. EDIT + OCT− macht rückgängig.
6. SAVE + Taste 5–8 speichert die Schleife als Teil A–D, SAVE + 1–4 spielt Teile ab dem nächsten Takt, SAVE + 14 nimmt den Song auf.

Zurück zur Original-Firmware laut SLOOP: Backup, dann M-UPGRADE mit V16. Findet M-UPGRADE das Gerät nicht, der [M-Flasher-Weg](#rückweg-zu-v15-mit-m-flasher).

## Für künftige Firmware-Wechsel

1. **Chip prüfen:** Das Gerät hat vermutlich den neuen Flash-Chip (in M-Flasher unter Device info bestätigen). Nur Firmware nehmen, die ihn unterstützt; die Liste steht auf der [Rettungsseite](https://thefirmwarehub.com/tools/rescue/#chip).
2. **Vorher sichern:** Mit M-Flasher ein Flash-Backup machen und es aufheben.
3. **Akku voll, USB direkt**, nichts anderes mit MIDI offen (M-UPGRADE, DAW, Browser-Tabs).
4. **Nicht wiederholt installieren**, wenn etwas scheitert. Erst Ursache klären.
5. **Diese Dateien aufheben:** `FM-1_015.fwsc` und M-Flasher.
6. **Ins Protokoll eintragen.**

## Offene Punkte

- Klären, ob die offizielle Workbench ein Auslesen oder eine Rettung nach abgebrochenem Flashen ermöglicht.
- Ablageort für Backups festlegen (M-Flasher-Backups liegen sonst nur unter `Dokumente\M-Flasher\backups`).
