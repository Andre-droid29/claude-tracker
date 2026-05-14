# claude-tracker

Single-file HTML widget zum Tracken der Claude-Nutzung (5h-Fenster + Wochenlimit). Konzipiert als **Android-Homescreen-Widget** über eine WebView-Wrapper-App, deployed via Cloudflare Pages.

![Status](https://img.shields.io/badge/status-live-d97757) ![Deploy](https://img.shields.io/badge/deploy-Cloudflare%20Pages-orange)

---

## Features

- 📊 **Live-Tracking** für 5h-Rolling-Window und 7d-Wochenlimit
- ⏱️ **Selbst-aktualisierende Reset-Timer** (1s-Tick)
- 💾 **Persistenter Speicher** via localStorage (überlebt App-Restart)
- 🎨 **Native Claude-App-Optik** — glasiger Dark-Look, Anthropic-Orange (`#d97757`), italic Fraunces-Header
- 📱 **Mobile-first 4x1 Widget-Layout** — passt in oberste Homescreen-Zeile
- ⚙️ **Plan-Profile** (Free / Pro / Max 5x / Max 20x) + **Custom-Limits** zum Kalibrieren
- ↩️ **Undo** der letzten Nachricht + **Reset**-Funktion
- 🚫 **Kein Build-Step, keine Dependencies** — pure HTML/CSS/JS

---

## Architektur

| Layer | Tech |
|---|---|
| UI | Pure HTML + CSS + Vanilla JS (single file) |
| Fonts | Google Fonts (Fraunces italic, Inter) |
| Persistenz | `localStorage` (key: `claude-tracker-v1`) |
| Hosting | Cloudflare Pages (Auto-Deploy on push) |
| Display auf Handy | WebView-Wrapper-App (z. B. „HTML Widget") |

**Datenfluss:**
```
Tap [+] → state.messages.push(Date.now()) → localStorage → setInterval(update, 1000) → DOM
```

5h-Fenster ist ein **rolling window**: jede Nachricht hat ein Timestamp, Nachrichten älter als 5h fallen automatisch aus dem aktiven Set. Reset-Timer = Ablauf des **ältesten Eintrags** im Fenster.

---

## Deployment (Cloudflare Pages)

1. **dash.cloudflare.com** → Workers & Pages → Create application → Pages → **Connect to Git**
2. Repository `Andre-droid29/claude-tracker` auswählen → **Begin setup**
3. Build settings **leer lassen** (keine Build-Befehle nötig — statisches HTML)
4. **Save and Deploy**

Nach ~30 Sek live unter `https://claude-tracker.pages.dev`.

**Optional Custom Domain:**
- Pages-Projekt → Custom domains → `tracker.homelab-hh.de`
- DNS-Eintrag erfolgt automatisch (Domain ist in Cloudflare verwaltet)

---

## Homescreen-Setup (Android)

1. Play Store → **„HTML Widget"** installieren (kostenlos)
2. Long-press auf Homescreen → Widgets → HTML Widget → **4x1** auf oberste Zeile
3. URL eintragen: `https://claude-tracker.pages.dev`
4. Hintergrund: **transparent**
5. Refresh-Intervall: **1 Min** (Failsafe; interne Timer ticken jede Sekunde)

---

## Limits

Anthropic veröffentlicht **keine exakten Zahlen**. Default-Werte sind Community-Schätzungen:

| Plan | 5h-Fenster | Woche |
|---|---|---|
| Free | 10 | 50 |
| Pro | 45 | 225 |
| Max 5x | 225 | 1.125 |
| Max 20x | 900 | 4.500 |

Bei Diskrepanz zur nativen Claude-App: **Custom-Limits** im Settings-Sheet aktivieren und kalibrieren.

---

## Bekannte Einschränkungen

- ❌ **Keine API-Anbindung** an Anthropic — Nachrichten werden manuell per Tap gezählt
- ❌ Daten **pro Gerät lokal** — kein Sync zwischen Handy und Desktop
- ❌ **Peak-Hours-Effekt** (Mo–Fr 14–20 Uhr DE) nicht abgebildet — Fenster läuft im echten Account schneller leer
- ✅ Reset-Timer **vollkommen autark** ohne externe Daten

---

## Tech-Stack

```
HTML5 + CSS3 + Vanilla JS (ES2017)
0 Build-Tools · 0 Frameworks · 0 Dependencies
2 Google Fonts (Fraunces, Inter) via CDN
```

Datei: `index.html` — alles inline, kein Bundle, kein Webpack.

---

## License

Privat — für persönlichen Gebrauch.
