[← Zurück zur Übersicht](../README.md)

# TypeScript -- Der vollständige Einstieg

## Inhaltsverzeichnis

- [Was ist TypeScript?](#was-ist-typescript)
- [Installation](#installation)
- [Grundlagen: Typen](#grundlagen-typen)
- [Interfaces & Types](#interfaces--types)
- [Generics](#generics)
- [Enums](#enums)
- [Type Assertions](#type-assertions)
- [tsconfig.json erklärt](#tsconfigjson-erklärt)
- [TypeScript mit Vite/React](#typescript-mit-vitereact)
- [Häufige Fehler & Lösungen](#häufige-fehler--lösungen)
- [Weiterführende Ressourcen](#weiterführende-ressourcen)

---

## Was ist TypeScript?

TypeScript ist eine von Microsoft entwickelte Programmiersprache, die auf JavaScript aufbaut. Man bezeichnet sie als **Superset** von JavaScript -- das bedeutet, dass jeder gültige JavaScript-Code auch gültiger TypeScript-Code ist. TypeScript erweitert JavaScript um ein **statisches Typsystem**, das bereits zur Entwicklungszeit (also bevor der Code im Browser oder in Node.js läuft) Fehler erkennen kann.

### Warum TypeScript statt JavaScript?

| Aspekt | JavaScript | TypeScript |
|---|---|---|
| **Typsystem** | Dynamisch (Fehler erst zur Laufzeit) | Statisch (Fehler schon beim Schreiben) |
| **Autovervollständigung** | Eingeschränkt | Hervorragend dank Typinformationen |
| **Refactoring** | Fehleranfällig | Sicher, da der Compiler Inkonsistenzen findet |
| **Dokumentation** | Muss manuell gepflegt werden | Typen dienen als lebendige Dokumentation |
| **Lernkurve** | Geringer | Etwas höher, aber schnell lohnend |

Ein einfaches Beispiel zeigt den Unterschied:

```typescript
// JavaScript -- der Fehler fällt erst zur Laufzeit auf
function addiere(a, b) {
  return a + b;
}
addiere("5", 3); // Ergibt "53" statt 8 -- ein stiller Fehler!

// TypeScript -- der Compiler warnt uns sofort
function addiere(a: number, b: number): number {
  return a + b;
}
addiere("5", 3); // Fehler: Argument of type 'string' is not assignable to parameter of type 'number'
```

TypeScript wird am Ende immer zu JavaScript **kompiliert** (transpiliert). Der Browser oder Node.js sieht also nie TypeScript direkt -- er führt das generierte JavaScript aus.

---

## Installation

### Voraussetzung

Du brauchst [Node.js](https://nodejs.org/) und damit auch `npm` auf deinem System. Falls du das noch nicht eingerichtet hast, schau dir die Homebrew-Anleitung an, um Node.js zu installieren.

### Lokal im Projekt (empfohlen)

Die empfohlene Variante ist, TypeScript als **Dev-Dependency** in deinem Projekt zu installieren:

```typescript
// Im Projektordner:
npm init -y                   // Falls noch keine package.json existiert
npm install -D typescript     // Installiert TypeScript als Dev-Dependency
```

Danach kannst du den Compiler über `npx` aufrufen:

```typescript
npx tsc --version             // Zeigt die installierte Version
npx tsc                       // Kompiliert dein Projekt
npx tsc --init                // Erstellt eine tsconfig.json
```

### Global (für schnelle Experimente)

Für schnelle Tests kannst du TypeScript auch global installieren:

```typescript
npm install -g typescript
tsc --version                 // Jetzt direkt ohne npx verfügbar
```

**Warum lokal besser ist:** In Teamprojekten stellt eine lokale Installation sicher, dass alle dieselbe TypeScript-Version verwenden. Die Version ist in der `package.json` festgehalten und wird mit `npm install` automatisch mitinstalliert.

### Nützliche Zusatztools

```typescript
// ts-node: TypeScript direkt ausführen (ohne manuelles Kompilieren)
npm install -D ts-node

// Nutzung:
npx ts-node meinScript.ts
```

---

## Grundlagen: Typen

TypeScript kennt eine Reihe von grundlegenden Typen. Hier sind die wichtigsten im Überblick.

### Primitive Typen: `string`, `number`, `boolean`

```typescript
let name: string = "Anna";
let alter: number = 22;
let istStudent: boolean = true;

// TypeScript kann Typen auch automatisch ableiten (Type Inference):
let stadt = "Berlin";    // TypeScript erkennt: string
let semester = 4;        // TypeScript erkennt: number
```

**Tipp:** Du musst nicht immer den Typ explizit angeben. Wenn TypeScript den Typ aus dem Wert ableiten kann, ist das explizite Annotieren überflüssig. Schreibe Typen explizit hin, wenn der Typ nicht offensichtlich ist oder wenn du eine Funktion definierst.

### Arrays

```typescript
// Variante 1: Typ gefolgt von []
let noten: number[] = [1.3, 2.0, 1.7, 3.0];

// Variante 2: Generic-Schreibweise
let module: Array<string> = ["Mathe", "Informatik", "Physik"];

// Readonly-Array (kann nicht verändert werden)
let konstanten: readonly number[] = [3.14, 2.71, 1.41];
// konstanten.push(42); // Fehler! Property 'push' does not exist on type 'readonly number[]'
```

### Objekte

```typescript
// Objekttyp inline definieren
let student: { name: string; matrikelnummer: number; aktiv: boolean } = {
  name: "Max Mustermann",
  matrikelnummer: 123456,
  aktiv: true,
};

// Optionale Eigenschaften mit ?
let kurs: { titel: string; dozent: string; raum?: string } = {
  titel: "Algorithmen und Datenstrukturen",
  dozent: "Prof. Müller",
  // raum ist optional und kann weggelassen werden
};
```

### Tupel

```typescript
// Ein Tupel hat eine feste Anzahl von Elementen mit festgelegten Typen
let koordinate: [number, number] = [52.52, 13.405];
let eintrag: [string, number] = ["Klausur", 1.7];

// Zugriff:
let breite = koordinate[0]; // number
let laenge = koordinate[1]; // number
```

### `any`

```typescript
// "any" schaltet die Typprüfung komplett aus -- VERMEIDEN wenn möglich!
let irgendetwas: any = "Hallo";
irgendetwas = 42;          // Kein Fehler
irgendetwas = true;        // Kein Fehler
irgendetwas.foo.bar.baz;   // Kein Fehler zur Kompilierzeit, aber Crash zur Laufzeit!
```

**Warnung:** `any` ist das Gegenteil von Typsicherheit. Es gibt Situationen, in denen es nötig ist (z. B. bei der Migration von JavaScript zu TypeScript), aber im Normallfall solltest du `any` vermeiden.

### `unknown`

```typescript
// "unknown" ist die typsichere Alternative zu "any"
let eingabe: unknown = "Hallo Welt";

// Du kannst nicht einfach darauf zugreifen:
// eingabe.toUpperCase(); // Fehler! Object is of type 'unknown'

// Erst nach einer Typprüfung:
if (typeof eingabe === "string") {
  console.log(eingabe.toUpperCase()); // Jetzt erlaubt!
}
```

`unknown` ist ideal für Werte, deren Typ du noch nicht kennst (z. B. API-Antworten). Du wirst gezwungen, den Typ erst zu prüfen, bevor du den Wert verwenden kannst.

### `void`

```typescript
// void bedeutet: Die Funktion gibt keinen Wert zurück
function logge(nachricht: string): void {
  console.log(nachricht);
  // kein return-Statement nötig
}
```

### `never`

```typescript
// never bedeutet: Diese Funktion kehrt NIEMALS zurück
function werfeFehler(nachricht: string): never {
  throw new Error(nachricht);
  // Code nach throw wird nie erreicht
}

// Nützlich für Exhaustive Checks:
type Ampelfarbe = "rot" | "gelb" | "grün";

function behandleAmpel(farbe: Ampelfarbe): string {
  switch (farbe) {
    case "rot":
      return "Stehen bleiben";
    case "gelb":
      return "Vorsicht";
    case "grün":
      return "Gehen";
    default:
      // Wenn alle Fälle abgedeckt sind, ist 'farbe' hier 'never'.
      // Falls jemand eine neue Farbe zum Union-Type hinzufügt,
      // aber den Switch vergisst, gibt es hier einen Kompilierfehler.
      const _exhaustiveCheck: never = farbe;
      return _exhaustiveCheck;
  }
}
```

### Union Types und Literal Types

```typescript
// Union Type: Ein Wert kann mehrere Typen haben
let id: string | number;
id = "abc-123";  // OK
id = 42;         // OK
// id = true;    // Fehler!

// Literal Types: Nur bestimmte Werte erlaubt
let richtung: "links" | "rechts" | "geradeaus";
richtung = "links";     // OK
// richtung = "oben";   // Fehler!

// Kombination in der Praxis:
type ApiAntwort =
  | { status: "erfolg"; daten: string }
  | { status: "fehler"; fehlermeldung: string };

function verarbeiteAntwort(antwort: ApiAntwort) {
  if (antwort.status === "erfolg") {
    console.log(antwort.daten);           // TypeScript weiß: hier existiert 'daten'
  } else {
    console.log(antwort.fehlermeldung);   // TypeScript weiß: hier existiert 'fehlermeldung'
  }
}
```

---

## Interfaces & Types

Beide Konstrukte dienen dazu, die Form von Objekten zu beschreiben. Die Unterschiede sind subtil, aber wichtig.

### Interfaces

```typescript
interface Student {
  name: string;
  matrikelnummer: number;
  semester: number;
  email?: string;                          // Optional
  readonly immatrikuliertAm: Date;         // Kann nach Erstellung nicht geändert werden
}

const anna: Student = {
  name: "Anna Schmidt",
  matrikelnummer: 654321,
  semester: 3,
  immatrikuliertAm: new Date("2023-10-01"),
};

// anna.immatrikuliertAm = new Date(); // Fehler! readonly-Eigenschaft
```

### Interfaces erweitern (extends)

```typescript
interface Person {
  name: string;
  alter: number;
}

interface Student extends Person {
  matrikelnummer: number;
  studiengang: string;
}

interface Tutor extends Student {
  kursId: string;
  stundenProWoche: number;
}

const tutor: Tutor = {
  name: "Lena",
  alter: 24,
  matrikelnummer: 111222,
  studiengang: "Informatik",
  kursId: "INF-101",
  stundenProWoche: 10,
};
```

### Interfaces fuer Funktionen

```typescript
interface Berechnung {
  (a: number, b: number): number;
}

const addiere: Berechnung = (a, b) => a + b;
const multipliziere: Berechnung = (a, b) => a * b;
```

### Type Aliases

```typescript
// Einfacher Typ-Alias
type MatrikelNummer = number;
type Studiengang = "Informatik" | "Mathematik" | "Physik" | "Chemie";

// Objekt-Typ
type Vorlesung = {
  titel: string;
  ects: number;
  dozent: string;
  studiengang: Studiengang;
};

// Union Type (nur mit type moeglich, NICHT mit interface)
type Ergebnis = Vorlesung | null;

// Intersection Type
type MitId = {
  id: string;
};

type VorlesungMitId = Vorlesung & MitId;
```

### Unterschied: Interface vs. Type

| Eigenschaft | `interface` | `type` |
|---|---|---|
| Objekte beschreiben | Ja | Ja |
| Erweitern (extends) | Ja | Mit `&` (Intersection) |
| Union Types | Nein | Ja |
| Primitive Aliase | Nein | Ja (`type ID = string`) |
| Declaration Merging | Ja (wird zusammengeführt) | Nein (Fehler bei Dopplung) |
| Computed Properties | Nein | Ja (`[K in Keys]: V`) |

**Faustregel:**

- Verwende **`interface`**, wenn du die Form eines Objekts beschreibst und es erweiterbar sein soll (z. B. für Props in React-Komponenten, API-Datenmodelle).
- Verwende **`type`**, wenn du Union Types, Intersections, Tuple Types oder Aliase fuer primitive Typen brauchst.

```typescript
// Declaration Merging -- ein wichtiger Unterschied:
// Interfaces koennen mehrfach deklariert werden und werden zusammengeführt:
interface Fenster {
  titel: string;
}
interface Fenster {
  breite: number;
}
// Fenster hat jetzt: { titel: string; breite: number }

// Bei Types geht das NICHT:
// type Fenster = { titel: string };
// type Fenster = { breite: number }; // Fehler: Duplicate identifier 'Fenster'
```

---

## Generics

Generics erlauben es dir, Funktionen, Interfaces und Klassen zu schreiben, die mit **verschiedenen Typen** arbeiten können, ohne die Typsicherheit aufzugeben. Stell dir Generics als "Typvariablen" vor.

### Einfaches Beispiel

```typescript
// Ohne Generics: Du müsstest für jeden Typ eine eigene Funktion schreiben
function erstesElementString(arr: string[]): string | undefined {
  return arr[0];
}
function erstesElementNumber(arr: number[]): number | undefined {
  return arr[0];
}

// Mit Generics: Eine Funktion fuer alle Typen
function erstesElement<T>(arr: T[]): T | undefined {
  return arr[0];
}

const ersteZahl = erstesElement([10, 20, 30]);     // Typ: number | undefined
const erstesWort = erstesElement(["Hallo", "Welt"]); // Typ: string | undefined
```

`T` ist hier ein Platzhalter fuer den Typ, der beim Aufruf eingesetzt wird. TypeScript leitet den konkreten Typ automatisch ab.

### Generics mit Constraints

```typescript
// T muss mindestens eine 'length'-Eigenschaft haben
function gibLaenge<T extends { length: number }>(wert: T): number {
  return wert.length;
}

gibLaenge("Hallo");        // OK: string hat .length
gibLaenge([1, 2, 3]);      // OK: Array hat .length
// gibLaenge(42);           // Fehler: number hat kein .length
```

### Generics mit Interfaces

```typescript
interface ApiAntwort<T> {
  daten: T;
  status: number;
  zeitstempel: Date;
}

// Verwendung mit verschiedenen Datentypen:
type StudentenAntwort = ApiAntwort<Student[]>;
type EinzelneVorlesung = ApiAntwort<Vorlesung>;

const antwort: ApiAntwort<string[]> = {
  daten: ["Informatik", "Mathematik"],
  status: 200,
  zeitstempel: new Date(),
};
```

### Mehrere Typparameter

```typescript
function zuordnen<K extends string, V>(schluessel: K, wert: V): Record<K, V> {
  return { [schluessel]: wert } as Record<K, V>;
}

const eintrag = zuordnen("name", "Max");  // Typ: Record<"name", string>
```

### Generics mit Default-Typen

```typescript
interface Sammlung<T = string> {
  elemente: T[];
  hinzufuegen(element: T): void;
}

// Ohne expliziten Typ: T ist string
const namen: Sammlung = {
  elemente: ["Anna", "Max"],
  hinzufuegen(element) {
    this.elemente.push(element);
  },
};

// Mit explizitem Typ:
const zahlen: Sammlung<number> = {
  elemente: [1, 2, 3],
  hinzufuegen(element) {
    this.elemente.push(element);
  },
};
```

---

## Enums

Enums (Aufzaehlungstypen) definieren eine Menge von benannten Konstanten.

### Numerische Enums

```typescript
enum Wochentag {
  Montag,      // 0
  Dienstag,    // 1
  Mittwoch,    // 2
  Donnerstag,  // 3
  Freitag,     // 4
  Samstag,     // 5
  Sonntag,     // 6
}

let heute: Wochentag = Wochentag.Mittwoch;
console.log(heute);                  // 2
console.log(Wochentag[2]);          // "Mittwoch" (Reverse Mapping)
```

### String Enums (empfohlen)

```typescript
enum StudienStatus {
  Eingeschrieben = "EINGESCHRIEBEN",
  Beurlaubt = "BEURLAUBT",
  Exmatrikuliert = "EXMATRIKULIERT",
  Absolviert = "ABSOLVIERT",
}

function pruefeStatus(status: StudienStatus): string {
  switch (status) {
    case StudienStatus.Eingeschrieben:
      return "Studiert aktiv";
    case StudienStatus.Beurlaubt:
      return "Pausiert";
    case StudienStatus.Exmatrikuliert:
      return "Nicht mehr eingeschrieben";
    case StudienStatus.Absolviert:
      return "Abschluss erreicht!";
  }
}
```

**Warum String Enums bevorzugen?** String Enums sind zur Laufzeit besser lesbar (du siehst `"EINGESCHRIEBEN"` statt `0` in Logs und beim Debugging) und haben kein Reverse Mapping, was den generierten Code kleiner haelt.

### const Enums

```typescript
// const Enums werden zur Kompilierzeit komplett aufgelöst (Inlining)
const enum Prioritaet {
  Niedrig = "NIEDRIG",
  Mittel = "MITTEL",
  Hoch = "HOCH",
  Kritisch = "KRITISCH",
}

let p = Prioritaet.Hoch;
// Im generierten JS steht direkt: let p = "HOCH";
// Der Enum-Code selbst verschwindet komplett
```

### Alternative: Union Types statt Enums

In modernen TypeScript-Projekten sieht man haeufig Union Types als Alternative zu Enums:

```typescript
// Statt eines Enums:
type Prioritaet = "niedrig" | "mittel" | "hoch" | "kritisch";

// Vorteil: Kein zusätzlicher Laufzeit-Code, einfacher, gut fuer einfache Faelle
// Nachteil: Kein Namensraum, kein Reverse Mapping, kein Iterieren ueber Werte
```

---

## Type Assertions

Type Assertions sagen dem TypeScript-Compiler: "Vertrau mir, ich weiss, welchen Typ dieser Wert hat." Sie aendern den Laufzeittyp nicht -- sie sind rein fuer den Compiler.

### as-Syntax (empfohlen)

```typescript
// Haeufiger Anwendungsfall: DOM-Elemente
const eingabefeld = document.getElementById("email") as HTMLInputElement;
eingabefeld.value = "test@uni.de"; // Ohne Assertion waere .value ein Fehler

// API-Daten
interface Nutzer {
  name: string;
  email: string;
}

const rohdaten: unknown = JSON.parse('{"name": "Max", "email": "max@uni.de"}');
const nutzer = rohdaten as Nutzer;
console.log(nutzer.name); // "Max"
```

### Angle-Bracket-Syntax

```typescript
// Alternative Schreibweise (NICHT in .tsx-Dateien verwenden, da Konflikt mit JSX):
const element = <HTMLDivElement>document.getElementById("container");
```

### Vorsicht bei Type Assertions

```typescript
// Type Assertions umgehen die Typprüfung -- das kann gefaehrlich sein:
const daten = "Hallo Welt" as unknown as number;
// TypeScript beschwert sich nicht, aber zur Laufzeit ist es immer noch ein String!

// Besser: Type Guards verwenden
function istNutzer(obj: unknown): obj is Nutzer {
  return (
    typeof obj === "object" &&
    obj !== null &&
    "name" in obj &&
    "email" in obj
  );
}

const vielleichtNutzer: unknown = JSON.parse(eingabe);
if (istNutzer(vielleichtNutzer)) {
  // Hier weiss TypeScript sicher: es ist ein Nutzer
  console.log(vielleichtNutzer.name);
}
```

### Non-Null Assertion (`!`)

```typescript
// Das Ausrufezeichen sagt: "Dieser Wert ist definitiv nicht null/undefined"
const element = document.getElementById("app")!;
// Ohne ! waere der Typ: HTMLElement | null
// Mit ! ist der Typ: HTMLElement

// Vorsicht: Wenn das Element doch nicht existiert, gibt es einen Laufzeitfehler!
// Besser:
const element2 = document.getElementById("app");
if (element2) {
  // Hier ist element2 sicher HTMLElement
  element2.textContent = "Geladen";
}
```

---

## tsconfig.json erklärt

Die `tsconfig.json` ist die zentrale Konfigurationsdatei fuer TypeScript-Projekte. Erstelle sie mit:

```typescript
npx tsc --init
```

Hier sind die wichtigsten Optionen im Detail:

### Grundlegendes Beispiel

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "strict": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "jsx": "react-jsx",
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

### Die wichtigsten Optionen

#### `target`

Bestimmt, in welche JavaScript-Version kompiliert wird.

```json
"target": "ES2020"
```

| Wert | Bedeutung |
|---|---|
| `ES5` | Kompatibel mit alten Browsern (IE11) |
| `ES2015`/`ES6` | Klassen, Arrow Functions, Promises |
| `ES2020` | Optional Chaining, Nullish Coalescing -- **gute Wahl fuer die meisten Projekte** |
| `ES2022` | Top-Level Await, `.at()` |
| `ESNext` | Neueste Features (aendert sich mit jeder TS-Version) |

#### `module`

Legt das Modulsystem fuer den generierten Code fest.

```json
"module": "ESNext"
```

| Wert | Verwendung |
|---|---|
| `CommonJS` | Node.js-Projekte (aelterer Stil mit `require()`) |
| `ESNext` | Moderne Projekte mit Bundler (Vite, Webpack) |
| `NodeNext` | Moderne Node.js-Projekte (ab Node 16+) |

#### `strict`

Aktiviert alle strengen Typprüfungen auf einmal. **Immer aktivieren!**

```json
"strict": true
```

Das umfasst unter anderem:
- `strictNullChecks` -- `null` und `undefined` sind eigene Typen
- `noImplicitAny` -- Variablen ohne Typ werden nicht automatisch zu `any`
- `strictFunctionTypes` -- Strengere Prüfung von Funktionsparametern
- `strictPropertyInitialization` -- Klasseneigenschaften müssen initialisiert werden

#### `outDir`

Wohin der kompilierte JavaScript-Code geschrieben wird.

```json
"outDir": "./dist"
```

#### `rootDir`

Wo sich deine TypeScript-Quelldateien befinden.

```json
"rootDir": "./src"
```

#### `jsx`

Wie JSX (React) behandelt wird.

```json
"jsx": "react-jsx"
```

| Wert | Bedeutung |
|---|---|
| `react` | Klassisches React (`React.createElement`) |
| `react-jsx` | Neues JSX-Transform (React 17+) -- **empfohlen** |
| `react-jsxdev` | Wie `react-jsx`, aber mit Debug-Informationen |
| `preserve` | JSX bleibt erhalten (Bundler kümmert sich darum) |

### `include` und `exclude`

```json
{
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist", "**/*.test.ts"]
}
```

- `include`: Welche Dateien kompiliert werden sollen
- `exclude`: Welche Dateien/Ordner ausgeschlossen werden

---

## TypeScript mit Vite/React

### Neues Projekt erstellen

Der schnellste Weg, ein React-Projekt mit TypeScript und Vite aufzusetzen:

```typescript
npm create vite@latest mein-projekt -- --template react-ts
cd mein-projekt
npm install
npm run dev
```

### Projektstruktur

```
mein-projekt/
├── public/
│   └── vite.svg
├── src/
│   ├── assets/
│   ├── App.tsx              // React-Komponente mit TypeScript
│   ├── App.css
│   ├── main.tsx             // Einstiegspunkt
│   └── vite-env.d.ts        // Vite-Typdefinitionen
├── index.html
├── package.json
├── tsconfig.json             // Haupt-TS-Konfiguration
├── tsconfig.app.json         // App-spezifische Konfiguration
├── tsconfig.node.json        // Konfiguration fuer Vite-Config
└── vite.config.ts            // Vite-Konfiguration (in TypeScript!)
```

### React-Komponenten mit TypeScript

```typescript
// Props mit Interface definieren
interface ButtonProps {
  label: string;
  onClick: () => void;
  variante?: "primaer" | "sekundaer";
  deaktiviert?: boolean;
}

function Button({ label, onClick, variante = "primaer", deaktiviert = false }: ButtonProps) {
  return (
    <button
      className={`btn btn-${variante}`}
      onClick={onClick}
      disabled={deaktiviert}
    >
      {label}
    </button>
  );
}

export default Button;
```

### useState mit TypeScript

```typescript
import { useState } from "react";

interface Aufgabe {
  id: number;
  text: string;
  erledigt: boolean;
}

function AufgabenListe() {
  // TypeScript leitet den Typ aus dem Initialwert ab:
  const [zaehler, setZaehler] = useState(0); // Typ: number

  // Bei komplexen Typen oder wenn der Initialwert den Typ nicht verrät:
  const [aufgaben, setAufgaben] = useState<Aufgabe[]>([]);
  const [fehler, setFehler] = useState<string | null>(null);

  const aufgabeHinzufuegen = (text: string) => {
    const neueAufgabe: Aufgabe = {
      id: Date.now(),
      text,
      erledigt: false,
    };
    setAufgaben((vorherige) => [...vorherige, neueAufgabe]);
  };

  return (
    <div>
      <p>Anzahl: {aufgaben.length}</p>
      {fehler && <p className="fehler">{fehler}</p>}
      {aufgaben.map((aufgabe) => (
        <div key={aufgabe.id}>
          <span>{aufgabe.text}</span>
        </div>
      ))}
    </div>
  );
}
```

### useEffect und Event-Handler

```typescript
import { useState, useEffect, ChangeEvent, FormEvent } from "react";

function SuchFormular() {
  const [suchbegriff, setSuchbegriff] = useState("");
  const [ergebnisse, setErgebnisse] = useState<string[]>([]);

  // Event-Handler mit typisierten Events
  const handleEingabe = (e: ChangeEvent<HTMLInputElement>) => {
    setSuchbegriff(e.target.value);
  };

  const handleAbsenden = (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    console.log("Suche nach:", suchbegriff);
  };

  // useEffect mit Fetch
  useEffect(() => {
    const ladeDaten = async () => {
      try {
        const antwort = await fetch(`/api/suche?q=${suchbegriff}`);
        const daten: string[] = await antwort.json();
        setErgebnisse(daten);
      } catch (err) {
        console.error("Fehler beim Laden:", err);
      }
    };

    if (suchbegriff.length > 2) {
      ladeDaten();
    }
  }, [suchbegriff]);

  return (
    <form onSubmit={handleAbsenden}>
      <input
        type="text"
        value={suchbegriff}
        onChange={handleEingabe}
        placeholder="Suchen..."
      />
      <button type="submit">Suchen</button>
    </form>
  );
}
```

### Props mit children

```typescript
import { ReactNode } from "react";

interface KartenProps {
  titel: string;
  children: ReactNode;
}

function Karte({ titel, children }: KartenProps) {
  return (
    <div className="karte">
      <h2>{titel}</h2>
      <div className="karte-inhalt">{children}</div>
    </div>
  );
}

// Verwendung:
function App() {
  return (
    <Karte titel="Willkommen">
      <p>Das ist der Inhalt der Karte.</p>
    </Karte>
  );
}
```

---

## Häufige Fehler & Lösungen

### 1. "Type 'X' is not assignable to type 'Y'"

Der haeufigste TypeScript-Fehler. Du versuchst, einen Wert einem inkompatiblen Typ zuzuweisen.

```typescript
// Fehler:
let alter: number = "zwanzig";
// Type 'string' is not assignable to type 'number'

// Lösung: Richtigen Typ verwenden
let alter: number = 20;
```

### 2. "Object is possibly 'null' or 'undefined'"

TypeScript warnt dich, dass ein Wert `null` oder `undefined` sein könnte.

```typescript
// Fehler:
const element = document.getElementById("app");
element.textContent = "Hallo";
// Object is possibly 'null'

// Lösung 1: Null-Check
if (element) {
  element.textContent = "Hallo";
}

// Lösung 2: Optional Chaining
element?.textContent;

// Lösung 3: Non-Null Assertion (nur wenn du sicher bist!)
element!.textContent = "Hallo";
```

### 3. "Property 'X' does not exist on type 'Y'"

Du greifst auf eine Eigenschaft zu, die im Typ nicht definiert ist.

```typescript
// Fehler:
interface Nutzer {
  name: string;
}
const nutzer: Nutzer = { name: "Max" };
console.log(nutzer.email);
// Property 'email' does not exist on type 'Nutzer'

// Lösung: Eigenschaft zum Interface hinzufuegen
interface Nutzer {
  name: string;
  email?: string; // Optional, falls nicht immer vorhanden
}
```

### 4. "Argument of type 'string' is not assignable to parameter of type '...'"

Haeufig bei String-Literalen:

```typescript
// Fehler:
type Farbe = "rot" | "gruen" | "blau";
const meineFarbe = "rot"; // TypeScript inferiert: string (nicht "rot")
function setzeFarbe(farbe: Farbe) {}
setzeFarbe(meineFarbe);
// Argument of type 'string' is not assignable to parameter of type 'Farbe'

// Lösung 1: Expliziter Typ
const meineFarbe: Farbe = "rot";

// Lösung 2: as const
const meineFarbe = "rot" as const; // Typ ist jetzt "rot" (nicht string)

// Lösung 3: Direkt uebergeben
setzeFarbe("rot");
```

### 5. "Cannot find module '...' or its corresponding type declarations"

TypeScript findet keine Typen fuer ein Modul.

```typescript
// Fehler bei: import styles from './App.module.css'
// Cannot find module './App.module.css' or its corresponding type declarations

// Lösung: Deklarationsdatei erstellen (z. B. src/types/css.d.ts)
declare module "*.module.css" {
  const classes: { [key: string]: string };
  export default classes;
}

declare module "*.svg" {
  const content: string;
  export default content;
}
```

### 6. "Parameter 'X' implicitly has an 'any' type"

Im `strict`-Modus verlangt TypeScript explizite Typen fuer Funktionsparameter.

```typescript
// Fehler:
function gruss(name) {
  return `Hallo ${name}`;
}
// Parameter 'name' implicitly has an 'any' type

// Lösung: Typ annotieren
function gruss(name: string): string {
  return `Hallo ${name}`;
}
```

### 7. "Type '{}' is missing the following properties"

Du hast ein Objekt erstellt, dem erforderliche Eigenschaften fehlen.

```typescript
// Fehler:
interface Konfiguration {
  host: string;
  port: number;
  debug: boolean;
}

const config: Konfiguration = { host: "localhost" };
// Type '{ host: string; }' is missing the following properties: port, debug

// Lösung 1: Alle Eigenschaften angeben
const config: Konfiguration = { host: "localhost", port: 3000, debug: false };

// Lösung 2: Optionale Eigenschaften verwenden
interface Konfiguration {
  host: string;
  port?: number;
  debug?: boolean;
}

// Lösung 3: Partial<T> fuer Teilobjekte
const teilConfig: Partial<Konfiguration> = { host: "localhost" };
```

---

## Weiterführende Ressourcen

### Offizielle Dokumentation

- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/) -- Die offizielle und umfassendste Ressource
- [TypeScript Playground](https://www.typescriptlang.org/play) -- TypeScript direkt im Browser ausprobieren
- [TypeScript Cheat Sheets](https://www.typescriptlang.org/cheatsheets) -- Kompakte Uebersichten zum Ausdrucken

### Interaktive Lernplattformen

- [TypeScript Exercises](https://typescript-exercises.github.io/) -- Progressive Uebungen von einfach bis fortgeschritten
- [Type Challenges](https://github.com/type-challenges/type-challenges) -- Fortgeschrittene Typ-Raetsel fuer TypeScript-Profis
- [Total TypeScript](https://www.totaltypescript.com/) -- Umfangreiche Kurse von Matt Pocock

### React + TypeScript

- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/) -- Best Practices fuer React mit TypeScript
- [Vite Dokumentation](https://vitejs.dev/guide/) -- Offizielle Vite-Docs

### Empfohlene VS Code Erweiterungen

- **TypeScript Importer** -- Automatischer Import von Modulen
- **Pretty TypeScript Errors** -- Lesbarere Fehlermeldungen
- **Total TypeScript** -- Inline-Erklaerungen fuer TypeScript-Syntax
- **Error Lens** -- Zeigt Fehler direkt in der Zeile an

### Bücher

- *Effective TypeScript* von Dan Vanderkam -- 62 konkrete Wege, TypeScript besser zu nutzen
- *Programming TypeScript* von Boris Cherny -- Umfassende Einfuehrung von den Grundlagen bis zu fortgeschrittenen Themen

---

> **Tipp zum Schluss:** Der beste Weg, TypeScript zu lernen, ist es einfach zu benutzen. Starte dein naechstes Projekt mit TypeScript und dem `strict`-Modus. Die Fehlermeldungen fühlen sich anfangs ueberwältigend an, aber nach ein paar Wochen wirst du sie als hilfreiche Hinweise schaetzen, die dich vor echten Bugs bewahren.
