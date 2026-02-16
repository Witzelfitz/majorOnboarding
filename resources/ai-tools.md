[← Zurück zur Übersicht](../README.md)

# KI-gestützte Entwicklungstools

Künstliche Intelligenz verändert die Softwareentwicklung grundlegend. Als Informatik-Studierende habt ihr die Möglichkeit, diese Werkzeuge von Anfang an kennenzulernen und sinnvoll einzusetzen. Dieser Guide gibt euch einen Überblick über die wichtigsten KI-Tools, zeigt euch, wie ihr sie nutzt -- und wo ihre Grenzen liegen.

---

## AI in der Softwareentwicklung -- Überblick

KI-Tools unterstützen Entwickler:innen heute in nahezu allen Phasen der Softwareentwicklung:

- **Code-Generierung**: Automatische Vervollständigung und Generierung von Code auf Basis natürlicher Sprache
- **Code-Erklärung**: Bestehenden Code verstehen und dokumentieren lassen
- **Debugging**: Fehler finden und Lösungsvorschläge erhalten
- **Refactoring**: Code verbessern und umstrukturieren
- **Testing**: Testfälle generieren lassen
- **Dokumentation**: Kommentare, READMEs und Docs erstellen

> **Wichtig:** Diese Tools sind Werkzeuge, keine Ersatz-Programmierer. Sie machen euch produktiver, ersetzen aber nicht das Verständnis der Grundlagen.

---

## GitHub Copilot

### Was ist GitHub Copilot?

GitHub Copilot ist ein KI-gestützter Programmierassistent von GitHub (Microsoft) und OpenAI. Er integriert sich direkt in euren Editor und schlägt Code in Echtzeit vor -- basierend auf dem Kontext eures aktuellen Projekts.

### Installation (VS Code)

1. Öffnet VS Code
2. Geht zu **Extensions** (`Cmd + Shift + X`)
3. Sucht nach **"GitHub Copilot"**
4. Installiert die Extension **GitHub Copilot** (diese enthält auch den Chat)
5. Meldet euch mit eurem GitHub-Account an

```
# Alternativ über die Kommandozeile:
code --install-extension GitHub.copilot
```

### Kostenlos für Studierende

GitHub Copilot ist für Studierende **kostenlos** über das [GitHub Student Developer Pack](https://education.github.com/pack) verfügbar.

So bekommt ihr Zugang:

1. Geht auf [education.github.com](https://education.github.com)
2. Klickt auf **"Get your Pack"**
3. Verifiziert euch mit eurer Hochschul-E-Mail-Adresse (z. B. `vorname.nachname@uni-xyz.de`)
4. Nach der Bestätigung könnt ihr Copilot in euren GitHub-Settings aktivieren

### Nutzung

**Inline-Suggestions**: Copilot schlägt automatisch Code vor, während ihr tippt. Akzeptiert mit `Tab`, lehnt ab mit `Esc`.

```python
# Beispiel: Schreibt einen Kommentar, Copilot generiert die Funktion
# Berechne den Durchschnitt einer Liste von Zahlen
def calculate_average(numbers):
    if not numbers:
        return 0
    return sum(numbers) / len(numbers)
```

**Copilot Chat**: Öffnet den Chat mit `Cmd + Shift + I` (oder über das Chat-Icon in der Sidebar). Hier könnt ihr Fragen stellen, Code erklären lassen oder Refactoring-Vorschläge erhalten.

Nützliche Chat-Befehle:

| Befehl       | Beschreibung                          |
|-------------|---------------------------------------|
| `/explain`  | Erklärt den markierten Code           |
| `/fix`      | Schlägt einen Fix für Fehler vor      |
| `/tests`    | Generiert Unit-Tests                  |
| `/doc`      | Erstellt Dokumentation                |

**Copilot Edits**: Mit Copilot Edits könnt ihr Änderungen über mehrere Dateien hinweg vornehmen. Öffnet den Edit-Modus über `Cmd + Shift + I`, wählt die relevanten Dateien aus und beschreibt die gewünschte Änderung in natürlicher Sprache. Copilot schlägt dann Edits vor, die ihr akzeptieren oder ablehnen könnt.

### Tipps für gute Prompts

- **Seid spezifisch**: "Erstelle eine Python-Funktion, die eine CSV-Datei einliest und die Spalte 'Alter' als Histogramm plottet" statt "Mach was mit CSV"
- **Gebt Kontext**: Beschreibt die Programmiersprache, Frameworks und den Zweck
- **Nutzt Kommentare**: Schreibt beschreibende Kommentare im Code -- Copilot nutzt diese als Kontext
- **Iteriert**: Wenn der erste Vorschlag nicht passt, formuliert um oder gebt mehr Details

---

## Claude

### Was ist Claude?

Claude ist ein KI-Modell von Anthropic. Es zeichnet sich besonders durch ein grosses Kontextfenster, starke Coding-Fähigkeiten und nuancierte Antworten aus.

### Nutzung über claude.ai

Auf [claude.ai](https://claude.ai) könnt ihr Claude direkt im Browser nutzen. Die kostenlose Version bietet Zugang zum aktuellen Modell mit begrenzter Nutzung. Für Studierende reicht das oft aus, um Fragen zu klären, Code zu besprechen oder Konzepte zu verstehen.

**Typische Anwendungsfälle:**

- Code schreiben und erklären lassen
- Fehler analysieren (Fehlermeldung + Code einfügen)
- Konzepte und Algorithmen erklären lassen
- Architektur-Entscheidungen diskutieren

### Claude Code (CLI)

Claude Code ist ein Kommandozeilen-Tool, das Claude direkt in euer Terminal bringt. Es kann euer gesamtes Projekt als Kontext nutzen und Dateien lesen, schreiben und bearbeiten.

```bash
# Installation
npm install -g @anthropic-ai/claude-code

# Starten im Projektverzeichnis
cd mein-projekt
claude

# Jetzt könnt ihr in natürlicher Sprache Anweisungen geben, z.B.:
# "Erkläre mir die Struktur dieses Projekts"
# "Finde den Bug in der Datei server.js"
# "Schreibe Tests für die User-Klasse"
```

### API-Zugang

Für eigene Projekte könnt ihr die Claude API nutzen:

```python
import anthropic

client = anthropic.Anthropic(api_key="euer-api-key")

message = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=1024,
    messages=[
        {"role": "user", "content": "Erkläre mir Rekursion in Python mit einem Beispiel."}
    ]
)

print(message.content[0].text)
```

> **Hinweis:** Die API ist kostenpflichtig (Pay-per-Use). Für den Einstieg reicht claude.ai in der Regel völlig aus.

### Stärken von Claude

- **Langes Kontextfenster**: Kann sehr grosse Codebasen und Dokumente auf einmal verarbeiten
- **Starkes Coding**: Besonders gut bei komplexen Programmieraufgaben und Code-Analyse
- **Differenzierte Antworten**: Benennt Unsicherheiten und Einschränkungen oft von sich aus
- **Instruktionstreue**: Folgt detaillierten Anweisungen zuverlässig

---

## Cursor

### Was ist Cursor?

Cursor ist ein KI-nativer Code-Editor, der auf VS Code basiert. Im Gegensatz zu Copilot (das ein Plugin ist) wurde Cursor von Grund auf als KI-gestützter Editor konzipiert. Er nutzt verschiedene LLMs (Claude, GPT-4, etc.) und bietet tiefe Integration in den Entwicklungsworkflow.

### Installation

1. Ladet Cursor von [cursor.com](https://cursor.com) herunter
2. Installiert die App (macOS: `.dmg` öffnen und in Applications ziehen)
3. Beim ersten Start könnt ihr eure VS Code Extensions und Settings importieren

```bash
# Cursor ist ein eigenständiger Editor, keine Extension
# Er ersetzt VS Code (oder läuft parallel dazu)
```

### Features

**Chat (`Cmd + L`)**: Stellt Fragen zu eurem Code, lasst euch Dinge erklären oder generiert neuen Code. Der Chat kennt den Kontext eures gesamten Projekts.

**Composer (`Cmd + I`)**: Erstellt oder bearbeitet Code über mehrere Dateien hinweg. Beschreibt, was ihr wollt, und Cursor generiert die Änderungen.

```
Beispiel-Prompt im Composer:
"Erstelle eine REST-API mit Express.js, die CRUD-Operationen für eine
Todo-Liste bereitstellt. Nutze eine SQLite-Datenbank und erstelle die
nötigen Routen, Models und Middleware."
```

**Tab-Completion**: Ähnlich wie Copilot, aber oft kontextbewusster. Cursor analysiert euer gesamtes Projekt und macht Vorschläge basierend auf eurem Code-Stil und eurer Projektstruktur.

### Unterschied zu VS Code + Copilot

| Eigenschaft          | VS Code + Copilot          | Cursor                          |
|---------------------|---------------------------|----------------------------------|
| Basis               | VS Code + Extension        | Fork von VS Code (eigenständig) |
| KI-Modelle          | OpenAI (GPT)              | Wählbar (Claude, GPT, etc.)     |
| Multi-File Editing  | Copilot Edits              | Composer (tief integriert)      |
| Codebase-Awareness  | Begrenzt                   | Indexiert gesamtes Projekt      |
| Preis               | Kostenlos (Student Pack)   | Free-Tier verfügbar, Pro ab $20/Monat |

> **Tipp:** Ihr müsst euch nicht für eins entscheiden. Viele Entwickler:innen nutzen beide Tools je nach Aufgabe.

---

## Weitere Tools

### ChatGPT

[ChatGPT](https://chat.openai.com) von OpenAI ist der bekannteste KI-Chatbot. Nützlich für:

- Allgemeine Programmierfragen
- Erklärungen von Konzepten
- Schnelle Code-Snippets
- Brainstorming und Planung

ChatGPT bietet auch einen **Canvas-Modus** zum gemeinsamen Arbeiten an Code und Text.

### v0.dev (UI-Generierung)

[v0.dev](https://v0.dev) von Vercel generiert UI-Komponenten aus Textbeschreibungen:

```
Prompt-Beispiel:
"Eine moderne Dashboard-Seite mit einer Sidebar-Navigation,
einer Kopfzeile mit Suchfeld und einem Raster aus Statistik-Karten."
```

- Generiert React-Komponenten mit Tailwind CSS und shadcn/ui
- Ideal für Prototyping und UI-Inspiration
- Code kann direkt in euer Projekt übernommen werden

### Bolt (bolt.new)

[Bolt](https://bolt.new) ist ein KI-Tool von StackBlitz, das Full-Stack-Anwendungen direkt im Browser generiert:

- Erstellt komplette Projekte (Frontend + Backend) aus einer Beschreibung
- Läuft in einer WebContainer-Umgebung (kein lokales Setup nötig)
- Gut zum schnellen Prototyping und Experimentieren

### Lovable

[Lovable](https://lovable.dev) (ehemals GPT Engineer) generiert ebenfalls Full-Stack-Anwendungen:

- Fokus auf schnelle, deploybare Prototypen
- Integriert mit Supabase für Backend/Datenbank
- Nützlich für Hackathons und schnelle MVPs

> **Hinweis zu diesen Tools:** Sie eignen sich hervorragend zum Prototyping und Lernen. Für Produktivcode solltet ihr den generierten Code aber immer verstehen und überprüfen.

---

## Best Practices

### AI als Werkzeug, nicht als Ersatz

KI-Tools machen euch produktiver, aber sie ersetzen nicht das Verständnis der Grundlagen. Nutzt sie als **Beschleuniger**, nicht als **Abkürzung**:

- **Lernt die Grundlagen**: Versteht Datenstrukturen, Algorithmen und Designprinzipien -- KI hilft euch dann, sie schneller umzusetzen
- **Debuggt selbst**: Versucht Fehler erst selbst zu verstehen, bevor ihr die KI fragt. So lernt ihr mehr
- **Hinterfragt Vorschläge**: Akzeptiert nicht blind, was die KI vorschlägt

### Code verstehen, den man verwendet

```python
# SCHLECHT: Code von der KI kopieren ohne ihn zu verstehen
# "Das funktioniert irgendwie, ich weiss aber nicht warum"

# GUT: Code verstehen und anpassen können
# "Die Funktion nutzt eine Hash-Map für O(1) Lookups,
#  weil wir häufig nach IDs suchen müssen"
```

**Faustregel:** Wenn ihr nicht erklären könnt, was euer Code macht, habt ihr ihn nicht verstanden -- egal ob er von euch oder einer KI stammt.

### Prompting-Tipps

1. **Kontext geben**: Beschreibt euer Projekt, die Sprache, das Framework und das Ziel
2. **Aufgaben aufteilen**: Statt "Bau mir eine App" lieber Schritt für Schritt vorgehen
3. **Beispiele liefern**: Zeigt der KI, was ihr erwartet (Input/Output-Beispiele)
4. **Einschränkungen nennen**: "Ohne externe Bibliotheken", "Muss in Python 3.10 laufen", etc.
5. **Iterieren**: Verfeinert eure Prompts basierend auf den Ergebnissen

```
# Schwacher Prompt:
"Mach eine Sortierfunktion"

# Starker Prompt:
"Schreibe eine Python-Funktion, die eine Liste von Dictionaries
nach dem Schlüssel 'datum' (Format: 'YYYY-MM-DD') absteigend sortiert.
Die Funktion soll None-Werte im Datum-Feld ans Ende stellen.
Füge Type Hints und einen Docstring hinzu."
```

### Datenschutz beachten

- **Keine sensiblen Daten** in Prompts eingeben (Passwörter, API-Keys, persönliche Daten)
- **Firmen- und Projektdaten**: Prüft die Nutzungsbedingungen eures Tools -- manche nutzen eure Eingaben zum Training
- **Universitäre Richtlinien**: Informiert euch über die KI-Richtlinien eurer Hochschule
- **`.gitignore`** beachten: Dateien, die nicht ins Repo gehören, sollten auch nicht an KI-Tools gesendet werden

```bash
# Beispiel: .env-Dateien niemals an KI-Tools senden
# Diese Datei enthält sensible Daten:
# .env
DATABASE_URL=postgres://user:password@localhost:5432/mydb
SECRET_KEY=super-geheimes-passwort  # NIEMALS teilen!
```

---

## Grenzen & Verantwortung

### Halluzinationen

KI-Modelle generieren manchmal **plausibel klingenden, aber falschen Code**. Das nennt man "Halluzinationen":

- **Erfundene APIs**: Die KI schlägt Funktionen vor, die es nicht gibt
- **Veraltete Syntax**: Code, der mit älteren Versionen einer Bibliothek funktioniert hätte
- **Logische Fehler**: Code, der kompiliert, aber das Falsche tut

**Gegenmaßnahmen:**

- Code immer testen, bevor ihr ihn übernehmt
- Offizielle Dokumentation konsultieren
- Bei Unsicherheit mehrere Quellen prüfen

### Veraltetes Wissen

KI-Modelle haben einen **Wissens-Cutoff** -- sie kennen nur Informationen bis zu einem bestimmten Datum:

- Neue Bibliotheksversionen oder API-Änderungen sind möglicherweise nicht bekannt
- Aktuelle Best Practices können sich geändert haben
- Neue Sicherheitslücken sind dem Modell eventuell unbekannt

> **Tipp:** Prüft bei Bibliotheken und Frameworks immer die **offizielle, aktuelle Dokumentation** und verlasst euch nicht ausschliesslich auf KI-generierte Antworten.

### Akademische Integrität

Der Einsatz von KI-Tools im Studium ist ein wichtiges Thema. Beachtet folgende Punkte:

- **Prüfungsordnung kennen**: Informiert euch, was an eurer Hochschule erlaubt ist und was nicht
- **Transparenz**: Wenn KI-Nutzung erlaubt ist, gebt an, wo und wie ihr sie eingesetzt habt
- **Eigenleistung**: Abgaben müssen eure eigene intellektuelle Leistung widerspiegeln
- **Lerneffekt hinterfragen**: Wenn ihr eine Aufgabe komplett von der KI lösen lasst, lernt ihr nichts daraus
- **Prüfungsvorbereitung**: In Klausuren habt ihr in der Regel keinen Zugang zu KI-Tools -- ihr müsst den Stoff also trotzdem verstehen

> **Grundregel:** Nutzt KI-Tools, um **besser und schneller zu lernen** -- nicht, um das Lernen zu umgehen. Euer Ziel ist es, kompetente Entwickler:innen zu werden, und dafür braucht ihr ein solides Fundament.

---

## Zusammenfassung

| Tool             | Hauptnutzen                        | Preis für Studierende        |
|-----------------|------------------------------------|-------------------------------|
| GitHub Copilot  | Code-Completion im Editor          | Kostenlos (Student Pack)      |
| Claude          | Chat, Code-Analyse, CLI            | Kostenlose Version verfügbar  |
| Cursor          | KI-nativer Editor                  | Free-Tier verfügbar           |
| ChatGPT         | Allgemeiner KI-Assistent           | Kostenlose Version verfügbar  |
| v0.dev          | UI-Generierung                     | Free-Tier verfügbar           |
| Bolt            | Full-Stack Prototyping             | Free-Tier verfügbar           |
| Lovable         | Schnelle App-Prototypen            | Free-Tier verfügbar           |

**Empfehlung für den Einstieg:** Holt euch das GitHub Student Developer Pack, installiert GitHub Copilot in VS Code und nutzt Claude oder ChatGPT als Ergänzung für Fragen und Erklärungen. Experimentiert mit den anderen Tools, wenn ihr ein Gefühl dafür entwickelt habt, was KI-Tools leisten können -- und was nicht.
