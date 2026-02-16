[← Zurück zur Übersicht](../README.md)

# Terminal

Das Terminal ist dein direkter Draht zum Betriebssystem. Statt Ordner per Maus zu öffnen, tippst du Befehle ein und bekommst sofort eine Antwort. In der Webentwicklung ist das Terminal unverzichtbar — ob Git, npm, Docker oder Deployment: fast alles läuft über die Kommandozeile.

---

## Inhaltsverzeichnis

- [Was ist das Terminal?](#was-ist-das-terminal)
- [macOS Terminal vs. iTerm2](#macos-terminal-vs-iterm2)
- [Zsh & Oh My Zsh](#zsh--oh-my-zsh)
- [Die wichtigsten Befehle](#die-wichtigsten-befehle)
- [Tipps & Tricks](#tipps--tricks)
- [Weiterführende Links](#weiterführende-links)

---

## Was ist das Terminal?

Das Terminal (auch *Shell*, *Kommandozeile* oder *CLI* — Command Line Interface) ist ein textbasiertes Programm, mit dem du direkt Befehle an dein Betriebssystem senden kannst. Es ist das Gegenstück zur grafischen Benutzeroberfläche (GUI).

**Warum sollte ich das Terminal verwenden?**

- **Schnelligkeit** — Viele Aufgaben gehen per Befehl deutlich schneller als per Mausklick.
- **Automatisierung** — Wiederkehrende Aufgaben lassen sich als Skripte speichern und automatisieren.
- **Entwickler-Tools** — Git, npm, Docker und viele weitere Tools laufen ausschliesslich im Terminal.
- **Remote-Zugriff** — Server werden fast immer über die Kommandozeile verwaltet (SSH).

**Die drei Begriffe, die man kennen sollte:**

| Begriff | Bedeutung |
|---|---|
| **Terminal** | Das Programm / Fenster, in das du tippst |
| **Shell** | Der Interpreter, der deine Befehle versteht (z.B. Zsh, Bash) |
| **CLI** | Command Line Interface — die textbasierte Schnittstelle eines Programms |

---

## macOS Terminal vs. iTerm2

macOS bringt ein Terminal von Haus aus mit. Es gibt aber auch Alternativen wie **iTerm2**, die den Arbeitsalltag deutlich angenehmer machen.

### macOS Terminal (vorinstalliert)

Das integrierte Terminal findest du unter **Programme > Dienstprogramme > Terminal** oder über die Spotlight-Suche (`Cmd + Leertaste`, dann "Terminal" eintippen).

### iTerm2 (empfohlen)

[iTerm2](https://iterm2.com/) ist ein kostenloses Ersatz-Terminal für macOS mit vielen nützlichen Zusatzfunktionen.

**Installation via Homebrew:**

```shell
brew install --cask iterm2
```

**Vorteile gegenüber dem Standard-Terminal:**

| Feature | macOS Terminal | iTerm2 |
|---|:---:|:---:|
| Split Panes (geteilte Fenster) | - | Ja |
| Autovervollständigung | Einfach | Erweitert |
| Suchfunktion im Output | Einfach | Erweitert |
| Hotkey-Fenster (Drop-Down) | - | Ja |
| Farbprofile & Themes | Wenig | Sehr viele |
| Shell-Integration | - | Ja |

> **Tipp:** Wenn du gerade erst anfängst, reicht das macOS Terminal vollkommen aus. iTerm2 lohnt sich, sobald du mehr Zeit in der Kommandozeile verbringst.

---

## Zsh & Oh My Zsh

### Zsh (Z Shell)

Seit macOS Catalina (2019) ist **Zsh** die Standard-Shell auf dem Mac. Du musst also nichts umstellen. Überprüfen kannst du es mit:

```shell
echo $SHELL
# Erwartete Ausgabe: /bin/zsh
```

Zsh bietet gegenüber der älteren Bash-Shell unter anderem:

- Bessere Autovervollständigung (Tab-Taste)
- Rechtschreibkorrektur bei Tippfehlern
- Globbing-Patterns (erweiterte Dateisuche)
- Plugin- und Theme-Unterstützung

### Oh My Zsh

[Oh My Zsh](https://ohmyz.sh/) ist ein Framework, das die Zsh-Konfiguration massiv vereinfacht. Es bringt **Themes**, **Plugins** und sinnvolle **Standardeinstellungen** mit.

**Installation:**

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

**Theme ändern:**

Die Konfigurationsdatei von Zsh ist `~/.zshrc`. Dort findest du die Zeile für das Theme:

```shell
# ~/.zshrc
ZSH_THEME="robbyrussell"   # Standard-Theme
```

Beliebte Alternativen:

| Theme | Beschreibung |
|---|---|
| `robbyrussell` | Standard — schlicht und übersichtlich |
| `agnoster` | Zeigt Git-Branch, Status, Pfad — sehr beliebt |
| `powerlevel10k` | Extrem anpassbar, schnell, mit Setup-Wizard |

> **Empfehlung:** [Powerlevel10k](https://github.com/romkatv/powerlevel10k) ist aktuell das beliebteste Theme. Es bietet einen interaktiven Setup-Wizard und sieht professionell aus.

**Installation von Powerlevel10k:**

```shell
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git \
  ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Dann in der `~/.zshrc`:

```shell
ZSH_THEME="powerlevel10k/powerlevel10k"
```

Terminal neu starten — der Setup-Wizard startet automatisch.

**Nützliche Plugins aktivieren:**

In der `~/.zshrc` findest du die Plugin-Zeile:

```shell
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
```

| Plugin | Funktion |
|---|---|
| `git` | Git-Aliase und Autovervollständigung (vorinstalliert) |
| `zsh-autosuggestions` | Schlägt Befehle vor, basierend auf deiner History |
| `zsh-syntax-highlighting` | Färbt gültige/ungültige Befehle farbig ein |

**Installation der externen Plugins:**

```shell
# zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-autosuggestions \
  ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting \
  ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

Nach jeder Änderung an der `~/.zshrc` musst du entweder ein neues Terminal-Fenster öffnen oder den folgenden Befehl ausführen:

```shell
source ~/.zshrc
```

---

## Die wichtigsten Befehle

### Navigation

```shell
pwd                   # Aktuelles Verzeichnis anzeigen (Print Working Directory)
ls                    # Dateien und Ordner im aktuellen Verzeichnis auflisten
ls -la                # Detaillierte Liste inkl. versteckter Dateien
cd ordner             # In einen Ordner wechseln
cd ..                 # Einen Ordner nach oben gehen
cd ~                  # Ins Home-Verzeichnis wechseln
cd -                  # Ins vorherige Verzeichnis zurückwechseln
```

### Dateien erstellen & bearbeiten

```shell
touch datei.txt       # Neue leere Datei erstellen
nano datei.txt        # Datei im Terminal-Editor bearbeiten
code datei.txt        # Datei in VS Code öffnen (falls eingerichtet)
open datei.txt        # Datei mit Standard-Programm öffnen (macOS)
cat datei.txt         # Dateiinhalt im Terminal anzeigen
head -n 10 datei.txt  # Erste 10 Zeilen anzeigen
tail -n 10 datei.txt  # Letzte 10 Zeilen anzeigen
```

### Dateien & Ordner verwalten

```shell
cp quelle.txt ziel.txt           # Datei kopieren
cp -r ordner/ kopie/             # Ordner rekursiv kopieren
mv alter_name.txt neuer_name.txt # Datei umbenennen oder verschieben
rm datei.txt                     # Datei löschen (unwiderruflich!)
rm -r ordner/                    # Ordner rekursiv löschen (Vorsicht!)
mkdir neuer_ordner               # Neuen Ordner erstellen
mkdir -p pfad/zu/ordner          # Verschachtelte Ordner auf einmal erstellen
```

> **Achtung:** `rm` löscht Dateien sofort und endgültig — es gibt keinen Papierkorb! Immer doppelt prüfen, bevor du `rm -r` verwendest.

### Suchen & Finden

```shell
find . -name "*.js"              # Alle .js-Dateien im aktuellen Verzeichnis finden
grep "suchbegriff" datei.txt     # Text in einer Datei suchen
grep -r "suchbegriff" ordner/    # Text rekursiv in allen Dateien suchen
grep -i "text" datei.txt         # Gross-/Kleinschreibung ignorieren
which node                       # Zeigt den Pfad eines installierten Programms
```

### Rechte & Berechtigungen

Jede Datei hat Berechtigungen für **Besitzer (u)**, **Gruppe (g)** und **Andere (o)**.

```shell
ls -l                            # Berechtigungen anzeigen
# Beispielausgabe: -rwxr-xr-- 1 user group 1234 Jan 01 12:00 datei.txt
```

Die Berechtigungen liest man so:

```
-rwxr-xr--
│└┬┘└┬┘└┬┘
│ │  │  └── Andere: r-- (nur lesen)
│ │  └───── Gruppe: r-x (lesen + ausführen)
│ └──────── Besitzer: rwx (lesen + schreiben + ausführen)
└────────── Dateityp (- = Datei, d = Ordner)
```

| Buchstabe | Bedeutung | Zahlenwert |
|:---:|---|:---:|
| `r` | Read (lesen) | 4 |
| `w` | Write (schreiben) | 2 |
| `x` | Execute (ausführen) | 1 |
| `-` | Keine Berechtigung | 0 |

```shell
chmod 755 script.sh              # Besitzer: alles, Gruppe/Andere: lesen+ausführen
chmod +x script.sh               # Ausführberechtigung hinzufügen
chown user:group datei.txt       # Besitzer und Gruppe ändern
```

### Systembefehle

```shell
clear                            # Terminal-Fenster leeren (oder Cmd + K)
history                          # Bisherige Befehle anzeigen
man ls                           # Handbuchseite für einen Befehl anzeigen (q = beenden)
whoami                           # Aktuellen Benutzernamen anzeigen
top                              # Laufende Prozesse anzeigen (q = beenden)
kill PID                         # Prozess mit einer bestimmten ID beenden
echo "Hallo Welt"               # Text ausgeben
```

### Pipes & Umleitung

Mit Pipes (`|`) kannst du die Ausgabe eines Befehls als Eingabe für einen anderen verwenden:

```shell
ls -la | grep ".js"              # Nur .js-Dateien aus der Liste filtern
history | grep "git"             # Alle Git-Befehle aus der History anzeigen
cat datei.txt | wc -l            # Zeilen in einer Datei zählen
```

Umleitung mit `>` und `>>`:

```shell
echo "Hallo" > datei.txt        # Schreibt "Hallo" in die Datei (überschreibt!)
echo "Welt" >> datei.txt        # Hängt "Welt" an die Datei an
ls -la > verzeichnis.txt         # Speichert die Ausgabe in einer Datei
```

---

## Tipps & Tricks

### Tastenkürzel

| Kürzel | Funktion |
|---|---|
| `Tab` | Autovervollständigung (Dateinamen, Befehle, Pfade) |
| `Tab Tab` | Alle Möglichkeiten anzeigen |
| `Ctrl + C` | Laufenden Befehl abbrechen |
| `Ctrl + L` | Bildschirm leeren (wie `clear`) |
| `Ctrl + A` | Cursor an den Zeilenanfang |
| `Ctrl + E` | Cursor ans Zeilenende |
| `Ctrl + W` | Letztes Wort löschen |
| `Ctrl + R` | In der Befehlshistory suchen |
| `Cmd + K` | Terminal-Output komplett leeren (macOS) |
| `Pfeil hoch/runter` | Durch vorherige Befehle blättern |

### Aliase (Abkürzungen)

Du kannst dir eigene Abkürzungen in der `~/.zshrc` definieren:

```shell
# ~/.zshrc — eigene Aliase
alias ll="ls -la"
alias gs="git status"
alias gp="git push"
alias dev="npm run dev"
alias c="clear"
```

Nach dem Speichern mit `source ~/.zshrc` aktivieren.

### Mehrere Befehle verketten

```shell
# Mit && — nächster Befehl nur bei Erfolg des vorherigen
mkdir projekt && cd projekt && npm init -y

# Mit ; — Befehle nacheinander ausführen (egal ob Fehler)
echo "Start"; ls; echo "Ende"
```

### Versteckte Dateien

Dateien und Ordner, die mit einem Punkt beginnen (z.B. `.zshrc`, `.gitignore`, `.env`), sind standardmässig versteckt. Mit `ls -a` werden sie sichtbar:

```shell
ls -a    # Alle Dateien anzeigen, inkl. versteckter
```

Im Finder kannst du versteckte Dateien mit `Cmd + Shift + .` ein- und ausblenden.

### `code .` einrichten

Um Ordner direkt aus dem Terminal in VS Code zu öffnen:

1. Öffne VS Code
2. `Cmd + Shift + P` drücken
3. "Shell Command: Install 'code' command in PATH" auswählen

Danach kannst du:

```shell
code .               # Aktuellen Ordner in VS Code öffnen
code datei.txt       # Einzelne Datei öffnen
code projekt/        # Bestimmten Ordner öffnen
```

---

## Weiterführende Links

- [Terminal Cheatsheet for Mac](https://github.com/0nn0/terminal-mac-cheatsheet) — Kompakte Befehlsübersicht
- [Oh My Zsh Wiki](https://github.com/ohmyzsh/ohmyzsh/wiki) — Offizielle Dokumentation
- [Powerlevel10k](https://github.com/romkatv/powerlevel10k) — Das beliebteste Zsh-Theme
- [Zsh Autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) — Plugin für Befehlsvorschläge
- [iTerm2 Dokumentation](https://iterm2.com/documentation.html) — Alle Features von iTerm2
- [ExplainShell](https://explainshell.com/) — Shell-Befehle visuell erklärt

---

[← Zurück zur Übersicht](../README.md)
