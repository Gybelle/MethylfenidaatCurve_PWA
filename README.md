# Rilatine Curve — PWA

Een simpele progressive web app die de werkingscurve van je Rilatine (methylfenidaat IR) toont, inclusief rebound. Je kan je inname-tijdstip ingeven via **Neem nu** of via het tijd-veld. De app onthoudt je keuze de rest van de dag en toont een oranje "Nu"-marker op de curve.

## Inhoud van de map

```
rilatine-pwa/
├── index.html                   ← de hele app (UI + logica)
├── manifest.webmanifest         ← PWA manifest
├── sw.js                        ← service worker (offline na 1e load)
├── icon-180.png                 ← Apple touch icon
├── icon-192.png
├── icon-512.png
└── icon-512-maskable.png
```

---

## Hosting

Een PWA werkt enkel via HTTPS. Kies één van onderstaande opties.

### Optie A — Netlify Drop (makkelijkst, geen account nodig)

1. Surf naar <https://app.netlify.com/drop>.
2. Sleep de volledige map `rilatine-pwa` in het drop-vak.
3. Je krijgt een URL terug, bv. `https://random-name-123.netlify.app`.
4. Open die URL in **Safari** op je iPhone (geen Chrome/Firefox, die kunnen geen PWA installeren op iOS).
5. Tik op het deel-icoon → **Zet op beginscherm**.
6. De app staat nu als icoon op je homescreen en opent fullscreen, zonder Safari-balk.

### Optie B — GitHub Pages

1. Maak een (publieke of private) GitHub repo.
2. Upload de inhoud van `rilatine-pwa/` naar de repo (root).
3. Settings → Pages → Source: `main` branch, folder `/ (root)`. Save.
4. Na een minuutje krijg je een URL: `https://jouwnaam.github.io/jouwrepo/`.
5. Open in Safari op iPhone → Deel → **Zet op beginscherm**.

### Optie C — iCloud (niet aanbevolen)

iCloud Drive toont HTML als downloadbaar bestand, niet als webpagina. Werkt dus niet voor een PWA.

---

## iOS installatie-stappen

Nadat je een URL hebt:

1. Open Safari op je iPhone.
2. Ga naar de URL.
3. Tik op het deel-icoon (vierkantje met pijl omhoog) onderaan.
4. Scroll omlaag → **Zet op beginscherm** → tik **Voeg toe**.
5. De app staat nu op je homescreen als **Rilatine**.

De eerste keer dat je hem opent moet je internet hebben (voor Chart.js). Daarna werkt de app volledig offline dankzij de service worker.

---

## Gebruik

- **Dosis-toggle** — kies 10 mg of 20 mg voor je volgende inname. De app onthoudt je laatste keuze.
- **Neem nu** — markeert het huidige tijdstip als inname met de geselecteerde dosis.
- **Tijd-veld + Voeg toe** — voor een ander tijdstip (bv. als je al een uur geleden hebt ingenomen).
- **Meerdere innames per dag** worden ondersteund: elke inname verschijnt als een rijtje met tijdstip, dosis, verwachte piek, einde en rebound-venster. Druk op **×** om een inname te verwijderen.
- **Alles wissen** — onderaan het inname-blokje, wist alles van vandaag.
- De combinatiecurve is de **som van alle doses**, elk geschaald naar hun mg (10 mg = halve hoogte van 20 mg). Twee overlappende doses kunnen dus boven "Piek" uitkomen.
- De oranje verticale lijn op de grafiek is **Nu**, met label en een stip op de gecombineerde curve.
- Het bovenste kaartje toont in welke fase je zit en hoelang geleden je laatste inname was.
- State wordt in `localStorage` bewaard met datum-stempel. Morgen begint de app leeg — geen verwarring met gisteren.

---

## Aanpassen

Alles zit in `index.html`:

- Referentie-dosis voor curve-hoogte 1.0: `const REFERENCE_MG = 20;`
- Kleuren: bovenaan `<style>` in de `:root`-block.
- Farmacokinetisch model: constanten bovenaan het `<script>`-blok.
- Extra dosis-opties (bv. 5 mg, 15 mg): pas de `<div class="dose-toggle">` in de HTML aan met extra knoppen `<button data-mg="15">15 mg</button>`.

Als je `index.html` aanpast, update dan ook `CACHE_NAME` in `sw.js` (bv. van `rilatine-v2` naar `rilatine-v3`) zodat iPhones de nieuwe versie laden in plaats van de oude uit de cache.
