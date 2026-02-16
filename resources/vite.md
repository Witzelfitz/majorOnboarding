[← Zurück zur Übersicht](../README.md)

# Vite -- Der moderne Build-Tool-Standard

## Was ist Vite (und warum nicht Webpack)?

Vite (französisch für "schnell") ist ein modernes Build-Tool, das von Evan You (dem Erfinder von Vue.js) entwickelt wurde. Es löst ein zentrales Problem, das viele Studierende beim Arbeiten mit Webpack kennen: **lange Wartezeiten beim Starten und Neuladen des Entwicklungsservers**.

### Das Problem mit Webpack

Webpack bündelt beim Start **alle** Dateien des Projekts in ein großes Bundle. Bei wachsender Projektgröße dauert das immer länger -- oft 30 Sekunden oder mehr.

### Wie Vite das löst

Vite nutzt zwei Schlüsseltechnologien:

1. **Native ES-Module im Browser**: Während der Entwicklung werden Dateien einzeln und bei Bedarf geladen, statt alles vorher zu bündeln.
2. **esbuild für Pre-Bundling**: Abhängigkeiten aus `node_modules` werden mit esbuild (geschrieben in Go) vorverarbeitet -- das ist 10-100x schneller als JavaScript-basierte Bundler.
3. **Rollup für den Production Build**: Für den finalen Build nutzt Vite Rollup, das optimierte und kleine Bundles erzeugt.

```
Webpack-Workflow:    Alle Dateien bündeln  →  Server starten  →  Seite laden
Vite-Workflow:       Server starten        →  Seite laden     →  Dateien bei Bedarf liefern
```

**Fazit:** Vite startet in Millisekunden statt Sekunden. Änderungen werden quasi sofort im Browser sichtbar.

## Projekt erstellen

Der einfachste Weg, ein Vite-Projekt zu erstellen, ist der offizielle Befehl:

```bash
npm create vite@latest
```

Der Wizard fragt dann nach Projektname, Framework und Variante. Alternativ kann man alles direkt angeben:

### Vanilla (reines HTML/CSS/JS)

```bash
npm create vite@latest mein-projekt -- --template vanilla
```

### React

```bash
npm create vite@latest mein-react-app -- --template react
```

### React mit TypeScript

```bash
npm create vite@latest mein-react-ts-app -- --template react-ts
```

### Vue

```bash
npm create vite@latest mein-vue-app -- --template vue
```

### Nach dem Erstellen

Egal welches Template man gewählt hat, die nächsten Schritte sind immer gleich:

```bash
cd mein-projekt
npm install
npm run dev
```

> **Tipp:** Es gibt noch weitere Templates wie `react-swc`, `react-swc-ts`, `vue-ts`, `svelte`, `svelte-ts`, `lit` und `preact`. Die vollständige Liste findet man in der [Vite-Dokumentation](https://vitejs.dev/guide/#scaffolding-your-first-vite-project).

## Projektstruktur erklärt

Nach dem Erstellen eines React-TypeScript-Projekts sieht die Struktur so aus:

```
mein-react-ts-app/
├── node_modules/          # Installierte Abhängigkeiten (nicht bearbeiten)
├── public/                # Statische Dateien (werden 1:1 kopiert)
│   └── vite.svg           # Beispiel-Asset
├── src/                   # Hier lebt dein Quellcode
│   ├── assets/            # Bilder, Fonts etc. (werden verarbeitet)
│   │   └── react.svg
│   ├── App.css            # Styles für die App-Komponente
│   ├── App.tsx            # Haupt-React-Komponente
│   ├── index.css          # Globale Styles
│   ├── main.tsx           # Einstiegspunkt der Anwendung
│   └── vite-env.d.ts      # TypeScript-Typen für Vite
├── .gitignore             # Dateien, die Git ignorieren soll
├── index.html             # Die HTML-Einstiegsdatei (liegt im Root!)
├── package.json           # Projektabhängigkeiten und Skripte
├── tsconfig.json          # TypeScript-Konfiguration
├── tsconfig.node.json     # TypeScript-Konfig für die Node-Umgebung
└── vite.config.ts         # Vite-Konfiguration
```

### Wichtige Punkte

- **`index.html` liegt im Root**, nicht in einem `public`-Ordner. Vite behandelt `index.html` als Einstiegspunkt und verarbeitet darin referenzierte Module.
- **`src/`** enthält allen Quellcode, der von Vite transformiert wird (JSX, TypeScript, CSS-Module etc.).
- **`public/`** enthält Dateien, die unverändert in den Build kopiert werden (z. B. `favicon.ico`, `robots.txt`).

## Dev-Server starten & Build erstellen

### Entwicklungsserver starten

```bash
npm run dev
```

Ausgabe:

```
  VITE v5.x.x  ready in 300 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: http://192.168.1.100:5173/
  ➜  press h + enter to show help
```

Der Server startet auf Port **5173** (Vites Standardport). Die Seite aktualisiert sich automatisch bei Codeänderungen.

### Vorschau des Production Builds

```bash
npm run preview
```

Dieser Befehl startet einen lokalen Server, der den Inhalt des `dist/`-Ordners ausliefert. Nützlich, um den fertigen Build lokal zu testen.

### Production Build erstellen

```bash
npm run build
```

Vite erstellt einen optimierten Build im `dist/`-Ordner:

```
dist/
├── assets/
│   ├── index-BnK3d7Gq.css    # Gebündeltes CSS (mit Hash)
│   └── index-DZl3v8Hp.js     # Gebündeltes JS (mit Hash)
└── index.html                 # Optimiertes HTML
```

Die Hashes in den Dateinamen sorgen für effizientes Browser-Caching: Ändert sich der Inhalt, ändert sich der Hash, und der Browser lädt die neue Version.

## Konfiguration (vite.config.ts)

Die Konfigurationsdatei `vite.config.ts` ist das Herzstück der Vite-Einstellungen. Hier ein umfassendes Beispiel:

```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import path from 'path'

export default defineConfig({
  // --- Plugins ---
  plugins: [
    react(),
  ],

  // --- Pfad-Aliases ---
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
      '@components': path.resolve(__dirname, './src/components'),
      '@utils': path.resolve(__dirname, './src/utils'),
      '@assets': path.resolve(__dirname, './src/assets'),
    },
  },

  // --- Dev-Server-Einstellungen ---
  server: {
    port: 3000,              // Standardport ändern
    open: true,              // Browser automatisch öffnen
    host: true,              // Im Netzwerk erreichbar machen
    proxy: {
      // API-Anfragen an einen Backend-Server weiterleiten
      '/api': {
        target: 'http://localhost:8080',
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/api/, ''),
      },
    },
  },

  // --- Build-Einstellungen ---
  build: {
    outDir: 'dist',          // Ausgabeordner (Standard: dist)
    sourcemap: true,         // Source Maps für Debugging
    minify: 'terser',        // Minifizierung (esbuild oder terser)
  },

  // --- CSS-Einstellungen ---
  css: {
    modules: {
      localsConvention: 'camelCase',  // CSS-Module-Klassennamen
    },
  },
})
```

### Aliases nutzen

Nach dem Einrichten der Aliases kann man Imports vereinfachen:

```ts
// Vorher (relativ und unübersichtlich):
import Button from '../../../components/Button'

// Nachher (mit Alias):
import Button from '@components/Button'
```

> **Wichtig für TypeScript:** Damit TypeScript die Aliases ebenfalls versteht, muss man sie auch in der `tsconfig.json` eintragen:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@components/*": ["src/components/*"],
      "@utils/*": ["src/utils/*"],
      "@assets/*": ["src/assets/*"]
    }
  }
}
```

## Umgebungsvariablen (.env Dateien)

Vite hat ein eingebautes System für Umgebungsvariablen. So funktioniert es:

### Dateien anlegen

```
mein-projekt/
├── .env                   # Wird immer geladen
├── .env.local             # Wird immer geladen, von Git ignoriert
├── .env.development       # Nur im Dev-Modus geladen
├── .env.development.local # Nur im Dev-Modus, von Git ignoriert
├── .env.production        # Nur im Production Build geladen
└── .env.production.local  # Nur im Production Build, von Git ignoriert
```

### Der VITE_ Prefix

**Nur Variablen mit dem Prefix `VITE_` werden im Client-Code verfügbar gemacht.** Das ist ein Sicherheitsmechanismus, damit man nicht versehentlich Geheimnisse in den Browser-Code einbaut.

```bash
# .env
VITE_API_URL=https://api.mein-backend.de
VITE_APP_TITLE=Meine Uni-App
GEHEIMER_API_KEY=abc123   # NICHT im Browser verfügbar!
```

### Im Code verwenden

```ts
// Zugriff auf Umgebungsvariablen
const apiUrl = import.meta.env.VITE_API_URL
const appTitle = import.meta.env.VITE_APP_TITLE

// Eingebaute Variablen
console.log(import.meta.env.MODE)       // "development" oder "production"
console.log(import.meta.env.DEV)        // true im Dev-Modus
console.log(import.meta.env.PROD)       // true im Production Build
console.log(import.meta.env.BASE_URL)   // Base-URL der Anwendung
```

### TypeScript-Unterstützung

Für Autovervollständigung kann man die Typen in `src/vite-env.d.ts` erweitern:

```ts
/// <reference types="vite/client" />

interface ImportMetaEnv {
  readonly VITE_API_URL: string
  readonly VITE_APP_TITLE: string
}

interface ImportMeta {
  readonly env: ImportMetaEnv
}
```

> **Sicherheitshinweis:** Trage `.env.local` und `.env.*.local` immer in die `.gitignore` ein. Sensible Daten wie API-Keys für serverseitige Dienste gehören **niemals** in Variablen mit dem `VITE_` Prefix.

## Statische Assets

Vite unterscheidet zwei Arten, wie Assets eingebunden werden:

### Assets in `src/assets/` (verarbeitet)

Diese Dateien werden von Vite verarbeitet, erhalten einen Content-Hash im Dateinamen und werden optimiert:

```tsx
import logo from './assets/logo.png'
import styles from './styles.module.css'

function App() {
  return (
    <div>
      <img src={logo} alt="Logo" />
    </div>
  )
}
```

Beim Import gibt Vite die finale URL als String zurück. Im Build wird `logo.png` zu etwas wie `assets/logo-a1b2c3d4.png`.

### Assets in `public/` (unverarbeitet)

Dateien im `public/`-Ordner werden 1:1 in den Build kopiert und sind immer unter dem Root-Pfad erreichbar:

```
public/
├── favicon.ico
├── robots.txt
└── data/
    └── config.json
```

```tsx
// Zugriff auf public-Dateien: immer mit / beginnen
<img src="/favicon.ico" alt="Favicon" />

// Auch per fetch erreichbar
const response = await fetch('/data/config.json')
```

### Wann was verwenden?

| Kriterium | `src/assets/` | `public/` |
|---|---|---|
| Content-Hashing | Ja | Nein |
| Optimierung | Ja (Komprimierung etc.) | Nein |
| Import im Code | `import bild from './assets/bild.png'` | Direkter Pfad: `"/bild.png"` |
| Anwendungsfälle | Bilder, Fonts, SVGs im Code | `favicon.ico`, `robots.txt`, große Dateien |

### Kleine Assets als Inline-Daten

Dateien unter 4 KB werden standardmäßig als Base64-Data-URL eingebettet, um HTTP-Requests zu sparen. Dieses Limit kann man in der Konfiguration anpassen:

```ts
// vite.config.ts
export default defineConfig({
  build: {
    assetsInlineLimit: 8192,  // 8 KB statt 4 KB
  },
})
```

## Hot Module Replacement (HMR) erklärt

HMR ist eine der wichtigsten Funktionen von Vite. Sie sorgt dafür, dass Codeänderungen **sofort** im Browser sichtbar werden, ohne die gesamte Seite neu zu laden.

### Wie funktioniert HMR?

1. Du änderst eine Datei (z. B. `Button.tsx`).
2. Vite erkennt die Änderung über einen File-Watcher.
3. Über eine WebSocket-Verbindung wird der Browser informiert.
4. **Nur das betroffene Modul** wird neu geladen -- der Rest der Anwendung bleibt intakt.
5. Der Zustand (State) der Anwendung bleibt erhalten.

```
Traditionelles Reload:
  Datei ändern → Seite komplett neu laden → State verloren → von vorn navigieren

HMR:
  Datei ändern → Nur geändertes Modul ersetzen → State bleibt → sofort weiterarbeiten
```

### HMR in der Praxis

Bei React-Projekten mit `@vitejs/plugin-react` funktioniert HMR automatisch für Komponenten. Ein Beispiel:

```tsx
// Button.tsx
function Button() {
  return <button className="btn-primary">Klick mich</button>
}
```

Wenn du `"Klick mich"` in `"Jetzt klicken"` änderst, wird nur die Button-Komponente aktualisiert. Formulardaten in anderen Komponenten, der aktuelle Router-Zustand und Scroll-Positionen bleiben erhalten.

### HMR für CSS

CSS-Änderungen werden ebenfalls per HMR eingespielt, ohne die Seite neu zu laden. Das funktioniert sowohl mit normalen `.css`-Dateien als auch mit CSS-Modulen.

### Wann macht HMR einen Full Reload?

In manchen Fällen kann Vite die Änderung nicht isoliert einspielen und lädt die gesamte Seite neu:

- Änderungen an `index.html`
- Änderungen an Konfigurationsdateien
- Änderungen, die den Modulbaum grundlegend verändern

## Nützliche Plugins

Vite hat ein umfangreiches Plugin-Ökosystem. Hier die wichtigsten Plugins für den Uni-Alltag:

### @vitejs/plugin-react

Das offizielle React-Plugin. Wird bei React-Templates automatisch eingerichtet.

```bash
npm install -D @vitejs/plugin-react
```

```ts
// vite.config.ts
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
})
```

### vite-plugin-svgr

SVG-Dateien als React-Komponenten importieren -- ideal für Icons.

```bash
npm install -D vite-plugin-svgr
```

```ts
// vite.config.ts
import svgr from 'vite-plugin-svgr'

export default defineConfig({
  plugins: [react(), svgr()],
})
```

```tsx
// Im Code
import { ReactComponent as Logo } from './logo.svg'

function Header() {
  return <Logo className="header-logo" />
}
```

### vite-tsconfig-paths

Liest Pfad-Aliases automatisch aus der `tsconfig.json`, sodass man sie nicht doppelt in `vite.config.ts` definieren muss.

```bash
npm install -D vite-tsconfig-paths
```

```ts
// vite.config.ts
import tsconfigPaths from 'vite-tsconfig-paths'

export default defineConfig({
  plugins: [react(), tsconfigPaths()],
})
```

### @vitejs/plugin-legacy

Erzeugt zusätzliche Bundles für ältere Browser, die keine nativen ES-Module unterstützen.

```bash
npm install -D @vitejs/plugin-legacy terser
```

```ts
// vite.config.ts
import legacy from '@vitejs/plugin-legacy'

export default defineConfig({
  plugins: [
    react(),
    legacy({
      targets: ['defaults', 'not IE 11'],
    }),
  ],
})
```

### vite-plugin-checker

Zeigt TypeScript- und ESLint-Fehler direkt als Overlay im Browser an -- sehr praktisch während der Entwicklung.

```bash
npm install -D vite-plugin-checker
```

```ts
// vite.config.ts
import checker from 'vite-plugin-checker'

export default defineConfig({
  plugins: [
    react(),
    checker({
      typescript: true,    // TypeScript-Fehler anzeigen
      eslint: {            // ESLint-Fehler anzeigen
        lintCommand: 'eslint "./src/**/*.{ts,tsx}"',
      },
    }),
  ],
})
```

### vite-plugin-pwa

Macht die Anwendung zu einer Progressive Web App mit Service Worker und Offline-Unterstützung.

```bash
npm install -D vite-plugin-pwa
```

```ts
// vite.config.ts
import { VitePWA } from 'vite-plugin-pwa'

export default defineConfig({
  plugins: [
    react(),
    VitePWA({
      registerType: 'autoUpdate',
      manifest: {
        name: 'Meine Uni-App',
        short_name: 'UniApp',
        theme_color: '#ffffff',
      },
    }),
  ],
})
```

---

> **Zusammenfassung:** Vite ist der empfohlene Einstieg in modernes Frontend-Tooling. Es ist schnell, einfach zu konfigurieren und hat ein wachsendes Ökosystem. Für die meisten Uni-Projekte reicht `npm create vite@latest` mit dem passenden Template, und man kann sofort produktiv arbeiten.
