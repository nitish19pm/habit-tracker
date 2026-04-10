# Habit Tracker — Project Context

## What this is
A habit tracking web app built with plain HTML, CSS, and JavaScript — no frameworks, no build tools. Single file: `index.html`.

## Live URLs
- **Production:** https://habit-tracker-eight-green.vercel.app
- **GitHub repo:** https://github.com/nitish19pm/habit-tracker
- **Hosting:** Vercel (auto-deploys on every `git push` to `main`)

## Backend
- **Supabase** for auth and database
- **Project URL:** https://azjegqxmybnvduqnhzfy.supabase.co
- **Auth:** Google OAuth (via Supabase + Google Cloud OAuth client)
- **Database:** Firestore table `habits` with Row Level Security — users only see their own habits

### Habits table schema
```sql
id uuid, user_id uuid, name text, color text, category text,
schedule text, custom_days integer[], reminder boolean,
reminder_time text, completions jsonb, freezes_left integer, created_at timestamp
```

## Features built so far
- **Add/edit/delete habits** via a modal (name, color, category, schedule, reminder time)
- **Daily check-off** with toggle, streak counter, 7-day dots
- **Flexible scheduling** — daily, weekdays, weekends, or custom days
- **Categories** — Health, Fitness, Learning, Mindfulness, Work, Social, Finance, Other
- **Color coding** — 8 color swatches with accent bar on each card
- **Streak freeze** — 2 freeze days per habit, protects streak without completing
- **Notes** — add a note to any habit on any day
- **Milestone celebrations** — animated popup at 7, 14, 21, 30, 50, 100 day streaks
- **Quick templates** — 10 pre-built habits (Drink water, Exercise, Meditate, etc.)
- **Stats view** — 1-week activity heatmap (default), completion rates, habit strength score (0-100)
- **CSV export** of full history
- **Light/dark mode toggle** (persisted)
- **Date navigation** — arrow buttons + tap-to-open calendar picker to view/edit any past 30 days
- **Past-date banner** — amber bar with "Back to Today" button when viewing past dates
- **Google sign-in** via Supabase OAuth
- **Guest mode** — full app without login, localStorage only, with banner prompting sign-in
- **Guest → signed-in migration** — guest habits auto-uploaded on first sign-in if no cloud habits exist
- **Browser notifications** — per-habit reminder time, requests permission on load
- **Midnight auto-refresh** — re-evaluates streaks when the day rolls over

## How to deploy changes
```bash
cd "c:\Users\shrad\.claude\projects\habit builder"
git add index.html
git commit -m "describe what changed"
git push
```
Vercel picks it up automatically. No build step needed.

## Key design decisions
- **Single file** — everything in `index.html`. No npm, no bundler, no node_modules.
- **`<script type="module">`** — needed for top-level `await` (Supabase session check)
- **Supabase JS loaded via CDN** — `<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2">`
- **LocalStorage** as fallback for guests and offline state (`habittracker_v2` key)
- **completions stored as JSONB** — each key is a date string `YYYY-MM-DD`, value is `true`, `false`, or `{ done, freeze, note }`
- **Row Level Security** — Supabase policy ensures `auth.uid() = user_id` on all queries
- **viewDate** — a global `Date` object tracking which date is currently being viewed (may differ from today)
- **Do NOT redeclare functions** inside `<script type="module">` — caused a silent crash. Instead call helpers at the end of the original function (e.g. `attachRipples()` at end of `render()`)

## What the user wants next (ideas discussed)
- More stats/insights (missed day patterns, best time of day)
- Sharing habits or progress socially
- PWA / installable on phone home screen

## User profile
- Beginner developer — explains things step by step
- Building this as a learning project, deploying publicly
- GitHub username: nitish19pm
- Comfortable with browser-based tools, learning Git/deployment workflow
