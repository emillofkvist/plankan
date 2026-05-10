# Design: Paustid-dropdown & Klar-overlay — Plankan

**Datum:** 2026-05-10

## Översikt

Två tillägg till Plankan:
1. Dropdown för att välja paustid (10 / 20 / 30 sek, standard 20)
2. Fullskärmsoverlay med "Klar! Bra jobbat!" när alla tre set är avklarade

## 1. Paustid-dropdown

Placeras i controls-raden:
```
[ Tid: 20s ]  [ Paus: 20s ]  Autostart [ ● ]
```

- Alternativ: 10 s, 20 s, 30 s — standard: **20 s**
- Samma `select`-stil som tidsväljaren
- `PAUSE_DURATION`-konstanten ersätts med `getPauseDuration()` som läser dropdown-värdet
- Om dropdown ändras medan en paus pågår: pausen återställs med ny tid
- Pausbannrarnas väntande-text uppdateras direkt vid ändring

## 2. Fullskärmsoverlay

Triggas när Set 3 (sista timern) är klar. Ingen paus efter sista setet.

- Mörkt halvtransparent overlay täcker hela skärmen
- Centrerad stor vit text: "Klar!" (rubrik) + "Bra jobbat!" (undertext)
- Klick var som helst på overlayet stänger det
- Vid stängning: alla timers och pausbanners återställs till startläge

## Vad som inte ändras

- Pausen triggas fortfarande automatiskt efter Set 1 och Set 2
- Autostart-toggle, tidsväljaren, beep-ljud, ring-animation — oförändrade
