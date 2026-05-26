# Teil 4 — AI-Brain — Installation Schritt für Schritt

**Datum:** 2026-05-16
**Status:** Tutorial-Teil (4 von 4) — vollständige technische Anleitung
**Lernziel:** Der Leser installiert das CLI-Werkzeug `ai-brain` via Claude Code, baut den Beispiel-Verzeichnisbaum, befüllt die Masterspaces, registriert den Hook und verifiziert die Pfad-Aktivierung an einer Probe-Anfrage.

## Wo wir stehen

In **Teil 3** haben wir die beiden Namens-Konventionen verstanden:

- `§` markiert die Wurzel des AI-Brains
- Jedes Verzeichnis mit einem `@`-Unterordner (dem **Masterspace**) ist ein Baumknoten

Jetzt setzen wir das konkret um — auf deinem eigenen System.

## Voraussetzungen

Bevor du beginnst, brauchst du:

- **Claude Code** installiert und mit einem Anthropic-Account authentifiziert. Installations-Hinweise: [docs.claude.com/en/docs/claude-code](https://docs.claude.com/en/docs/claude-code).
- **macOS** mit Terminal-App (am einfachsten — diese Anleitung zeigt den Mac-Weg). Für **Linux** oder **Windows/WSL** findest du Plattform-Hinweise im [Anhang der tool-ai-brain README](https://github.com/aibex-hub/tool-ai-brain#appendix-claude-code-in-a-bash-shell).
- ein gewöhnliches Bash-Terminal (`Terminal.app`, `iTerm2`, o.ä.).

## Claude Code als Shell-Tool

Bevor wir loslegen, ein wichtiger Hinweis: **Claude Code läuft direkt in deiner Bash-Shell**. Auf dem Mac öffnest du `Terminal.app`, navigierst mit `cd` in irgendein Verzeichnis und startest:

```bash
claude
```

Damit landest du in Claude Codes interaktivem Prompt. Von hier aus kannst du Claude Anweisungen geben, Dateien lesen lassen, Code schreiben lassen — und, was wir gleich nutzen werden, **Tools aus dem Internet installieren lassen**.

Verlassen kannst du Claude jederzeit mit `exit` oder `Ctrl-D`.

## Was wir bauen

Wir installieren in **Schritt 1** das CLI-Werkzeug **`ai-brain`**, das die AIbex-Gruppe als Open-Source-Tool unter Apache-2.0-Lizenz bei [github.com/aibex-hub](https://github.com/aibex-hub) bereitstellt. `ai-brain` automatisiert das Hook-Setup, sammelt Kontext-Dateien entlang des cwd-Pfads ein und bringt selbst ein eingebettetes Tutorial mit (`ai-brain --tutorial`).

Anschließend bauen wir den Lieblingsrezepte/Elektronik-Beispielbaum aus Teil 3 als reale Verzeichnisstruktur, befüllen ein paar Masterspaces, registrieren den Hook und führen eine Probe-Anfrage aus.

## Schritt 1 — `ai-brain` installieren (via Claude Code)

Ein hilfreicher Effekt davon, dass Claude Code in der Shell läuft: es kann selbst Tools aus dem Internet installieren, indem es deren README liest und die Install-Anweisung ausführt. Genau diesen Weg nutzen wir hier.

Starte Claude Code aus einem beliebigen Verzeichnis (z. B. deinem Home-Ordner):

```bash
claude
```

Gib im Claude-Prompt diese Anweisung ein:

```
installiere ai-brain von github user aibex-hub
```

Claude liest daraufhin die README des Repos [github.com/aibex-hub/tool-ai-brain](https://github.com/aibex-hub/tool-ai-brain), findet dort die **Curl Installation Formula**, zeigt sie dir an und führt sie nach deiner Bestätigung aus. Das Installations-Skript fragt interaktiv, in welches Verzeichnis aus deinem `$PATH` `ai-brain` kopiert werden soll — wähle bevorzugt `~/bin` (falls vorhanden) oder `/usr/local/bin`.

Wenn die Installation durchgelaufen ist, verlasse Claude wieder:

```
exit
```

## Schritt 2 — Smoke-Test in der Shell

Zurück im Bash-Prompt, prüfe die Installation mit drei kleinen Aufrufen:

```bash
ai-brain --version
```

erwartet eine Versionsnummer (z. B. `1.3.4`).

```bash
ai-brain -?
```

zeigt die Kurz-Usage mit allen Subkommandos.

```bash
ai-brain --check
```

verifiziert, dass alle Pflicht-Abhängigkeiten (`ec`, `jq`, `claude`) und die optionale (`tree`) installiert sind. Bei `FAIL`-Zeilen schlägt der Check eine konkrete Fix-Anweisung vor.

Wenn alle drei Aufrufe sauber durchlaufen, ist `ai-brain` einsatzbereit.

## Schritt 3 — Den Beispiel-Verzeichnisbaum anlegen

Wir bauen den Lieblingsrezepte/Elektronik-Baum aus Teil 3 als reale Verzeichnisstruktur in deinem Home-Ordner. Mit *einem* Bash-Befehl:

```bash
mkdir -p \
  ~/§Privat/@ \
  ~/§Privat/Lieblingsrezepte/@ \
  ~/§Privat/Lieblingsrezepte/Lieblingsspeisen/@ \
  ~/§Privat/Lieblingsrezepte/Lieblingsgetränke/@ \
  ~/§Privat/Elektronikentwicklung/@ \
  ~/§Privat/Elektronikentwicklung/Leistungselektronik/@ \
  ~/§Privat/Elektronikentwicklung/HF-Elektronik/@
```

## Schritt 4 — Baum inspizieren

```bash
tree -a ~/§Privat
```

Du solltest sieben Baumknoten sehen, jeder mit eigenem `@`-Masterspace:

```
§Privat/
├── @/
├── Lieblingsrezepte/
│   ├── @/
│   ├── Lieblingsspeisen/
│   │   └── @/
│   └── Lieblingsgetränke/
│       └── @/
└── Elektronikentwicklung/
    ├── @/
    ├── Leistungselektronik/
    │   └── @/
    └── HF-Elektronik/
        └── @/
```

## Schritt 5 — Erste Beispiel-Inhalte einfüllen

Damit der Aktivierungs-Mechanismus später etwas zum Einsammeln hat, füllen wir drei der Masterspaces mit kurzen Beispiel-Texten. Du kannst die Inhalte später beliebig ändern oder erweitern. Die folgenden Blöcke sind so gebaut, dass du sie **vollständig am Stück in die Shell kopieren** kannst — vom `cat`-Befehl bis zum abschließenden `EOF`.

**Wurzel-Masterspace** — Dinge, die für alle Anfragen gelten:

```bash
cat > ~/§Privat/@/persoenliche-allgemein.md <<'EOF'
# Persönliche allgemeine Präferenzen

- Sprache: Deutsch
- Anrede: Du
- Stil: knapp, sachlich, direkt
- Bei Code-Beispielen: kommentiere sparsam, nur wo es nicht-offensichtlich ist
EOF
```

**Rezepte-Masterspace** — gilt für alle Rezepte-Fragen:

```bash
cat > ~/§Privat/Lieblingsrezepte/@/rezept-vorlieben.md <<'EOF'
# Rezept-Vorlieben

- Wenig Zucker, wenn möglich
- Bevorzugt mediterrane und alpenländische Küche
- Keine Erdnüsse (Allergie im Haushalt)
- Mengenangaben in Gramm und Esslöffeln, nicht in Cups
EOF
```

**Speisen-Unterast** — speisenspezifisch:

```bash
cat > ~/§Privat/Lieblingsrezepte/Lieblingsspeisen/@/speisen-praeferenzen.md <<'EOF'
# Speisen-Präferenzen

- Bevorzugte Garmethoden: Schmoren, Niedertemperatur, Backofen
- Lieblings-Aromen: Rosmarin, Thymian, Knoblauch, Zitrone
- Vegetarisch ist willkommen, muss aber nicht sein
EOF
```

Die übrigen Masterspaces lassen wir vorerst leer — der Mechanismus funktioniert auch mit leeren `@`-Ordnern.

## Schritt 6 — Den befüllten Baum inspizieren

```bash
tree -a ~/§Privat
```

Jetzt erscheinen die drei `.md`-Dateien innerhalb der entsprechenden `@`-Ordner — der Wissensbaum ist mit Inhalt befüllt.

## Schritt 7 — Den Hook registrieren

```bash
ai-brain --setup
```

Was im Hintergrund passiert: `ai-brain --setup` trägt den Hook **idempotent** in deine globale Claude-Code-Konfiguration `~/.claude/settings.json` ein (per `jq`-Merge — andere Top-Level-Keys wie `"theme"` oder bereits vorhandene Hooks bleiben unberührt). Ab sofort ruft Claude Code bei *jeder* Anfrage `ai-brain --context` auf; das Hook-Skript prüft selbstständig, ob das aktuelle Verzeichnis innerhalb eines AI-Brains (also unterhalb einer `§`-Wurzel mit `@`-Unterordnern auf dem Pfad) liegt — und liefert nur dann Kontext. Sonst läuft es still durch.

Mehrfaches Ausführen von `--setup` schadet nicht: der Hook wird nicht doppelt eingetragen.

## Schritt 8 — Die erste Probe-Anfrage

Wechsle in einen der Baumknoten und starte Claude Code:

```bash
cd ~/§Privat/Lieblingsrezepte/Lieblingsspeisen
claude
```

Stelle deine erste Frage, zum Beispiel:

> *Schlag mir eine Apfelstrudel-Variation mit weniger Zucker vor.*

Wenn alles funktioniert, antwortet Claude:

- mit einer Variation, die deine *persönlichen Präferenzen* berücksichtigt (Sprache, Stil aus dem Wurzel-Masterspace)
- die *Rezept-Vorlieben* respektiert (weniger Zucker, keine Erdnüsse, Mengen in Gramm)
- und die *speisenspezifische Note* aufnimmt (mediterrane Aromen, langsame Garmethoden)

Die Elektronik-Inhalte bleiben außen vor — sie liegen außerhalb des aktivierten Pfads.

## Schritt 9 — Verifizieren, dass es funktioniert hat

Wie kontrollierst du, dass der Kontext tatsächlich geladen wurde? Drei Wege:

### Direkte Kontroll-Frage stellen

```
Welche persönlichen Präferenzen kennst du? Liste sie aus dem mir vorliegenden Kontext.
```

Wenn der Hook funktioniert hat, antwortet Claude mit den drei eingespielten Masterspace-Inhalten. Wenn nicht, sagt Claude *"Ich habe keine spezifischen Präferenzen zu dir."*

### Kontext-Output direkt prüfen

Du kannst den Kontext-Befehl auch ohne Claude direkt aufrufen — er ist ein eigenständiges Skript und unabhängig vom Hook-Mechanismus:

```bash
cd ~/§Privat/Lieblingsrezepte/Lieblingsspeisen
ai-brain --context
```

Du solltest die drei oben angelegten `.md`-Inhalte sehen, eingerahmt in `<skill src="…">`-Tags — und sonst nichts.

### In andere Ordner wechseln und Verhalten vergleichen

```bash
cd ~/§Privat/Elektronikentwicklung/Leistungselektronik
claude
```

Stelle dieselbe Kontroll-Frage. Diesmal sollte Claude *keine* der Rezepte-Inhalte zurückgeben — sie liegen jetzt in einem nicht-aktivierten Seitenast. Wohl aber den Wurzel-Masterspace (`persoenliche-allgemein.md`), weil der entlang jedes Pfads aktiv ist.

Wenn dieser Vergleich klappt, hast du **die Pfad-Aktivierung in Aktion bewiesen**.

## Schritt 10 (optional) — Hook wieder entfernen

Wenn du den Hook später wieder loswerden möchtest:

```bash
ai-brain --cleanup
```

Das entfernt die `ai-brain --context`-Einträge aus deiner globalen `~/.claude/settings.json` — andere Hooks und Einstellungen (`theme` etc.) bleiben unberührt. Die Masterspaces und die Verzeichnisstruktur unter `§Privat/` bleiben ebenfalls erhalten.

## Troubleshooting — typische Stolpersteine

| Symptom | Mögliche Ursache | Lösung |
|---|---|---|
| `claude: command not found` | Claude Code nicht installiert / nicht auf PATH | siehe [Claude Code Docs](https://docs.claude.com/en/docs/claude-code) |
| `ai-brain: command not found` nach Schritt 1 | Install-Verzeichnis nicht auf PATH | `~/bin` zum PATH hinzufügen oder das Skript in ein PATH-Verzeichnis kopieren |
| `ai-brain --check` meldet **FAIL** für `jq` | `jq` fehlt | `brew install jq` (Mac) oder `apt install jq` (Linux) |
| `ai-brain --check` meldet **FAIL** für `ec` | Bluccino-Helper `ec` fehlt | siehe [bluccino/tool-ec](https://github.com/bluccino/tool-ec) |
| Kontext-Antwort ignoriert Masterspace-Inhalte | Hook läuft nicht oder findet `@`-Ordner nicht | `ai-brain --context` aus dem aktuellen Verzeichnis ausführen und Output prüfen |
| *"hook timeout"* in Claude Code | Hook-Befehl zu langsam | bei Cloud-Drive-gemounteten Pfaden (z. B. Google Drive) kann der `find`-Lauf langsam sein — Masterspaces ggf. lokal halten |
| Dateinamen mit Sonderzeichen wie `§` werden nicht gefunden | UTF-8-Encoding-Problem | sicherstellen, dass Terminal und Filesystem UTF-8 verwenden |

## Was du jetzt kannst

Am Ende von Teil 4 hast du:

1. **`ai-brain` installiert** — via Claude Code aus dem GitHub-Repo `aibex-hub/tool-ai-brain`, mit `--version` / `--check` verifiziert.
2. **Ein funktionierendes AI-Brain auf deinem System** — `§Privat/` mit sieben Wissens-Knoten und drei gefüllten Masterspaces.
3. **Den Hook global registriert** — `ai-brain --setup` hat ihn in `~/.claude/settings.json` eingetragen; weil der Hook bei jedem cwd selbst entscheidet, ob er Kontext liefert, ist das unbedenklich.
4. **Eine erste Probe-Anfrage durchgeführt** und gesehen, dass der Kontext entlang des Pfads aktiviert wird.
5. **Drei Verifikations-Wege** in der Hand, mit denen du jederzeit kontrollieren kannst, ob die Aktivierung funktioniert.
6. **Den Lieblingsrezepte/Elektronik-Beispielbaum als Vorlage** — du kannst die Verzeichnisnamen, Masterspace-Inhalte und Themenbereiche frei abändern und auf deine eigenen Wissensgebiete anpassen.

## Nächste Schritte (über das Tutorial hinaus)

Mit den bisherigen vier Teilen hast du das **minimale, vollständige Konzept** in der Hand. Was sich nun anbietet:

- **Eigene Themenbereiche** aufnehmen: lösche die Beispiel-Verzeichnisse und ersetze sie durch deine tatsächlichen Arbeitsgebiete.
- **Vorhandene Projekte** in das AI-Brain integrieren: lege einfach `@`-Unterordner an passenden Stellen an und fülle sie mit projektspezifischen Notizen.
- **Beobachten und Anpassen**: ändere die `.md`-Inhalte, wenn du merkst, dass Claude zu allgemein oder zu spezifisch antwortet — der Masterspace ist *deine* Stellschraube für Adäquanz.
- **Spickzettel jederzeit abrufbar**: `ai-brain --tutorial` zeigt eine kompakte, copy-paste-fähige Schritt-für-Schritt-Variante dieser Anleitung als Terminal-Output.

Das AI-Brain ist kein fertiges Produkt, sondern ein **Werkzeug, das mit dir wächst**. Beim ersten Mal ist es klein. Mit jeder Woche wird es besser an dein Denken angepasst — vorausgesetzt, du nutzt es aktiv und kuratierst seine Inhalte.

## Glückwunsch

Du hast jetzt einen lauffähigen AI-Brain — und die theoretische Grundlage in den Teilen 1 bis 3, um zu verstehen, *warum* er das tut, was er tut. Damit endet das Tutorial. Was bleibt, ist die eigene Erfahrung.

Viel Erfolg.
