# Mementomorium

Offizielle One-Page-Website der Rock/Metal-Band **Mementomorium** — gebaut mit [Astro](https://astro.build) und [Tailwind CSS](https://tailwindcss.com) (v4), mobil-optimiert, ohne unnötigen Client-JS-Ballast.

## Setup

```bash
npm install
npm run dev       # Dev-Server mit Hot-Reload, siehe ausgegebene URL
npm run build     # Statischer Production-Build nach dist/
npm run preview   # dist/ lokal ansehen
```

## Struktur

```
.
├── astro.config.mjs        # Astro-Konfiguration (inkl. Tailwind-Vite-Plugin)
├── src/
│   ├── layouts/
│   │   └── BaseLayout.astro    # <head>, Google Fonts, globales CSS
│   ├── components/
│   │   ├── Header.astro        # Nav + mobiles Menü + Scroll-Effekt
│   │   ├── Hero.astro          # Bandname, Slogan, CTA
│   │   ├── Music.astro         # Player-Embed + Streaming-Links
│   │   ├── TourDates.astro     # Tourdaten-Liste (Daten im Frontmatter)
│   │   ├── Gallery.astro       # Foto-Grid
│   │   ├── Contact.astro       # Kontaktformular + Booking
│   │   └── Footer.astro
│   ├── pages/
│   │   └── index.astro         # Setzt alle Komponenten zur Seite zusammen
│   └── styles/
│       └── global.css          # Tailwind-Import + Farbpalette/Fonts als @theme
└── public/
    └── assets/img/              # Platzhalterbilder (SVG), 1:1 unter / erreichbar
```

## Wo ihr eure eigenen Inhalte einsetzt

### 1. Bandfoto / Hero-Hintergrund
- Datei: `public/assets/img/hero-placeholder.svg`
- Eigenes Bild reinlegen (z. B. `public/assets/img/hero.jpg`) und in `src/components/Hero.astro` den Pfad anpassen:
  ```astro
  style="background-image: url('/assets/img/hero.jpg');"
  ```
- Empfohlen: mind. 1600×1000 px, komprimiert (JPG/WebP) für schnelle Ladezeiten.

### 2. Slogan / Bio-Satz
In `src/components/Hero.astro`, der `<p>` direkt unter der Überschrift.

### 3. Musik-Player (Spotify/Bandcamp)
- In `src/components/Music.astro` steckt ein `<iframe>` mit Platzhalter-`src`.
- **Spotify:** Auf eurem Artist-/Album-Profil auf „Teilen“ → „Einbetten“ klicken, den Code kopieren und den `src`-Wert übernehmen.
- **Bandcamp:** Im Bandcamp-Player-Editor „Embed“ wählen, den generierten `src`-Link übernehmen.
- Die Streaming-Links (Spotify/Apple Music/YouTube) stehen als kleines Array `streamLinks` im Frontmatter der Komponente — dort `href` auf eure echten Profil-/Songlinks setzen.

### 4. Tourdaten
In `src/components/TourDates.astro` ist die Liste der Shows ein simples Array (`shows`) im Frontmatter:
```js
const shows = [
  { day: "14", month: "NOV", year: "2026", venue: "Underground Hall", location: "Berlin, Deutschland", ticketUrl: "#" },
  // weitere Termine hier ergänzen/entfernen
];
```
Termin hinzufügen = neuen Eintrag ins Array schreiben, kein HTML-Duplizieren nötig.

### 5. Galerie
- Bilder liegen unter `public/assets/img/gallery-01.svg` bis `gallery-06.svg`.
- `src/components/Gallery.astro` generiert die Grid-Items automatisch aus einer Liste (`photos`). Für mehr/weniger Bilder einfach die Anzahl in `Array.from({ length: 6 }, ...)` anpassen und passende Dateien unter `public/assets/img/` ablegen.
- Empfohlen: quadratische Bilder (z. B. 800×800 px), als JPG/WebP komprimiert.

### 6. Kontakt & Booking
- Booking-E-Mail-Adresse und Social-Links stehen im Frontmatter von `src/components/Contact.astro` (`bookingEmail`, `socialLinks`).
- **Wichtig:** Das Kontaktformular ist aktuell nur eine clientseitige Demo (`<script>` am Ende von `Contact.astro`) — es zeigt nur eine Bestätigung an, versendet aber noch keine echte E-Mail. Für echten Versand z. B.:
  - einen Formular-Service wie Formspree, Netlify Forms oder Basin einbinden (meist nur `action`-Attribut am `<form>` anpassen), oder
  - eine Astro-Server-Route/API-Endpoint bzw. Serverless-Function anbinden, die die Formulardaten per E-Mail verschickt.

### 7. Farben & Schrift anpassen
Alle Design-Tokens liegen zentral in `src/styles/global.css` unter `@theme`:
```css
@theme {
  --color-ink: #0a0a0a;
  --color-blood: #b3121b;
  --color-charcoal: #1c1c1e;
  --font-display: "Cinzel", serif;
  --font-body: "Oswald", sans-serif;
}
```
Jede `--color-*`/`--font-*`-Variable erzeugt automatisch passende Tailwind-Utility-Klassen (z. B. `bg-ink`, `text-blood-bright`, `font-display`) — Wert hier ändern reicht, keine Suche-und-Ersetze im Markup nötig. Schriftarten werden zusätzlich über Google Fonts in `src/layouts/BaseLayout.astro` geladen.

## Für Mitentwickler: Tailwind-Konventionen in diesem Projekt

- **Styling ausschließlich über Utility-Klassen** direkt im Markup der `.astro`-Dateien, kein zusätzliches Komponenten-CSS. `global.css` enthält nur den Tailwind-Import, die `@theme`-Tokens und die eine Keyframe-Animation, die sich nicht sinnvoll als Utility ausdrücken lässt.
- **Interaktive Zustände (Mobile-Menü, Header-Scroll-Effekt)** laufen über `data-*`-Attribute + Tailwinds `data-[...]:`/`group-data-[...]:`/`peer-data-[...]:`-Varianten statt über zusätzliche CSS-Klassen oder manuelles Klassen-Toggling im JS — siehe `src/components/Header.astro`. Neue Interaktionen bitte nach demselben Muster umsetzen.
- **Tourdaten und Galerie-Bilder sind Daten, kein Markup** — Listen im Komponenten-Frontmatter erweitern/kürzen, siehe oben.

## CI-Checks

Bei jedem Push/PR auf `main` laufen automatisch (siehe `.github/workflows/ci.yml`):

- **Build & Lint** — `astro build`, `astro check` (Typen/Props), `stylelint` (`src/**/*.css`), `eslint` (`src/**/*.{js,astro}`, via `eslint-plugin-astro`), `html-validate` gegen das gebaute `dist/index.html`
- **Link-Check** — prüft alle internen Anker (`#musik`, `#tourdaten`, …) und externen Links im Build-Output auf Erreichbarkeit (`lychee`)
- **Lighthouse** — Performance-, Accessibility-, Best-Practices- und SEO-Audit gegen den Production-Build

Lokal ausführen:

```bash
npm install
npm run build
npm run lint        # astro check + stylelint + eslint
npm run lint:html   # html-validate gegen dist/ (nach npm run build)
```

## Hinweise zur Performance

- Astro liefert standardmäßig **kein Client-JavaScript** aus, außer für Komponenten, die es explizit brauchen (hier: Mobile-Menü/Header-Scroll in `Header.astro`, Formular-Demo in `Contact.astro`).
- Tailwind entfernt im Build automatisch alle ungenutzten Utility-Klassen — es wird nur das CSS ausgeliefert, das die Seite tatsächlich verwendet.
- Platzhalterbilder sind leichte SVGs; sobald ihr echte Fotos einsetzt, achtet auf komprimierte JPG/WebP-Dateien, um die Ladezeit niedrig zu halten.
- `loading="lazy"` ist bereits auf Galerie- und Player-Elementen gesetzt.
