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
| Installierte Version | _noch eintragen_ |

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

Meldung des Melodee-Installers: *„The firmware on your FM-1 takes no updates. Hold OCT− while switching it on, press Back to official V15, then install again.“* Dabei bricht er ab, bevor er etwas schreibt (*„stopped after 0 requests: nothing was written“*). Das Gerät ist in diesem Fall unverändert.

Ablauf laut Installer:

1. FM-1 ausschalten.
2. **OCT−** gedrückt halten und das Gerät einschalten.
3. Im Installer **„Back to official V15“** wählen. Das ist selbst ein Flashvorgang, es gelten die [Risiken](#risiken).
4. Danach die Installation von Melodee erneut starten.

## Firmware-Quellen

| Quelle | Art | Hinweis |
|---|---|---|
| M-Vave, offizieller Updater | Original | Weg zurück zum Werkszustand |
| [Melodee](https://github.com/keremimo/melodee/wiki) | Community-Firmware | Installer mit Backup und Rückweg zu V15 |
| The Firmware Hub | Sammlung alternativer Firmware | Herkunft je Datei prüfen |
| [M-Vave Workbench und SDK](https://synthtopia.com/content/2026/10/20/m-vave-officially-opens-fm-1-firmware-to-developers-with-new-workbench-sdk) | Offizielle Entwicklerwerkzeuge (seit Oktober 2026) | Grundlage für eigene Firmware |

## Risiken

- **Abgebrochenes Flashen kann das Gerät unbrauchbar machen.** Ein [Reverse-Engineering-Projekt](https://zeli.app/de/story/49994065) zum Chip JieLi AC791N/WL82 warnt, dass es danach keine bewiesene Rettung gibt.
- Deshalb beim Flashen: Akku voll bzw. stabile Stromversorgung, gutes USB-Kabel direkt am Rechner (kein Hub), Vorgang nicht unterbrechen.
- Firmware nur aus nachvollziehbarer Quelle aufspielen; bei Dateien von Sammelseiten Herkunft und Version notieren.

## Protokoll

| Datum | Von | Nach | Werkzeug | Ergebnis |
|---|---|---|---|---|
| 2026-11-18 | Original (Version unbekannt) | Melodee | Melodee-Installer | Abbruch ohne Schreibvorgang: „takes no updates“, Weg über V15 nötig |

## Offene Punkte

- Installierte Firmware-Version am Gerät ablesen und oben eintragen.
- Klären, ob die offizielle Workbench ein Auslesen oder eine Rettung nach abgebrochenem Flashen ermöglicht.
- Ablageort für Backups festlegen.
