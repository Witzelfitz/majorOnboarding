[← Zurück zur Übersicht](../README.md)

# Deployment -- Deine App ins Internet bringen

Deployment bedeutet, dass du deine lokal entwickelte Webseite oder App auf einem Server bereitstellst, damit sie im Internet erreichbar ist. Statt `localhost:3000` bekommt dein Projekt eine echte URL wie `https://mein-projekt.vercel.app`.

In diesem Guide lernst du die drei beliebtesten kostenlosen Plattformen kennen: **Vercel**, **Netlify** und **GitHub Pages**. Alle drei sind perfekt fuer Studierende geeignet, da sie grosszuegige kostenlose Tarife anbieten.

---

## Inhaltsverzeichnis

1. [Was ist Deployment?](#was-ist-deployment)
2. [Vercel](#vercel)
3. [Netlify](#netlify)
4. [GitHub Pages](#github-pages)
5. [Vergleich der Plattformen](#vergleich-der-plattformen)
6. [Eigene Domain einrichten](#eigene-domain-einrichten)
7. [Tipps und Best Practices](#tipps-und-best-practices)

---

## Was ist Deployment?

Wenn du lokal an deinem Projekt arbeitest, laeuft es nur auf deinem Rechner. **Deployment** ist der Prozess, bei dem dein Code auf einen Server hochgeladen wird, damit andere Leute deine App ueber das Internet erreichen koennen.

### Der typische Deployment-Ablauf

```
Lokale Entwicklung → Git Push → Build-Prozess → Live im Internet
```

1. Du schreibst Code auf deinem Rechner
2. Du pushst deinen Code zu GitHub
3. Die Deployment-Plattform erkennt den Push und startet einen Build
4. Deine App wird gebaut (z.B. `npm run build`)
5. Die fertige App wird auf einem Server bereitgestellt
6. Du bekommst eine URL, unter der deine App erreichbar ist

### Wichtige Begriffe

| Begriff | Erklaerung |
|---|---|
| **Build** | Der Prozess, bei dem dein Quellcode in eine optimierte Version umgewandelt wird |
| **Production** | Die Live-Version deiner App, die Nutzer sehen |
| **Preview/Staging** | Eine Testversion, um Aenderungen zu pruefen, bevor sie live gehen |
| **CI/CD** | Continuous Integration / Continuous Deployment -- automatisiertes Testen und Deployen |
| **Umgebungsvariablen** | Geheime Werte (API-Keys, Passwoerter), die nicht im Code stehen sollen |
| **Rollback** | Zurueckkehren zu einer frueheren, funktionierenden Version |

---

## Vercel

[Vercel](https://vercel.com) ist die Plattform hinter Next.js und eignet sich hervorragend fuer React-, Next.js-, Svelte- und andere Frontend-Projekte. Die Einrichtung ist extrem einfach.

### Account erstellen

1. Gehe zu [vercel.com](https://vercel.com) und klicke auf **Sign Up**
2. Waehle **Continue with GitHub** (empfohlen, da es die Verbindung zu deinen Repos vereinfacht)
3. Autorisiere Vercel fuer deinen GitHub-Account
4. Fertig -- du hast jetzt einen Vercel-Account

### Projekt verbinden

#### Ueber die Weboberflaeche

1. Klicke im Vercel-Dashboard auf **Add New... → Project**
2. Waehle das GitHub-Repository aus, das du deployen moechtest
3. Vercel erkennt automatisch das Framework (z.B. Next.js, Vite, etc.)
4. Klicke auf **Deploy**

#### Ueber die CLI

Installiere zuerst die Vercel CLI:

```bash
npm install -g vercel
```

Dann im Projektordner:

```bash
# Einloggen
vercel login

# Projekt verbinden und deployen
vercel

# Fuer ein Production-Deployment
vercel --prod
```

### Automatische Deployments

Sobald dein Projekt verbunden ist, deployed Vercel automatisch bei jedem Push:

- **Push auf `main`/`master`** → Production Deployment (deine Live-URL)
- **Push auf andere Branches** → Preview Deployment (eigene temporaere URL)
- **Pull Request erstellt** → Preview Deployment mit Kommentar im PR

Das bedeutet: Du musst dich nach der Einrichtung um nichts mehr kuemmern. Einfach pushen und Vercel erledigt den Rest.

### Umgebungsvariablen

Umgebungsvariablen sind wichtig, um sensible Daten wie API-Keys sicher zu speichern. Schreibe solche Werte **niemals** direkt in deinen Code.

#### Ueber die Weboberflaeche

1. Gehe zu deinem Projekt in Vercel
2. Klicke auf **Settings → Environment Variables**
3. Gib den Namen und den Wert ein, z.B.:
   - Name: `DATABASE_URL`
   - Value: `postgresql://user:password@host:5432/mydb`
4. Waehle die Umgebungen aus (Production, Preview, Development)
5. Klicke auf **Save**

#### Ueber die CLI

```bash
# Variable hinzufuegen
vercel env add DATABASE_URL

# Alle Variablen anzeigen
vercel env ls

# Variablen lokal verwenden (erstellt eine .env.local Datei)
vercel env pull
```

> **Wichtig:** Fuege `.env.local` und `.env` immer zu deiner `.gitignore` hinzu, damit deine Secrets nicht auf GitHub landen.

### Custom Domains

1. Gehe zu **Settings → Domains** in deinem Vercel-Projekt
2. Gib deine Domain ein, z.B. `mein-projekt.de`
3. Vercel zeigt dir die noetigen DNS-Einstellungen an
4. Konfiguriere die DNS-Eintraege bei deinem Domain-Anbieter (siehe [Eigene Domain einrichten](#eigene-domain-einrichten))
5. Vercel erstellt automatisch ein SSL-Zertifikat (HTTPS)

```
Empfohlene DNS-Einstellung fuer Vercel:
Typ: CNAME
Name: www
Wert: cname.vercel-dns.com

Typ: A
Name: @
Wert: 76.76.21.21
```

---

## Netlify

[Netlify](https://netlify.com) ist eine weitere beliebte Plattform mit einigen besonderen Features wie integrierte Formulare und einfache Redirect-Regeln.

### Account erstellen

1. Gehe zu [netlify.com](https://netlify.com) und klicke auf **Sign up**
2. Waehle **GitHub** als Login-Methode
3. Autorisiere Netlify
4. Fertig

### Projekt verbinden

#### Ueber die Weboberflaeche

1. Klicke auf **Add new site → Import an existing project**
2. Waehle **GitHub** als Git-Provider
3. Waehle dein Repository aus
4. Konfiguriere die Build-Einstellungen (werden oft automatisch erkannt)
5. Klicke auf **Deploy site**

#### Ueber die CLI

```bash
# CLI installieren
npm install -g netlify-cli

# Einloggen
netlify login

# Im Projektordner: Projekt verbinden
netlify init

# Manuell deployen
netlify deploy

# Production Deployment
netlify deploy --prod
```

### Build-Settings

Die Build-Einstellungen legen fest, wie dein Projekt gebaut wird. Du kannst sie in der Weboberflaeche oder ueber eine Konfigurationsdatei festlegen.

#### Weboberflaeche

Unter **Site settings → Build & deploy → Build settings**:

- **Build command:** z.B. `npm run build`
- **Publish directory:** z.B. `dist` oder `build` oder `.next`
- **Node version:** z.B. `18`

#### Konfigurationsdatei (netlify.toml)

Erstelle eine `netlify.toml` im Hauptverzeichnis deines Projekts:

```toml
[build]
  command = "npm run build"
  publish = "dist"

[build.environment]
  NODE_VERSION = "18"

# Einstellungen fuer Preview-Deployments
[context.deploy-preview]
  command = "npm run build"

# Einstellungen fuer Branch-Deployments
[context.branch-deploy]
  command = "npm run build"
```

### Formulare

Netlify bietet eingebaute Formularverarbeitung -- kein Backend noetig. Perfekt fuer Kontaktformulare.

Fuege einfach das Attribut `netlify` zu deinem HTML-Formular hinzu:

```html
<form name="kontakt" method="POST" data-netlify="true">
  <input type="text" name="name" placeholder="Dein Name" required />
  <input type="email" name="email" placeholder="Deine E-Mail" required />
  <textarea name="nachricht" placeholder="Deine Nachricht" required></textarea>
  <button type="submit">Absenden</button>
</form>
```

Fuer React/JSX:

```jsx
<form name="kontakt" method="POST" data-netlify="true">
  {/* Wichtig: Verstecktes Feld fuer den Formularnamen */}
  <input type="hidden" name="form-name" value="kontakt" />
  <input type="text" name="name" placeholder="Dein Name" required />
  <input type="email" name="email" placeholder="Deine E-Mail" required />
  <textarea name="nachricht" placeholder="Deine Nachricht" required />
  <button type="submit">Absenden</button>
</form>
```

Eingereichte Formulare findest du unter **Forms** im Netlify-Dashboard.

### Redirects

Redirects sind nuetzlich fuer Single-Page-Apps (SPA) und URL-Weiterleitungen. Du kannst sie ueber eine `_redirects`-Datei oder die `netlify.toml` konfigurieren.

#### _redirects Datei

Erstelle eine Datei `_redirects` in deinem Build-Verzeichnis (z.B. `public/`):

```
# SPA-Fallback: Alle Routen zur index.html weiterleiten
/*    /index.html   200

# Alte URL auf neue URL umleiten
/alter-pfad    /neuer-pfad    301

# Externe Weiterleitung
/docs    https://docs.example.com    302

# API-Proxy (um CORS-Probleme zu vermeiden)
/api/*    https://mein-backend.example.com/api/:splat    200
```

#### In der netlify.toml

```toml
[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

[[redirects]]
  from = "/alter-pfad"
  to = "/neuer-pfad"
  status = 301
```

---

## GitHub Pages

[GitHub Pages](https://pages.github.com) ist direkt in GitHub integriert und eignet sich besonders fuer statische Webseiten, Dokumentation und Portfolio-Seiten.

### Einrichten

#### Methode 1: Ueber die Repository-Einstellungen

1. Gehe zu deinem Repository auf GitHub
2. Klicke auf **Settings → Pages**
3. Waehle unter **Source** entweder:
   - **Deploy from a branch** → Waehle den Branch (z.B. `main`) und den Ordner (`/ (root)` oder `/docs`)
   - **GitHub Actions** → Fuer benutzerdefinierte Build-Prozesse
4. Klicke auf **Save**
5. Deine Seite ist unter `https://dein-username.github.io/repo-name/` erreichbar

#### Methode 2: Statische Seite direkt deployen

Fuer eine einfache HTML-Seite reicht es, eine `index.html` im Root oder im `docs/`-Ordner zu haben:

```
mein-repo/
├── index.html
├── style.css
└── script.js
```

### GitHub Actions Workflow fuer automatisches Deployment

Fuer Projekte mit einem Build-Schritt (React, Vue, Svelte, etc.) brauchst du einen GitHub Actions Workflow. Erstelle die Datei `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  # Wird bei jedem Push auf main ausgeloest
  push:
    branches: [main]
  # Erlaubt manuelles Ausloesen
  workflow_dispatch:

# Berechtigungen fuer GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Nur ein Deployment gleichzeitig
concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Repository auschecken
        uses: actions/checkout@v4

      - name: Node.js einrichten
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: "npm"

      - name: Abhaengigkeiten installieren
        run: npm ci

      - name: Projekt bauen
        run: npm run build

      - name: GitHub Pages konfigurieren
        uses: actions/configure-pages@v4

      - name: Build-Artefakte hochladen
        uses: actions/upload-pages-artifact@v3
        with:
          path: "./dist"  # Passe den Pfad an dein Projekt an (dist, build, out, etc.)

  deploy:
    environment:
      name: github-pages
      url: ${{ github.pages_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Auf GitHub Pages deployen
        uses: actions/deploy-pages@v4
```

> **Hinweis fuer Vite-Projekte:** Setze in der `vite.config.js` die `base`-Option auf deinen Repository-Namen:
>
> ```js
> export default defineConfig({
>   base: '/mein-repo-name/',
>   // ... weitere Einstellungen
> })
> ```

> **Hinweis fuer React Router / Vue Router:** Wenn du Client-seitiges Routing verwendest, erstelle eine `404.html` im Build-Ordner, die zur `index.html` weiterleitet, da GitHub Pages kein serverseitiges Routing unterstuetzt.

---

## Vergleich der Plattformen

### Feature-Uebersicht

| Feature | Vercel | Netlify | GitHub Pages |
|---|---|---|---|
| **Kostenloser Tarif** | Ja (Hobby) | Ja (Starter) | Ja |
| **Automatische Deployments** | Ja | Ja | Ja (mit GitHub Actions) |
| **Preview Deployments** | Ja | Ja | Nein (nur mit Workaround) |
| **Serverless Functions** | Ja | Ja | Nein |
| **Formulare** | Nein (extern) | Ja (integriert) | Nein |
| **Custom Domains** | Ja | Ja | Ja |
| **SSL/HTTPS** | Automatisch | Automatisch | Automatisch |
| **Build-Minuten/Monat** | 6000 | 300 | 2000 (GitHub Actions) |
| **Bandbreite/Monat** | 100 GB | 100 GB | 100 GB |
| **Maximale Seitengroesse** | Keine Angabe | Keine Angabe | 1 GB |
| **Server-Side Rendering** | Ja | Ja (eingeschraenkt) | Nein |
| **Edge Functions** | Ja | Ja | Nein |
| **Analytics** | Ja (kostenpflichtig) | Ja (kostenpflichtig) | Nein |

### Kosten

| Plattform | Kostenloser Tarif | Erster bezahlter Tarif |
|---|---|---|
| **Vercel** | Hobby (fuer persoenliche Projekte) | Pro: $20/Monat |
| **Netlify** | Starter (1 Mitglied) | Pro: $19/Monat |
| **GitHub Pages** | Immer kostenlos (fuer oeffentliche Repos) | Privat: GitHub Pro $4/Monat |

### Wann welche Plattform?

| Situation | Empfehlung |
|---|---|
| Next.js Projekt | **Vercel** -- beste Integration, da Vercel Next.js entwickelt |
| Einfache statische Seite / Portfolio | **GitHub Pages** -- am einfachsten, keine Registrierung noetig |
| Seite mit Kontaktformular | **Netlify** -- integrierte Formularverarbeitung |
| React/Vue/Svelte SPA | **Vercel** oder **Netlify** -- beide hervorragend |
| Open-Source Dokumentation | **GitHub Pages** -- direkt im Repo, keine Extra-Plattform |
| Projekt mit API-Routen | **Vercel** oder **Netlify** -- Serverless Functions verfuegbar |

---

## Eigene Domain einrichten

Eine eigene Domain wie `mein-projekt.de` macht dein Projekt professioneller. Hier lernst du die DNS-Grundlagen und wie du eine Domain mit deiner Deployment-Plattform verbindest.

### DNS Grundlagen

Das **Domain Name System (DNS)** ist wie ein Telefonbuch fuer das Internet. Es uebersetzt menschenlesbare Domainnamen (z.B. `example.com`) in IP-Adressen (z.B. `76.76.21.21`).

#### Wichtige DNS-Record-Typen

| Record-Typ | Zweck | Beispiel |
|---|---|---|
| **A** | Verweist eine Domain auf eine IPv4-Adresse | `example.com → 76.76.21.21` |
| **AAAA** | Verweist eine Domain auf eine IPv6-Adresse | `example.com → 2606:4700::1` |
| **CNAME** | Verweist eine Domain auf eine andere Domain | `www.example.com → cname.vercel-dns.com` |
| **TXT** | Textinformationen (Verifizierung, SPF, etc.) | `example.com → "v=spf1 ..."` |
| **MX** | E-Mail-Server fuer die Domain | `example.com → mail.example.com` |

#### Begriffe

- **Registrar:** Der Anbieter, bei dem du deine Domain kaufst (z.B. Namecheap, Cloudflare, IONOS)
- **Nameserver:** Die Server, die die DNS-Eintraege fuer deine Domain verwalten
- **TTL (Time to Live):** Wie lange ein DNS-Eintrag zwischengespeichert wird (in Sekunden)
- **Propagation:** Die Zeit, bis DNS-Aenderungen weltweit uebernommen werden (bis zu 48 Stunden, meistens aber wenige Minuten)

### Schritt-fuer-Schritt: Domain verbinden

#### 1. Domain kaufen

Beliebte und guenstige Registrare:

- **Cloudflare Registrar** -- Domains zum Einkaufspreis, gute DNS-Verwaltung
- **Namecheap** -- Guenstige `.dev` und `.io` Domains
- **IONOS** -- Beliebt in Deutschland, `.de` Domains ab ca. 1 Euro/Jahr

#### 2. DNS-Eintraege konfigurieren

Gehe in die DNS-Verwaltung deines Registrars und erstelle die noetige Eintraege.

**Fuer Vercel:**

```
Typ:  A
Name: @
Wert: 76.76.21.21

Typ:  CNAME
Name: www
Wert: cname.vercel-dns.com
```

**Fuer Netlify:**

```
Typ:  A
Name: @
Wert: 75.2.60.5

Typ:  CNAME
Name: www
Wert: dein-site-name.netlify.app
```

**Fuer GitHub Pages:**

```
Typ:  A
Name: @
Wert: 185.199.108.153

Typ:  A
Name: @
Wert: 185.199.109.153

Typ:  A
Name: @
Wert: 185.199.110.153

Typ:  A
Name: @
Wert: 185.199.111.153

Typ:  CNAME
Name: www
Wert: dein-username.github.io
```

#### 3. Domain in der Plattform hinzufuegen

- **Vercel:** Settings → Domains → Domain eingeben
- **Netlify:** Domain settings → Add custom domain
- **GitHub Pages:** Settings → Pages → Custom domain eingeben + `CNAME`-Datei im Repo erstellen

#### 4. SSL-Zertifikat pruefen

Alle drei Plattformen erstellen automatisch ein kostenloses SSL-Zertifikat (Let's Encrypt). Es kann einige Minuten dauern, bis HTTPS funktioniert. Pruefe den Status in den Einstellungen der jeweiligen Plattform.

### DNS-Aenderungen pruefen

Mit diesen Befehlen kannst du pruefen, ob deine DNS-Eintraege korrekt sind:

```bash
# A-Record pruefen
dig example.com A +short

# CNAME-Record pruefen
dig www.example.com CNAME +short

# Oder mit nslookup
nslookup example.com

# DNS-Propagation online pruefen:
# https://www.whatsmydns.net/
```

---

## Tipps und Best Practices

### Preview Deployments nutzen

Preview Deployments sind eine der nuetzlichsten Funktionen moderner Deployment-Plattformen. Bei jedem Pull Request wird automatisch eine eigene Version deiner App deployed.

**Vorteile:**

- Du kannst Aenderungen testen, bevor sie live gehen
- Teamkollegen koennen deine Aenderungen direkt im Browser anschauen
- Jeder PR bekommt automatisch einen Kommentar mit dem Preview-Link

**So nutzt du Preview Deployments:**

1. Erstelle einen neuen Branch fuer dein Feature:

```bash
git checkout -b feature/neue-startseite
```

2. Mache deine Aenderungen und pushe den Branch:

```bash
git add .
git commit -m "Neue Startseite gestaltet"
git push origin feature/neue-startseite
```

3. Erstelle einen Pull Request auf GitHub
4. Vercel/Netlify erstellt automatisch ein Preview Deployment
5. Pruefe die Aenderungen ueber den Preview-Link
6. Wenn alles passt, merge den PR -- das Production Deployment wird automatisch aktualisiert

### Rollbacks durchfuehren

Manchmal geht beim Deployment etwas schief. Ein Rollback bringt deine App auf eine fruehere, funktionierende Version zurueck.

**Vercel:**

1. Gehe zu deinem Projekt → **Deployments**
2. Finde das letzte funktionierende Deployment
3. Klicke auf die drei Punkte (⋯) → **Promote to Production**

Oder ueber die CLI:

```bash
# Alle Deployments auflisten
vercel ls

# Ein bestimmtes Deployment promoten
vercel promote [deployment-url]
```

**Netlify:**

1. Gehe zu **Deploys** in deinem Netlify-Projekt
2. Klicke auf das gewuenschte Deployment
3. Klicke auf **Publish deploy**

Oder ueber die CLI:

```bash
# Deployment-Liste anzeigen
netlify deploys

# Bestimmtes Deployment veroeffentlichen
netlify deploy --prod --dir=.netlify/state
```

**GitHub Pages:**

Bei GitHub Pages revertierst du den problematischen Commit:

```bash
# Letzten Commit rueckgaengig machen (erstellt einen neuen Revert-Commit)
git revert HEAD
git push origin main
```

### Weitere Tipps

**Umgebungsvariablen sicher verwalten:**

```bash
# .gitignore -- Diese Dateien NIEMALS committen
.env
.env.local
.env.production
.env.*.local
```

Erstelle stattdessen eine `.env.example` mit leeren Platzhaltern als Dokumentation:

```bash
# .env.example -- Kann ins Repo committed werden
DATABASE_URL=
API_KEY=
NEXT_PUBLIC_ANALYTICS_ID=
```

**Build-Caching aktivieren:**

Alle drei Plattformen cachen deine `node_modules` automatisch. Falls du Probleme mit dem Cache hast:

- **Vercel:** Settings → General → Build Cache → Clear
- **Netlify:** Deploys → Trigger deploy → Clear cache and deploy site
- **GitHub Actions:** Cache manuell loeschen unter Actions → Caches

**Monitoring und Fehlersuche:**

```bash
# Vercel: Logs in Echtzeit anzeigen
vercel logs [deployment-url]

# Netlify: Funktionslogs anzeigen
netlify functions:log

# Build-Logs findest du im Dashboard jeder Plattform unter dem jeweiligen Deployment
```

**Performance optimieren:**

- Nutze `next/image` oder aehnliche Bibliotheken fuer Bildoptimierung
- Aktiviere Komprimierung (wird von allen Plattformen automatisch gemacht)
- Pruefe deine Seitengroesse mit [PageSpeed Insights](https://pagespeed.web.dev/)
- Halte deine Bundle-Groesse klein -- nutze `npm run build` mit dem `--analyze`-Flag, falls dein Framework das unterstuetzt

---

> **Zusammenfassung:** Fuer den Anfang empfehlen wir Vercel oder Netlify, da beide Plattformen den gesamten Deployment-Prozess automatisieren und kostenlose Tarife fuer Studierende anbieten. GitHub Pages ist ideal fuer einfache statische Seiten. Sobald du dich mit einer Plattform wohlfuehlst, probiere die anderen aus -- der Wechsel ist unkompliziert, da alle drei mit Git-Repositories arbeiten.
