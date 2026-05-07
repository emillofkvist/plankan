# Autostart & Paus — Implementationsplan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Lägg till en Autostart-toggle, automatisk 30-sekunders paus mellan timer-set, och komprimera layouten så alla tre kort ryms utan scrollning.

**Architecture:** Alla ändringar sker i en enda fil (`index.html`). JS-logiken utökas med ett `pauses`-array och funktionerna `startPause`/`stopPause`. CSS komprimeras och kompletteras med stilar för toggle och pausbanner.

**Tech Stack:** Vanilla HTML/CSS/JS, inga beroenden.

---

## Filstruktur

- **Ändra:** `index.html` (enda fil — CSS, HTML och JS)

---

### Task 1: Komprimera kortlayouten (CSS)

**Files:**
- Modify: `index.html` (CSS-sektionen)

- [ ] **Steg 1: Ändra `.ring-container`**

Hitta:
```css
        .ring-container {
            position: relative;
            width: 150px;
            height: 150px;
        }
```
Ersätt med:
```css
        .ring-container {
            position: relative;
            width: 100px;
            height: 100px;
        }
```

- [ ] **Steg 2: Ändra `.ring-center` font-size**

Hitta:
```css
        .ring-center {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 2.4rem;
            font-weight: 700;
            color: #fff;
            line-height: 1;
            letter-spacing: -1px;
        }

        .ring-center.waiting {
            font-size: 1.6rem;
            color: #5ede80;
        }

        .ring-center.klar {
            font-size: 1.2rem;
            color: #555;
            letter-spacing: 1px;
            text-transform: uppercase;
        }
```
Ersätt med:
```css
        .ring-center {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 1.6rem;
            font-weight: 700;
            color: #fff;
            line-height: 1;
            letter-spacing: -1px;
        }

        .ring-center.waiting {
            font-size: 1.1rem;
            color: #5ede80;
        }

        .ring-center.klar {
            font-size: 0.9rem;
            color: #555;
            letter-spacing: 1px;
            text-transform: uppercase;
        }
```

- [ ] **Steg 3: Komprimera `.timer-card`**

Hitta:
```css
        .timer-card {
            background: #1a1a1a;
            border-radius: 20px;
            padding: 28px 24px 24px;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 18px;
            border: 1px solid #252525;
            transition: opacity 0.4s;
        }
```
Ersätt med:
```css
        .timer-card {
            background: #1a1a1a;
            border-radius: 20px;
            padding: 14px 16px 12px;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            border: 1px solid #252525;
            transition: opacity 0.4s;
        }
```

- [ ] **Steg 4: Komprimera `.start-btn`**

Hitta:
```css
        .start-btn {
            background: #5ede80;
            color: #111;
            border: none;
            border-radius: 12px;
            padding: 13px 0;
            font-size: 0.95rem;
            font-weight: 700;
            letter-spacing: 1px;
            text-transform: uppercase;
            cursor: pointer;
            width: 100%;
            max-width: 180px;
            transition: background 0.2s, transform 0.1s, opacity 0.2s;
        }
```
Ersätt med:
```css
        .start-btn {
            background: #5ede80;
            color: #111;
            border: none;
            border-radius: 12px;
            padding: 8px 0;
            font-size: 0.9rem;
            font-weight: 700;
            letter-spacing: 1px;
            text-transform: uppercase;
            cursor: pointer;
            width: 100%;
            max-width: 140px;
            transition: background 0.2s, transform 0.1s, opacity 0.2s;
        }
```

- [ ] **Steg 5: Minska `h1` och ta bort gap på `.timers`**

Hitta:
```css
        h1 {
            font-size: 2rem;
            font-weight: 700;
            letter-spacing: 3px;
            text-transform: uppercase;
            margin-bottom: 28px;
            color: #fff;
        }
```
Ersätt med:
```css
        h1 {
            font-size: 2rem;
            font-weight: 700;
            letter-spacing: 3px;
            text-transform: uppercase;
            margin-bottom: 12px;
            color: #fff;
        }
```

Hitta:
```css
        .timers {
            display: flex;
            flex-direction: column;
            gap: 20px;
            width: 100%;
            max-width: 340px;
        }
```
Ersätt med:
```css
        .timers {
            display: flex;
            flex-direction: column;
            gap: 0;
            width: 100%;
            max-width: 340px;
        }
```

- [ ] **Steg 6: Ändra `.set-label` font-size**

Hitta:
```css
        .set-label {
            font-size: 0.8rem;
            color: #555;
            letter-spacing: 2px;
            text-transform: uppercase;
        }
```
Ersätt med:
```css
        .set-label {
            font-size: 0.7rem;
            color: #555;
            letter-spacing: 2px;
            text-transform: uppercase;
        }
```

- [ ] **Steg 7: Verifiera i webbläsaren**

Öppna `index.html` i webbläsaren. Alla tre kort ska synas utan scrollning. Ingen funktionalitet har ändrats än.

- [ ] **Steg 8: Commit**

```bash
git add index.html
git commit -m "style: compact card layout to fit all sets without scrolling"
```

---

### Task 2: Controls-rad med Autostart-toggle (HTML + CSS)

**Files:**
- Modify: `index.html` (CSS-sektionen och HTML-sektionen)

- [ ] **Steg 1: Lägg till CSS för controls-rad och toggle**

Hitta:
```css
        .dropdown-wrapper {
            display: flex;
            align-items: center;
            gap: 12px;
            margin-bottom: 36px;
        }

        .dropdown-wrapper label {
            font-size: 0.9rem;
            color: #888;
            letter-spacing: 1px;
            text-transform: uppercase;
        }
```
Ersätt med:
```css
        .controls-row {
            display: flex;
            align-items: center;
            gap: 24px;
            margin-bottom: 16px;
        }

        .dropdown-wrapper {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .dropdown-wrapper label {
            font-size: 0.9rem;
            color: #888;
            letter-spacing: 1px;
            text-transform: uppercase;
        }

        .toggle-wrapper {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 0.85rem;
            color: #888;
            letter-spacing: 1px;
            text-transform: uppercase;
            cursor: pointer;
            user-select: none;
        }

        .toggle-input {
            display: none;
        }

        .toggle-slider {
            width: 40px;
            height: 22px;
            background: #333;
            border-radius: 11px;
            position: relative;
            transition: background 0.2s;
            flex-shrink: 0;
        }

        .toggle-slider::after {
            content: '';
            position: absolute;
            width: 16px;
            height: 16px;
            background: #888;
            border-radius: 50%;
            top: 3px;
            left: 3px;
            transition: transform 0.2s, background 0.2s;
        }

        .toggle-input:checked + .toggle-slider {
            background: #1a3d25;
        }

        .toggle-input:checked + .toggle-slider::after {
            background: #5ede80;
            transform: translateX(18px);
        }
```

- [ ] **Steg 2: Uppdatera HTML — wrap dropdown och toggle i `.controls-row`**

Hitta:
```html
    <div class="dropdown-wrapper">
        <label for="duration">Tid</label>
        <select id="duration">
            <option value="20" selected>20 s</option>
            <option value="25">25 s</option>
            <option value="30">30 s</option>
            <option value="35">35 s</option>
            <option value="40">40 s</option>
            <option value="45">45 s</option>
            <option value="50">50 s</option>
            <option value="55">55 s</option>
            <option value="60">60 s</option>
        </select>
    </div>
```
Ersätt med:
```html
    <div class="controls-row">
        <div class="dropdown-wrapper">
            <label for="duration">Tid</label>
            <select id="duration">
                <option value="20" selected>20 s</option>
                <option value="25">25 s</option>
                <option value="30">30 s</option>
                <option value="35">35 s</option>
                <option value="40">40 s</option>
                <option value="45">45 s</option>
                <option value="50">50 s</option>
                <option value="55">55 s</option>
                <option value="60">60 s</option>
            </select>
        </div>
        <label class="toggle-wrapper">
            Autostart
            <input type="checkbox" class="toggle-input" id="autostart" checked>
            <span class="toggle-slider"></span>
        </label>
    </div>
```

- [ ] **Steg 3: Verifiera i webbläsaren**

Tidsväljaren och Autostart-toggle ska sitta i samma rad. Toggle ska vara aktiv (grön) som standard och gå att klicka på.

- [ ] **Steg 4: Commit**

```bash
git add index.html
git commit -m "feat: add autostart toggle to controls row"
```

---

### Task 3: Pausbanners (HTML + CSS)

**Files:**
- Modify: `index.html` (CSS-sektionen och JS `init()`-funktionen)

- [ ] **Steg 1: Lägg till CSS för pausbanner**

Lägg till följande CSS direkt efter `.start-btn.running { ... }` blocket (i slutet av `<style>`-sektionen):

```css
        .pause-banner {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 6px;
            padding: 10px 24px;
            opacity: 0.3;
            transition: opacity 0.3s;
        }

        .pause-banner[data-state="active"] {
            opacity: 1;
        }

        .pause-banner[data-state="done"] {
            opacity: 0.15;
        }

        .pause-text {
            font-size: 0.7rem;
            color: #888;
            letter-spacing: 2px;
            text-transform: uppercase;
        }

        .pause-banner[data-state="active"] .pause-text {
            color: #5ede80;
        }

        .pause-bar {
            width: 100%;
            height: 3px;
            background: #252525;
            border-radius: 2px;
            overflow: hidden;
        }

        .pause-bar-fill {
            height: 100%;
            background: #5ede80;
            width: 100%;
        }
```

- [ ] **Steg 2: Uppdatera `init()` för att lägga in pausbanners**

Hitta:
```javascript
        function init() {
            const container = document.getElementById('timers');
            for (let i = 0; i < 3; i++) {
                container.appendChild(buildTimer(i));
            }
        }
```
Ersätt med:
```javascript
        function buildPauseBanner(pauseIndex) {
            const banner = document.createElement('div');
            banner.className = 'pause-banner';
            banner.id = `pause-${pauseIndex}`;
            banner.dataset.state = 'waiting';
            banner.innerHTML = `
                <span class="pause-text" id="pause-text-${pauseIndex}">Paus 0:30</span>
                <div class="pause-bar"><div class="pause-bar-fill" id="pause-fill-${pauseIndex}"></div></div>
            `;
            return banner;
        }

        function init() {
            const container = document.getElementById('timers');
            for (let i = 0; i < 3; i++) {
                container.appendChild(buildTimer(i));
                if (i < 2) {
                    container.appendChild(buildPauseBanner(i));
                }
            }
        }
```

- [ ] **Steg 3: Uppdatera `buildTimer()` — ändra SVG-storlek**

Hitta i `buildTimer`:
```javascript
                    <svg class="ring-svg" width="150" height="150" viewBox="0 0 130 130">
```
Ersätt med:
```javascript
                    <svg class="ring-svg" width="100" height="100" viewBox="0 0 130 130">
```

- [ ] **Steg 4: Verifiera i webbläsaren**

Sidan ska visa: kort, pausbanner, kort, pausbanner, kort. Pausbannern är grå och diskret. Ringen har rätt storlek.

- [ ] **Steg 5: Commit**

```bash
git add index.html
git commit -m "feat: add pause banners between timer cards"
```

---

### Task 4: Pauslogik och Autostart (JS)

**Files:**
- Modify: `index.html` (JS-sektionen)

- [ ] **Steg 1: Lägg till konstanter och `pauses`-array**

Hitta:
```javascript
        const RADIUS = 58;
        const CIRCUMFERENCE = 2 * Math.PI * RADIUS;

        let activeIndex = null;

        const timers = [
            { state: 'idle', interval: null, delayTimeout: null },
            { state: 'idle', interval: null, delayTimeout: null },
            { state: 'idle', interval: null, delayTimeout: null },
        ];
```
Ersätt med:
```javascript
        const RADIUS = 58;
        const CIRCUMFERENCE = 2 * Math.PI * RADIUS;
        const PAUSE_DURATION = 30;

        let activeIndex = null;

        const timers = [
            { state: 'idle', interval: null, delayTimeout: null },
            { state: 'idle', interval: null, delayTimeout: null },
            { state: 'idle', interval: null, delayTimeout: null },
        ];

        const pauses = [
            { interval: null },
            { interval: null },
        ];
```

- [ ] **Steg 2: Lägg till `formatTime`, `stopPause` och `startPause`**

Lägg till följande tre funktioner direkt efter `playBeep()` funktionen (före `setRing`):

```javascript
        function formatTime(s) {
            return `${Math.floor(s / 60)}:${String(s % 60).padStart(2, '0')}`;
        }

        function stopPause(pauseIndex) {
            const p = pauses[pauseIndex];
            if (p.interval) { clearInterval(p.interval); p.interval = null; }
            const banner = document.getElementById(`pause-${pauseIndex}`);
            const text = document.getElementById(`pause-text-${pauseIndex}`);
            const fill = document.getElementById(`pause-fill-${pauseIndex}`);
            if (banner) {
                banner.dataset.state = 'waiting';
                text.textContent = `Paus ${formatTime(PAUSE_DURATION)}`;
                fill.style.transition = 'none';
                fill.style.width = '100%';
            }
        }

        function startPause(pauseIndex) {
            const p = pauses[pauseIndex];
            const banner = document.getElementById(`pause-${pauseIndex}`);
            const text = document.getElementById(`pause-text-${pauseIndex}`);
            const fill = document.getElementById(`pause-fill-${pauseIndex}`);

            let remaining = PAUSE_DURATION;
            banner.dataset.state = 'active';
            text.textContent = `Paus ${formatTime(remaining)}`;
            fill.style.transition = 'none';
            fill.style.width = '100%';

            setTimeout(() => {
                fill.style.transition = `width ${PAUSE_DURATION}s linear`;
                fill.style.width = '0%';
            }, 50);

            p.interval = setInterval(() => {
                remaining--;
                text.textContent = `Paus ${formatTime(remaining)}`;

                if (remaining <= 0) {
                    clearInterval(p.interval);
                    p.interval = null;
                    banner.dataset.state = 'done';

                    const nextIndex = pauseIndex + 1;
                    if (document.getElementById('autostart').checked) {
                        startTimer(nextIndex);
                    } else {
                        const btn = document.getElementById(`btn-${nextIndex}`);
                        btn.disabled = false;
                        btn.textContent = 'Starta';
                    }
                }
            }, 1000);
        }
```

- [ ] **Steg 3: Stoppa pauser när en ny timer startas**

I `startTimer(index)`, lägg till en loop för att stoppa alla aktiva pauser direkt i början av funktionen (efter `if (timers[index].state === 'done') return;`):

Hitta:
```javascript
        function startTimer(index) {
            if (timers[index].state === 'done') return;

            // Stop whatever is currently running
            if (activeIndex !== null && activeIndex !== index) {
                stopTimer(activeIndex, true);
            }
```
Ersätt med:
```javascript
        function startTimer(index) {
            if (timers[index].state === 'done') return;

            for (let i = 0; i < pauses.length; i++) {
                if (pauses[i].interval) stopPause(i);
            }

            // Stop whatever is currently running
            if (activeIndex !== null && activeIndex !== index) {
                stopTimer(activeIndex, true);
            }
```

- [ ] **Steg 4: Trigga `startPause` när en timer avslutas**

I `startTimer`, hitta blocket inuti `setInterval` där timern är klar:

```javascript
                    if (remaining <= 0) {
                        clearInterval(t.interval);
                        t.interval = null;
                        t.state = 'done';
                        activeIndex = null;

                        setRing(index, 0, duration);
                        text.textContent = 'Klar';
                        text.className = 'ring-center klar';
                        document.getElementById(`card-${index}`).classList.add('done');
                        btn.textContent = 'Klar';
                        btn.className = 'start-btn';
                        btn.disabled = true;

                        playBeep();
                    } else {
```
Ersätt med:
```javascript
                    if (remaining <= 0) {
                        clearInterval(t.interval);
                        t.interval = null;
                        t.state = 'done';
                        activeIndex = null;

                        setRing(index, 0, duration);
                        text.textContent = 'Klar';
                        text.className = 'ring-center klar';
                        document.getElementById(`card-${index}`).classList.add('done');
                        btn.textContent = 'Klar';
                        btn.className = 'start-btn';
                        btn.disabled = true;

                        playBeep();

                        if (index < 2) {
                            startPause(index);
                        }
                    } else {
```

- [ ] **Steg 5: Stoppa pauser när duration ändras**

Hitta:
```javascript
        document.getElementById('duration').addEventListener('change', () => {
            for (let i = 0; i < 3; i++) {
                if (timers[i].state !== 'done') {
                    stopTimer(i, true);
                }
            }
        });
```
Ersätt med:
```javascript
        document.getElementById('duration').addEventListener('change', () => {
            for (let i = 0; i < 2; i++) {
                stopPause(i);
            }
            for (let i = 0; i < 3; i++) {
                if (timers[i].state !== 'done') {
                    stopTimer(i, true);
                }
            }
        });
```

- [ ] **Steg 6: Verifiera Autostart PÅ**

Öppna `index.html`. Toggle ska vara grön (på).
1. Klicka Starta på Set 1 — timern räknar ned
2. När Set 1 är klar: pausbannern mellan Set 1 och 2 räknar ned från 0:30
3. När pausen är klar: Set 2 startar automatiskt
4. När Set 2 är klar: pausbannern mellan Set 2 och 3 räknar ned
5. Set 3 startar automatiskt

- [ ] **Steg 7: Verifiera Autostart AV**

Stäng av toggle (grå).
1. Klicka Starta på Set 1 — timern räknar ned
2. När Set 1 är klar: pausbannern räknar ned
3. När pausen är klar: Set 2:s knapp aktiveras men startar inte automatiskt
4. Klicka Starta på Set 2 manuellt — fungerar

- [ ] **Steg 8: Commit**

```bash
git add index.html
git commit -m "feat: pause countdown and autostart logic between sets"
```

---

### Task 5: Push till GitHub

- [ ] **Steg 1: Push**

```bash
git push
```

- [ ] **Steg 2: Verifiera**

Kontrollera att alla commits är pushade och att GitHub visar rätt version.
