[← Zurück zur Übersicht](../README.md)

# Node.js, nvm, npm & pnpm -- Ein Leitfaden für Studierende

Dieses Dokument erklärt dir Schritt für Schritt, was Node.js ist, wie du es installierst, Versionen verwaltest und mit Paketmanagern arbeitest. Am Ende kannst du eigenständig Node.js-Projekte aufsetzen und Abhängigkeiten verwalten.

---

## Was ist Node.js?

Node.js ist eine **JavaScript-Runtime**, die auf Googles V8-Engine basiert. Damit kannst du JavaScript nicht nur im Browser, sondern auch auf deinem Computer (z. B. auf einem Server) ausführen.

**Warum ist das wichtig?**

- Viele moderne Webentwicklungs-Tools (Bundler, Linter, Frameworks) laufen auf Node.js.
- Fullstack-Entwicklung mit einer einzigen Sprache (JavaScript/TypeScript) wird möglich.
- Ein riesiges Ökosystem an Paketen steht über npm zur Verfügung.

```text
Browser                         Server / Lokaler Rechner
┌──────────────────┐            ┌──────────────────┐
│  JavaScript      │            │  JavaScript      │
│  + Web-APIs      │            │  + Node.js-APIs  │
│  (DOM, fetch..)  │            │  (fs, http, ..)  │
│  V8-Engine       │            │  V8-Engine       │
└──────────────────┘            └──────────────────┘
```

Du kannst jede beliebige `.js`-Datei direkt ausführen:

```shell
node meinScript.js
```

---

## Installation via nvm (Node Version Manager)

**Installiere Node.js niemals direkt über die Website.** Verwende stattdessen **nvm** (Node Version Manager). So kannst du jederzeit zwischen verschiedenen Node-Versionen wechseln -- das ist im Studium und in Teamprojekten extrem hilfreich.

### Schritt 1 -- nvm installieren

**macOS / Linux:**

```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

Nach der Installation musst du dein Terminal **neu starten** oder folgenden Befehl ausführen:

```shell
source ~/.bashrc
# oder, falls du zsh nutzt:
source ~/.zshrc
```

**Windows:**

Unter Windows verwendest du [nvm-windows](https://github.com/coreybutler/nvm-windows). Lade den Installer von der Releases-Seite herunter und folge dem Setup-Assistenten.

### Schritt 2 -- Prüfen, ob nvm installiert ist

```shell
nvm --version
# Ausgabe z. B.: 0.40.1
```

Wenn du eine Versionsnummer siehst, hat alles geklappt.

### Schritt 3 -- Node.js installieren

```shell
# Aktuelle LTS-Version installieren (empfohlen)
nvm install --lts

# Oder eine bestimmte Version installieren
nvm install 20
nvm install 18.19.0
```

> **Tipp:** Verwende im Studium immer die aktuelle **LTS-Version** (Long Term Support). Diese ist am stabilsten.

### Schritt 4 -- Installation überprüfen

```shell
node --version
# z. B.: v20.11.0

npm --version
# z. B.: 10.2.4
```

---

## nvm-Befehle im Überblick

| Befehl | Beschreibung |
|---|---|
| `nvm install <version>` | Installiert eine bestimmte Node-Version |
| `nvm install --lts` | Installiert die neueste LTS-Version |
| `nvm use <version>` | Wechselt zur angegebenen Version (für die aktuelle Shell) |
| `nvm list` | Zeigt alle lokal installierten Versionen an |
| `nvm list-remote --lts` | Zeigt alle verfügbaren LTS-Versionen zum Installieren |
| `nvm alias default <version>` | Setzt die Standard-Version für neue Terminals |
| `nvm current` | Zeigt die aktuell aktive Version an |
| `nvm uninstall <version>` | Deinstalliert eine Version |

### Beispiel-Workflow

```shell
# LTS installieren und als Standard setzen
nvm install --lts
nvm alias default lts/*

# Prüfen, welche Versionen installiert sind
nvm list

# Für ein bestimmtes Projekt eine andere Version nutzen
nvm install 18
nvm use 18
node --version
# -> v18.x.x

# Zurück zur Standardversion
nvm use default
```

---

## Die .nvmrc Datei

In Teamprojekten ist es wichtig, dass alle die **gleiche Node-Version** verwenden. Dafür gibt es die Datei `.nvmrc` im Projektverzeichnis.

### Erstellen

Lege im Wurzelverzeichnis deines Projekts eine Datei namens `.nvmrc` an:

```text
20
```

Das ist der gesamte Inhalt -- nur die gewünschte Versionsnummer (Major-Version oder exakte Version wie `20.11.0`).

### Verwenden

Wenn du in ein Projektverzeichnis mit `.nvmrc` wechselst, führe einfach aus:

```shell
nvm use
```

nvm liest automatisch die Version aus `.nvmrc` und wechselt dorthin. Falls die Version noch nicht installiert ist:

```shell
nvm install
```

> **Tipp:** Du kannst deine Shell so konfigurieren, dass `nvm use` automatisch beim Wechsel in ein Verzeichnis ausgeführt wird. Eine Anleitung findest du in der [nvm-Dokumentation](https://github.com/nvm-sh/nvm#deeper-shell-integration).

---

## npm Grundlagen

**npm** (Node Package Manager) wird automatisch mit Node.js installiert. Es ist das Standard-Werkzeug, um Pakete (Bibliotheken, Frameworks, Tools) zu installieren und Projekte zu verwalten.

### Ein neues Projekt erstellen

```shell
# Erstellt eine package.json mit interaktivem Assistenten
npm init

# Erstellt eine package.json mit Standardwerten (schneller)
npm init -y
```

### Pakete installieren

```shell
# Paket als Projektabhängigkeit installieren
npm install express

# Paket als Entwicklungsabhängigkeit installieren (z. B. Test-Tools)
npm install --save-dev jest

# Alle Abhängigkeiten aus package.json installieren (z. B. nach git clone)
npm install

# Paket global installieren (selten nötig)
npm install -g nodemon
```

### Pakete deinstallieren

```shell
npm uninstall express
npm uninstall --save-dev jest
```

### Scripts ausführen

In der `package.json` kannst du eigene Scripts definieren und mit `npm run` ausführen:

```shell
# Script ausführen
npm run build
npm run test
npm run dev

# Kurzformen für bestimmte Scripts
npm start    # entspricht npm run start
npm test     # entspricht npm run test
```

### npx -- Pakete einmalig ausführen

Mit `npx` kannst du Pakete ausführen, ohne sie dauerhaft zu installieren:

```shell
# Ein neues React-Projekt erstellen, ohne create-react-app global zu installieren
npx create-react-app mein-projekt

# Ein lokal installiertes CLI-Tool ausführen
npx eslint .
```

### Die wichtigsten npm-Befehle

| Befehl | Beschreibung |
|---|---|
| `npm init -y` | Neues Projekt erstellen |
| `npm install` | Alle Abhängigkeiten installieren |
| `npm install <paket>` | Paket hinzufügen |
| `npm install --save-dev <paket>` | Entwicklungsabhängigkeit hinzufügen |
| `npm uninstall <paket>` | Paket entfernen |
| `npm run <script>` | Script aus package.json ausführen |
| `npm update` | Pakete aktualisieren |
| `npm outdated` | Veraltete Pakete anzeigen |
| `npm list --depth=0` | Installierte Pakete anzeigen |
| `npx <paket>` | Paket einmalig ausführen |

---

## package.json erklärt

Die `package.json` ist die **zentrale Konfigurationsdatei** deines Node.js-Projekts. Sie beschreibt dein Projekt und listet alle Abhängigkeiten auf.

### Aufbau einer typischen package.json

```json
{
  "name": "mein-projekt",
  "version": "1.0.0",
  "description": "Ein Beispielprojekt für die Vorlesung",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": "jest",
    "build": "tsc",
    "lint": "eslint ."
  },
  "dependencies": {
    "express": "^4.18.2",
    "dotenv": "^16.3.1"
  },
  "devDependencies": {
    "jest": "^29.7.0",
    "nodemon": "^3.0.2",
    "eslint": "^8.56.0"
  }
}
```

### Die wichtigsten Felder

**`scripts`** -- Hier definierst du Befehle, die du mit `npm run <name>` ausführen kannst. Typische Scripts:

- `start` -- Startet die Anwendung
- `dev` -- Startet die Anwendung im Entwicklungsmodus (z. B. mit Hot-Reload)
- `test` -- Führt die Tests aus
- `build` -- Erstellt einen Produktions-Build
- `lint` -- Prüft den Code auf Stilfehler

**`dependencies`** -- Pakete, die deine Anwendung **zur Laufzeit** braucht (z. B. Express, React). Diese werden auch in der Produktion installiert.

**`devDependencies`** -- Pakete, die nur **während der Entwicklung** gebraucht werden (z. B. Test-Frameworks, Linter, TypeScript-Compiler). Diese werden in der Produktion nicht installiert.

### Versionierung in package.json

```text
"express": "^4.18.2"
              │ │  │
              │ │  └── Patch (Bugfixes)
              │ └──── Minor (neue Features, abwärtskompatibel)
              └────── Major (Breaking Changes)

^  = Erlaubt Updates innerhalb der Major-Version (^4.18.2 -> 4.x.x)
~  = Erlaubt Updates innerhalb der Minor-Version (~4.18.2 -> 4.18.x)
Ohne Präfix = Exakt diese Version
```

---

## package-lock.json erklärt

Die `package-lock.json` wird **automatisch von npm generiert**. Du bearbeitest sie nie manuell.

### Wozu dient sie?

- Sie speichert die **exakten Versionen** aller installierten Pakete (inklusive aller Unterpakete).
- Sie stellt sicher, dass `npm install` bei allen Teammitgliedern **identische** Abhängigkeiten installiert.
- Ohne die Lock-Datei könnten verschiedene Entwickler unterschiedliche Patch-Versionen bekommen.

### Wichtige Regeln

```text
package.json         --> Beschreibt, WELCHE Pakete du brauchst (mit Versionsbereichen)
package-lock.json    --> Speichert, WELCHE exakten Versionen installiert wurden
node_modules/        --> Enthält die tatsächlich heruntergeladenen Pakete
```

- **Committe** `package-lock.json` immer in Git.
- **Committe niemals** den Ordner `node_modules/`. Trage ihn in deine `.gitignore` ein:

```text
# .gitignore
node_modules/
```

---

## pnpm als Alternative

**pnpm** (Performant npm) ist ein alternativer Paketmanager, der gegenüber npm einige Vorteile bietet.

### Vorteile von pnpm

| Eigenschaft | npm | pnpm |
|---|---|---|
| **Speicherplatz** | Jedes Projekt kopiert alle Pakete in eigenes `node_modules` | Pakete werden zentral gespeichert und per Symlink verknüpft |
| **Installationsgeschwindigkeit** | Langsamer bei vielen Abhängigkeiten | Deutlich schneller durch Content-Addressable Storage |
| **Disk Usage** | Hoch bei vielen Projekten | Sehr gering, da Pakete nur einmal gespeichert werden |
| **Strictness** | Erlaubt Zugriff auf nicht deklarierte Abhängigkeiten | Strikte `node_modules`-Struktur verhindert "Phantom Dependencies" |

### pnpm installieren

```shell
# Via npm (einfachste Methode)
npm install -g pnpm

# Via Corepack (in Node.js 16+ enthalten)
corepack enable
corepack prepare pnpm@latest --activate

# Via Homebrew (macOS)
brew install pnpm
```

### Die wichtigsten pnpm-Befehle

pnpm ist so entworfen, dass die Befehle npm sehr ähnlich sind:

| npm-Befehl | pnpm-Befehl | Beschreibung |
|---|---|---|
| `npm install` | `pnpm install` | Alle Abhängigkeiten installieren |
| `npm install <paket>` | `pnpm add <paket>` | Paket hinzufügen |
| `npm install -D <paket>` | `pnpm add -D <paket>` | Dev-Abhängigkeit hinzufügen |
| `npm uninstall <paket>` | `pnpm remove <paket>` | Paket entfernen |
| `npm run <script>` | `pnpm run <script>` oder `pnpm <script>` | Script ausführen |
| `npx <paket>` | `pnpm dlx <paket>` | Paket einmalig ausführen |
| `npm update` | `pnpm update` | Pakete aktualisieren |

### Beispiel-Workflow mit pnpm

```shell
# Neues Projekt starten
pnpm init

# Abhängigkeiten installieren
pnpm add express
pnpm add -D typescript @types/node

# Alle Abhängigkeiten installieren (z. B. nach git clone)
pnpm install

# Script ausführen
pnpm dev
```

> **Hinweis:** pnpm erzeugt eine `pnpm-lock.yaml` statt `package-lock.json`. Committe diese Datei in Git. Verwende in einem Projekt **immer nur einen** Paketmanager.

---

## Häufige Probleme & Lösungen

### "command not found: nvm"

**Problem:** Nach der Installation von nvm wird der Befehl nicht gefunden.

**Lösung:**

```shell
# Prüfe, ob die nvm-Konfiguration in deiner Shell-Konfiguration steht
# Für zsh:
cat ~/.zshrc | grep NVM_DIR

# Für bash:
cat ~/.bashrc | grep NVM_DIR

# Falls nichts gefunden wird, füge folgendes hinzu:
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

# Danach Terminal neu starten oder:
source ~/.zshrc   # bzw. source ~/.bashrc
```

### "EACCES: permission denied"

**Problem:** Fehler bei globaler Installation von Paketen.

**Lösung:** Wenn du nvm verwendest, sollte dieses Problem nicht auftreten. Falls doch:

```shell
# NIEMALS sudo mit npm verwenden!
# Stattdessen: Stelle sicher, dass du nvm nutzt
nvm use default
which npm
# Sollte einen Pfad in ~/.nvm/ anzeigen
```

### "node_modules" ist riesig

**Problem:** Der `node_modules`-Ordner nimmt viel Speicherplatz ein.

**Lösung:**

```shell
# Lösche node_modules und installiere neu
rm -rf node_modules
npm install

# Oder wechsle zu pnpm, um Speicherplatz zu sparen
pnpm import          # Konvertiert package-lock.json zu pnpm-lock.yaml
rm -rf node_modules
pnpm install
```

### "Cannot find module ..."

**Problem:** Ein Modul wird nicht gefunden, obwohl es in `package.json` steht.

**Lösung:**

```shell
# Abhängigkeiten neu installieren
rm -rf node_modules package-lock.json
npm install
```

### Falsche Node-Version im Projekt

**Problem:** Ein Projekt erfordert eine bestimmte Node-Version.

**Lösung:**

```shell
# Prüfe, ob eine .nvmrc-Datei existiert
cat .nvmrc

# Installiere und nutze die dort angegebene Version
nvm install
nvm use

# Prüfe die aktive Version
node --version
```

### "ERR! peer dep missing" oder Peer-Dependency-Konflikte

**Problem:** Zwei Pakete erfordern unterschiedliche Versionen einer gemeinsamen Abhängigkeit.

**Lösung:**

```shell
# Ab npm 7+ werden Peer-Dependency-Konflikte als Fehler angezeigt
# Erzwinge die Installation (mit Vorsicht):
npm install --legacy-peer-deps

# Oder analysiere den Konflikt:
npm ls <paketname>
```

### Port bereits belegt ("EADDRINUSE")

**Problem:** Beim Starten eines Servers wird gemeldet, dass der Port schon benutzt wird.

**Lösung:**

```shell
# Finde heraus, welcher Prozess den Port belegt (Beispiel: Port 3000)
lsof -i :3000

# Beende den Prozess
kill -9 <PID>

# Oder starte dein Projekt auf einem anderen Port
PORT=3001 npm start
```

---

## Zusammenfassung

| Thema | Das Wichtigste |
|---|---|
| **Node.js** | JavaScript-Runtime für den Server/lokalen Rechner |
| **nvm** | Verwaltet mehrere Node-Versionen -- immer verwenden |
| **npm** | Standard-Paketmanager, kommt mit Node.js |
| **pnpm** | Schnellere, speichereffiziente Alternative zu npm |
| **package.json** | Beschreibt dein Projekt und seine Abhängigkeiten |
| **package-lock.json** | Sperrt exakte Versionen -- immer committen |
| **.nvmrc** | Legt die Node-Version pro Projekt fest |
| **node_modules/** | Niemals committen -- in `.gitignore` eintragen |
