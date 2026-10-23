# 🥷 Shinobi's Journal

> *"Every mission leaves a lesson. Every failure sharpens the shinobi."*

**Live App**: [https://jaikarthik-codes.github.io/shinobis-journal/](https://jaikarthik-codes.github.io/shinobis-journal/)

**Shinobi's Journal** is a minimalist, zero-friction, single-file web application designed to track daily learning, master new skills, and write structured Markdown logs directly into a private **Obsidian vault** using GitHub's REST API.

Hosted for free on **GitHub Pages**, it requires no backend servers, no build steps, and no frameworks. Everything runs locally in your browser.

---

## 🎯 Why I Made This

### 1. The Friction Problem with Habit Tracking
When grinding through coding tutorials, books, or technical projects, opening heavy desktop applications just to log a few lines of progress introduces friction. Friction breaks consistency. And when consistency breaks, skill acquisition stalls.

I wanted an interface that:
- Opens instantly on mobile or desktop via a browser bookmark.
- Feels like logging a training mission in a shinobi arcade terminal.
- Captures the three essential pillars of every day: **What I grinded**, **What I unlocked**, and **What blocked me**.

### 2. Full Ownership with Obsidian
Proprietary habit trackers and journaling apps lock your reflections behind subscription paywalls or proprietary databases. 

**Shinobi's Journal** writes clean, standard Markdown with native Obsidian bidirectional wikilinks (`[[Skill]]` and `[[YYYY-MM-DD]]`). Your reflections live in your own private GitHub repository alongside your Obsidian knowledge vault. You retain 100% data ownership.

### 3. The Ninja Mindset: Relentless Iteration
In martial arts and ninja tradition, progress isn't measured in unbroken perfection, but in daily discipline and resilience:
- **Endeavors**: Your daily training arc. What did you grind through?
- **Key Takeaways**: New techniques unlocked. What do you understand today that you didn't yesterday?
- **Bottlenecks**: The boss fight. What stayed unbeaten today that needs a rematch tomorrow?
- **Chakra Nature**: The skills you levelled up (SQL, Python, Docker, Architecture, etc.).

---

## ⚡ How It Works & How It Helps You

### 1. Forgiving Streak Engine
Most streak counters reset to zero after a single missed day, which often triggers demotivation. Shinobi's Journal implements a **forgiving backward walk**:
- **One rest day per week is permitted** and will never break your streak.
- Missed rest days never add to the count, but they do not punish you.
- It calculates:
  - **Current Streak**: Active training run.
  - **Longest Streak**: Your all-time record, recomputed from your entire vault history.
  - **Total Days Logged**: Wins and losses both count. This number only goes up.
  - **Weekly Activity**: Visual 7-day rolling window ending today (`S M T W T F S`) with filled (logged) and dashed (rest day) indicators.

### 2. Chakra Nature: Bidirectional Skill Linking
Whenever you work on a skill (e.g. *SQL*, *Docker*, *System Design*):
1. **Existing Skills**: Tagging an existing skill automatically adds the current date `- [[YYYY-MM-DD]]` under its `## Log` section in `Skills/<Skill>/<Skill>.md`.
2. **New Skills**: Entering a new skill automatically initializes its own folder and dedicated master note in your Obsidian vault.
3. **Graph Connection**: The daily note includes `**Skills:** [[SQL]] · [[Docker]]`, which immediately draws relational edges across your Obsidian Graph View.

### 3. Single-Commit Atomic Pushes
Using the **GitHub Git Data API** (Trees, Blobs, Commits, Refs), saving an episode executes in **exactly one atomic commit**:
- `Daily/YYYY-MM-DD.md` is created or updated.
- All new and touched `Skills/<Skill>/<Skill>.md` notes are updated simultaneously.
- If remote changes occurred in the vault (e.g., from an Obsidian sync), the app automatically rebases up to 3 times to ensure zero merge conflicts or lost data.

### 4. Zero Data Leakage & Local-First Security
- **No intermediary backend**: Requests go directly from `fetch()` in your browser to `api.github.com`.
- **Local Storage**: Your Personal Access Token (PAT) and repository details are saved solely in your browser's `localStorage` when you click **Save**.
- **Instant Disconnect**: One-click disconnect immediately wipes all tokens and cached state from the browser.

### 5. Local Timezone & DST Resilience
All date parsing and streak math use the browser's local calendar (e.g., UTC+5:30 in India) normalized at local noon. It avoids UTC timezone offset bugs common in simple `toISOString()` calls.

---

## 🎨 Visual Target & Design System

The visual design is inspired by high-contrast retro anime and editorial UI dashboards:

- **Typography**:
  - `Archivo` (Variable Width 125, Weight 850–900): Punchy section headers, big stat numbers, and primary push button.
  - `Inter`: Clean, highly legible body typography and inputs.
- **Palette**:
  - **Blue** (`#0047AB`): Header banner, primary actions, Endeavors card, selected skill chips.
  - **Orange** (`#E8612C`): Header accent border, Today button, Key Takeaways card, Add/Save buttons.
  - **Navy** (`#0B1B3A`): Body text, Total Days accent, filled weekly activity dots.
  - **Crimson** (`#A3182A`): Bottlenecks card, Disconnect action.
  - **Cream** (`#F4E8CF`): Card surfaces.
  - **Parchment** (`#FFFDF2`): Textareas and input backgrounds.
  - **Page Background** (`#F7F2EE`): Warm canvas.

---

## 📁 Vault File Structure

Your vault will maintain this clean organization:

```
your-obsidian-vault/
├── Daily/
│   ├── 2026-10-21.md
│   ├── 2026-10-22.md
│   └── 2026-10-23.md
└── Skills/
    ├── SQL/
    │   └── SQL.md
    ├── Python/
    │   └── Python.md
    └── Docker/
        └── Docker.md
```

### Daily Note Example (`Daily/2026-10-23.md`):
```markdown
---
date: 2026-10-23
skills:
  - SQL
  - Data Visualization
---
# 2026-10-23

**Skills:** [[SQL]] · [[Data Visualization]]

## Endeavors
Finished the joins chapter and rebuilt the exercises from memory.

## Key Takeaways
- Window functions evaluate after WHERE and HAVING clauses.
- Lateral joins allow row-by-row subquery correlation.

## Bottlenecks
Partition indexing on large partitioned datasets. Needs review tomorrow.
```

### Skill Master Note Example (`Skills/SQL/SQL.md`):
```markdown
---
skill: SQL
---
# SQL

## Log
- [[2026-10-20]]
- [[2026-10-23]]
```

---

## 🚀 Quick Setup & Usage

### Step 1: Create a GitHub Personal Access Token (PAT)
1. Go to **GitHub Settings** → **Developer Settings** → **Personal Access Tokens** → **Fine-grained tokens** (or Classic tokens).
2. Set token scope:
   - **Repository access**: Select your private Obsidian vault repo (e.g., `learning-vault`).
   - **Repository permissions**:
     - `Contents`: **Read and Write**.
3. Generate and copy the token (`github_pat_...`).

### Step 2: Open Shinobi's Journal
1. Host `index.html` on **GitHub Pages** (or open `index.html` directly in your browser).
2. Click the **GitHub: Not connected** pill in the top-right corner.
3. Fill in:
   - **GitHub username**: Your username (e.g. `Jaikarthik-codes`)
   - **Vault repository**: Your private vault repository name (e.g. `learning-vault`)
   - **Branch**: `main`
   - **Access token**: Your generated PAT
4. Click **Test connection** (verifies read/write permissions), then click **Save**.

### Step 3: Train & Push Daily
1. Fill in your daily **Endeavors**, **Key Takeaways**, and **Bottlenecks**.
2. Select or add the **Chakra Nature** skills you grinded.
3. Click **Preview the episode** to review the rendered Markdown.
4. Hit **END EPISODE: PUSH TO GITHUB**. Your Obsidian vault is immediately updated!

---

## 🛠️ Tech Stack

- **HTML5 & Vanilla JavaScript** (Zero frameworks, zero dependencies, zero build step)
- **CSS3 Variables & Flexbox/Grid** (Responsive down to mobile)
- **Google Fonts** (`Archivo` & `Inter`)
- **GitHub REST & Git Data APIs**

---

## 📜 License

MIT License. Crafted for lifelong learners and disciplined builders.
