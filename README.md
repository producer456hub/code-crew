# Code Quiz: Stat! 🦴

A gamified, single-file study game for **53 skeletal-system terms** from **Bio 40A, Chapters 6 & 7** — bone tissue and the axial skeleton.

**▶ Play it:** https://producer456hub.github.io/code-crew/

## How to play
Pick a tile from the 6-category board (a fresh **random board each game** is drawn from a **106-question bank — two questions per term**, one on *structure* and one on *action/function*) and answer before the timer runs out (the timer is optional — toggle **TIMED MODE** off any time). Each question is multiple-choice with four options.

- 🔥 **Combos** — consecutive correct answers raise your multiplier (up to ×3).
- ⏱️ **Speed bonus** — faster answers score more.
- ⚡ **Bonus tiles** — two random tiles each board pay **double points**.
- ❤️ **Patient HP** — right answers heal the patient, wrong ones hurt them (scaled by tile value). Two pixel medic-bots react, banter, drop bone facts, and deliver a **diagnosis** at the end.
- 🧠 **Adaptive** — terms you miss (or haven't seen) resurface more often until you nail them.
- 🏅 **Ranks & 🏆 leaderboard** — finish to earn a rank, review missed terms, and add your name to the high-score board.

## Categories (53 terms)
| Category | What it covers |
|---|---|
| **Bone Formation** | ossification (endochondral/intramembranous), ossification centers, modeling, remodeling |
| **Cells, Matrix & Marrow** | osteogenic cells, osteoid, perichondrium, nutrient foramen, projection, red/yellow marrow, hematopoiesis, osteoporosis |
| **Growth Plate & Embryology** | the four epiphyseal-plate zones, notochord, somite, sclerotome, mesenchyme |
| **Calcium & Hormones** | hyper-/hypocalcemia, calcium homeostasis, calcitriol, growth hormone, thyroxine (T4) |
| **Skull** | cranial bones (frontal, occipital, parietal, ethmoid, sphenoid, temporal) + facial bones (maxilla, mandible, zygomatic, palatine, nasal, lacrimal, inferior nasal conchae, vomer) |
| **Spine & Thorax** | axial skeleton, cervical/thoracic/lumbar vertebrae, sacrum, coccyx, ribs, sternum |

## High-score leaderboard
By default scores save to **this device** (localStorage). To make the board **global** (everyone shares one leaderboard), wire it to a free Supabase project. Setup (~3–5 min, no credit card):

1. Create a free project at https://supabase.com.
2. In the project's **SQL editor**, run:
   ```sql
   create table scores (
     id bigint generated always as identity primary key,
     name text not null,
     score int not null,
     correct int not null,
     created_at timestamptz default now()
   );
   alter table scores enable row level security;
   -- allow anyone to read the board:
   create policy "public read"  on scores for select using (true);
   -- allow anyone to add a score:
   create policy "public insert" on scores for insert with check (
     char_length(name) <= 16 and score >= 0 and score < 100000 and correct between 0 and 30
   );
   ```
3. In **Project Settings → API**, copy the **Project URL** and the **anon public** key.
4. Paste both into the `LB = { ... }` block at the top of the `<script>` in `index.html`, commit, and push.

The `anon` key is meant to be public (client-side); Row Level Security above limits writes to sane values.

## Tech
Pure HTML/CSS/JS in one file (`index.html`) — no build, no dependencies. Sound is synthesized via the Web Audio API; effects use a `<canvas>` particle system. Leaderboard uses Supabase's REST API (with a localStorage fallback). Touch-friendly + responsive.
