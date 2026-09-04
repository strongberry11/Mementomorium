# Mementomorium

Offizielle One-Page-Website der Rock/Metal-Band **Mementomorium** — reines HTML/CSS/JS, ohne Framework, mobil-optimiert.

## Struktur

```
.
├── index.html            # Gesamte Seite (Hero, Musik, Tourdaten, Galerie, Kontakt)
├── css/
│   └── styles.css        # Alle Styles (Farben, Layout, Responsive)
├── js/
│   └── main.js           # Mobile Navigation, Header-Scroll-Effekt, Kontaktformular
└── assets/
    └── img/
        ├── hero-placeholder.svg   # Platzhalter-Hintergrund im Hero-Bereich
        └── gallery-01.svg … 06    # Platzhalterbilder für die Galerie
```

Einfach `index.html` im Browser öffnen oder z. B. mit `npx serve` lokal hosten — es sind keine Build-Schritte nötig.

## CI-Checks

Bei jedem Push/PR auf `main` laufen automatisch (siehe `.github/workflows/ci.yml`):

- **Lint HTML/CSS/JS** — `html-validate`, `stylelint`, `eslint` (Konfigs: `.htmlvalidate.json`, `.stylelintrc.json`, `eslint.config.mjs`)
- **Link-Check** — prüft alle internen Anker (`#musik`, `#tourdaten`, …) und externen Links auf Erreichbarkeit (`lychee`)
- **Lighthouse** — Performance-, Accessibility-, Best-Practices- und SEO-Audit

Lokal ausführen:

```bash
npm install
npm run lint          # HTML + CSS + JS in einem Rutsch
npm run lint:html
npm run lint:css
npm run lint:js
```

## Wo ihr eure eigenen Inhalte einsetzt

### 1. Bandfoto / Hero-Hintergrund
- Datei: `assets/img/hero-placeholder.svg`
- Ersetzt sie einfach durch euer eigenes Bild (z. B. `hero.jpg`) und passt in `index.html` den Pfad an:
  ```html
  <div class="hero-bg" style="background-image: url('assets/img/hero.jpg');">
  ```
- Empfohlen: mind. 1600×1000 px, komprimiert (JPG/WebP) für schnelle Ladezeiten.

### 2. Slogan / Bio-Satz
- In `index.html` im Bereich `<section id="hero">`:
  ```html
  <p class="hero-tagline">Euer Text hier</p>
  ```

### 3. Musik-Player (Spotify/Bandcamp)
- In `index.html` im Bereich `<section id="musik">` steckt ein `<iframe>` mit Platzhalter-`src`.
- **Spotify:** Auf eurem Artist-/Album-Profil auf „Teilen“ → „Einbetten“ klicken, den Code kopieren und den `src`-Wert übernehmen.
- **Bandcamp:** Im Bandcamp-Player-Editor „Embed“ wählen, den generierten `src`-Link übernehmen (funktioniert genauso als `<iframe>`).
- Die drei Buttons darunter (`Spotify`, `Apple Music`, `YouTube`) verlinken aktuell auf die Startseiten der Plattformen — ersetzt die `href`-Werte durch eure echten Profil-/Songlinks.

### 4. Tourdaten
- In `index.html` im Bereich `<section id="tourdaten">` gibt es pro Konzert einen `<li class="tour-item">`-Block mit Datum, Venue, Ort und Ticket-Link.
- Aktuell sind 3 Platzhalter-Termine eingetragen. Kopiert einen `<li>`-Block, um weitere Termine hinzuzufügen, oder löscht nicht benötigte.
- Den Ticket-Link tragt ihr im jeweiligen `href="#"` des `<a class="btn btn-outline tour-ticket">`-Elements ein.

### 5. Galerie
- Bilder liegen in `assets/img/gallery-01.svg` bis `gallery-06.svg`.
- Ersetzt sie durch eigene Fotos (gleicher Dateiname behalten oder Pfade in `index.html` anpassen).
- Für mehr/weniger Bilder einfach weitere `<div class="gallery-item"><img ...></div>`-Blöcke im Bereich `<section id="galerie">` hinzufügen oder entfernen.
- Empfohlen: quadratische Bilder (z. B. 800×800 px), als JPG/WebP komprimiert.

### 6. Kontakt & Booking
- Booking-E-Mail-Adresse: in `index.html` im Bereich `<section id="kontakt">`:
  ```html
  <a class="booking-email" href="mailto:booking@mementomorium-band.de">booking@mementomorium-band.de</a>
  ```
  Text und `mailto:`-Adresse anpassen.
- Social-Media-Links: im selben Bereich unter `.social-links` die `href="#"`-Werte durch eure echten Profil-URLs ersetzen.
- **Wichtig:** Das Kontaktformular ist aktuell nur eine clientseitige Demo (siehe `js/main.js`, Abschnitt „Kontaktformular“) — es zeigt nur eine Bestätigung an, versendet aber noch keine echte E-Mail. Für einen echten Versand könnt ihr z. B.:
  - einen Formular-Service wie Formspree, Netlify Forms oder Basin einbinden (meist nur `action`-Attribut im `<form>` anpassen), oder
  - ein eigenes Backend/eine Serverless-Function anbinden, die die Formulardaten per E-Mail verschickt.

### 7. Farben & Schrift anpassen
- Alle Grundfarben sind als CSS-Variablen ganz oben in `css/styles.css` definiert (`:root { ... }`), z. B.:
  ```css
  --color-black: #0a0a0a;
  --color-red: #b3121b;
  --color-anthracite: #1c1c1e;
  ```
- Schriftarten (`Cinzel` für Überschriften, `Oswald` für Fließtext) werden über Google Fonts im `<head>` von `index.html` geladen und können dort ausgetauscht werden.

## Hinweise zur Performance

- Keine externen Frameworks, keine Build-Tools nötig — die Seite lädt nur HTML, CSS, JS und die Google-Fonts.
- Platzhalterbilder sind leichte SVGs; sobald ihr echte Fotos einsetzt, achtet auf komprimierte JPG/WebP-Dateien, um die Ladezeit niedrig zu halten.
- `loading="lazy"` ist bereits auf Galerie- und Player-Elementen gesetzt.
