# Plankan — Projektsammanfattning

> Den här filen hålls uppdaterad efter varje förändring och är avsedd att ge full kontext vid framtida konversationer.

## Vad är Plankan?

En planktimerapp byggd som en enda HTML-fil (`index.html`). Används för att tajma plankövningar i 3 set med visuell framstegsindikator och ljudsignal.

## Teknisk stack

- Ren HTML/CSS/JS — inga ramverk, inga beroenden, ingen byggprocess
- Körs direkt i webbläsaren som en lokal fil eller via GitHub Pages
- GitHub-repo: `https://github.com/emillofkvist/plankan`

## Nuvarande funktionalitet (efter senaste uppdatering)

### Timers
- 3 set, vardera med en cirkulär framstegsring (SVG, radius 58 i viewBox-koordinater, display 100px)
- Varaktighet väljs via dropdown: 20–60 sekunder
- En timer åt gången kan köra
- 1 sekunds fördröjning innan nedräkning börjar (visar ▶)
- Beep-ljud (tre snabba pip via Web Audio API) när timern är klar

### Paus mellan set
- När ett set avslutas startar automatiskt en paus (valbar tid: 10/20/30 sek, standard 20 sek)
- Paustiden väljs via dropdown i controls-raden
- Pausen visas som en slank banner (pausbanner) mellan korten
- Bannern har tre tillstånd:
  - **waiting** — grå, diskret (0.3 opacity), visar "Paus 0:20"
  - **active** — grön, full opacity, nedräkning + framstegslinje töms från höger till vänster
  - **done** — mycket diskret (0.15 opacity)

### Autostart-toggle
- Placerad i controls-raden, standard: **på**
- **Autostart PÅ:** efter pausen startar nästa set automatiskt
- **Autostart AV:** efter pausen aktiveras nästa sets Starta-knapp men startar inte automatiskt

### Klar-overlay
- Visas automatiskt när Set 3 är avklarat (ingen paus efter sista setet)
- Fullskärmsoverlay (`position: fixed, z-index: 100`) med "Klar!" och "Bra jobbat!" i stor vit text
- Klick var som helst stänger overlayet och återställer alla timers till startläge via `resetAll()`

### Layout
- Mörkt tema, grön accentfärg `#5ede80`
- Kompakt vertikal layout: alla 3 kort + 2 pausbanners ryms utan scrollning
- Controls-rad uppifrån: `[ Tid ]  [ Paus ]  Autostart [ toggle ]`
- Ordning: Set 1 → Pausbanner → Set 2 → Pausbanner → Set 3

## Filstruktur

```
index.html                          — hela appen (HTML + CSS + JS)
docs/
  superpowers/
    specs/
      2026-05-07-autostart-paus-design.md
      2026-05-10-pausval-och-klarmedelande-design.md
    plans/
      2026-05-07-autostart-paus.md
      2026-05-10-pausval-och-klarmedelande.md
PROJEKT.md                          — den här filen
```

## Viktiga JS-funktioner och datamodell

```javascript
RADIUS = 58                // SVG-koordinater för ringen
CIRCUMFERENCE = 2π × 58   // används för stroke-dashoffset
getDuration()              // läser duration-dropdown (20–60 sek)
getPauseDuration()         // läser pause-duration dropdown (10/20/30 sek)

timers = [                 // en post per set
  { state, interval, delayTimeout }
]
// state: 'idle' | 'running' | 'done'

pauses = [                 // en post per mellanrum (set 0→1 och set 1→2)
  { interval }
]

// Nyckelrfunktioner:
startTimer(index)          // startar timer, stoppar alla aktiva pauser
startPause(pauseIndex)     // startar 30s-paus, läser getPauseDuration() vid start
stopPause(pauseIndex)      // avbryter paus, återställer banner
resetAll()                 // återställer alla timers + pauser (används av overlay)
showCompletionOverlay()    // visar "Klar! Bra jobbat!"-overlayet
```

## Designbeslut att känna till

- SVG viewBox är alltid `0 0 130 130` oavsett displaystorlek — RADIUS-konstanten i JS behöver inte ändras om displaystorleken ändras
- Pausbannern tar alltid upp plats i DOM (aldrig `display:none`) för att korten inte ska hoppa när pausen börjar
- `startTimer()` stoppar alla aktiva pauser i börjankoden — om man klickar Starta manuellt avbryts pågående paus
- Duration-ändring återställer timers + pauser; pausduration-ändring återställer bara pauser
- `resetAll()` force-resetar även timers i 'done'-state (till skillnad från `stopTimer` som skippar done)

## Commit-historik

```
c67b06a feat: show completion overlay when all sets are done
ed80988 feat: add pause duration selector (10/20/30s, default 20s)
4c0d40a feat: pause countdown and autostart logic between sets
fd33bba feat: add pause banners between timer cards
47ccccb feat: add autostart toggle to controls row
1fcb1bf style: compact card layout to fit all sets without scrolling
9bc93d1 Initial commit: Plankan tränings-timer
```
