# AGENTS.md

Hinweise für KI-Assistenten (Claude Code, Copilot, Cursor, Codex usw.) in diesem Repo.

## Inhalt

Notizen zur Firmware des M-Vave FM-1: Backup, Wiederherstellung, Firmware-Quellen und Risiken. Alles steht in [README.md](README.md). Kein Code, kein Build, keine Tests.

## Konventionen

- **Sprache:** Deutsch.
- **Keine fremden Firmware-Binärdateien** einchecken; Quelle und Version stattdessen im README notieren.
- **Branches:** Neue Branches bekommen einen Typ-Präfix (`feat/`, `fix/`, `chore/`, `docs/` …) und einen kurzen, kebab-case geschriebenen Namen, der den Inhalt beschreibt, z. B. `docs/flash-protokoll`. Keine generischen oder zufälligen Namen wie `claude/<zufall>`. Ein kurzes Suffix, das ein Werkzeug anhängt (die Id einer Agent-Session), ist in Ordnung.
- **LF-Zeilenenden** überall, erzwungen durch `.gitattributes`.
