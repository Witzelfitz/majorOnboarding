[← Zurück zur Übersicht](../README.md)

# GitHub

GitHub ist die weltweit grösste Plattform für Softwareentwicklung. Hier werden Projekte gehostet, gemeinsam daran gearbeitet und Code reviewed. Dieser Guide erklärt dir alles, was du als Studentin oder Student brauchst, um GitHub produktiv zu nutzen.

---

## Was ist GitHub?

**Git** ist ein Versionskontrollsystem, das lokal auf deinem Rechner läuft. Es verfolgt Änderungen an deinen Dateien und ermöglicht es dir, verschiedene Versionen deines Codes zu verwalten.

**GitHub** ist eine Plattform, die auf Git aufbaut und zusätzliche Funktionen bietet:

| Git (lokal) | GitHub (remote) |
|---|---|
| Versionskontrolle auf deinem Rechner | Hosting von Repositories in der Cloud |
| Commits, Branches, Merges | Pull Requests, Code Reviews |
| Funktioniert offline | Zusammenarbeit im Team |
| Kommandozeilen-Tool | Weboberfläche + API |
| — | Issues, Projects, GitHub Pages |
| — | CI/CD mit GitHub Actions |

Kurz gesagt: **Git** ist das Werkzeug, **GitHub** ist die Plattform, die darauf aufbaut.

> **Alternativen zu GitHub:** Es gibt auch andere Git-Plattformen wie GitLab oder Bitbucket. Die Konzepte sind überall ähnlich, aber GitHub ist die am weitesten verbreitete Plattform.

---

## Account erstellen & einrichten

### Account erstellen

1. Gehe auf [github.com](https://github.com) und klicke auf **Sign up**.
2. Verwende deine **Hochschul-E-Mail-Adresse** — damit bekommst du Zugang zum Student Developer Pack (mehr dazu weiter unten).
3. Wähle einen professionellen Benutzernamen — er wird auf all deinen Beiträgen sichtbar sein.

### Profil einrichten

Nach der Registrierung solltest du dein Profil vervollständigen:

- **Profilbild** hochladen
- **Name** und **Bio** ausfüllen
- **Hochschule** als Organisation oder in der Bio angeben

### Git mit GitHub verbinden

Stelle sicher, dass dein lokales Git die gleiche E-Mail-Adresse verwendet wie dein GitHub-Account:

```shell
git config --global user.name "Dein Name"
git config --global user.email "deine.email@stud.fhgr.ch"
```

Prüfe die aktuelle Konfiguration:

```shell
git config --global --list
```

---

## SSH-Keys einrichten

SSH-Keys ermöglichen eine sichere, passwortlose Verbindung zwischen deinem Rechner und GitHub. Einmal eingerichtet, musst du dich nie wieder manuell authentifizieren.

### Schritt 1 — Prüfen, ob bereits ein SSH-Key existiert

```shell
ls -la ~/.ssh
```

Falls Dateien wie `id_ed25519` und `id_ed25519.pub` vorhanden sind, hast du bereits einen Key. Du kannst direkt zu Schritt 3 springen.

### Schritt 2 — Neuen SSH-Key generieren

```shell
ssh-keygen -t ed25519 -C "deine.email@stud.fhgr.ch"
```

- Bestätige den Speicherort mit **Enter** (Standard: `~/.ssh/id_ed25519`).
- Vergib optional eine Passphrase für zusätzliche Sicherheit (oder lasse sie leer mit **Enter**).

### Schritt 3 — SSH-Key zum SSH-Agent hinzufügen

Starte den SSH-Agent:

```shell
eval "$(ssh-agent -s)"
```

Füge deinen privaten Key hinzu:

```shell
ssh-add ~/.ssh/id_ed25519
```

Auf macOS kannst du den Key auch im Schlüsselbund speichern, damit er nach einem Neustart erhalten bleibt:

```shell
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

### Schritt 4 — Öffentlichen Key auf GitHub hinterlegen

Kopiere den öffentlichen Key in die Zwischenablage:

```shell
pbcopy < ~/.ssh/id_ed25519.pub
```

Dann auf GitHub:

1. Gehe zu **Settings** → **SSH and GPG keys** → **New SSH key**.
2. Vergib einen **Titel** (z. B. „MacBook Pro FH").
3. Füge den Key ein und klicke auf **Add SSH key**.

### Schritt 5 — Verbindung testen

```shell
ssh -T git@github.com
```

Bei Erfolg erscheint eine Meldung wie:

```
Hi dein-username! You've successfully authenticated, but GitHub does not provide shell access.
```

> **Wichtig:** Verwende ab jetzt immer die **SSH-URL** beim Klonen von Repositories (beginnt mit `git@github.com:...`), nicht die HTTPS-URL.

---

## Repository erstellen & klonen

### Neues Repository auf GitHub erstellen

1. Klicke oben rechts auf **+** → **New repository**.
2. Vergib einen Namen (z. B. `mein-projekt`).
3. Wähle **Public** oder **Private**.
4. Aktiviere **Add a README file** — das erstellt automatisch einen initialen Commit.
5. Wähle eine passende **.gitignore**-Vorlage (z. B. `Node` für Node.js-Projekte).
6. Wähle eine **Lizenz** (für Open-Source-Projekte z. B. `MIT`).
7. Klicke auf **Create repository**.

### Repository klonen

Kopiere die SSH-URL deines Repositories und klone es:

```shell
git clone git@github.com:dein-username/mein-projekt.git
```

Wechsle in das Verzeichnis:

```shell
cd mein-projekt
```

### Lokales Projekt mit GitHub verbinden

Falls du bereits ein lokales Projekt hast und es auf GitHub pushen willst:

```shell
# Im Projektverzeichnis
git init
git add .
git commit -m "Initial commit"
git remote add origin git@github.com:dein-username/mein-projekt.git
git branch -M main
git push -u origin main
```

### Änderungen pushen und pullen

Nachdem du lokal Commits erstellt hast:

```shell
# Änderungen auf GitHub hochladen
git push

# Änderungen von GitHub herunterladen
git pull
```

---

## Pull Requests

Pull Requests (PRs) sind das Herzstück der Zusammenarbeit auf GitHub. Sie ermöglichen es, Änderungen vorzuschlagen, zu diskutieren und zu reviewen, bevor sie in den Hauptbranch gemergt werden.

### Pull Request erstellen

**1. Erstelle einen neuen Branch und arbeite darin:**

```shell
git checkout -b feature/neue-navigation
```

**2. Mache deine Änderungen und committe sie:**

```shell
git add .
git commit -m "Add responsive navigation component"
git push -u origin feature/neue-navigation
```

**3. Erstelle den Pull Request auf GitHub:**

- Gehe auf die Repository-Seite — GitHub zeigt dir automatisch einen Banner mit **Compare & pull request**.
- Alternativ: Klicke auf **Pull requests** → **New pull request**.
- Wähle den **Base-Branch** (z. B. `main`) und den **Compare-Branch** (dein Feature-Branch).
- Schreibe einen aussagekräftigen **Titel** und eine **Beschreibung**.
- Weise optional **Reviewer**, **Labels** und ein **Project** zu.
- Klicke auf **Create pull request**.

> **Tipp:** Du kannst einen PR auch über die GitHub CLI erstellen:
> ```shell
> gh pr create --title "Add responsive navigation" --body "Beschreibung der Änderungen"
> ```

### Pull Request reviewen

Als Reviewer gehst du wie folgt vor:

1. Öffne den Pull Request und klicke auf den Tab **Files changed**.
2. Gehe die Änderungen Zeile für Zeile durch.
3. Klicke auf das **+**-Symbol neben einer Zeile, um einen Kommentar zu hinterlassen.
4. Wähle am Ende zwischen:
   - **Comment** — allgemeiner Kommentar ohne Entscheidung
   - **Approve** — du stimmst den Änderungen zu
   - **Request changes** — Änderungen sind nötig, bevor gemergt wird

### Pull Request mergen

Sobald der PR approved ist und alle Checks bestanden hat:

1. Klicke auf **Merge pull request**.
2. Wähle eine Merge-Strategie:
   - **Create a merge commit** — behält die vollständige History
   - **Squash and merge** — fasst alle Commits in einen zusammen (empfohlen für Feature-Branches)
   - **Rebase and merge** — setzt die Commits linear auf den Base-Branch
3. Lösche den Feature-Branch nach dem Merge (GitHub bietet das automatisch an).

Lokalen Branch aufräumen:

```shell
git checkout main
git pull
git branch -d feature/neue-navigation
```

---

## Issues & Projects

### Issues

Issues sind das Ticket-System von GitHub. Du kannst sie nutzen für:

- **Bugs melden** — „Login-Button funktioniert nicht auf Mobile"
- **Features vorschlagen** — „Dark Mode implementieren"
- **Aufgaben tracken** — „Unit Tests für API schreiben"

**Issue erstellen:**

1. Gehe im Repository auf **Issues** → **New issue**.
2. Vergib einen klaren **Titel**.
3. Beschreibe das Problem oder die Aufgabe in der **Beschreibung**.
4. Nutze **Labels** (z. B. `bug`, `enhancement`, `documentation`).
5. Weise das Issue einem **Assignee** zu.

**Issues mit Commits und PRs verknüpfen:**

Du kannst Issues automatisch schliessen, indem du in deiner Commit-Message oder PR-Beschreibung bestimmte Schlüsselwörter verwendest:

```
git commit -m "Fix mobile login button layout (closes #12)"
```

Schlüsselwörter: `closes`, `fixes`, `resolves` (jeweils gefolgt von `#Issue-Nummer`).

### Projects

GitHub Projects ist ein Kanban-Board, das direkt in GitHub integriert ist. Es eignet sich hervorragend für die Projektplanung im Team.

**Project erstellen:**

1. Gehe auf **Projects** → **New project**.
2. Wähle eine Vorlage (z. B. **Board** für Kanban oder **Table** für Tabellenansicht).
3. Füge Spalten hinzu wie `To Do`, `In Progress`, `In Review`, `Done`.
4. Verknüpfe Issues und Pull Requests mit dem Board.

> **Tipp:** In Gruppenprojekten an der Hochschule ist ein GitHub Project Board eine einfache und effektive Art, die Arbeit im Team zu organisieren.

---

## GitHub Pages

Mit GitHub Pages kannst du kostenlos statische Websites direkt aus einem Repository hosten — perfekt für Portfolios, Dokumentationen oder Projektpräsentationen.

### GitHub Pages aktivieren

1. Gehe in deinem Repository zu **Settings** → **Pages**.
2. Wähle unter **Source** den Branch (z. B. `main`) und den Ordner (`/ (root)` oder `/docs`).
3. Klicke auf **Save**.
4. Nach wenigen Minuten ist deine Website unter `https://dein-username.github.io/repository-name/` erreichbar.

### Einfache Website erstellen

Erstelle eine `index.html` im Root deines Repositories:

```html
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mein Portfolio</title>
</head>
<body>
    <h1>Willkommen auf meiner Website</h1>
    <p>Diese Seite wird mit GitHub Pages gehostet.</p>
</body>
</html>
```

Committe und pushe die Datei — die Website wird automatisch aktualisiert.

### Persönliche Website

Für eine persönliche Website unter `https://dein-username.github.io/` erstelle ein Repository mit dem Namen `dein-username.github.io`. Die `index.html` im Root wird automatisch als Startseite verwendet.

> **Tipp:** GitHub Pages unterstützt auch [Jekyll](https://jekyllrb.com/), einen statischen Site-Generator. Damit kannst du mit Markdown-Dateien eine komplette Website bauen, ohne HTML schreiben zu müssen.

---

## GitHub Student Developer Pack

Als Studentin oder Student bekommst du über das **GitHub Student Developer Pack** kostenlosen Zugang zu professionellen Entwicklertools.

### Zugang beantragen

1. Gehe auf [education.github.com/pack](https://education.github.com/pack).
2. Klicke auf **Get your pack**.
3. Verifiziere deinen Studierendenstatus mit deiner Hochschul-E-Mail oder einem Studierendenausweis.
4. Die Freischaltung dauert in der Regel wenige Minuten bis Tage.

### Ausgewählte Vorteile

| Tool | Vorteil |
|---|---|
| **GitHub Pro** | Unbegrenzte private Repos, erweiterte Features |
| **GitHub Copilot** | KI-gestützte Code-Vervollständigung (kostenlos) |
| **JetBrains IDEs** | Alle IDEs kostenlos (IntelliJ, WebStorm, etc.) |
| **Namecheap** | Kostenlose `.me`-Domain für ein Jahr |
| **DigitalOcean** | 200 $ Cloud-Credits |
| **Notion** | Pro-Plan kostenlos |
| **Figma** | Education-Plan kostenlos |

> **Wichtig:** Beantrage das Student Developer Pack möglichst früh im Studium — du profitierst so am längsten davon.

---

## Best Practices

### Gute Commit-Messages

Commit-Messages sollten klar beschreiben, **was** und **warum** etwas geändert wurde. Verwende das folgende Format:

```
<typ>: <kurze Beschreibung>

[optionaler Body mit Details]
```

**Gute Beispiele:**

```
feat: add user authentication with JWT
fix: resolve crash on empty search input
docs: update API documentation for v2 endpoints
style: format code according to ESLint rules
refactor: extract validation logic into separate module
test: add unit tests for payment service
```

**Schlechte Beispiele:**

```
update
fix bug
asdf
WIP
changes
```

**Gängige Typen (Conventional Commits):**

| Typ | Bedeutung |
|---|---|
| `feat` | Neues Feature |
| `fix` | Bugfix |
| `docs` | Dokumentation |
| `style` | Formatierung (kein Code-Change) |
| `refactor` | Code-Umstrukturierung |
| `test` | Tests hinzufügen oder ändern |
| `chore` | Build-Prozess, Dependencies |

### Branch-Naming

Verwende ein konsistentes Schema für Branch-Namen:

```
<typ>/<kurze-beschreibung>
```

**Beispiele:**

```shell
feature/user-login
feature/dark-mode
fix/mobile-navigation
fix/api-timeout
docs/readme-update
chore/update-dependencies
```

**Regeln:**

- Nur Kleinbuchstaben verwenden
- Wörter mit Bindestrichen `-` trennen
- Kurz und beschreibend halten
- Keine Sonderzeichen oder Leerzeichen

### Weitere Best Practices

- **Regelmässig committen** — kleine, logische Einheiten statt riesiger Commits.
- **Branches nutzen** — arbeite nie direkt auf `main`. Erstelle immer einen Feature-Branch.
- **Pull Requests für alles** — auch in Einzelprojekten helfen PRs, Änderungen zu dokumentieren.
- **`.gitignore` pflegen** — halte `node_modules/`, `.env`, Build-Artefakte und IDE-Konfigurationen aus dem Repository.
- **README schreiben** — jedes Projekt verdient eine gute README mit Setup-Anleitung und Beschreibung.
- **Issues nutzen** — auch bei kleinen Projekten helfen Issues, den Überblick zu behalten.

---

<div align="center">
  <sub>Fachhochschule Graubünden — Major Media Applications</sub>
</div>
