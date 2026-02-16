[← Zurück zur Übersicht](../README.md)

# Git -- Versionskontrolle von Anfang an

Git ist das mit Abstand wichtigste Werkzeug, das du im Studium (und danach im Beruf) fuer die Zusammenarbeit an Code verwenden wirst. Diese Anleitung bringt dich von der Installation bis zum sicheren Umgang mit Branches und Merge-Konflikten.

---

## Inhaltsverzeichnis

1. [Was ist Git?](#was-ist-git)
2. [Installation](#installation)
3. [Erste Konfiguration](#erste-konfiguration)
4. [Grundlegende Befehle](#grundlegende-befehle)
5. [Branching & Merging](#branching--merging)
6. [.gitignore erklaert](#gitignore-erklaert)
7. [Haeufige Probleme & Loesungen](#haeufige-probleme--loesungen)
8. [Nuetzliche Aliases](#nuetzliche-aliases)
9. [Weiterfuehrende Links](#weiterfuehrende-links)

---

## Was ist Git?

**Git** ist ein *verteiltes Versionskontrollsystem* (engl. _Distributed Version Control System_, DVCS). Es wurde 2005 von Linus Torvalds -- dem Erfinder von Linux -- entwickelt.

### Was bedeutet Versionskontrolle?

Stell dir vor, du schreibst eine Hausarbeit und speicherst staendig neue Kopien:

```
hausarbeit_v1.docx
hausarbeit_v2_final.docx
hausarbeit_v2_final_FINAL.docx
hausarbeit_v2_final_FINAL_korrigiert.docx
```

Versionskontrolle loest genau dieses Problem. Git merkt sich **jede Aenderung** an deinen Dateien -- wer was wann geaendert hat -- und ermoeglicht es dir, jederzeit zu einem frueheren Stand zurueckzukehren.

### Warum Git?

| Vorteil | Erklaerung |
|---|---|
| **Verteiltes System** | Jeder hat eine vollstaendige Kopie der gesamten Historie -- du kannst auch offline arbeiten. |
| **Branching** | Du kannst parallel an verschiedenen Features arbeiten, ohne den Hauptcode zu gefaehrden. |
| **Zusammenarbeit** | Mehrere Personen koennen gleichzeitig am selben Projekt arbeiten. |
| **Sicherheit** | Jede Aenderung wird mit einem kryptografischen Hash (SHA-1) gesichert. |
| **Industriestandard** | Git wird von praktisch jedem Softwareunternehmen weltweit eingesetzt. |

### Wichtige Begriffe

| Begriff | Bedeutung |
|---|---|
| **Repository (Repo)** | Ein Projektordner, der von Git verwaltet wird. |
| **Commit** | Ein Snapshot deines Projekts zu einem bestimmten Zeitpunkt. |
| **Branch** | Ein unabhaengiger Entwicklungszweig. |
| **Remote** | Eine entfernte Kopie deines Repos (z. B. auf GitHub). |
| **Staging Area (Index)** | Ein Zwischenbereich, in dem du Aenderungen fuer den naechsten Commit vorbereitest. |
| **Working Directory** | Dein lokaler Arbeitsordner mit den aktuellen Dateien. |

---

## Installation

### macOS

Auf macOS hast du zwei gaengige Wege:

#### Option 1: Xcode Command Line Tools (empfohlen)

Die einfachste Methode -- Git wird zusammen mit anderen Entwicklerwerkzeugen installiert:

```shell
xcode-select --install
```

Es oeffnet sich ein Dialog. Klicke auf **Installieren** und warte, bis der Vorgang abgeschlossen ist.

Pruefe anschliessend die Installation:

```shell
git --version
# Ausgabe z. B.: git version 2.44.0
```

#### Option 2: Homebrew

Falls du [Homebrew](https://brew.sh) bereits nutzt (oder es installieren moechtest), kannst du Git auch darueber beziehen. Homebrew liefert oft eine aktuellere Version:

```shell
# Homebrew installieren (falls noch nicht vorhanden)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Git installieren
brew install git
```

> **Tipp:** Nach der Installation ueber Homebrew musst du eventuell ein neues Terminalfenster oeffnen, damit der Pfad aktualisiert wird. Pruefe mit `which git`, ob `/opt/homebrew/bin/git` (Apple Silicon) bzw. `/usr/local/bin/git` (Intel) verwendet wird.

---

## Erste Konfiguration

Bevor du loslegst, solltest du Git mitteilen, wer du bist. Diese Informationen werden in jedem Commit gespeichert.

### Name und E-Mail setzen

```shell
git config --global user.name "Dein Name"
git config --global user.email "deine.email@uni-beispiel.de"
```

> **Hinweis:** Verwende die gleiche E-Mail-Adresse wie bei deinem GitHub-/GitLab-Konto, damit deine Commits richtig zugeordnet werden.

### Standard-Branch auf `main` setzen

Aeltere Git-Versionen verwenden `master` als Standardnamen fuer den Hauptbranch. Der aktuelle Konventionsstandard ist `main`:

```shell
git config --global init.defaultBranch main
```

### Standard-Editor festlegen

Git oeffnet manchmal einen Texteditor (z. B. fuer Commit-Nachrichten). Du kannst deinen bevorzugten Editor setzen:

```shell
# Visual Studio Code
git config --global core.editor "code --wait"

# Nano (einfacher Terminal-Editor)
git config --global core.editor "nano"

# Vim (fuer Fortgeschrittene)
git config --global core.editor "vim"
```

### Konfiguration ueberpruefen

```shell
git config --list
```

Oder einzelne Werte abfragen:

```shell
git config user.name
git config user.email
```

Die globale Konfiguration wird in der Datei `~/.gitconfig` gespeichert. Du kannst sie auch direkt bearbeiten:

```shell
cat ~/.gitconfig
```

---

## Grundlegende Befehle

### Uebersicht

| Befehl | Beschreibung |
|---|---|
| `git init` | Neues Repository im aktuellen Ordner erstellen |
| `git clone <url>` | Bestehendes Repository herunterladen |
| `git status` | Aktuellen Zustand des Repos anzeigen |
| `git add <datei>` | Aenderungen zur Staging Area hinzufuegen |
| `git commit -m "..."` | Gestagete Aenderungen als Commit speichern |
| `git push` | Lokale Commits zum Remote-Server hochladen |
| `git pull` | Aenderungen vom Remote-Server herunterladen und einarbeiten |
| `git log` | Commit-Historie anzeigen |

### `git init` -- Ein neues Repository anlegen

```shell
mkdir mein-projekt
cd mein-projekt
git init
```

Git erstellt im Ordner einen versteckten Unterordner `.git/`, in dem die gesamte Versionshistorie gespeichert wird.

```
Initialized empty Git repository in /Users/du/mein-projekt/.git/
```

> **Wichtig:** Loesche den `.git/`-Ordner niemals manuell, sonst geht die gesamte Historie verloren.

### `git clone` -- Ein bestehendes Repository kopieren

```shell
# Ueber HTTPS
git clone https://github.com/benutzername/repo-name.git

# Ueber SSH (empfohlen, wenn SSH-Key eingerichtet)
git clone git@github.com:benutzername/repo-name.git

# In einen bestimmten Ordner klonen
git clone https://github.com/benutzername/repo-name.git mein-ordner
```

### `git status` -- Was hat sich geaendert?

```shell
git status
```

Beispielausgabe:

```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)

        modified:   index.html

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        style.css

no changes added to commit (use "git add" and/or "git commit -a")
```

Git unterscheidet drei Zustaende:

```
 Working Directory  --->  Staging Area  --->  Repository
     (bearbeitet)      (git add)          (git commit)
```

### `git add` -- Aenderungen vormerken

```shell
# Einzelne Datei hinzufuegen
git add index.html

# Mehrere Dateien hinzufuegen
git add index.html style.css

# Alle geaenderten und neuen Dateien im aktuellen Verzeichnis hinzufuegen
git add .

# Interaktiv einzelne Teile einer Datei hinzufuegen (fuer Fortgeschrittene)
git add -p
```

> **Tipp:** Verwende `git add .` mit Bedacht. Es fuegt wirklich *alles* hinzu -- auch Dateien, die du vielleicht nicht committen moechtest. Nutze daher immer eine passende `.gitignore`-Datei (siehe unten).

### `git commit` -- Aenderungen festschreiben

```shell
# Commit mit Nachricht
git commit -m "Startseite erstellt und CSS hinzugefuegt"

# Commit mit ausfuehrlicher Nachricht (oeffnet Editor)
git commit

# Kurzform: add + commit fuer bereits getrackte Dateien
git commit -am "Tippfehler in der Navigation korrigiert"
```

#### Gute Commit-Nachrichten schreiben

Eine gute Commit-Nachricht ist kurz, praezise und beschreibt **was** geaendert wurde und **warum**:

```
# Gut:
git commit -m "Fix: Navigationslinks auf Mobilgeraeten korrigiert"
git commit -m "Feature: Suchfunktion auf der Startseite hinzugefuegt"
git commit -m "Refactor: CSS-Klassen nach BEM-Konvention umbenannt"

# Schlecht:
git commit -m "fix"
git commit -m "aenderungen"
git commit -m "asdf"
```

**Konvention fuer die Betreffzeile:**

- Maximal ca. 50 Zeichen
- Imperativ verwenden ("Fuege hinzu", nicht "Hinzugefuegt")
- Kein Punkt am Ende

### `git push` -- Aenderungen hochladen

```shell
# Zum Remote-Repository pushen
git push

# Beim ersten Push einen Upstream-Branch setzen
git push -u origin main

# Einen bestimmten Branch pushen
git push origin feature/login
```

> **Hinweis:** Bevor du pushen kannst, brauchst du ein Remote-Repository (z. B. auf GitHub oder GitLab). Beim Klonen wird das automatisch eingerichtet. Bei `git init` musst du es manuell hinzufuegen:
>
> ```shell
> git remote add origin https://github.com/benutzername/repo-name.git
> ```

### `git pull` -- Aenderungen herunterladen

```shell
# Aenderungen vom Remote-Branch holen und einarbeiten
git pull

# Entspricht:
git fetch + git merge
```

`git pull` holt die neuesten Aenderungen vom Server und fuehrt sie mit deinem lokalen Stand zusammen. Wenn du und jemand anders die gleiche Stelle bearbeitet haben, kann es zu einem **Merge-Konflikt** kommen (siehe Abschnitt weiter unten).

### `git log` -- Historie anschauen

```shell
# Vollstaendige Historie
git log

# Kompakte Einzeilenansicht
git log --oneline

# Mit grafischer Darstellung der Branches
git log --oneline --graph --all

# Die letzten 5 Commits
git log -5

# Commits eines bestimmten Autors
git log --author="Dein Name"

# Commits, die eine bestimmte Datei betreffen
git log -- index.html
```

Beispielausgabe von `git log --oneline --graph --all`:

```
* e4a1c3f (HEAD -> main) Readme aktualisiert
| * 7b2d9a1 (feature/login) Login-Formular erstellt
|/
* 3f8c2e5 Projektstruktur angelegt
* 1a0b4d2 Initial commit
```

---

## Branching & Merging

Branches sind eines der maechtigsten Features von Git. Sie erlauben es dir, isoliert an neuen Features oder Bugfixes zu arbeiten, ohne den Hauptbranch zu beeinflussen.

### Konzept

```
main:       A --- B --- C ----------- F (merge)
                         \           /
feature:                  D --- E ---
```

Du zweigst vom Hauptbranch ab, arbeitest in Ruhe an deinem Feature und fuehrst die Aenderungen anschliessend wieder zusammen.

### Branches anzeigen

```shell
# Lokale Branches auflisten
git branch

# Alle Branches (lokal + remote)
git branch -a

# Branch anzeigen, auf dem du dich befindest
git branch --show-current
```

### Branch erstellen

```shell
# Neuen Branch erstellen
git branch feature/login

# Neuen Branch erstellen UND direkt wechseln
git switch -c feature/login
# oder (aeltere Syntax):
git checkout -b feature/login
```

> **Namenskonvention:** Verwende aussagekraeftige Branch-Namen mit Praefix:
> - `feature/beschreibung` -- fuer neue Features
> - `fix/beschreibung` -- fuer Bugfixes
> - `refactor/beschreibung` -- fuer Umstrukturierungen

### Branch wechseln

```shell
# Moderne Syntax (Git >= 2.23)
git switch main
git switch feature/login

# Klassische Syntax
git checkout main
git checkout feature/login
```

> **Tipp:** `git switch` wurde eingefuehrt, um die vielen Funktionen von `git checkout` klarer aufzuteilen. Fuer das Wechseln von Branches ist `switch` die empfohlene Variante.

### Branches zusammenfuehren (Merge)

Wenn dein Feature fertig ist, fuehrst du den Branch zurueck in `main`:

```shell
# 1. Zuerst zum Zielbranch wechseln
git switch main

# 2. Den Feature-Branch in main mergen
git merge feature/login
```

Es gibt zwei Arten von Merges:

| Merge-Typ | Wann? | Was passiert? |
|---|---|---|
| **Fast-Forward** | Keine neuen Commits auf `main` seit dem Abzweigen | Der `main`-Zeiger wird einfach vorgeschoben. Kein extra Merge-Commit. |
| **3-Way-Merge** | Es gab neue Commits auf beiden Branches | Git erstellt einen Merge-Commit, der beide Historien vereint. |

### Branch loeschen

```shell
# Lokalen Branch loeschen (nur wenn bereits gemergt)
git branch -d feature/login

# Lokalen Branch erzwungen loeschen (auch wenn nicht gemergt)
git branch -D feature/login

# Remote-Branch loeschen
git push origin --delete feature/login
```

---

## .gitignore erklaert

Die Datei `.gitignore` teilt Git mit, welche Dateien und Ordner **nicht** versioniert werden sollen. Das ist wichtig fuer:

- Kompilierte Dateien und Build-Artefakte
- Abhaengigkeiten (z. B. `node_modules/`)
- Persoenliche Konfiguration und IDE-Einstellungen
- Sensible Daten (Passwoerter, API-Keys, `.env`-Dateien)
- Betriebssystemdateien (z. B. `.DS_Store` auf macOS)

### Grundlegende Syntax

```gitignore
# Kommentare beginnen mit #

# Einzelne Datei ignorieren
geheim.txt

# Alle Dateien mit bestimmter Endung ignorieren
*.log
*.tmp
*.class

# Ganzen Ordner ignorieren
node_modules/
build/
dist/

# Alle Dateien in einem Ordner ignorieren, aber den Ordner behalten
logs/*
!logs/.gitkeep

# Verschachtelte Muster
**/test/*.out

# Negation: Datei NICHT ignorieren (Ausnahme)
!wichtige-datei.log
```

### Beispiel fuer ein typisches Webprojekt

```gitignore
# Abhaengigkeiten
node_modules/
vendor/

# Build-Artefakte
dist/
build/
*.min.js
*.min.css

# Umgebungsvariablen und Secrets
.env
.env.local
.env.production

# IDE-Einstellungen
.vscode/
.idea/
*.swp
*.swo
*~

# Betriebssystem-Dateien
.DS_Store
Thumbs.db

# Logs
*.log
npm-debug.log*

# Testabdeckung
coverage/
```

### Beispiel fuer ein Java-/Kotlin-Projekt

```gitignore
# Kompilierte Dateien
*.class
*.jar
*.war

# Build-Ordner
build/
target/
out/

# IDE
.idea/
*.iml
.eclipse/

# Gradle / Maven
.gradle/
!gradle-wrapper.jar
```

### Bereits getrackte Dateien nachtraeglich ignorieren

Wenn du eine Datei bereits committet hast und sie jetzt ignorieren moechtest:

```shell
# Datei aus dem Git-Index entfernen (bleibt aber lokal erhalten)
git rm --cached geheim.txt

# Dann den neuen .gitignore-Eintrag committen
git add .gitignore
git commit -m "geheim.txt aus Versionskontrolle entfernt"
```

> **Tipp:** Auf [github.com/github/gitignore](https://github.com/github/gitignore) findest du fertige `.gitignore`-Vorlagen fuer viele Sprachen und Frameworks.

---

## Haeufige Probleme & Loesungen

### Merge-Konflikte

Ein Merge-Konflikt entsteht, wenn zwei Branches die **gleiche Stelle** in einer Datei unterschiedlich veraendert haben.

So sieht ein Konflikt in der Datei aus:

```
<<<<<<< HEAD
<h1>Willkommen auf unserer Webseite</h1>
=======
<h1>Startseite - Mein Projekt</h1>
>>>>>>> feature/neue-startseite
```

**Loesung:**

1. Oeffne die Datei und entscheide, welche Version du behalten willst (oder kombiniere beide).
2. Entferne die Konfliktmarkierungen (`<<<<<<<`, `=======`, `>>>>>>>`).
3. Speichere die Datei.
4. Fuege die geloeste Datei hinzu und committe:

```shell
git add index.html
git commit -m "Merge-Konflikt in index.html geloest"
```

> **Tipp:** Visual Studio Code hat ein hervorragendes eingebautes Tool zum Loesen von Merge-Konflikten. Es zeigt dir die Unterschiede farblich an und bietet Buttons wie "Accept Current Change", "Accept Incoming Change" und "Accept Both Changes".

### Letzten Commit rueckgaengig machen

```shell
# Commit rueckgaengig machen, Aenderungen behalten (in Staging Area)
git reset --soft HEAD~1

# Commit rueckgaengig machen, Aenderungen behalten (im Working Directory)
git reset --mixed HEAD~1

# Commit UND Aenderungen komplett verwerfen (VORSICHT!)
git reset --hard HEAD~1
```

> **Warnung:** `git reset --hard` loescht Aenderungen unwiderruflich. Verwende es nur, wenn du dir absolut sicher bist.

### Aenderungen an einer Datei verwerfen

```shell
# Einzelne Datei auf den letzten Commit-Stand zuruecksetzen
git checkout -- index.html
# oder (moderne Syntax):
git restore index.html

# Alle lokalen Aenderungen verwerfen
git restore .
```

### Aenderungen zwischenspeichern (Stash)

Wenn du den Branch wechseln musst, aber noch unfertige Aenderungen hast:

```shell
# Aenderungen zwischenspeichern
git stash

# Zwischengespeicherte Aenderungen wieder anwenden
git stash pop

# Liste aller Stashes anzeigen
git stash list

# Bestimmten Stash anwenden
git stash apply stash@{0}
```

### Falsche Datei committet

```shell
# Datei aus dem letzten Commit entfernen, aber lokal behalten
git reset HEAD~1 -- geheim.txt
git commit --amend
```

### Push wird abgelehnt (Remote hat neuere Commits)

```
! [rejected]        main -> main (non-fast-forward)
```

**Loesung:**

```shell
# Zuerst die Remote-Aenderungen holen und einarbeiten
git pull --rebase origin main

# Dann erneut pushen
git push
```

### Detached HEAD -- Was nun?

Wenn du einen bestimmten Commit ausgecheckt hast, bist du im "Detached HEAD"-Zustand:

```shell
# Du siehst: "HEAD detached at abc1234"

# Zurueck zum letzten Branch:
git switch main

# Oder: Einen neuen Branch aus dem aktuellen Zustand erstellen
git switch -c mein-neuer-branch
```

---

## Nuetzliche Aliases

Git-Aliases sind Abkuerzungen fuer haeufig genutzte Befehle. Du richtest sie einmalig ein und sparst danach viel Tipparbeit.

### Aliases einrichten

```shell
# Kompakte Log-Ansicht
git config --global alias.lg "log --oneline --graph --all --decorate"

# Status-Kurzform
git config --global alias.st "status -sb"

# Alle Aenderungen stagen und committen
git config --global alias.ac "!git add -A && git commit"

# Letzten Commit anzeigen
git config --global alias.last "log -1 HEAD --stat"

# Alle Branches mit letztem Commit anzeigen
git config --global alias.branches "branch -a -v"

# Unstage: Datei aus Staging Area entfernen
git config --global alias.unstage "restore --staged"

# Aenderungen im Vergleich zum letzten Commit
git config --global alias.diff-last "diff HEAD~1 HEAD"
```

### Aliases verwenden

```shell
git lg          # statt: git log --oneline --graph --all --decorate
git st          # statt: git status -sb
git last        # Zeigt den letzten Commit
git unstage datei.txt  # Nimmt datei.txt aus der Staging Area
```

### Alle konfigurierten Aliases anzeigen

```shell
git config --get-regexp alias
```

---

## Weiterfuehrende Links

### Offizielle Dokumentation

- [Git Dokumentation (offiziell)](https://git-scm.com/doc) -- Vollstaendige Referenz aller Befehle
- [Pro Git Buch (kostenlos, online)](https://git-scm.com/book/de/v2) -- Umfassendes Buch, auch auf Deutsch verfuegbar

### Interaktive Lernressourcen

- [Learn Git Branching](https://learngitbranching.js.org/?locale=de_DE) -- Interaktives Tutorial mit Visualisierungen (auf Deutsch)
- [GitHub Skills](https://skills.github.com/) -- Praxisnahe Kurse direkt auf GitHub
- [Oh My Git!](https://ohmygit.org/) -- Ein Spiel zum Git-Lernen

### Spickzettel

- [Git Cheat Sheet (GitHub)](https://education.github.com/git-cheat-sheet-education.pdf) -- Kompakte Uebersicht zum Ausdrucken
- [Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials) -- Gut strukturierte Erklaerungen mit Grafiken

### Tools

- [GitHub Desktop](https://desktop.github.com/) -- Grafische Oberflaeche fuer Git (gut fuer Einsteiger)
- [GitKraken](https://www.gitkraken.com/) -- Maechtige grafische Git-Oberflaeche
- [VS Code Git-Integration](https://code.visualstudio.com/docs/sourcecontrol/overview) -- Git direkt in Visual Studio Code nutzen

---

> **Letzter Tipp:** Git lernt man am besten durch regelmaessiges Anwenden. Erstelle ein kleines Uebungsprojekt, probiere alle Befehle aus und hab keine Angst vor Fehlern -- dafuer ist Versionskontrolle schliesslich da!
