[← Zurück zur Übersicht](../README.md)

# Visual Studio Code -- Der Editor fuer dein Studium

## Was ist VS Code?

Visual Studio Code (kurz **VS Code**) ist ein kostenloser, quelloffener Code-Editor von Microsoft. Er ist leichtgewichtig, extrem erweiterbar und hat sich als De-facto-Standard unter Entwicklern etabliert. VS Code unterstuetzt nahezu jede Programmiersprache, bietet ein integriertes Terminal, Git-Integration und tausende Erweiterungen (Extensions).

> **Nicht verwechseln:** VS Code ist _nicht_ dasselbe wie Visual Studio (die grosse IDE von Microsoft). VS Code ist deutlich schlanker und flexibler.

**Warum VS Code?**

- Laeuft auf macOS, Windows und Linux
- Riesiges Extension-Oekosystem
- Integriertes Terminal und Git-Unterstuetzung
- Intelligente Code-Vervollstaendigung (IntelliSense)
- Kostenlos und Open Source
- Sehr aktive Community und regelmaessige Updates

---

## Installation

### Option 1: Homebrew (empfohlen fuer macOS)

Wenn du [Homebrew](https://brew.sh) bereits installiert hast, genuegt ein einziger Befehl:

```bash
brew install --cask visual-studio-code
```

### Option 2: Manueller Download

1. Gehe auf [https://code.visualstudio.com](https://code.visualstudio.com)
2. Lade die macOS-Version herunter (Universal oder Apple Silicon, je nach Mac)
3. Entpacke die `.zip`-Datei
4. Verschiebe `Visual Studio Code.app` in deinen `Programme`-Ordner

### Aktualisierung

VS Code aktualisiert sich normalerweise automatisch. Falls du Homebrew verwendest, kannst du auch manuell updaten:

```bash
brew upgrade --cask visual-studio-code
```

---

## Shell-Befehl `code` einrichten

Mit dem Befehl `code` kannst du VS Code direkt aus dem Terminal oeffnen. Das ist extrem praktisch:

```bash
# Aktuellen Ordner in VS Code oeffnen
code .

# Eine bestimmte Datei oeffnen
code meine-datei.py

# Einen bestimmten Ordner oeffnen
code ~/Projekte/mein-projekt
```

### Einrichtung

1. Oeffne VS Code
2. Oeffne die Command Palette mit `Cmd + Shift + P`
3. Tippe `Shell Command` ein
4. Waehle **Shell Command: Install 'code' command in PATH**
5. Starte dein Terminal neu

Alternativ kannst du den Pfad auch manuell in deine Shell-Konfiguration eintragen:

```bash
# Fuer zsh (~/.zshrc) oder bash (~/.bashrc):
export PATH="/Applications/Visual Studio Code.app/Contents/Resources/app/bin:$PATH"
```

Danach Terminal neu starten oder `source ~/.zshrc` ausfuehren.

---

## Empfohlene Extensions

Extensions installierst du ueber die Sidebar (`Cmd + Shift + X`) oder ueber das Terminal:

```bash
code --install-extension <extension-id>
```

Hier eine kuratierte Liste fuer den Einstieg:

### ESLint

- **ID:** `dbaeumer.vscode-eslint`
- **Beschreibung:** Analysiert deinen JavaScript- und TypeScript-Code in Echtzeit auf Fehler, stilistische Probleme und potenzielle Bugs. Unverzichtbar fuer jedes JS/TS-Projekt.
- **Installation:** `code --install-extension dbaeumer.vscode-eslint`

### Prettier -- Code Formatter

- **ID:** `esbenp.prettier-vscode`
- **Beschreibung:** Formatiert deinen Code automatisch nach einheitlichen Regeln. Unterstuetzt JavaScript, TypeScript, HTML, CSS, JSON, Markdown und mehr. Spart Diskussionen ueber Code-Stil im Team.
- **Installation:** `code --install-extension esbenp.prettier-vscode`

### GitLens

- **ID:** `eamodio.gitlens`
- **Beschreibung:** Erweitert die eingebaute Git-Funktionalitaet massiv. Zeigt dir direkt im Code, wer welche Zeile wann geaendert hat (Blame-Annotations), bietet einen visuellen Commit-Verlauf und erleichtert Code-Reviews.
- **Installation:** `code --install-extension eamodio.gitlens`

### Auto Rename Tag

- **ID:** `formulahendry.auto-rename-tag`
- **Beschreibung:** Wenn du ein HTML- oder XML-Tag umbenennst, wird das zugehoerige oeffnende oder schliessende Tag automatisch mit umbenannt. Spart Zeit und vermeidet Fehler.
- **Installation:** `code --install-extension formulahendry.auto-rename-tag`

### Path Intellisense

- **ID:** `christian-kohler.path-intellisense`
- **Beschreibung:** Vervollstaendigt Dateipfade automatisch, waehrend du tippst. Funktioniert in Imports, require-Statements, HTML-src-Attributen und mehr.
- **Installation:** `code --install-extension christian-kohler.path-intellisense`

### Thunder Client

- **ID:** `rangav.vscode-thunder-client`
- **Beschreibung:** Ein leichtgewichtiger REST-API-Client direkt in VS Code. Perfekt zum Testen von APIs, ohne Postman oder ein anderes externes Tool oeffnen zu muessen.
- **Installation:** `code --install-extension rangav.vscode-thunder-client`

### Error Lens

- **ID:** `usernamehw.errorlens`
- **Beschreibung:** Zeigt Fehler, Warnungen und Hinweise direkt inline im Code an, statt sie nur in der Problemliste zu verstecken. Du siehst sofort, wo etwas nicht stimmt.
- **Installation:** `code --install-extension usernamehw.errorlens`

### Material Icon Theme

- **ID:** `PKief.material-icon-theme`
- **Beschreibung:** Ersetzt die Standard-Datei-Icons durch huebsche Material-Design-Icons. Macht die Dateiuebersicht in der Sidebar deutlich uebersichtlicher, weil du Dateitypen auf einen Blick erkennst.
- **Installation:** `code --install-extension PKief.material-icon-theme`

### Live Server

- **ID:** `ritwickdey.LiveServer`
- **Beschreibung:** Startet einen lokalen Entwicklungsserver mit automatischem Neuladen (Hot Reload). Ideal fuer die Frontend-Entwicklung mit HTML, CSS und JavaScript.
- **Installation:** `code --install-extension ritwickdey.LiveServer`

### GitHub Copilot

- **ID:** `GitHub.copilot`
- **Beschreibung:** KI-gestuetzte Code-Vervollstaendigung von GitHub/OpenAI. Schlaegt ganze Funktionen, Kommentare und Code-Bloecke vor. Als Student bekommst du ueber das [GitHub Student Developer Pack](https://education.github.com/pack) kostenlosen Zugang.
- **Installation:** `code --install-extension GitHub.copilot`

### Alle Extensions auf einmal installieren

```bash
code --install-extension dbaeumer.vscode-eslint \
  && code --install-extension esbenp.prettier-vscode \
  && code --install-extension eamodio.gitlens \
  && code --install-extension formulahendry.auto-rename-tag \
  && code --install-extension christian-kohler.path-intellisense \
  && code --install-extension rangav.vscode-thunder-client \
  && code --install-extension usernamehw.errorlens \
  && code --install-extension PKief.material-icon-theme \
  && code --install-extension ritwickdey.LiveServer \
  && code --install-extension GitHub.copilot
```

---

## Wichtige Shortcuts (macOS)

### Navigation

| Shortcut | Aktion |
|---|---|
| `Cmd + P` | Schnell eine Datei oeffnen (Quick Open) |
| `Cmd + Shift + P` | Command Palette oeffnen |
| `Cmd + B` | Sidebar ein-/ausblenden |
| `Cmd + J` | Panel (Terminal, Output etc.) ein-/ausblenden |
| `Cmd + Shift + E` | Explorer-Sidebar oeffnen |
| `Cmd + Shift + X` | Extensions-Sidebar oeffnen |
| `Ctrl + Tab` | Zwischen offenen Tabs wechseln |
| `Cmd + 1/2/3` | Zwischen Editor-Gruppen wechseln |
| `Cmd + W` | Aktuellen Tab schliessen |
| `Cmd + Shift + T` | Zuletzt geschlossenen Tab wieder oeffnen |

### Suche

| Shortcut | Aktion |
|---|---|
| `Cmd + F` | In aktueller Datei suchen |
| `Cmd + Shift + F` | In allen Dateien suchen (globale Suche) |
| `Cmd + H` | Suchen und Ersetzen (aktuelle Datei) |
| `Cmd + Shift + H` | Suchen und Ersetzen (alle Dateien) |
| `Cmd + G` | Zu einer bestimmten Zeile springen |
| `Cmd + Shift + O` | Zu einem Symbol in der Datei springen |
| `Cmd + T` | Zu einem Symbol im Workspace springen |

### Bearbeitung

| Shortcut | Aktion |
|---|---|
| `Cmd + D` | Naechstes Vorkommen des markierten Worts auswaehlen |
| `Cmd + Shift + L` | Alle Vorkommen des markierten Worts auswaehlen |
| `Option + Click` | Zusaetzlichen Cursor setzen (Multi-Cursor) |
| `Option + Shift + Up/Down` | Zeile duplizieren (nach oben/unten) |
| `Option + Up/Down` | Zeile nach oben/unten verschieben |
| `Cmd + Shift + K` | Ganze Zeile loeschen |
| `Cmd + Enter` | Neue Zeile darunter einfuegen |
| `Cmd + Shift + Enter` | Neue Zeile darueber einfuegen |
| `Cmd + /` | Zeile aus-/einkommentieren |
| `Cmd + Shift + A` | Block-Kommentar ein-/ausschalten |
| `Cmd + X` | Zeile ausschneiden (ohne Markierung) |
| `Cmd + C` | Zeile kopieren (ohne Markierung) |
| `Tab` | Einruecken |
| `Shift + Tab` | Einrueckung entfernen |

### Terminal

| Shortcut | Aktion |
|---|---|
| `` Ctrl + ` `` | Integriertes Terminal oeffnen/schliessen |
| `` Ctrl + Shift + ` `` | Neues Terminal erstellen |
| `Cmd + \` | Editor teilen (Split) |

### Code Intelligence

| Shortcut | Aktion |
|---|---|
| `F12` | Zur Definition springen |
| `Option + F12` | Definition als Peek anzeigen |
| `Shift + F12` | Alle Referenzen anzeigen |
| `F2` | Symbol umbenennen (Refactoring) |
| `Cmd + .` | Quick Fix / Code Actions |
| `Ctrl + Space` | IntelliSense manuell ausloesen |

---

## Nuetzliche Settings

VS Code wird ueber eine JSON-Datei konfiguriert. Oeffne sie mit `Cmd + ,` (grafisch) oder `Cmd + Shift + P` und dann `Preferences: Open User Settings (JSON)`.

### Empfohlene `settings.json` fuer den Einstieg

```json
{
  // Editor-Grundeinstellungen
  "editor.fontSize": 14,
  "editor.tabSize": 2,
  "editor.wordWrap": "on",
  "editor.minimap.enabled": false,
  "editor.bracketPairColorization.enabled": true,
  "editor.guides.bracketPairs": "active",
  "editor.linkedEditing": true,
  "editor.stickyScroll.enabled": true,

  // Automatisches Speichern
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 1000,

  // Formatierung
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },

  // Terminal
  "terminal.integrated.fontSize": 13,
  "terminal.integrated.defaultProfile.osx": "zsh",

  // Dateien
  "files.trimTrailingWhitespace": true,
  "files.insertFinalNewline": true,
  "files.trimFinalNewlines": true,

  // Explorer
  "explorer.confirmDelete": false,
  "explorer.confirmDragAndDrop": false,
  "explorer.compactFolders": false,

  // Git
  "git.autofetch": true,
  "git.confirmSync": false,
  "git.enableSmartCommit": true,

  // Theme und Icons
  "workbench.iconTheme": "material-icon-theme",

  // Telemetrie deaktivieren
  "telemetry.telemetryLevel": "off"
}
```

### Sprachspezifische Settings

Du kannst Einstellungen auch pro Sprache ueberschreiben:

```json
{
  "[python]": {
    "editor.tabSize": 4,
    "editor.defaultFormatter": "ms-python.black-formatter"
  },
  "[markdown]": {
    "editor.wordWrap": "on",
    "editor.quickSuggestions": {
      "other": false,
      "comments": false,
      "strings": false
    }
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

---

## Workspace vs User Settings

VS Code unterscheidet zwischen zwei Ebenen von Einstellungen:

### User Settings (Benutzereinstellungen)

- Gelten **global** fuer alle Projekte
- Gespeichert unter `~/Library/Application Support/Code/User/settings.json`
- Oeffnen mit: `Cmd + Shift + P` -> `Preferences: Open User Settings (JSON)`

### Workspace Settings (Arbeitsbereichseinstellungen)

- Gelten **nur fuer das aktuelle Projekt**
- Gespeichert im Projektordner unter `.vscode/settings.json`
- Oeffnen mit: `Cmd + Shift + P` -> `Preferences: Open Workspace Settings (JSON)`
- **Ueberschreiben** User Settings fuer dieses Projekt

### Wann Workspace Settings nutzen?

- Wenn ein Projekt andere Formatierungsregeln braucht (z.B. Tab-Groesse 4 statt 2)
- Wenn bestimmte Extensions nur fuer ein Projekt relevant sind
- Wenn du Einstellungen mit deinem Team teilen willst (`.vscode/settings.json` ins Git-Repository committen)

**Beispiel fuer Workspace Settings** (`.vscode/settings.json`):

```json
{
  "editor.tabSize": 4,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "files.exclude": {
    "**/.git": true,
    "**/node_modules": true,
    "**/.DS_Store": true
  },
  "search.exclude": {
    "**/node_modules": true,
    "**/dist": true
  }
}
```

> **Tipp:** Du kannst im `.vscode`-Ordner auch eine `extensions.json` anlegen, um deinem Team Extensions zu empfehlen:
>
> ```json
> {
>   "recommendations": [
>     "dbaeumer.vscode-eslint",
>     "esbenp.prettier-vscode",
>     "eamodio.gitlens"
>   ]
> }
> ```
>
> Wenn jemand das Projekt oeffnet, schlaegt VS Code diese Extensions automatisch vor.

---

## Integriertes Terminal

Das integrierte Terminal ist einer der groessten Vorteile von VS Code. Du musst nicht staendig zwischen Editor und Terminal wechseln.

### Terminal oeffnen

- Shortcut: `` Ctrl + ` `` (Backtick-Taste)
- Menue: `Terminal` -> `New Terminal`
- Command Palette: `Terminal: Create New Terminal`

### Mehrere Terminals

Du kannst mehrere Terminal-Instanzen gleichzeitig oeffnen:

- **Neues Terminal:** `` Ctrl + Shift + ` ``
- **Terminal teilen (Split):** `Cmd + \` im Terminal-Panel
- **Zwischen Terminals wechseln:** Dropdown im Terminal-Panel oder `Cmd + Shift + [` / `Cmd + Shift + ]`

### Terminal-Typen

Du kannst verschiedene Shells verwenden:

- **zsh** (Standard auf macOS)
- **bash**
- **fish** (falls installiert)
- **node** (JavaScript REPL)
- **python** (Python REPL)

Aendere die Standard-Shell in den Settings:

```json
{
  "terminal.integrated.defaultProfile.osx": "zsh"
}
```

### Praktische Terminal-Features

- **Link-Erkennung:** Dateipfade und URLs im Terminal sind klickbar (`Cmd + Click`)
- **Kopieren/Einfuegen:** `Cmd + C` / `Cmd + V` funktionieren im Terminal
- **Suchen im Terminal:** `Cmd + F` oeffnet die Suche im Terminal-Output
- **Terminal-Ausgabe loeschen:** `Cmd + K` (im Terminal)
- **Schriftgroesse aendern:** `Cmd + +` / `Cmd + -`

---

## Tips & Tricks

### 1. Multi-Cursor meistern

Multi-Cursor ist eines der maechtigsten Features in VS Code:

- `Cmd + D` -- waehle das naechste Vorkommen eines Worts aus (wiederholbar)
- `Cmd + Shift + L` -- waehle **alle** Vorkommen auf einmal aus
- `Option + Click` -- setze einen zusaetzlichen Cursor an beliebiger Stelle
- `Option + Cmd + Up/Down` -- fuege Cursor ueber/unter der aktuellen Zeile hinzu

### 2. Emmet fuer schnelles HTML

VS Code hat Emmet eingebaut. Tippe Abkuerzungen und druecke `Tab`:

```
div.container>ul>li*5>a{Link $}
```

Erzeugt:

```html
<div class="container">
  <ul>
    <li><a href="">Link 1</a></li>
    <li><a href="">Link 2</a></li>
    <li><a href="">Link 3</a></li>
    <li><a href="">Link 4</a></li>
    <li><a href="">Link 5</a></li>
  </ul>
</div>
```

### 3. Snippets nutzen

VS Code bietet eingebaute Snippets. Tippe z.B. in einer JavaScript-Datei:

- `log` -> `console.log()`
- `for` -> For-Schleife
- `if` -> If-Statement

Du kannst auch eigene Snippets definieren unter `Cmd + Shift + P` -> `Snippets: Configure Snippets`.

### 4. Zen Mode

Fuer ablenkungsfreies Arbeiten:

- `Cmd + K`, dann `Z` -- aktiviert den Zen Mode
- Blendet Sidebar, Tabs, Statusbar aus
- Nochmal `Esc Esc` zum Verlassen

### 5. Dateien schnell oeffnen

`Cmd + P` ist dein bester Freund:

- Tippe einen Dateinamen (auch nur Teile davon)
- Nutze `:` gefolgt von einer Zeilennummer, um direkt zu einer Zeile zu springen: `datei.js:42`
- Nutze `@` um zu einem Symbol in der Datei zu springen
- Nutze `#` um workspace-weit nach Symbolen zu suchen

### 6. Git direkt in VS Code

- **Source Control Panel:** `Cmd + Shift + G`
- Aenderungen sehen, stagen, committen -- alles ohne Terminal
- Mit GitLens siehst du Blame-Informationen direkt im Code
- Merge-Konflikte werden visuell dargestellt mit Optionen zum Akzeptieren

### 7. Keyboard-Shortcuts anpassen

Wenn dir ein Shortcut nicht gefaellt, aendere ihn:

- `Cmd + K`, dann `Cmd + S` -- oeffnet die Shortcut-Uebersicht
- Suche nach einer Aktion und weise einen neuen Shortcut zu

### 8. Settings Sync aktivieren

Damit du deine Einstellungen, Extensions und Shortcuts auf mehreren Geraeten synchron halten kannst:

1. `Cmd + Shift + P` -> `Settings Sync: Turn On`
2. Melde dich mit deinem GitHub- oder Microsoft-Konto an
3. Waehle aus, was synchronisiert werden soll

### 9. Projekt-Ordner per Terminal oeffnen

```bash
# Aktuellen Ordner in VS Code oeffnen
code .

# Neues Fenster oeffnen
code -n .

# Datei in bestehendem Fenster oeffnen
code -r datei.txt

# Diff zwischen zwei Dateien anzeigen
code --diff datei1.txt datei2.txt
```

### 10. Command Palette -- das Schweizer Taschenmesser

`Cmd + Shift + P` oeffnet die Command Palette. Von hier aus erreichst du **jede** Funktion in VS Code. Ein paar nuetzliche Befehle:

- `Reopen Closed Editor` -- zuletzt geschlossene Datei wieder oeffnen
- `Transform to Uppercase/Lowercase` -- Text umwandeln
- `Sort Lines Ascending/Descending` -- Zeilen sortieren
- `Join Lines` -- Zeilen zusammenfuehren
- `Toggle Word Wrap` -- Zeilenumbruch umschalten
- `Change Language Mode` -- Syntax-Highlighting aendern

---

> **Viel Erfolg mit VS Code!** Bei Fragen oder Problemen hilft die offizielle Dokumentation unter [https://code.visualstudio.com/docs](https://code.visualstudio.com/docs) weiter.
