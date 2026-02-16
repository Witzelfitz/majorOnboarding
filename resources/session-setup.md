[← Zurück zur Übersicht](../README.md)

# Session 1 — Setup & Werkzeuge einrichten

In dieser Session richten wir gemeinsam die gesamte Entwicklungsumgebung ein. Am Ende hat jede:r ein funktionierendes Terminal, Git, Node.js, npm und Cursor auf dem Rechner — und wir testen alles mit einem gemeinsamen Smoke-Test.

---

## Checkliste

Hake jeden Punkt ab, sobald er bei dir funktioniert:

- [ ] **Terminal** läuft (Ghostty / PowerShell / Windows Terminal)
- [ ] **Homebrew** ist installiert *(nur macOS)*
- [ ] **Git** ist installiert und konfiguriert (`git --version` ✓)
- [ ] **Node.js & npm** funktionieren (`node --version` und `npm --version` ✓)
- [ ] **Cursor** ist installiert und öffnet Projekte
- [ ] **Smoke-Test** erfolgreich abgeschlossen

---

## Ablauf

### 1. Terminal einrichten (~15 Min.)

Das Terminal ist die Grundlage für alles Weitere. Ohne funktionierendes Terminal geht nichts.

#### macOS — Ghostty (empfohlen)

[Ghostty](https://ghostty.org/) ist ein modernes, GPU-beschleunigtes Terminal mit hervorragender Performance und Schriftdarstellung.

**Installation via Homebrew** (falls Homebrew bereits vorhanden):

```shell
brew install --cask ghostty
```

**Manueller Download:**

1. Gehe auf [ghostty.org](https://ghostty.org/)
2. Lade die macOS-Version herunter
3. Verschiebe `Ghostty.app` in den `Programme`-Ordner
4. Beim ersten Öffnen: Rechtsklick → Öffnen (wegen macOS Gatekeeper)

> **Alternativ:** Falls Ghostty Probleme macht, ist das vorinstallierte macOS Terminal oder [iTerm2](https://iterm2.com/) (`brew install --cask iterm2`) ein guter Fallback. Siehe [Terminal-Guide](terminal.md).

#### Windows — PowerShell / Windows Terminal

Windows-Nutzer verwenden **PowerShell** (vorinstalliert) oder das **Windows Terminal** aus dem Microsoft Store.

**Windows Terminal installieren:**

1. Öffne den **Microsoft Store**
2. Suche nach **"Windows Terminal"**
3. Installieren und als Standard-Terminal festlegen

**PowerShell prüfen:**

```powershell
$PSVersionTable.PSVersion
# Erwartete Ausgabe: Version 5.1 oder höher
```

> **Tipp:** Windows Terminal unterstützt Tabs, Split Panes und verschiedene Shells (PowerShell, CMD, WSL) in einem Fenster.

#### Erster Test

Öffne dein Terminal und teste:

```shell
pwd          # Aktuelles Verzeichnis anzeigen (macOS/Linux)
# bzw.
Get-Location # PowerShell-Äquivalent (Windows)
```

Wenn du eine Pfadangabe siehst, funktioniert dein Terminal.

**✅ Checkpoint:** Terminal ist offen und reagiert auf Befehle.

---

### 2. Homebrew installieren (~10 Min., nur macOS)

Homebrew ist der Paketmanager für macOS und erleichtert die Installation aller weiteren Tools.

> **Windows-Nutzer:** Überspringt diesen Schritt. Unter Windows installiert ihr Tools direkt über Installer oder den Paketmanager `winget` (in Windows 11 vorinstalliert).

#### Xcode Command Line Tools (Voraussetzung)

```shell
xcode-select --install
```

Es öffnet sich ein Dialog — klicke auf **Installieren** und warte.

#### Homebrew installieren

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

**Wichtig — PATH einrichten (Apple Silicon Macs):**

Das Installationsskript zeigt am Ende Befehle an, die du ausführen musst. Typischerweise:

```shell
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

#### Prüfen

```shell
brew --version
# Erwartete Ausgabe z.B.: Homebrew 4.x.x
```

> Ausführlicher Guide: [Homebrew](homebrew.md)

**✅ Checkpoint:** `brew --version` zeigt eine Versionsnummer.

---

### 3. Git einrichten (~15 Min.)

Git ist das Versionskontrollsystem, das wir im gesamten Studium verwenden.

#### Installation prüfen / durchführen

**macOS:**

```shell
git --version
```

Falls Git noch nicht installiert ist:

```shell
# Option A: Über Xcode Command Line Tools (wahrscheinlich schon erledigt)
xcode-select --install

# Option B: Über Homebrew
brew install git
```

**Windows:**

1. Lade [Git for Windows](https://git-scm.com/download/win) herunter
2. Führe den Installer aus (Standardeinstellungen sind in Ordnung)
3. **Wichtig:** Wähle im Installer "Git from the command line and also from 3rd-party software"
4. Starte dein Terminal neu

```powershell
git --version
# Erwartete Ausgabe z.B.: git version 2.44.0
```

#### Konfiguration (alle Betriebssysteme)

Jede:r muss Name und E-Mail setzen — diese Informationen stehen in jedem Commit:

```shell
git config --global user.name "Dein Name"
git config --global user.email "vorname.nachname@stud.fhgr.ch"
```

Standard-Branch auf `main` setzen:

```shell
git config --global init.defaultBranch main
```

#### Konfiguration prüfen

```shell
git config user.name
git config user.email
```

> Ausführlicher Guide: [Git](git.md)

**✅ Checkpoint:** `git --version` funktioniert und Name/E-Mail sind gesetzt.

---

### 4. Node.js & npm installieren (~15 Min.)

Node.js ist die JavaScript-Runtime, npm der zugehörige Paketmanager. Wir installieren Node.js über **nvm** (Node Version Manager), damit wir später einfach zwischen Versionen wechseln können.

#### nvm installieren

**macOS / Linux:**

```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

Danach Terminal **neu starten** oder:

```shell
source ~/.zshrc
```

**Windows:**

1. Lade [nvm-windows](https://github.com/coreybutler/nvm-windows/releases) herunter (Setup-Installer)
2. Führe den Installer aus
3. Starte dein Terminal neu

#### Node.js installieren

```shell
# Aktuelle LTS-Version installieren
nvm install --lts

# Als Standard-Version setzen
nvm alias default lts/*     # macOS/Linux
# bzw.
nvm alias default lts       # Windows (nvm-windows)
```

#### Prüfen

```shell
node --version
# Erwartete Ausgabe z.B.: v22.x.x

npm --version
# Erwartete Ausgabe z.B.: 10.x.x
```

Falls `nvm` nicht gefunden wird → Terminal neu starten. Weitere Hilfe: [Node.js Guide](nodejs.md#häufige-probleme--lösungen).

> Ausführlicher Guide: [Node.js, nvm, npm & pnpm](nodejs.md)

**✅ Checkpoint:** `node --version` und `npm --version` zeigen Versionsnummern.

---

### 5. Cursor installieren (~10 Min.)

Cursor ist der KI-native Code-Editor, den wir im Studium verwenden. Er basiert auf VS Code, kann also alle VS-Code-Extensions nutzen.

#### Installation

**macOS (Homebrew):**

```shell
brew install --cask cursor
```

**macOS (manuell) / Windows:**

1. Gehe auf [cursor.com](https://cursor.com)
2. Lade die Version für dein Betriebssystem herunter
3. Installiere die App (macOS: `.dmg` → in Programme ziehen / Windows: Installer ausführen)

#### Erster Start

1. Cursor öffnen
2. Falls ihr vorher VS Code genutzt habt: Cursor bietet an, **Extensions und Settings zu importieren** — das ist empfehlenswert
3. Meldet euch mit eurem Account an (oder erstellt einen)

#### Shell-Befehl `cursor` einrichten

Damit ihr Projekte direkt vom Terminal aus öffnen könnt:

1. In Cursor: `Cmd + Shift + P` (macOS) / `Ctrl + Shift + P` (Windows)
2. Tippe: `Shell Command: Install 'cursor' command in PATH`
3. Bestätigen
4. Terminal neu starten

Test:

```shell
cursor --version
```

#### Empfohlene Extensions

Öffnet das Extensions-Panel (`Cmd + Shift + X` / `Ctrl + Shift + X`) und installiert:

- **ESLint** — Code-Analyse
- **Prettier** — Code-Formatierung
- **GitLens** — Erweiterte Git-Integration
- **Error Lens** — Fehler direkt im Code anzeigen

> Mehr zu KI-Tools und Cursor: [AI Tools](ai-tools.md#cursor)

**✅ Checkpoint:** `cursor --version` funktioniert und der Editor öffnet sich.

---

### 6. Smoke-Test — Alles zusammen testen (~10 Min.)

Jetzt testen wir, ob alle Tools zusammenspielen. Führe die folgenden Befehle **Schritt für Schritt** in deinem Terminal aus:

```shell
# 1. Neuen Ordner erstellen und hineinwechseln
mkdir onboarding-test
cd onboarding-test

# 2. Git-Repository initialisieren
git init

# 3. Node.js-Projekt erstellen
npm init -y

# 4. Projekt in Cursor öffnen
cursor .
```

In Cursor angekommen:

5. Öffne das integrierte Terminal (`` Ctrl + ` ``)
6. Erstelle eine Datei:

```shell
echo "console.log('Hello from Onboarding!');" > index.js
```

7. Führe sie aus:

```shell
node index.js
# Erwartete Ausgabe: Hello from Onboarding!
```

8. Mache deinen ersten Commit:

```shell
git add .
git commit -m "Initial commit - Setup erfolgreich"
```

#### Erwartetes Ergebnis

```
✓ Terminal funktioniert
✓ Git erstellt Repos und Commits
✓ Node.js führt JavaScript aus
✓ npm erstellt package.json
✓ Cursor öffnet Projekte und hat ein integriertes Terminal
```

**✅ Checkpoint:** Alle Schritte erfolgreich — deine Entwicklungsumgebung steht!

---

## Troubleshooting

### `command not found: brew`

Homebrew ist nicht im PATH. Für Apple-Silicon-Macs:

```shell
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
source ~/.zprofile
```

### `command not found: nvm`

Terminal neu starten. Falls das nicht hilft:

```shell
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

Füge diese Zeilen in deine `~/.zshrc` ein und starte das Terminal neu.

### `command not found: git` (Windows)

Git for Windows muss neu installiert werden. Achte darauf, die Option "Git from the command line and also from 3rd-party software" auszuwählen.

### `command not found: cursor`

Shell-Befehl in Cursor installieren: `Cmd/Ctrl + Shift + P` → `Shell Command: Install 'cursor' command in PATH`. Danach Terminal neu starten.

### Generelle Probleme

- **Terminal neu starten** löst die meisten PATH-Probleme
- **Rechner neu starten** als letzte Option
- Fragen im Plenum stellen — wahrscheinlich hat jemand das gleiche Problem

---

## Zusammenfassung

| Tool | Prüfbefehl | Erwartete Ausgabe |
|---|---|---|
| Terminal | `pwd` / `Get-Location` | Ein Dateipfad |
| Homebrew | `brew --version` | `Homebrew 4.x.x` |
| Git | `git --version` | `git version 2.x.x` |
| Node.js | `node --version` | `v22.x.x` |
| npm | `npm --version` | `10.x.x` |
| Cursor | `cursor --version` | Versionsnummer |

---

[← Zurück zur Übersicht](../README.md)
