[← Zurück zur Übersicht](../README.md)

# Homebrew -- Der Paketmanager für macOS

## Was ist Homebrew?

Homebrew ist der beliebteste Paketmanager für macOS. Ein Paketmanager ermöglicht es dir, Software über das Terminal zu installieren, zu aktualisieren und zu verwalten -- ganz ohne manuelles Herunterladen von Websites oder Herumklicken in Installationsassistenten.

Stell dir Homebrew wie einen App Store vor, den du komplett über die Kommandozeile bedienst. Du tippst einen Befehl ein, und Homebrew kümmert sich um den Rest: Download, Installation, Abhängigkeiten und Updates.

**Warum Homebrew nutzen?**

- Schnelle Installation von Entwickler-Tools (Git, Node.js, Python, etc.)
- Einfache Updates aller installierten Pakete mit einem einzigen Befehl
- Reproduzierbare Setups im Team durch Brewfiles
- Saubere Deinstallation ohne Rückstände

## Installation

### Voraussetzung

Du brauchst die Xcode Command Line Tools. Falls noch nicht installiert, öffne das Terminal und führe aus:

```shell
xcode-select --install
```

### Homebrew installieren

Kopiere den folgenden Befehl und führe ihn im Terminal aus:

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Das Installationsskript erklärt dir jeden Schritt und fragt nach deinem Passwort (das ist normal -- Homebrew braucht Administratorrechte für die Ersteinrichtung).

### Installation prüfen

Nach der Installation kannst du testen, ob alles geklappt hat:

```shell
brew --version
```

Falls der Befehl `brew` nicht gefunden wird, musst du Homebrew noch zu deinem PATH hinzufügen. Das Installationsskript zeigt dir am Ende die nötigen Befehle an. Für Apple-Silicon-Macs (M1/M2/M3/M4) sieht das typischerweise so aus:

```shell
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

## Wichtigste Befehle

### Pakete suchen

Bevor du etwas installierst, kannst du nach verfügbaren Paketen suchen:

```shell
brew search node
```

Das zeigt dir alle Pakete, die "node" im Namen enthalten.

### Informationen zu einem Paket anzeigen

Um mehr über ein Paket zu erfahren (Version, Beschreibung, Abhängigkeiten):

```shell
brew info node
```

### Paket installieren

```shell
brew install node
```

Homebrew lädt das Paket herunter, löst Abhängigkeiten auf und installiert alles automatisch.

### Installierte Pakete auflisten

```shell
brew list
```

Um nur die Pakete zu sehen, die du selbst installiert hast (ohne Abhängigkeiten):

```shell
brew leaves
```

### Paketquellen aktualisieren

Dieser Befehl aktualisiert die Liste der verfügbaren Pakete und deren Versionen:

```shell
brew update
```

### Installierte Pakete upgraden

Nach einem `update` kannst du deine installierten Pakete auf die neueste Version bringen:

```shell
brew upgrade
```

Um nur ein bestimmtes Paket zu upgraden:

```shell
brew upgrade node
```

### Paket deinstallieren

```shell
brew uninstall node
```

### Zusammenfassung der Befehle

| Befehl | Beschreibung |
|---|---|
| `brew search <name>` | Nach Paketen suchen |
| `brew info <name>` | Paketinformationen anzeigen |
| `brew install <name>` | Paket installieren |
| `brew uninstall <name>` | Paket deinstallieren |
| `brew update` | Paketquellen aktualisieren |
| `brew upgrade` | Alle Pakete auf neueste Version bringen |
| `brew list` | Installierte Pakete anzeigen |
| `brew leaves` | Nur manuell installierte Pakete anzeigen |

## Homebrew Cask -- GUI-Anwendungen installieren

Homebrew kann nicht nur Kommandozeilen-Tools installieren, sondern auch ganz normale macOS-Apps mit grafischer Oberfläche. Diese Funktion heißt **Cask**.

### Beispiele

```shell
brew install --cask visual-studio-code
brew install --cask firefox
brew install --cask docker
brew install --cask figma
brew install --cask slack
```

### Nach Cask-Apps suchen

```shell
brew search --cask zoom
```

### Cask-Apps auflisten

```shell
brew list --cask
```

### Cask-App deinstallieren

```shell
brew uninstall --cask firefox
```

Der große Vorteil: Du musst nicht mehr auf Websites gehen, DMG-Dateien herunterladen und Apps manuell in den Applications-Ordner ziehen. Ein Befehl erledigt alles.

## Nützliche Pakete für Webentwicklung

Hier eine Auswahl an Paketen, die für die Webentwicklung besonders hilfreich sind:

### Grundlegende Tools

```shell
# Versionskontrolle
brew install git

# Node.js und npm (JavaScript-Runtime)
brew install node

# Python
brew install python

# Schnellerer Node-Version-Manager
brew install fnm
```

### Datenbanken

```shell
# PostgreSQL
brew install postgresql@17

# MySQL
brew install mysql

# Redis (Caching und Message Broker)
brew install redis
```

### Hilfreiche CLI-Tools

```shell
# Schnellere Alternative zu cat mit Syntax-Highlighting
brew install bat

# Moderne Alternative zu ls
brew install eza

# Schnelle Dateisuche
brew install fd

# Schnelle Textsuche in Dateien
brew install ripgrep

# JSON im Terminal verarbeiten
brew install jq

# HTTP-Anfragen komfortabel senden
brew install httpie

# Terminal-Multiplexer (mehrere Sitzungen in einem Fenster)
brew install tmux
```

### GUI-Apps für Entwicklung

```shell
brew install --cask visual-studio-code
brew install --cask iterm2
brew install --cask docker
brew install --cask postman
brew install --cask tableplus
```

## Brewfile -- Reproduzierbares Team-Setup

Ein **Brewfile** ist eine Textdatei, in der alle benötigten Pakete und Apps aufgelistet sind. Damit kann jedes Teammitglied mit einem einzigen Befehl die gleiche Entwicklungsumgebung einrichten.

### Brewfile erstellen

Erstelle eine Datei namens `Brewfile` im Wurzelverzeichnis deines Projekts:

```shell
touch Brewfile
```

### Beispiel-Brewfile

```ruby
# Taps (zusätzliche Paketquellen)
tap "homebrew/bundle"

# Kommandozeilen-Tools
brew "git"
brew "node"
brew "python"
brew "fnm"
brew "postgresql@17"
brew "redis"
brew "ripgrep"
brew "jq"
brew "httpie"

# GUI-Anwendungen
cask "visual-studio-code"
cask "docker"
cask "iterm2"
cask "figma"
cask "slack"

# Mac App Store Apps (benötigt mas-cli)
# mas "Xcode", id: 497799835
```

### Brewfile verwenden

Um alle Pakete aus einem Brewfile zu installieren:

```shell
brew bundle
```

Homebrew liest die Datei `Brewfile` im aktuellen Verzeichnis und installiert alles, was noch fehlt.

### Brewfile aus bestehender Installation erzeugen

Wenn du bereits alles installiert hast und ein Brewfile daraus generieren möchtest:

```shell
brew bundle dump
```

Das erzeugt ein Brewfile basierend auf deinen aktuell installierten Paketen.

### Prüfen, ob alles installiert ist

```shell
brew bundle check
```

Dieser Befehl zeigt dir, ob alle Pakete aus dem Brewfile vorhanden sind.

**Tipp:** Nimm das Brewfile in die Versionskontrolle (Git) auf. So hat jedes Teammitglied Zugriff darauf und kann sein Setup schnell einrichten.

## Troubleshooting

### `brew doctor` -- Probleme diagnostizieren

Wenn etwas nicht funktioniert, ist `brew doctor` dein erster Anlaufpunkt:

```shell
brew doctor
```

Der Befehl prüft deine Homebrew-Installation und gibt dir konkrete Hinweise, was zu tun ist.

### `command not found: brew`

Das bedeutet, dass Homebrew nicht in deinem PATH liegt. Lösung für Apple-Silicon-Macs:

```shell
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
source ~/.zprofile
```

Für Intel-Macs:

```shell
echo 'eval "$(/usr/local/bin/brew shellenv)"' >> ~/.zprofile
source ~/.zprofile
```

### Paket lässt sich nicht installieren

Versuche zunächst, Homebrew zu aktualisieren:

```shell
brew update
brew install <paketname>
```

Falls das nicht hilft, kann ein Cleanup helfen:

```shell
brew cleanup
brew install <paketname>
```

### Berechtigungsprobleme

Falls Berechtigungsfehler auftreten:

```shell
sudo chown -R $(whoami) $(brew --prefix)/*
```

### Homebrew ist langsam

Alte Downloads und veraltete Versionen belegen Speicherplatz und können Homebrew verlangsamen:

```shell
# Alte Versionen und Cache aufräumen
brew cleanup

# Anzeigen, was aufgeräumt werden würde (ohne zu löschen)
brew cleanup -n
```

### Paket wird von einem anderen Paket blockiert

Manchmal gibt es Konflikte zwischen Paketen. Du kannst ein Paket erzwungen verlinken:

```shell
brew link --overwrite <paketname>
```

### Homebrew komplett zurücksetzen

Falls gar nichts mehr geht, kannst du Homebrew neu installieren:

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh)"
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

**Achtung:** Dabei gehen alle über Homebrew installierten Pakete verloren. Erstelle vorher ein Brewfile mit `brew bundle dump`, damit du alles wieder installieren kannst.
