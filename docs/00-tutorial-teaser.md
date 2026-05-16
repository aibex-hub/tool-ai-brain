# AI-Brain Tutorial — Teaser und Wegweiser

**Datum:** 2026-05-16
**Status:** Tutorial-Teil (0 von 4) — Teaser und Wegweiser
**Betrifft:** Adäquanz-Hypothese, AI-Brain-Architektur, Hook-basierte Kontext-Aktivierung, `ai-brain` CLI

## Das Problem, das du wahrscheinlich schon kennst

Du arbeitest mit Claude oder einem anderen KI-Modell — und die Antworten sind manchmal **brillant**, manchmal aber **frei erfunden**: Funktionen, die es nie gab, Quellen, die nicht existieren, Statistiken aus dem Nichts. Diese Halluzinationen haben viele Ursachen, aber eine besonders unterschätzte: **der Kontext, mit dem das KI-Modell arbeitet, war schlecht gewählt**.

Die naheliegende Antwort darauf lautet: *"Dann gib der KI einfach mehr Kontext."* Aber das funktioniert nicht. Zu viel Kontext schadet genauso wie zu wenig — die wichtigen Informationen ertrinken im Rauschen, und das Modell greift zu Plausiblem statt zu Korrektem.

> *"Z'weng und z'vü is's Narren-Zü."* — Mizzi, oberösterreichisch
>
> *(Zu wenig und zu viel ist des Narren Ziel.)*

## Was du in vier Teilen lernst

Das **AI-Brain-Tutorial** führt dich vom konzeptionellen Problem bis zum lauffähigen System auf deinem eigenen Rechner. In vier kompakten Teilen:

**Teil 1 — Halluzination und Adäquanz.** Wir gehen dem Halluzinations-Problem auf den Grund und formulieren eine handhabbare Antwort: die **Adäquanz-Hypothese**. Für jede KI-Anfrage gibt es einen günstigen Bereich von Kontext — *passend und ausreichend, ohne Überschuss*. Beide Extreme sind das Narren-Zü.

**Teil 2 — AI-Brain als Architektur-Idee.** Eine überraschend einfache Lösung: organisiere Wissen als **Baum**, und aktiviere bei jeder Anfrage nur den **Pfad** vom passenden Wissens-Knoten zur Wurzel. Was nicht auf dem Pfad liegt, bleibt draußen. Adäquanz entsteht damit von selbst — vorausgesetzt der Baum ist vernünftig strukturiert.

**Teil 3 — Aufbau und Aktivierung.** Konkretisierung mit nur **zwei Konventionen**: `§` markiert die Wurzel eines AI-Brains, `@` markiert die Kontext-Ordner darin. Mehr braucht es nicht — das Dateisystem trägt das Prinzip.

**Teil 4 — Installation Schritt für Schritt.** Vom leeren Verzeichnis bis zur ersten Antwort, die mit aktiviertem Kontext erfolgt. Du installierst das Tool `ai-brain` direkt aus der Shell heraus (Claude Code übernimmt das aus dem GitHub-Repo), baust einen Beispiel-Wissensbaum, registrierst den Hook mit einem einzigen Befehl und verifizierst, dass die Pfad-Aktivierung wirklich funktioniert.

## Was du danach hast

- Ein **eigenes AI-Brain** auf deinem System, das mit dir wächst und sich an dein Denken anpasst.
- Eine **mentale Landkarte** dafür, was guten KI-Kontext ausmacht — übertragbar auf jedes KI-Werkzeug, nicht nur Claude.
- Ein **lauffähiges Werkzeug** (`ai-brain`), das die Hook-Registrierung und das Einsammeln des Kontexts automatisiert.

## Warum dieser Weg, und nicht ein anderer?

Die meisten Vorschläge zur Kontext-Optimierung sind entweder **wahlfrei aufwendig** (RAG-Pipelines, Vektor-Datenbanken, semantische Suche) oder **wahlfrei schwach** (alles in eine `CLAUDE.md` packen und hoffen). AI-Brain wählt einen dritten Weg: **eine strukturelle Konvention, die der Mensch selbst kuratiert, und die der Computer mechanisch ausführt**. Kein Embedding-Modell, keine Tuning-Parameter — nur Verzeichnisse und ein kleines Shell-Skript. Die Adäquanz entsteht aus der Struktur, nicht aus statistischer Magie.

Wer den vier Tutorial-Teilen folgt, hat das **minimale, vollständige Konzept** in der Hand und kann es ab Tag 1 auf eigene Wissensgebiete anpassen.

## Einstieg

Die vier Tutorial-Teile liegen im selben Ordner wie dieser Teaser:

| Reihenfolge | Datei |
|---|---|
| 1 | `01 Halluzination und Adäquanz.md` |
| 2 | `02 AI-Brain — Idee einer KI-Architektur.md` |
| 3 | `03 AI-Brain — Aufbau und Aktivierung (vereinfacht).md` |
| 4 | `04 AI-Brain — Installation Schritt für Schritt.md` |

Zeitaufwand: ca. 30 Minuten Lesen, ca. 10 Minuten für die praktische Installation.

Viel Erfolg.
