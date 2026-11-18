# M-Vave FM-1 Firmware

Notizen zur Firmware des Pocket-Synthesizers **M-Vave FM-1**: wie man vor einem Firmware-Wechsel sichert, wie man zur Original-Firmware zurückkommt, woher alternative Firmware stammt und was dabei schiefgehen kann.

Stand: 2026-11-18. Startpunkt aus einer Websuche, noch nicht am eigenen Gerät geprüft.

## Gerät

| | |
|---|---|
| Hersteller | M-Vave (Cuvave) |
| Modell | FM-1 |
| Chip | JieLi AC791N / WL82 (laut Reverse-Engineering-Projekt, siehe [Risiken](#risiken)) |
| Original-Firmware | V15 (aktuellster offizieller Stand, auf den der Melodee-Installer zurückführt) |
| Installierte Firmware | Blop (Stand 2026-11-18), Version noch eintragen |

## Backup

- **Ein Dump der Original-Firmware ist nicht vorgesehen.** Es gibt kein dokumentiertes Werkzeug, das die Firmware vom Gerät ausliest. Die Original-Firmware wird deshalb nicht gesichert, sondern bei Bedarf neu aufgespielt (siehe [Wiederherstellung](#wiederherstellung)).
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

M-UPGRADE kann also nicht direkt von Blop zurück auf die Original-Firmware. Der Rückweg muss über den „Back to official V15“-Weg der Community-Firmware laufen. Wie genau das bei Blop geht, ist noch offen.

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
