# Code Quiz: Stat! 🚑

A gamified, single-file study game for **57 anatomy & physiology terms** useful to **EMTs, ECG techs, and phlebotomists**.

**▶ Play it:** https://producer456hub.github.io/code-crew/

## How to play
Pick a tile from the 6-category board (a fresh **random board each game** is drawn from a 57-term bank) and answer the call before the timer runs out (the timer is optional — toggle **TIMED MODE** off any time).

- 🔥 **Combos** — consecutive correct answers raise your multiplier (up to ×3).
- ⏱️ **Speed bonus** — faster answers score more.
- ⚡ **Bonus tiles** — two random tiles each board pay **double points**.
- ❤️ **Patient HP** — right answers heal the patient, wrong ones hurt them (scaled by tile value). Two pixel medic-bots react, banter, drop term facts & ECG tips, and deliver a **diagnosis** at shift's end.
- 🏅 **Ranks & 🏆 leaderboard** — finish to earn a rank, review missed terms, and add your name to the high-score board.

## High-score leaderboard (optional global setup)
Out of the box, scores save to **this device only** (localStorage). To make the board **global** (everyone sees everyone's scores), wire it to a free Supabase project:

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

The `anon` key is meant to be public (client-side); Row Level Security above limits writes to sane values. Note: a public board can still be spammed — fine for a personal study game.

## Terms covered
Marrow & blood, calcium & hormones, bone development & ossification, growth-plate zones & embryology, and the full axial skeleton (skull, face, spine, chest).

## Tech
Pure HTML/CSS/JS in one file (`index.html`) — no build, no dependencies. Sound is synthesized via the Web Audio API; effects use a `<canvas>` particle system. Leaderboard uses Supabase's REST API (with a localStorage fallback). Touch-friendly + responsive.
