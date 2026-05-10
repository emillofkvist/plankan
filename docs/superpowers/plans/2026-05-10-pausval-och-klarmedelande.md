# Paustid-dropdown & Klar-overlay — Implementationsplan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Lägg till en dropdown för paustid (10/20/30 s, standard 20) och ett fullskärmsoverlay som visas när alla tre set är klara.

**Architecture:** Alla ändringar i `index.html`. Konstanten `PAUSE_DURATION` ersätts med `getPauseDuration()`. Overlayet är ett fixed-position element som visas/döljs med en CSS-klass.

**Tech Stack:** Vanilla HTML/CSS/JS.

---

## Filstruktur

- **Ändra:** `index.html`
- **Ändra:** `PROJEKT.md`

---

### Task 1: Paustid-dropdown

**Files:**
- Modify: `index.html`

- [ ] **Steg 1: Lägg till dropdown i HTML**

Hitta:
```html
        <label class="toggle-wrapper">
            Autostart
            <input type="checkbox" class="toggle-input" id="autostart" checked>
            <span class="toggle-slider"></span>
        </label>
```
Ersätt med:
```html
        <div class="dropdown-wrapper">
            <label for="pause-duration">Paus</label>
            <select id="pause-duration">
                <option value="10">10 s</option>
                <option value="20" selected>20 s</option>
                <option value="30">30 s</option>
            </select>
        </div>
        <label class="toggle-wrapper">
            Autostart
            <input type="checkbox" class="toggle-input" id="autostart" checked>
            <span class="toggle-slider"></span>
        </label>
```

- [ ] **Steg 2: Lägg till `getPauseDuration()` och ta bort konstanten**

Hitta:
```javascript
        const PAUSE_DURATION = 30;
```
Ersätt med:
```javascript
        function getPauseDuration() {
            return parseInt(document.getElementById('pause-duration').value);
        }
```

- [ ] **Steg 3: Uppdatera `stopPause` att använda `getPauseDuration()`**

Hitta:
```javascript
                text.textContent = `Paus ${formatTime(PAUSE_DURATION)}`;
```
Ersätt med:
```javascript
                text.textContent = `Paus ${formatTime(getPauseDuration())}`;
```

- [ ] **Steg 4: Uppdatera `startPause` att använda `getPauseDuration()`**

Hitta:
```javascript
            let remaining = PAUSE_DURATION;
            banner.dataset.state = 'active';
            text.textContent = `Paus ${formatTime(remaining)}`;
            fill.style.transition = 'none';
            fill.style.width = '100%';

            setTimeout(() => {
                fill.style.transition = `width ${PAUSE_DURATION}s linear`;
                fill.style.width = '0%';
            }, 50);
```
Ersätt med:
```javascript
            const pauseDuration = getPauseDuration();
            let remaining = pauseDuration;
            banner.dataset.state = 'active';
            text.textContent = `Paus ${formatTime(remaining)}`;
            fill.style.transition = 'none';
            fill.style.width = '100%';

            setTimeout(() => {
                fill.style.transition = `width ${pauseDuration}s linear`;
                fill.style.width = '0%';
            }, 50);
```

- [ ] **Steg 5: Uppdatera `buildPauseBanner` att visa rätt starttid**

Hitta:
```javascript
                <span class="pause-text" id="pause-text-${pauseIndex}">Paus 0:30</span>
```
Ersätt med:
```javascript
                <span class="pause-text" id="pause-text-${pauseIndex}">Paus ${formatTime(getPauseDuration())}</span>
```

- [ ] **Steg 6: Lägg till change-lyssnare för pause-dropdown**

Hitta:
```javascript
        document.getElementById('duration').addEventListener('change', () => {
```
Lägg till följande block DIREKT FÖRE den raden:
```javascript
        document.getElementById('pause-duration').addEventListener('change', () => {
            for (let i = 0; i < 2; i++) {
                stopPause(i);
            }
        });

```

- [ ] **Steg 7: Verifiera i webbläsaren**

Öppna `index.html`. Controls-raden ska visa: `[ Tid ]  [ Paus ]  Autostart`. Paustiden ska ändras när du väljer 10 eller 30 s. Pausbannerns väntande-text ska uppdateras vid ändring.

- [ ] **Steg 8: Commit**

```bash
git add index.html
git commit -m "feat: add pause duration selector (10/20/30s, default 20s)"
```

---

### Task 2: Fullskärmsoverlay vid avklarade set

**Files:**
- Modify: `index.html`

- [ ] **Steg 1: Lägg till CSS för overlay**

Lägg till följande direkt efter `.pause-bar-fill { ... }` blocket och före `</style>`:

```css
        .completion-overlay {
            display: none;
            position: fixed;
            inset: 0;
            background: rgba(17, 17, 17, 0.95);
            z-index: 100;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 16px;
            cursor: pointer;
        }

        .completion-overlay.visible {
            display: flex;
        }

        .completion-title {
            font-size: 3.5rem;
            font-weight: 700;
            color: #fff;
            letter-spacing: 2px;
        }

        .completion-subtitle {
            font-size: 1.5rem;
            color: #888;
            letter-spacing: 1px;
        }
```

- [ ] **Steg 2: Lägg till overlay-element i HTML**

Hitta:
```html
    <div class="timers" id="timers"></div>
```
Ersätt med:
```html
    <div class="timers" id="timers"></div>
    <div class="completion-overlay" id="completion-overlay">
        <div class="completion-title">Klar!</div>
        <div class="completion-subtitle">Bra jobbat!</div>
    </div>
```

- [ ] **Steg 3: Lägg till `resetAll()` och `showCompletionOverlay()`**

Lägg till dessa två funktioner direkt efter `stopTimer`-funktionen (före `startTimer`):

```javascript
        function resetAll() {
            for (let i = 0; i < 2; i++) stopPause(i);
            for (let i = 0; i < 3; i++) {
                const t = timers[i];
                if (t.delayTimeout) { clearTimeout(t.delayTimeout); t.delayTimeout = null; }
                if (t.interval) { clearInterval(t.interval); t.interval = null; }
                t.state = 'idle';
                resetTimerUI(i);
            }
            activeIndex = null;
        }

        function showCompletionOverlay() {
            document.getElementById('completion-overlay').classList.add('visible');
        }
```

- [ ] **Steg 4: Trigga overlay när sista timern är klar**

Hitta:
```javascript
                        if (index < 2) {
                            startPause(index);
                        }
```
Ersätt med:
```javascript
                        if (index < 2) {
                            startPause(index);
                        } else {
                            showCompletionOverlay();
                        }
```

- [ ] **Steg 5: Lägg till click-lyssnare på overlay**

Lägg till följande rad i slutet av `init()`-funktionen, direkt före den avslutande `}`:

```javascript
            document.getElementById('completion-overlay').addEventListener('click', () => {
                document.getElementById('completion-overlay').classList.remove('visible');
                resetAll();
            });
```

- [ ] **Steg 6: Verifiera i webbläsaren**

Sätt tid till 20 s. Starta Set 1, vänta tills klart, låt paus och Set 2 + Set 3 köra klart (autostart på). Overlayet ska dyka upp. Klick på overlayet ska stänga det och återställa alla tre kort till startläge.

- [ ] **Steg 7: Commit**

```bash
git add index.html
git commit -m "feat: show completion overlay when all sets are done"
```

---

### Task 3: Uppdatera PROJEKT.md och pusha

**Files:**
- Modify: `PROJEKT.md`

- [ ] **Steg 1: Uppdatera PROJEKT.md**

I sektionen "Nuvarande funktionalitet", uppdatera:

Under "Paus mellan set", ändra "30-sekunders paus" till "valbar paus (10/20/30 sek, standard 20 sek, väljs via dropdown)".

Lägg till ny sektion efter "Autostart-toggle":

```markdown
### Klar-overlay
- Visas automatiskt när Set 3 är avklarat
- Fullskärmsoverlay med "Klar!" och "Bra jobbat!" i stor vit text
- Klick var som helst stänger overlayet och återställer alla timers till startläge
```

Under "Viktiga JS-konstanter", ändra:
```
PAUSE_DURATION = 30      // sekunder
```
till:
```
getPauseDuration()       // läser pause-duration dropdown (10/20/30 sek)
```

Lägg till `resetAll()` och `showCompletionOverlay()` i commit-historiken.

- [ ] **Steg 2: Commit och push**

```bash
git add PROJEKT.md
git commit -m "docs: update project summary with pause selector and completion overlay"
git push
```
