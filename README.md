# Forgotten Places · Забытые места · Унутулган жерлер · 被遗忘的地方

**An interactive map of Kyrgyzstan's heritage sites that shows which monuments are at risk and lets anyone act to save them.**

Built by team Muras for the hackathon cultural heritage preservation track, November 2026.

> ⚠️ **Prototype with demo data.** The 10 monuments on the map are real places, but their condition statuses, problems and confirmation counts are illustrative. They are not real assessments.

---

## The problem

Kyrgyzstan has about 2,000 immovable historical and cultural monuments, and 583 of them are on the State List of monuments of national significance (Ministry of Culture data, reported by 24.kg). Mazars, petroglyphs and caravanserais are crumbling, but there is no single place that shows the condition of each site. People who notice damage often don't know whom to tell.

## The solution

Forgotten Places is a live map where:

1. **Anyone can add a site** with a photo, a description and its condition.
2. **The community verifies it.** A moderator approves every new point, and five "I was here too" confirmations give it a **Verified** badge.
3. **A site at risk becomes an official appeal in one tap.** It goes to the Ministry of Culture or the local administration, with the site's name, coordinates, problem and confirmations filled in.

---

## Features

| Area | What it does |
|---|---|
| 🗺️ **Map** | Schematic map of Kyrgyzstan with colour-coded points: 🟢 good, 🟡 deteriorating, 🔴 at risk, 🔵 saved. Zoom with the buttons, mouse wheel or pinch, and drag to move. |
| 🔍 **Search and filters** | Search by name in any of the 4 languages. Filter by type, region and condition. |
| 📊 **Live stats** | Sites on the map, sites at risk, appeals sent, sites saved. |
| 🪪 **Site card** | History and description in Kyrgyz, Russian, English and Chinese, plus the reported problem, coordinates and a link to Google image search for that site. |
| ✅ **Community check** | "I was here too, it's accurate" button with a progress bar. A site is verified at 5 confirmations. |
| 💬 **Talk to the monument** | Chat in which the monument answers in the first person, in the visitor's language, using **only the facts on its card**. |
| 🚨 **Urgent protection** | Every red site appears here automatically. |
| 📝 **Appeal generator** | Ready official letter in Russian, Kyrgyz or English. You can copy it, download it as .doc, or open it in an email app. |
| 📍 **I'm here** | Tap the map to mark a place, then add a name, type, region, condition, description and photos. The point goes to moderation. |
| 🛡️ **Moderation** | Approve or reject new points, change a site's condition, or mark a site as **Saved**. |
| 🌐 **4 languages** | The whole interface switches between KG / RU / EN / 中文. |
| 💻📱 **Responsive** | Full-screen layout with a side menu on laptops, and a bottom tab bar on phones. |

---

## Running it

This is a **single self-contained HTML file** (`forgotten-places.html`). There is no build step and no dependencies to install.

### Locally
Download the file and double-click it to open it in any modern browser (Chrome, Edge, Firefox or Safari).

### As a website
Any static hosting works:

- **GitHub Pages:** rename the file to `index.html`, push it to a repository, then go to **Settings → Pages → Deploy from branch**.
- **Netlify:** drag and drop the file (or a folder containing it) at app.netlify.com/drop.
- **Vercel or Cloudflare Pages:** deploy it as a static site.

### Two versions

| | claude.ai artifact link | Standalone HTML file |
|---|---|---|
| AI chat with the monument | ✅ Real AI (Claude) | ⚙️ Built-in rule-based answers from card data |
| Download appeal as .doc | ✅ | ❌ (Copy and Email still work) |
| Everything else | ✅ | ✅ |

To get real AI in the standalone version, add a small backend endpoint that calls an LLM API. Send it the prompt built in `ask()` and return the text. **Never put an API key in the front-end code.**

---

## How it works

### Tech
- Plain **HTML, CSS and vanilla JavaScript** in one file, with no frameworks.
- The map is a hand-built **SVG** that uses a simple equirectangular projection, with no tile server needed. The border is **schematic and approximate**.
- Fonts are **EB Garamond** and **DM Sans** from Google Fonts. Without internet, it falls back to system fonts.
- State is saved in **`localStorage`** (key `fp-proto-v2`), so everything you do stays in your browser only.

### Data model
Each site is one object in `SEED.sites`:

```js
{
  id: "burana",
  type: "build",            // mazar | petro | carav | build | settle
  region: "Chuy",           // Batken | Chuy | Issyk-Kul | Jalal-Abad | Naryn | Osh | Talas
  status: "yellow",         // green | yellow | red | blue
  lat: 42.746, lon: 75.250,
  conf: 7,                  // confirmations (verified at >= 5)
  name:    { en, ru, ky, zh },
  desc:    { en, ru, ky, zh },
  problem: { en, ru, ky, zh } | null,
  age:     { en, ru, ky, zh },
  photos:  []               // data-URLs of uploaded images
}
```

New reports go into `SEED.pending` until a moderator approves them.

### AI chat guardrails
The prompt tells the model to:
- speak as the monument, in the first person;
- use **only** the facts on the card (name, type, location, description, age, condition, confirmations);
- say "I don't know that yet" instead of inventing dates, names or numbers;
- reply in the language of the visitor's question, in 3 short sentences or fewer.

### Code map (inside the `<script>` tag)
| Section | Purpose |
|---|---|
| `I`, `t()` | Translation dictionary and lookup for all UI text |
| `TYPES`, `STATUS`, `REGIONS` | Reference data with labels in 4 languages |
| `SEED` | Demo sites and one pending report |
| `mapSVG()`, `attachZoom()` | Map rendering, zoom and pan |
| `vMap`, `vUrgent`, `vAdd`, `vMod` | The four tabs |
| `openSite()`, `ask()`, `offline()` | Site card and chat |
| `appealText()`, `openAppeal()` | Official appeal generator |

---

## Demo tips

- Before presenting, go to **Moderate → Reset demo data** to return everything to the starting state (Saimaluu-Tash has 3 of 5 confirmations).
- On the claude.ai version, ask the chat one question beforehand so the browser has already given AI permission.
- Suggested demo path: **Map** (search, filter, zoom) → **Saimaluu-Tash card** (languages, confirm, chat) → **Urgent** (create appeal) → **I'm here** (add a point) → **Moderate** (approve it).

---

## Roadmap

- 🛰️ Satellite "time machine" showing how a site changed over the years
- 🧊 3D models as digital copies of sites
- 📬 Tracker of replies from agencies
- 🚨 Alarm button for vandalism
- 🏫 "Adopt a monument" for schools and volunteers
- 🔳 QR-code quests for tourists
- 💰 Fundraising for restoration
- 🔐 Real backend: user accounts, shared database, moderator roles, photo storage
- 🗺️ Real map tiles, and hiding exact coordinates of fragile sites (such as petroglyphs) on the public map
- 📶 Offline mode for reports from the mountains
