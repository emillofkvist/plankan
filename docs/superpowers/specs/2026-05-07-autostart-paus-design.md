# Design: Autostart & Paus — Plankan

**Datum:** 2026-05-07

## Översikt

Plankan är en planktimerapp med 3 set. Denna förändring lägger till:
1. En Autostart-toggle som styr om nästa timer startar automatiskt efter pausen
2. En alltid synlig pausbanner mellan korten som räknar ned 30 sekunder
3. Kompakt layout så att alla tre kort + pausbanners ryms på skärmen utan scrollning

## Funktionsbeskrivning

### Autostart-toggle

- Placeras i samma rad som tidsväljaren: `[ Tid: 20s ]    Autostart  [ ● ]`
- Standard: **på** (Autostart aktivt)
- Byter läge omedelbart; pågående timer påverkas inte men nästa paus/timer följer det nya läget
- Accentfärg `#5ede80` när aktiv

### Pausbanner

En slank banner (~48px hög) visas alltid mellan Set 1–2 och Set 2–3 så att layouten inte hoppar.

Tre tillstånd:

| Tillstånd | Utseende |
|-----------|----------|
| **Väntande** | Grå, diskret — visar bara `─── Paus 30s ───` |
| **Aktiv** | Grön framstegslinje + nedräkning, t.ex. `Paus 0:24` |
| **Klar** | Tonas ut; nästa korts Starta-knapp aktiveras |

Pausen på 30 sekunder startar **automatiskt** när föregående timer är klar — oavsett läge.

### Beteende per läge

**Autostart AV (manuellt):**
```
Timer klar → Paus 30s räknar ned → Paus klar → nästa kort väntar på Starta-klick
```

**Autostart PÅ:**
```
Timer klar → Paus 30s räknar ned → Nästa timer startar automatiskt
```

## Layout

Kompakt vertikal layout (fasta mått):

```
[ PLANKAN ]
[ Tid: 20s ]  Autostart [ ● ]

[ Set 1 ]  ring 100px  [ Starta ]
──── Paus 30s ──────────────────
[ Set 2 ]  ring 100px  [ Starta ]
──── Paus 30s ──────────────────
[ Set 3 ]  ring 100px  [ Starta ]
```

Förändringar för att passa utan scrollning:
- Ring: 150px → 100px
- Kortets padding: minskas
- Set-etikett och knappar görs smalare

## Vad som inte ändras

- Beep-ljud när timer är klar
- Tidsväljaren (20–60 sek)
- En timer åt gången kan köra
- Mörkt tema, grön accentfärg
