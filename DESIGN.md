# DESIGN.md — Case Triage Board

## Overview

Single-file HTML application for hospital social workers to triage patient case loads. One patient per card. Cards live in configurable columns on a Kanban-style board. A global sidebar aggregates all checklist items across patients, sortable by priority, for moment-to-moment task triage. Runs on GitHub Pages with zero server — all data stays in the browser (IndexedDB + JSON export).

## Data Model

```
Board
  ├── id, name
  ├── columns: []              (default 4: Untouched / Active / Blocked / Watching)
  │     └── Column
  │           ├── id, name
  │           └── cards: []
  │
  └── cards: []
        └── Card
              ├── id
              ├── room: string            (editable, e.g. "304")
              ├── name: string            (editable, e.g. "Margaret Chen")
              ├── stars: 0–N             (card-level importance, clickable bar)
              ├── accent: string|null     (burgundy|navy|forest|ochre|null)
              ├── starsOn: boolean        (per-card toggle)
              ├── checklistOn: boolean    (per-card toggle)
              ├── notes: []
              │     └── Note
              │           ├── id
              │           ├── text: string
              │           ├── checked: boolean
              │           ├── priority: 0–3  (dot indicator, 0 = none)
              │           └── createdAt: ISO datetime
              └── columnId
```

**Persistence:** IndexedDB per board. JSON export/import for backup and workstation transfer.
**Uniqueness:** Cards and notes have UUIDs. Boards have user-given names.

## Features

### 0.1.x — Core Board (current target)

**Board-level**
- [x] Prototype card interaction (v4 — done)
- [ ] Four configurable columns: add, remove, rename
- [ ] Minimal tab bar to switch between named boards
- [ ] Drag-and-drop cards between columns
- [ ] Add/delete cards
- [ ] IndexedDB auto-save on every change (debounced 300ms)
- [ ] JSON export/import
- [ ] Top-level hamburger menu (☰ in header)

**Card-level**
- [ ] Editable room number + patient name (room first)
- [ ] Star bar: click to add ★, click a filled star to remove. Card-level importance. Toggle on/off per card via ⋮ menu.
- [ ] Checklist bar: click to add ✓, click to remove. Dual-source: manual clicks + completed note blocks. Toggle on/off per card. When off, note checkboxes hidden, strikethrough removed.
- [ ] Note blocks: each is a contenteditable paragraph with checkbox, priority dots, and creation date. Checked notes contribute to check tally.
- [ ] Accent color picker per card (burgundy, navy, forest, ochre, none)

**Global sidebar (via ☰ top-level menu)**
- [ ] Slide-out drawer showing all unchecked notes across all cards
- [ ] Each item shows: text, patient name + room, priority dots (0–3), creation date
- [ ] Click item's checkbox → syncs back to card
- [ ] Sort by priority (dots) or by most recent
- [ ] Also contains: theme/palette picker, board switcher

### 0.2.x — Themes & Polish
- [ ] Multiple color palettes (dark editorial, light warm, cool clinical, pastel)
- [ ] Light/dark mode toggle
- [ ] Font options (serif, sans-serif)
- [ ] Star/dot visual preference

### 0.3.x — Setup & Personalization
- [ ] First-run onboarding quiz: "How do you work?" → configures columns, palette, defaults
- [ ] Per-board defaults (column names, palette, whether stars/checklist start on)

## Visual Language

### Typography
- **Primary:** Georgia / Charter serif stack
- **Headers:** Italic weight for section labels
- **Body:** 14px serif, 1.65 line height
- **UI chrome:** System sans-serif for menus, buttons, badges

### Palette (current — "Dark Editorial")
- Background: #1b1b18 (warm dark newsprint)
- Card: #21211d
- Text: #d2cec4 (warm off-white)
- Dim text: #9b968a
- Borders/rules: #5e5b52
- Accents: burgundy #8b4a4a, navy #4a5f7a, forest #4a7050, ochre #a0843a
- Stars: #c49b2a (warm gold)
- Checks: #6a8d6d (sage green)

### Spacing
- Max content width: 560px (comfortable reading measure)
- Card padding: 16px
- Card gap: 16px
- Tight but comfortable — editorial, not cramped

### Interaction patterns
- Bars: click empty space to add, click filled icon to remove
- No + or — buttons — the bar itself is the control
- Burger menus: ⋮ per card, ☰ top-level for sidebar/settings
- Hover-reveal for delete buttons (notes, cards)
- Animations: subtle — fades, scales under 200ms

## Architecture

- **Single file:** `index.html` with embedded CSS and JS
- **No dependencies:** vanilla DOM APIs, IndexedDB, no frameworks
- **No build step:** edit the file, refresh the browser
- **No server:** GitHub Pages serves static HTML. All data is client-side.
- **Code conventions:** readable, hackable. Short functions, named clearly. `const` at top.

## Version Roadmap

| Version | What |
|---------|------|
| 0.1.0 | Full board: columns, cards, notes, stars, checks, IndexedDB, export, global sidebar |
| 0.1.x | Bug fixes and polish from use |
| 0.2.0 | Multiple color themes + light/dark mode |
| 0.3.0 | Setup quiz + per-board defaults |

---

*Last revised: 2026-06-02*
