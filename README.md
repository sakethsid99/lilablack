# Nexus Studio — LILA Games Player Journey Visualization Tool
# Working web based tool link: https://lilablack.vercel.app/
An interactive tool for level designers to explore player behaviour across matches: where players go, how they die, what they loot, and how fights unfold over time.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18 + Vite 6 |
| Map rendering | HTML5 Canvas 2D API |
| Data preprocessing | Python 3, pandas, pyarrow |
| Source data | Apache Parquet (player telemetry) |
| Served data | Static JSON files (`public/data/`) |
| Styling | Inline styles + CSS variables (no CSS framework) |

---

## Prerequisites

- Node.js 18+
- Python 3.9+ with `pandas` and `pyarrow` installed

```bash
pip install pandas pyarrow
```

---

## Setup

```bash
# 1. Clone the repo
git clone <repo-url>
cd lilablack

# 2. Install frontend dependencies
npm install

# 3. Preprocess parquet data into JSON (only needed once, or after new data arrives)
python3 preprocess.py

# 4. Start the dev server
npm run dev
```

Open [http://localhost:5173](http://localhost:5173).

---

## Project Structure

```
lilablack/
├── preprocess.py              # Parquet → JSON conversion script
├── player_data/               # Raw parquet source files
│   └── README.md              # Coordinate mapping reference
├── public/
│   ├── data/
│   │   ├── index.json         # Match index (day → map → match stats)
│   │   └── {Day}/{Map}.json   # Full event data per day+map
│   └── maps/                  # Minimap images (1024×1024 px)
└── src/
    ├── App.jsx                # Root state + data fetching
    ├── components/
    │   ├── NavBar.jsx          # Day / Map / Match selectors + search bar
    │   ├── MapCanvas.jsx       # Canvas renderer + heatmap + pan/zoom
    │   ├── BottomPanel.jsx     # Timeline + filters layout
    │   ├── Timeline.jsx        # Playback controls + scrubber
    │   ├── EventFilters.jsx    # Per-event-type toggle checkboxes
    │   ├── MatchSummary.jsx    # Players / bots / kills / deaths / loot counts
    │   └── SearchBar.jsx       # Natural-language match search
    └── utils/
        └── searchQuery.js      # Keyword → filter function parser
```

---

## Build for Production

```bash
npm run build
# Output in dist/
```

---

## Environment Variables

None required. All data is served as static files from `public/data/`.

---

## Key Features

- **Interactive minimap** — pan (drag) and zoom (scroll wheel or +/− buttons) over per-map minimaps
- **Timeline playback** — scrub or play through a match at 1×/2×/5×/10× speed with MM:SS timestamps
- **Event markers** — each event type has a distinct shape and colour (circle, triangle, diamond, star, etc.)
- **Player path trails** — dashed lines connect each player's position events in chronological order
- **Heatmap overlays** — Kill Zones (🔥), Death Zones (💀), and Traffic (〰) density maps
- **Event filters** — toggle individual event types on/off (Movement / Combat / Environment / Item)
- **Match summary** — live count of players, bots, kills, deaths, and loot events for the current match
- **Natural-language search** — find matches by event type, map, date, or bot count without any AI (e.g. *"show me matches with KilledByStorm on Lockdown"*)
