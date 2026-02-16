[← Zurück zur Übersicht](../README.md)

# Docker — Ein praktischer Einstieg

Docker ist eines der wichtigsten Werkzeuge in der modernen Softwareentwicklung. Mit Docker kannst du Anwendungen in sogenannten **Containern** verpacken, die überall gleich laufen — egal ob auf deinem Laptop, dem Rechner deiner Kommilitonin oder einem Server in der Cloud.

---

## Was ist Docker?

Docker ist eine Plattform, die es ermöglicht, Anwendungen in isolierten Umgebungen (Containern) auszuführen. Aber was unterscheidet Container eigentlich von virtuellen Maschinen?

### Container vs. Virtuelle Maschinen

| Eigenschaft | Container | Virtuelle Maschine (VM) |
|---|---|---|
| Betriebssystem | Teilt den Kernel des Host-Systems | Eigenes vollständiges Betriebssystem |
| Startzeit | Sekunden | Minuten |
| Größe | Megabytes | Gigabytes |
| Performance | Nahezu nativ | Overhead durch Virtualisierung |
| Isolierung | Prozess-Level | Hardware-Level |
| Ressourcenverbrauch | Gering | Hoch |

**Einfach gesagt:** Eine VM ist wie ein ganzer Computer im Computer. Ein Container ist eher wie eine leichtgewichtige, abgeschottete Umgebung, die sich den Betriebssystem-Kern mit dem Host teilt.

```
┌──────────────────────────┐    ┌──────────────────────────┐
│      Virtuelle Maschine  │    │        Container         │
│  ┌────────┐ ┌────────┐   │    │  ┌────────┐ ┌────────┐  │
│  │ App A  │ │ App B  │   │    │  │ App A  │ │ App B  │  │
│  ├────────┤ ├────────┤   │    │  ├────────┤ ├────────┤  │
│  │ Libs   │ │ Libs   │   │    │  │ Libs   │ │ Libs   │  │
│  ├────────┤ ├────────┤   │    │  └────────┘ └────────┘  │
│  │Gast-OS │ │Gast-OS │   │    │  ┌──────────────────┐   │
│  └────────┘ └────────┘   │    │  │  Docker Engine    │   │
│  ┌──────────────────┐    │    │  └──────────────────┘   │
│  │   Hypervisor     │    │    │  ┌──────────────────┐   │
│  └──────────────────┘    │    │  │    Host-OS        │   │
│  ┌──────────────────┐    │    │  └──────────────────┘   │
│  │    Host-OS       │    │    └──────────────────────────┘
│  └──────────────────┘    │
└──────────────────────────┘
```

---

## Installation (Docker Desktop für macOS)

Die einfachste Art, Docker auf dem Mac zu nutzen, ist **Docker Desktop**.

### Schritt 1 — Download

Lade Docker Desktop von der offiziellen Webseite herunter:

> [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)

Wähle die passende Version für deinen Mac:
- **Apple Silicon (M1/M2/M3/M4)** — die meisten neueren Macs
- **Intel** — ältere Mac-Modelle

### Schritt 2 — Installation

1. Öffne die heruntergeladene `.dmg`-Datei.
2. Ziehe das Docker-Symbol in den Applications-Ordner.
3. Starte Docker Desktop aus dem Applications-Ordner.
4. Bestätige die Sicherheitsabfrage von macOS.

### Schritt 3 — Überprüfung

Öffne ein Terminal und prüfe, ob alles funktioniert:

```shell
docker --version
# Beispielausgabe: Docker version 27.x.x, build xxxxxxx

docker run hello-world
# Sollte eine Willkommensnachricht ausgeben
```

Falls `docker run hello-world` eine Erfolgsmeldung ausgibt, ist alles korrekt eingerichtet.

### Alternativ: Installation über Homebrew

```shell
brew install --cask docker
```

---

## Wichtige Konzepte

Bevor du loslegst, solltest du vier zentrale Begriffe verstehen:

### Images

Ein **Image** ist eine schreibgeschützte Vorlage, aus der Container erstellt werden. Stell dir ein Image wie eine Blaupause oder ein Rezept vor. Es enthält alles, was eine Anwendung braucht: Code, Laufzeitumgebung, Bibliotheken, Umgebungsvariablen und Konfiguration.

```shell
# Verfügbare Images auf deinem Rechner anzeigen
docker images
```

Images werden aus **Dockerfiles** gebaut oder von **Docker Hub** (einer Art App Store für Docker Images) heruntergeladen.

### Container

Ein **Container** ist eine laufende Instanz eines Images. Wenn ein Image das Rezept ist, dann ist der Container das fertige Gericht. Du kannst aus einem Image beliebig viele Container starten.

```shell
# Laufende Container anzeigen
docker ps

# Alle Container anzeigen (auch gestoppte)
docker ps -a
```

### Volumes

**Volumes** sind der Mechanismus, um Daten dauerhaft zu speichern. Container sind von Natur aus **vergänglich** — wenn du einen Container löschst, sind auch die Daten darin weg. Volumes lösen dieses Problem, indem sie Daten außerhalb des Containers auf dem Host-System speichern.

```shell
# Volume erstellen
docker volume create meine-daten

# Volume beim Starten eines Containers einbinden
docker run -v meine-daten:/app/data mein-image
```

**Typische Anwendungsfälle:**
- Datenbank-Dateien persistent speichern
- Konfigurationsdateien in Container einbinden
- Während der Entwicklung Quellcode in den Container mounten

### Networks

**Networks** ermöglichen die Kommunikation zwischen Containern. Standardmäßig läuft jeder Container isoliert. Über ein gemeinsames Netzwerk können Container miteinander sprechen — zum Beispiel eine Web-App mit ihrer Datenbank.

```shell
# Netzwerk erstellen
docker network create mein-netzwerk

# Container in einem Netzwerk starten
docker run --network mein-netzwerk --name web mein-web-image
docker run --network mein-netzwerk --name db postgres
```

Innerhalb desselben Netzwerks können Container sich über ihren **Namen** erreichen (z. B. `db:5432`).

---

## Wichtigste Befehle

Hier eine Übersicht der Befehle, die du am häufigsten brauchen wirst:

### Container starten

```shell
# Einfachster Start
docker run nginx

# Im Hintergrund starten (-d = detached)
docker run -d nginx

# Mit Port-Mapping: Host-Port 8080 → Container-Port 80
docker run -d -p 8080:80 nginx

# Mit benutzerdefiniertem Namen
docker run -d -p 8080:80 --name mein-webserver nginx

# Mit Umgebungsvariablen
docker run -d -e POSTGRES_PASSWORD=geheim postgres
```

### Container verwalten

```shell
# Laufende Container anzeigen
docker ps

# Alle Container anzeigen (auch gestoppte)
docker ps -a

# Container stoppen
docker stop mein-webserver

# Container starten (bereits erstellten)
docker start mein-webserver

# Container löschen
docker rm mein-webserver

# Container stoppen und sofort löschen
docker rm -f mein-webserver
```

### Images verwalten

```shell
# Alle lokalen Images auflisten
docker images

# Image von Docker Hub herunterladen
docker pull node:20

# Image aus einem Dockerfile bauen
docker build -t meine-app .

# Image mit Tag bauen
docker build -t meine-app:1.0 .

# Ungenutzte Images aufräumen
docker image prune
```

### Logs und Debugging

```shell
# Logs eines Containers anzeigen
docker logs mein-webserver

# Logs live verfolgen (wie tail -f)
docker logs -f mein-webserver

# Befehl in laufendem Container ausführen
docker exec mein-webserver ls /usr/share/nginx/html

# Interaktive Shell in einem Container öffnen
docker exec -it mein-webserver /bin/bash
```

> **Tipp:** `docker exec -it <container> /bin/bash` ist extrem nützlich zum Debuggen. Damit kannst du dich direkt in einen laufenden Container "einloggen" und dich umschauen.

### Schnelle Befehlsreferenz

| Befehl | Beschreibung |
|---|---|
| `docker run <image>` | Container aus Image starten |
| `docker ps` | Laufende Container anzeigen |
| `docker stop <container>` | Container stoppen |
| `docker rm <container>` | Container löschen |
| `docker images` | Lokale Images auflisten |
| `docker pull <image>` | Image herunterladen |
| `docker build -t <name> .` | Image aus Dockerfile bauen |
| `docker logs <container>` | Logs anzeigen |
| `docker exec -it <container> sh` | Shell im Container öffnen |

---

## Dockerfile erstellen

Ein **Dockerfile** ist eine Textdatei, die Schritt für Schritt beschreibt, wie ein Docker Image aufgebaut wird. Jede Zeile ist eine Anweisung.

### Die wichtigsten Anweisungen

| Anweisung | Beschreibung |
|---|---|
| `FROM` | Basis-Image, auf dem aufgebaut wird |
| `WORKDIR` | Arbeitsverzeichnis im Container setzen |
| `COPY` | Dateien vom Host in den Container kopieren |
| `RUN` | Befehl während des Build-Prozesses ausführen |
| `EXPOSE` | Port dokumentieren, den die App nutzt |
| `CMD` | Standardbefehl beim Starten des Containers |

### Beispiel: Node.js Anwendung

Angenommen, du hast ein einfaches Node.js-Projekt mit folgender Struktur:

```
mein-projekt/
├── package.json
├── package-lock.json
├── src/
│   └── server.js
├── Dockerfile
└── .dockerignore
```

So sieht das Dockerfile aus:

```dockerfile
# Schritt 1: Basis-Image wählen
# Wir nutzen das offizielle Node.js 20 Image auf Alpine Linux (schlank)
FROM node:20-alpine

# Schritt 2: Arbeitsverzeichnis im Container festlegen
WORKDIR /app

# Schritt 3: Abhängigkeiten installieren
# Zuerst nur package.json kopieren (für besseres Layer-Caching)
COPY package.json package-lock.json ./

# npm ci ist für reproduzierbare Builds besser als npm install
RUN npm ci --only=production

# Schritt 4: Quellcode kopieren
COPY src/ ./src/

# Schritt 5: Port dokumentieren
# EXPOSE ist rein informativ — der Port muss beim Start trotzdem gemappt werden
EXPOSE 3000

# Schritt 6: Startbefehl definieren
CMD ["node", "src/server.js"]
```

### Image bauen und starten

```shell
# Image bauen (-t gibt dem Image einen Namen/Tag)
docker build -t meine-node-app .

# Container starten
docker run -d -p 3000:3000 --name backend meine-node-app

# Prüfen, ob der Container läuft
docker ps

# Logs anschauen
docker logs backend
```

Deine App ist jetzt unter `http://localhost:3000` erreichbar.

---

## .dockerignore

Die Datei `.dockerignore` funktioniert wie `.gitignore` — sie legt fest, welche Dateien **nicht** in den Docker Build-Kontext kopiert werden sollen. Das macht den Build schneller und das Image kleiner.

Erstelle eine `.dockerignore`-Datei im Projektverzeichnis:

```
# Abhängigkeiten (werden im Container neu installiert)
node_modules/

# Git-Daten
.git/
.gitignore

# Entwicklungsdateien
.env
.env.local
*.md
LICENSE

# IDE-Konfiguration
.vscode/
.idea/

# Betriebssystem-Dateien
.DS_Store
Thumbs.db

# Test- und Build-Artefakte
coverage/
dist/
*.log
```

> **Wichtig:** Ohne `.dockerignore` wird der gesamte `node_modules/`-Ordner in den Build-Kontext geschickt, was den Build erheblich verlangsamt — obwohl wir die Abhängigkeiten im Container sowieso neu installieren.

---

## Docker Compose

Sobald dein Projekt aus mehreren Diensten besteht (z. B. Web-App, Datenbank, Cache), wird es umständlich, jeden Container einzeln zu starten. **Docker Compose** löst dieses Problem: Du beschreibst alle Dienste in einer einzigen YAML-Datei und startest alles mit einem Befehl.

### Beispiel: Web-App mit PostgreSQL-Datenbank

Erstelle eine Datei namens `docker-compose.yml` (oder `compose.yml`) im Projektverzeichnis:

```yaml
# Docker Compose Konfiguration
services:
  # Dienst 1: Die Web-Anwendung
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgresql://appuser:sicheresPasswort@db:5432/meinedb
    depends_on:
      db:
        condition: service_healthy
    restart: unless-stopped

  # Dienst 2: PostgreSQL Datenbank
  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: meinedb
      POSTGRES_USER: appuser
      POSTGRES_PASSWORD: sicheresPasswort
    ports:
      - "5432:5432"
    volumes:
      - postgres-daten:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U appuser -d meinedb"]
      interval: 5s
      timeout: 5s
      retries: 5
    restart: unless-stopped

# Benanntes Volume für persistente Datenbank-Daten
volumes:
  postgres-daten:
```

### Was passiert hier?

- **`web`** wird aus dem lokalen Dockerfile gebaut (`build: .`).
- **`db`** nutzt das offizielle PostgreSQL-Image von Docker Hub.
- **`depends_on`** mit `condition: service_healthy` stellt sicher, dass die Datenbank bereit ist, bevor die Web-App startet.
- **`volumes`** sorgt dafür, dass die Datenbank-Daten erhalten bleiben, auch wenn der Container neu erstellt wird.
- Die Web-App verbindet sich über den Hostnamen `db` (= der Service-Name) mit der Datenbank.

### Erweitertes Beispiel mit Redis Cache

```yaml
services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=postgresql://appuser:sicheresPasswort@db:5432/meinedb
      - REDIS_URL=redis://cache:6379
    depends_on:
      db:
        condition: service_healthy
      cache:
        condition: service_started

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: meinedb
      POSTGRES_USER: appuser
      POSTGRES_PASSWORD: sicheresPasswort
    volumes:
      - postgres-daten:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U appuser -d meinedb"]
      interval: 5s
      timeout: 5s
      retries: 5

  cache:
    image: redis:7-alpine
    ports:
      - "6379:6379"

volumes:
  postgres-daten:
```

---

## Docker Compose Befehle

```shell
# Alle Dienste starten (im Vordergrund)
docker compose up

# Alle Dienste im Hintergrund starten
docker compose up -d

# Images neu bauen und starten
docker compose up -d --build

# Logs aller Dienste anzeigen
docker compose logs

# Logs eines bestimmten Dienstes live verfolgen
docker compose logs -f web

# Alle Dienste stoppen und Container entfernen
docker compose down

# Dienste stoppen UND Volumes löschen (Achtung: Datenverlust!)
docker compose down -v

# Nur einen Dienst neu bauen
docker compose build web

# Status aller Dienste anzeigen
docker compose ps
```

### Kurzreferenz

| Befehl | Beschreibung |
|---|---|
| `docker compose up -d` | Alle Dienste im Hintergrund starten |
| `docker compose down` | Alle Dienste stoppen und aufräumen |
| `docker compose logs -f` | Logs live verfolgen |
| `docker compose build` | Images neu bauen |
| `docker compose ps` | Status der Dienste anzeigen |
| `docker compose exec web sh` | Shell in einem Dienst öffnen |

> **Hinweis:** Der Befehl lautet `docker compose` (ohne Bindestrich). Die ältere Variante `docker-compose` (mit Bindestrich) ist veraltet und wird in neueren Docker-Versionen nicht mehr empfohlen.

---

## Best Practices

### Multi-Stage Builds

Multi-Stage Builds ermöglichen es, das finale Image klein zu halten, indem der Build-Prozess vom Produktions-Image getrennt wird. Das ist besonders bei kompilierten Sprachen oder Frontend-Builds nützlich.

**Beispiel: React-Frontend mit Multi-Stage Build**

```dockerfile
# ---- Stage 1: Build ----
FROM node:20-alpine AS build

WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci

COPY . .
RUN npm run build

# ---- Stage 2: Production ----
FROM nginx:alpine

# Nur die fertigen Build-Dateien werden ins finale Image kopiert
COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**Warum ist das besser?**
- Das Build-Stage enthält Node.js, npm und alle devDependencies — das braucht man in Produktion nicht.
- Das finale Image enthält nur nginx und die statischen Dateien.
- Statt ca. 1 GB (Node + alle Abhängigkeiten) ist das Image nur ca. 40 MB groß.

### Layer-Caching effektiv nutzen

Jede Anweisung im Dockerfile erzeugt einen **Layer**. Docker cached diese Layer und baut nur die Schichten neu, die sich geändert haben. Die Reihenfolge der Anweisungen ist daher entscheidend.

**Schlecht:** (Cache wird bei jeder Code-Änderung vollständig invalidiert)

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY . .
RUN npm ci
CMD ["node", "src/server.js"]
```

**Gut:** (Abhängigkeiten werden nur neu installiert, wenn sich package.json ändert)

```dockerfile
FROM node:20-alpine
WORKDIR /app

# Schritt 1: Abhängigkeiten (ändert sich selten)
COPY package.json package-lock.json ./
RUN npm ci

# Schritt 2: Quellcode (ändert sich oft)
COPY . .

CMD ["node", "src/server.js"]
```

**Faustregel:** Dinge, die sich selten ändern, nach oben — Dinge, die sich häufig ändern, nach unten.

### Weitere Best Practices

- **Verwende spezifische Image-Tags:** `node:20-alpine` statt `node:latest`. Mit `latest` kann sich das Basis-Image unerwartet ändern und der Build bricht.
- **Nutze Alpine-basierte Images:** Sie sind deutlich kleiner. `node:20-alpine` (~180 MB) vs. `node:20` (~1 GB).
- **Ein Prozess pro Container:** Packe nicht Web-Server und Datenbank in denselben Container. Nutze Docker Compose für mehrere Dienste.
- **Nutze `.dockerignore`:** Verhindert, dass unnötige Dateien den Build verlangsamen.
- **Verwende `npm ci` statt `npm install`:** `npm ci` sorgt für reproduzierbare Builds basierend auf der `package-lock.json`.
- **Vermeide `root` in Produktion:** Erstelle einen nicht-privilegierten Benutzer im Dockerfile:

```dockerfile
FROM node:20-alpine
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
WORKDIR /app
COPY --chown=appuser:appgroup . .
USER appuser
CMD ["node", "src/server.js"]
```

---

## Nützliche Tools

### Docker Desktop Dashboard

Docker Desktop bringt eine grafische Oberfläche mit, die besonders für Einsteiger hilfreich ist:

- **Container-Übersicht:** Alle laufenden und gestoppten Container auf einen Blick.
- **Logs:** Container-Logs direkt in der GUI ansehen, ohne Terminal.
- **Shell-Zugriff:** Mit einem Klick eine Shell in einem Container öffnen.
- **Resource-Monitoring:** CPU- und Speicherverbrauch der Container überwachen.
- **Volume-Management:** Volumes einsehen und verwalten.
- **Image-Verwaltung:** Lokale Images durchsuchen und aufräumen.

> **Tipp:** Unter *Settings → Resources* kannst du einstellen, wie viel RAM und CPU Docker nutzen darf. Auf einem Mac mit 8 GB RAM empfiehlt es sich, Docker nicht mehr als 4 GB zuzuweisen.

### VS Code Docker Extension

Die offizielle **Docker Extension** für Visual Studio Code macht die Arbeit mit Docker deutlich komfortabler:

**Installation:**
1. Öffne VS Code.
2. Gehe zu Extensions (`Cmd + Shift + X`).
3. Suche nach "Docker" (Herausgeber: Microsoft).
4. Klicke auf "Install".

**Funktionen:**
- **Syntax-Highlighting** für Dockerfiles und Compose-Dateien.
- **IntelliSense / Autovervollständigung** für Dockerfile-Anweisungen.
- **Sidebar-Panel** mit Übersicht über Container, Images, Volumes und Networks.
- **Rechtsklick-Aktionen:** Container starten, stoppen, Logs anzeigen — direkt aus VS Code.
- **Dockerfile generieren:** Über die Command Palette (`Cmd + Shift + P` → "Docker: Add Docker Files to Workspace") ein Dockerfile automatisch erstellen lassen.

### Weitere empfehlenswerte Tools

| Tool | Beschreibung |
|---|---|
| [Lazydocker](https://github.com/jesseduffield/lazydocker) | Terminal-UI für Docker — perfekt für alle, die lieber im Terminal arbeiten |
| [Dive](https://github.com/wagoodman/dive) | Analysiert Docker Image Layer und hilft, die Image-Größe zu reduzieren |
| [Docker Hub](https://hub.docker.com) | Die offizielle Registry für Docker Images — hier findest du Images für fast alles |

---

## Zusammenfassung

Docker mag auf den ersten Blick komplex wirken, aber die grundlegende Idee ist einfach: **Packe deine Anwendung samt aller Abhängigkeiten in einen Container, der überall gleich läuft.**

Die wichtigsten Schritte auf einen Blick:

1. **Dockerfile** schreiben, das beschreibt, wie dein Image gebaut wird.
2. **Image bauen** mit `docker build -t meine-app .`
3. **Container starten** mit `docker run -d -p 3000:3000 meine-app`
4. Bei mehreren Diensten: **Docker Compose** nutzen mit `docker compose up -d`

Wenn du diese Grundlagen beherrschst, hast du ein solides Fundament, auf dem du aufbauen kannst. Viel Erfolg!
