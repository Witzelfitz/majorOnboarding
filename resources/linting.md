[← Zurück zur Übersicht](../README.md)

# Linting und Formatierung mit ESLint & Prettier

Code-Qualitaet ist in der professionellen Softwareentwicklung unverzichtbar. Zwei Werkzeuge helfen dabei enorm: **ESLint** (findet Fehler und erzwingt Konventionen) und **Prettier** (formatiert den Code automatisch). In diesem Guide lernst du, wie du beides einrichtest und sinnvoll kombinierst.

---

## Inhaltsverzeichnis

1. [Was ist Linting? Was ist Formatierung?](#was-ist-linting-was-ist-formatierung)
2. [ESLint](#eslint)
3. [Prettier](#prettier)
4. [ESLint und Prettier zusammen nutzen](#eslint-und-prettier-zusammen-nutzen)
5. [VS Code Integration](#vs-code-integration)
6. [npm Scripts einrichten](#npm-scripts-einrichten)
7. [Pre-Commit Hooks mit Husky und lint-staged](#pre-commit-hooks-mit-husky-und-lint-staged)
8. [Beispiel-Setup: React + TypeScript Projekt](#beispiel-setup-react--typescript-projekt)

---

## Was ist Linting? Was ist Formatierung?

Diese beiden Begriffe werden oft verwechselt, meinen aber unterschiedliche Dinge:

### Linting

Ein **Linter** analysiert deinen Code auf **inhaltliche Probleme**. Dazu gehoeren:

- Variablen, die deklariert aber nie benutzt werden
- Fehlende `key`-Props in React-Listen
- Unsichere Vergleiche mit `==` statt `===`
- Nicht erreichbarer Code nach einem `return`
- Veraltete oder fehlerhafte Patterns

Ein Linter sagt dir: *"Dein Code hat ein Problem, das zu Bugs fuehren kann."*

### Formatierung

Ein **Formatter** kuemmert sich um das **Aussehen** des Codes. Dazu gehoeren:

- Einrueckung (Tabs vs. Spaces, Anzahl der Spaces)
- Semikolons am Zeilenende (ja oder nein)
- Einfache vs. doppelte Anfuehrungszeichen
- Zeilenlaenge
- Leerzeilen und Abstaende

Ein Formatter sagt dir: *"Dein Code funktioniert, sieht aber uneinheitlich aus."*

### Warum beides?

| Aspekt | Linting (ESLint) | Formatierung (Prettier) |
|---|---|---|
| Ziel | Fehler und schlechte Patterns finden | Einheitliches Code-Aussehen |
| Beispiel | `no-unused-vars`, `no-undef` | Einrueckung, Semikolons |
| Kann Fehler verhindern? | Ja | Nein |
| Automatisch behebbar? | Teilweise (`--fix`) | Ja (immer) |

**Fazit:** ESLint und Prettier ergaenzen sich. ESLint findet Fehler, Prettier macht den Code huebsch. Zusammen sorgen sie dafuer, dass dein Code sowohl korrekt als auch einheitlich formatiert ist.

---

## ESLint

ESLint ist der Standard-Linter fuer JavaScript und TypeScript. Er ist extrem konfigurierbar und unterstuetzt Plugins fuer React, TypeScript und viele andere Bibliotheken.

### Installation

Installiere ESLint als Dev-Dependency in deinem Projekt:

```shell
npm install --save-dev eslint
```

Fuer TypeScript- und React-Projekte brauchst du zusaetzliche Plugins:

```shell
npm install --save-dev @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint-plugin-react eslint-plugin-react-hooks
```

### Konfiguration mit Flat Config (eslint.config.js)

Seit ESLint v9 ist die sogenannte **Flat Config** der Standard. Anstatt einer `.eslintrc`-Datei verwendest du jetzt eine `eslint.config.js` (oder `eslint.config.mjs`) im Projektstammverzeichnis.

```js
// eslint.config.js
import js from "@eslint/js";
import tseslint from "@typescript-eslint/eslint-plugin";
import tsparser from "@typescript-eslint/parser";
import react from "eslint-plugin-react";
import reactHooks from "eslint-plugin-react-hooks";

export default [
  // Basis-Regeln von ESLint
  js.configs.recommended,

  // Konfiguration fuer TypeScript-Dateien
  {
    files: ["**/*.{ts,tsx}"],
    languageOptions: {
      parser: tsparser,
      parserOptions: {
        ecmaVersion: "latest",
        sourceType: "module",
        ecmaFeatures: {
          jsx: true,
        },
      },
    },
    plugins: {
      "@typescript-eslint": tseslint,
      react: react,
      "react-hooks": reactHooks,
    },
    rules: {
      // TypeScript-Regeln
      "@typescript-eslint/no-unused-vars": "warn",
      "@typescript-eslint/no-explicit-any": "warn",

      // React-Regeln
      "react/react-in-jsx-scope": "off", // Nicht noetig seit React 17
      "react/prop-types": "off", // TypeScript uebernimmt die Typ-Pruefung
      "react-hooks/rules-of-hooks": "error",
      "react-hooks/exhaustive-deps": "warn",
    },
    settings: {
      react: {
        version: "detect",
      },
    },
  },

  // Dateien und Ordner, die ignoriert werden sollen
  {
    ignores: ["node_modules/", "dist/", "build/"],
  },
];
```

### Wichtige Konzepte der Flat Config

- **Array-Struktur:** Die Konfiguration ist ein Array von Konfigurations-Objekten. Jedes Objekt kann Regeln, Plugins und Dateimuster enthalten.
- **files:** Bestimmt, fuer welche Dateien die Regeln gelten (z.B. nur `.ts`- und `.tsx`-Dateien).
- **ignores:** Dateien und Ordner, die ESLint ignorieren soll.
- **plugins:** Werden als Objekte importiert und nicht mehr als Strings angegeben.

### Wichtigste Regeln

Hier eine Auswahl der nuetzlichsten ESLint-Regeln:

| Regel | Was sie tut | Empfehlung |
|---|---|---|
| `no-unused-vars` | Warnt bei unbenutzten Variablen | `"warn"` |
| `no-console` | Warnt bei `console.log()` im Code | `"warn"` |
| `eqeqeq` | Erzwingt `===` statt `==` | `"error"` |
| `no-undef` | Verbietet undefinierte Variablen | `"error"` |
| `no-var` | Verbietet `var`, erzwingt `let`/`const` | `"error"` |
| `prefer-const` | Erzwingt `const`, wenn Variable nicht neu zugewiesen wird | `"warn"` |
| `no-debugger` | Verbietet `debugger`-Statements | `"error"` |

Regeln koennen drei Werte haben:

- `"off"` oder `0` -- Regel deaktiviert
- `"warn"` oder `1` -- Warnung (gelb), Build laeuft weiter
- `"error"` oder `2` -- Fehler (rot), Build schlaegt fehl

### Plugins

#### eslint-plugin-react

Dieses Plugin stellt Regeln bereit, die speziell fuer React-Code gelten:

- `react/jsx-no-duplicate-props` -- Keine doppelten Props in JSX
- `react/jsx-key` -- Erzwingt `key`-Prop bei Listen
- `react/no-direct-mutation-state` -- Verbietet direkte State-Mutation
- `react-hooks/rules-of-hooks` -- Erzwingt die Regeln der Hooks
- `react-hooks/exhaustive-deps` -- Prueft Dependency-Arrays von Hooks

#### @typescript-eslint

Dieses Plugin erweitert ESLint um TypeScript-spezifische Regeln:

- `@typescript-eslint/no-unused-vars` -- Wie `no-unused-vars`, aber fuer TypeScript
- `@typescript-eslint/no-explicit-any` -- Warnt bei Verwendung von `any`
- `@typescript-eslint/explicit-function-return-type` -- Erzwingt Rueckgabetypen
- `@typescript-eslint/no-non-null-assertion` -- Warnt bei `!`-Operator

### ESLint ausfuehren

```shell
# Alle Dateien im src-Ordner pruefen
npx eslint src/

# Fehler automatisch beheben (wo moeglich)
npx eslint src/ --fix

# Nur TypeScript-Dateien pruefen
npx eslint "src/**/*.{ts,tsx}"
```

---

## Prettier

Prettier ist ein **opinionated Code Formatter**. Das bedeutet: Prettier hat feste Vorstellungen davon, wie Code aussehen sollte, und laesst nur wenig Spielraum fuer Anpassungen. Das ist gewollt -- so gibt es im Team keine Diskussionen ueber Code-Stil.

### Installation

```shell
npm install --save-dev prettier
```

### Konfiguration (.prettierrc)

Erstelle eine `.prettierrc`-Datei im Projektstammverzeichnis:

```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "all",
  "printWidth": 80
}
```

### Wichtigste Optionen erklaert

| Option | Typ | Standard | Beschreibung |
|---|---|---|---|
| `semi` | Boolean | `true` | Semikolons am Zeilenende? `true` = ja, `false` = nein |
| `singleQuote` | Boolean | `false` | Einfache Anfuehrungszeichen? `true` = `'text'`, `false` = `"text"` |
| `tabWidth` | Zahl | `2` | Anzahl der Spaces pro Einrueckungsebene |
| `trailingComma` | String | `"all"` | Komma nach dem letzten Element? `"all"`, `"es5"` oder `"none"` |
| `printWidth` | Zahl | `80` | Maximale Zeilenlaenge, bevor Prettier umbreicht |

#### Weitere nuetzliche Optionen

```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "all",
  "printWidth": 80,
  "useTabs": false,
  "bracketSpacing": true,
  "arrowParens": "always",
  "endOfLine": "lf"
}
```

- **useTabs:** `false` = Spaces, `true` = Tabs
- **bracketSpacing:** Leerzeichen in Objekten: `{ foo: bar }` vs. `{foo: bar}`
- **arrowParens:** Klammern bei Arrow Functions mit einem Parameter: `(x) => x` vs. `x => x`
- **endOfLine:** Zeilenende-Zeichen (`"lf"` fuer Unix/Mac, `"crlf"` fuer Windows)

### .prettierignore

Damit Prettier bestimmte Dateien nicht anfasst, erstelle eine `.prettierignore`:

```
node_modules/
dist/
build/
coverage/
*.min.js
```

### Prettier ausfuehren

```shell
# Alle Dateien im src-Ordner formatieren
npx prettier --write src/

# Pruefen, ob alles formatiert ist (ohne Aenderungen)
npx prettier --check src/

# Einzelne Datei formatieren
npx prettier --write src/App.tsx
```

---

## ESLint und Prettier zusammen nutzen

ESLint und Prettier koennen sich in die Quere kommen, weil ESLint auch einige Formatierungs-Regeln hat (z.B. `indent`, `quotes`, `semi`). Wenn beide aktiv sind, kann es passieren, dass ESLint eine Formatierung verlangt, die Prettier wieder rueckgaengig macht.

### Die Loesung: eslint-config-prettier

Das Paket `eslint-config-prettier` **deaktiviert alle ESLint-Regeln, die mit Prettier in Konflikt stehen**. So ist klar aufgeteilt: ESLint kuemmert sich um Code-Qualitaet, Prettier um die Formatierung.

```shell
npm install --save-dev eslint-config-prettier
```

Fuege es in deiner `eslint.config.js` als letztes Element ein, damit es alle vorherigen Formatierungs-Regeln ueberschreibt:

```js
// eslint.config.js
import js from "@eslint/js";
import tseslint from "@typescript-eslint/eslint-plugin";
import tsparser from "@typescript-eslint/parser";
import react from "eslint-plugin-react";
import reactHooks from "eslint-plugin-react-hooks";
import prettierConfig from "eslint-config-prettier";

export default [
  js.configs.recommended,

  {
    files: ["**/*.{ts,tsx}"],
    languageOptions: {
      parser: tsparser,
      parserOptions: {
        ecmaVersion: "latest",
        sourceType: "module",
        ecmaFeatures: { jsx: true },
      },
    },
    plugins: {
      "@typescript-eslint": tseslint,
      react: react,
      "react-hooks": reactHooks,
    },
    rules: {
      "@typescript-eslint/no-unused-vars": "warn",
      "@typescript-eslint/no-explicit-any": "warn",
      "react/react-in-jsx-scope": "off",
      "react-hooks/rules-of-hooks": "error",
      "react-hooks/exhaustive-deps": "warn",
      eqeqeq: "error",
      "no-var": "error",
      "prefer-const": "warn",
    },
    settings: {
      react: { version: "detect" },
    },
  },

  // WICHTIG: prettierConfig muss am Ende stehen!
  prettierConfig,

  { ignores: ["node_modules/", "dist/", "build/"] },
];
```

### Warum diese Reihenfolge wichtig ist

Die Flat Config wird von oben nach unten verarbeitet. Spaetere Eintraege ueberschreiben fruehere. Da `prettierConfig` am Ende steht, werden alle ESLint-Formatierungsregeln deaktiviert, die mit Prettier kollidieren wuerden.

---

## VS Code Integration

Damit ESLint und Prettier direkt beim Entwickeln in VS Code funktionieren, brauchst du zwei Extensions und eine kleine Konfiguration.

### Extensions installieren

Oeffne VS Code und installiere diese beiden Extensions:

1. **ESLint** (`dbaeumer.vscode-eslint`) -- Zeigt ESLint-Fehler direkt im Editor an
2. **Prettier - Code formatter** (`esbenp.prettier-vscode`) -- Formatiert Code mit Prettier

Du kannst sie ueber die Kommandozeile installieren:

```shell
code --install-extension dbaeumer.vscode-eslint
code --install-extension esbenp.prettier-vscode
```

### Einstellungen konfigurieren (settings.json)

Oeffne die VS Code-Einstellungen als JSON (`Cmd + Shift + P` -> "Preferences: Open Settings (JSON)") und fuege folgendes hinzu:

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact"
  ]
}
```

### Was passiert jetzt beim Speichern?

1. **Prettier** formatiert die Datei automatisch (Einrueckung, Semikolons, Zeilenlaenge, etc.)
2. **ESLint** behebt automatisch alle Fehler, die mit `--fix` behebbar sind (z.B. `prefer-const`)

Das bedeutet: Du schreibst einfach drauflos, und beim Speichern wird alles automatisch aufgeraeumt.

### Team-Konfiguration mit .vscode/settings.json

Damit alle im Team die gleichen Einstellungen nutzen, lege eine `.vscode/settings.json` im Projekt an:

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  }
}
```

Und eine `.vscode/extensions.json`, die empfohlene Extensions vorschlaegt:

```json
{
  "recommendations": [
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode"
  ]
}
```

---

## npm Scripts einrichten

Damit du Linting und Formatierung bequem ueber die Kommandozeile ausfuehren kannst, richte npm Scripts in deiner `package.json` ein:

```json
{
  "scripts": {
    "lint": "eslint src/",
    "lint:fix": "eslint src/ --fix",
    "format": "prettier --write src/",
    "format:check": "prettier --check src/"
  }
}
```

### Scripts erklaert

| Script | Befehl | Was es tut |
|---|---|---|
| `npm run lint` | `eslint src/` | Prueft den Code auf Fehler und zeigt sie an |
| `npm run lint:fix` | `eslint src/ --fix` | Behebt automatisch behebbare ESLint-Fehler |
| `npm run format` | `prettier --write src/` | Formatiert alle Dateien im `src/`-Ordner |
| `npm run format:check` | `prettier --check src/` | Prueft, ob alle Dateien formatiert sind (ohne Aenderungen) |

### Kombiniertes Script

Du kannst auch ein Script erstellen, das beides nacheinander ausfuehrt:

```json
{
  "scripts": {
    "lint": "eslint src/",
    "lint:fix": "eslint src/ --fix",
    "format": "prettier --write src/",
    "format:check": "prettier --check src/",
    "check": "npm run lint && npm run format:check"
  }
}
```

Mit `npm run check` pruefst du dann in einem Schritt, ob der Code sowohl lint-fehlerfrei als auch korrekt formatiert ist. Das ist besonders nuetzlich in CI/CD-Pipelines.

---

## Pre-Commit Hooks mit Husky und lint-staged

Was passiert, wenn jemand im Team vergisst, `npm run lint` auszufuehren, bevor er seinen Code committet? Dann landen ungelintete oder unformatierte Dateien im Repository. Genau das verhindern **Pre-Commit Hooks**.

### Was sind Pre-Commit Hooks?

Ein Pre-Commit Hook ist ein Script, das automatisch **vor jedem `git commit`** ausgefuehrt wird. Wenn das Script fehlschlaegt (z.B. weil ESLint Fehler findet), wird der Commit abgebrochen.

### Husky

**Husky** macht es einfach, Git Hooks in einem Node.js-Projekt einzurichten.

```shell
npm install --save-dev husky
npx husky init
```

Der Befehl `npx husky init` erstellt einen `.husky/`-Ordner und fuegt ein `prepare`-Script in deine `package.json` ein.

### lint-staged

**lint-staged** sorgt dafuer, dass Linting und Formatierung nur auf den Dateien ausgefuehrt werden, die gerade **gestaged** sind (also die, die du mit `git add` hinzugefuegt hast). Das ist deutlich schneller, als das gesamte Projekt zu pruefen.

```shell
npm install --save-dev lint-staged
```

Fuege die Konfiguration in deine `package.json` ein:

```json
{
  "lint-staged": {
    "src/**/*.{ts,tsx}": [
      "eslint --fix",
      "prettier --write"
    ],
    "src/**/*.{json,css,md}": [
      "prettier --write"
    ]
  }
}
```

### Hook einrichten

Erstelle (oder bearbeite) die Datei `.husky/pre-commit`:

```shell
npx lint-staged
```

### So funktioniert der Ablauf

1. Du machst Aenderungen an Dateien und fuehrst `git add` aus.
2. Du fuehrst `git commit -m "mein commit"` aus.
3. **Husky** faengt den Commit ab und startet das Pre-Commit-Script.
4. **lint-staged** nimmt sich nur die gestageten Dateien.
5. Auf diesen Dateien werden `eslint --fix` und `prettier --write` ausgefuehrt.
6. Wenn alles fehlerfrei ist, wird der Commit durchgefuehrt.
7. Wenn ESLint Fehler findet, die nicht automatisch behebbar sind, wird der Commit **abgebrochen**. Du musst die Fehler dann manuell beheben.

---

## Beispiel-Setup: React + TypeScript Projekt

Hier ein komplettes Setup von Anfang bis Ende fuer ein React + TypeScript-Projekt.

### 1. Projekt erstellen (falls noch nicht vorhanden)

```shell
npm create vite@latest mein-projekt -- --template react-ts
cd mein-projekt
npm install
```

### 2. Dev-Dependencies installieren

```shell
npm install --save-dev \
  eslint \
  @eslint/js \
  @typescript-eslint/parser \
  @typescript-eslint/eslint-plugin \
  eslint-plugin-react \
  eslint-plugin-react-hooks \
  eslint-config-prettier \
  prettier \
  husky \
  lint-staged
```

### 3. ESLint konfigurieren (eslint.config.js)

```js
// eslint.config.js
import js from "@eslint/js";
import tseslint from "@typescript-eslint/eslint-plugin";
import tsparser from "@typescript-eslint/parser";
import react from "eslint-plugin-react";
import reactHooks from "eslint-plugin-react-hooks";
import prettierConfig from "eslint-config-prettier";

export default [
  js.configs.recommended,

  {
    files: ["**/*.{ts,tsx}"],
    languageOptions: {
      parser: tsparser,
      parserOptions: {
        ecmaVersion: "latest",
        sourceType: "module",
        ecmaFeatures: { jsx: true },
      },
    },
    plugins: {
      "@typescript-eslint": tseslint,
      react: react,
      "react-hooks": reactHooks,
    },
    rules: {
      // TypeScript
      "@typescript-eslint/no-unused-vars": "warn",
      "@typescript-eslint/no-explicit-any": "warn",

      // React
      "react/react-in-jsx-scope": "off",
      "react/prop-types": "off",
      "react-hooks/rules-of-hooks": "error",
      "react-hooks/exhaustive-deps": "warn",

      // Allgemein
      eqeqeq: "error",
      "no-var": "error",
      "prefer-const": "warn",
      "no-console": "warn",
    },
    settings: {
      react: { version: "detect" },
    },
  },

  prettierConfig,
  { ignores: ["node_modules/", "dist/", "build/"] },
];
```

### 4. Prettier konfigurieren (.prettierrc)

```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "all",
  "printWidth": 80,
  "bracketSpacing": true,
  "arrowParens": "always",
  "endOfLine": "lf"
}
```

### 5. .prettierignore erstellen

```
node_modules/
dist/
build/
coverage/
```

### 6. npm Scripts in package.json

```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "lint": "eslint src/",
    "lint:fix": "eslint src/ --fix",
    "format": "prettier --write src/",
    "format:check": "prettier --check src/",
    "check": "npm run lint && npm run format:check",
    "prepare": "husky"
  },
  "lint-staged": {
    "src/**/*.{ts,tsx}": [
      "eslint --fix",
      "prettier --write"
    ],
    "src/**/*.{json,css,md}": [
      "prettier --write"
    ]
  }
}
```

### 7. Husky einrichten

```shell
npx husky init
```

Inhalt von `.husky/pre-commit`:

```shell
npx lint-staged
```

### 8. VS Code Einstellungen (.vscode/settings.json)

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  }
}
```

### 9. Testen

```shell
# Lint-Pruefung starten
npm run lint

# Code formatieren
npm run format

# Alles auf einmal pruefen
npm run check
```

---

## Zusammenfassung

| Werkzeug | Aufgabe | Konfigurationsdatei |
|---|---|---|
| **ESLint** | Code-Qualitaet pruefen | `eslint.config.js` |
| **Prettier** | Code formatieren | `.prettierrc` |
| **eslint-config-prettier** | Konflikte vermeiden | (in `eslint.config.js` eingebunden) |
| **Husky** | Git Hooks verwalten | `.husky/pre-commit` |
| **lint-staged** | Nur geaenderte Dateien pruefen | `package.json` (`"lint-staged"`) |

**Die goldene Regel:** ESLint findet die Fehler, Prettier macht den Code schoen, und Husky + lint-staged stellen sicher, dass nichts Ungeprüftes ins Repository gelangt.
