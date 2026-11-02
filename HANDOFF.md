# Faizan Ahmed Portfolio — Handoff

Last updated: 2026-11-02. Read this first when continuing work on the portfolio (any account, any session).

## 1. At a glance

| | |
|---|---|
| Owner | Faizan Ahmed — game developer (Unity & Unreal), indie studio **Rift Scar Games** |
| Repo | `https://github.com/faizanahmed089/Faizan-Ahmed-Portfolio` — branch **`main`** only |
| Local folder | `D:\Portfolio\Faizan-Ahmed-Portfolio` (Windows) |
| Live site | `https://faizanahmed089.github.io/Faizan-Ahmed-Portfolio/` — GitHub Pages, served straight from `main` (no build step). A push to `main` goes live within a minute or two. |
| Stack | One static file, `index.html` (HTML + inline `<style>` + inline `<script>`). No framework, no npm, no build. Fonts from Google Fonts. |
| Git identity | `FaizanAhmed <faizanahmed21@hotmail.com>`; `core.autocrlf=true` (the LF→CRLF warning on commit is normal) |

## 2. Files

| Path | What it is |
|---|---|
| `index.html` | The whole site (~1850 lines). Section map in §4. |
| `assets/<game>.mp4` | Hero gameplay videos, one per game: `aerial-war`, `vanguard`, `brain-trio`, `tower-tumble`. The user records and drops these in himself (full size, 1080p, with audio). |
| `assets/<game>.jpg` | Poster (still shown while that video loads). 1280 px wide, made with ffmpeg (§7). |
| `assets/Faizan-Ahmed-CV.pdf` | The CV that the site's "Download CV" and Resume buttons open. Copy of the PDF in Downloads (§6). |
| `.claude/launch.json` | Local preview server config for Claude Code (`python -m http.server 5173`). |
| `HANDOFF.md` | This file. |

## 3. Design (current theme)

Dark "neon game HUD" theme, built in October 2026.

- **Colours** (CSS variables at the top of `<style>`): background `--bg #06070d`, `--violet #8b5cf6`, `--cyan #22d3ee`, `--pink #f0509b`, `--amber #fbbf24`, `--lime #4ade80` (the "Open to Work" pill), `--blue`.
- **Fonts:** Chakra Petch (headings, buttons — `--font-display`), Inter (body), JetBrains Mono (labels, tags, chips).
- **Background:** fixed aurora glows + dot grid (`body::before/::after`); the hero has a scrolling perspective grid floor and a floating glow orb (`.viewport::before/::after`).
- **Cards:** glass panels, numbered `01, 02…` by a CSS counter, gradient top line, cursor-following spotlight (`--mx/--my` set by JS), lift + glow on hover.
- **Zones:** each project group is a `.project-zone` with an accent pair (`--accent/--accent-2`): `.zone-indie` = amber→pink, `.zone-game` = violet→cyan, `.zone-ai` = blue→cyan.
- **Motion:** scroll-reveal fade-in (`.js .reveal`, added by JS only, so content is visible without JS). Everything respects `prefers-reduced-motion`.
- **Nav bar:** 3-column grid so "Projects / Contact" sit exactly centred; "Open to Work" pill on the right. On phones (≤520 px) links go left, pill right. The left slot (name/role) is commented out on purpose — the user did that; leave it.

## 4. Page structure (`index.html`)

CSS sections are marked `/* ---------- Name ---------- */`: Toolbar, Layout, Hero, Hero video, Projects, Contact, Scroll reveal, Responsive.

1. **`<nav class="toolbar">`** — Projects (`#game-projects`), Contact (`#contact`), Open to Work pill.
2. **`<section id="hero">`** — two columns on desktop (text left, video player right); stacked on ≤980 px in this order: name/role → player → description/skills → buttons (done with `display:contents` + `order`).
   - Text: kicker "Game Developer Portfolio", name **FAIZAN AHMED** (big gradient), role "Senior Software Engineer // Unity & Unreal", 3 description paragraphs, skill chips.
   - Buttons (`.hero-cta`): **🎮 View Projects** → `#game-projects`, **📄 Download CV** → `assets/Faizan-Ahmed-CV.pdf` (new tab), **Get in Touch** → `#contact`. Made slightly smaller (`.hero-cta .btn`) so all three fit on one row at 1280 px.
   - Player: see §5.
3. **`<section id="projects">`** — heading "Portfolio Showcase", then:
   - **AI Engineering zone** — fully **commented out** (`<!-- AI ENGINEERING ZONE ... -->`). The user hid it on purpose; keep it hidden unless asked.
   - **Indie Originals** (`id="game-projects"`, class `zone-indie`, label "01 // Originals") — the user's own games (§6).
   - **Production Work** (`id="other-projects"`, class `zone-game`, label "02 // Production") — description: "Games, gameplay systems and tools I built in production for studios and clients, using Unity and Unreal Engine — made to ship, perform and scale." Cards (each with a "▶ Video" Google Drive link unless noted): Island Environment — Unity Showcase; ATA — ACPlus Therapy App (📱 App Store link); Monopoly Customization; Seamless Firebase Auth; NPC Interaction Framework; 2D Character Rigging; Fluid Simulation Shader; FPS Survival Shooter (Unreal); Level Transition System (Unreal); Mesh Merging System (Unreal); Grid-Based System (Unreal).
4. **`<section id="contact">`** — Email card (Copy Email button shows "Copied ✓" for 1.8 s), Resume card (Download → opens `assets/Faizan-Ahmed-CV.pdf`).
5. **Footer** — "© 2026 Faizan Ahmed — Senior Software Engineer | Unity & Unreal".
6. **`<script>`** — copyEmail, hero player (§5), scroll reveal, card spotlight.

Removed earlier at the user's request (do not bring back): Shooting Game, Aerial War UI/UX Design, Bike Racing Game, Car Racing Game, AI Nexus for Unity. The old "Game Development" heading became the two zones above.

## 5. Hero video player

A per-game player in a tilted glowing frame (flattens on hover).

- **Game tabs** above the video ("01 AERIAL WAR", "02 VANGUARD", …), built by JS; active tab = violet→pink gradient. Clicking a tab switches video.
- **‹ › arrows** on the video (SVG chevrons, centred) step to previous/next game, wrapping around.
- **Auto-advance:** when a video ends, the next game plays; after the last it returns to the first. (Videos do not loop individually.)
- **Controls bar** at the bottom of the video: seek bar for the current video (click/drag, hover tooltip with time, ←/→ keys skip 5 s), round play/pause button, game name, `0:12 / 0:49`.
- Muted autoplay; pauses when the hero is scrolled out of view; no autoplay for reduced-motion users. Only the current video is downloaded.
- **The game list is one array in the script:**

```js
const heroGames = [
  { title: "Aerial War", src: "assets/aerial-war.mp4", poster: "assets/aerial-war.jpg" },
  { title: "Vanguard", src: "assets/vanguard.mp4", poster: "assets/vanguard.jpg" },
  { title: "Brain Trio", src: "assets/brain-trio.mp4", poster: "assets/brain-trio.jpg" },
  { title: "Tower Tumble", src: "assets/tower-tumble.mp4", poster: "assets/tower-tumble.jpg" }
];
```

  Order here = tab order. The `<video>` tag in the HTML also hard-codes the **first** game's `src` and `poster` (fallback without JS) — keep it in sync with the first entry.
- History: started as one reel with a chaptered seek bar; the user preferred one video per game with tabs + arrows. Layouts "video as background" and "full-screen cinematic" were tried and rejected; the framed side-by-side layout ("A") was chosen.

## 6. The games (Indie Originals)

Order everywhere (cards **and** hero tabs): **Aerial War, Vanguard, Brain Trio, Tower Tumble**. All tags pattern: `Unity 6`, genre, `Game Design`, `Mobile`, `Monetization`. Card buttons do **not** include a video link (the hero already shows the videos).

| Game | Card title | Buttons on the portfolio card | Unity project |
|---|---|---|---|
| Aerial War | Aerial War — Vertical Shoot 'Em Up | 📱 Play Store, 🌐 Play HTML5 | — |
| Vanguard | Vanguard — Mobile First-Person Shooter | 📱 Play Store | `D:\Unity Projects\GameFramework` (kit name "Ironline FPS Kit") |
| Brain Trio | Brain Trio — 3-in-1 Brain Puzzle Game | 📱 Play Store, 🌐 Play HTML5 | `D:\Unity Projects\My project` (kit name "Puzzle Kit") |
| Tower Tumble | Tower Tumble — Hyper-Casual Tower Climber | 📱 Play Store, 🌐 Play HTML5 | — |

Links:

| Game | Play Store | HTML5 (Playgama) |
|---|---|---|
| Aerial War | `https://play.google.com/store/apps/details?id=com.riftscargames.aerialwar` | `https://playgama.ai/play/xpvgootimue5ce` |
| Vanguard | `https://play.google.com/store/apps/details?id=com.riftscargames.vanguard` | — |
| Brain Trio | `https://play.google.com/store/apps/details?id=com.riftscargames.braintrio` | `https://playgama.ai/play/cdkmv7hhpmye42` |
| Tower Tumble | `https://play.google.com/store/apps/details?id=com.riftscargames.towertumble` | `https://playgama.ai/play/ao4sgqeeyvvegw` |

Retired links (were removed on purpose, don't re-add): Unity Play builds (`play.unity.com/...`) and YouTube video links for all four games.

Naming history: Tower Tumble was "Peak Tumble"; Brain Trio was briefly "Puzzle Rush" (the project and an old video said "Puzzle Kit"). Use the current names.

The "Play HTML5" label was chosen so employers immediately see he builds HTML5/web games (alternatives offered: "HTML5 Build", "HTML5 / WebGL", "Play on Web (HTML5)").

Card description pattern (keep new games consistent): paragraph 1 = genre, controls, content counts and modes; paragraph 2 = "Built a full meta game (…) on a data-driven architecture, with Ads, IAP and leaderboard systems behind clean, provider-ready interfaces." When a new game arrives, read its Unity project's README / HANDOFF for the facts and show the drafted text to the user to check.

## 7. How-tos

**Preview locally.** In Claude Code: `preview_start` with name `portfolio` (from `.claude/launch.json`), page at `http://localhost:5173`. Without Claude: `python -m http.server 5173` in the repo folder. Opening `index.html` as a file also works but videos are more reliable over http.

**Add a game.**
1. User drops `assets/<slug>.mp4`.
2. Make the poster: `ffmpeg -v error -y -ss 5 -i assets/<slug>.mp4 -frames:v 1 -vf "scale=1280:-2" -q:v 4 assets/<slug>.jpg` (pick a frame that shows the game's title or good gameplay; a contact sheet helps: `-vf "fps=1/8,scale=400:-2,tile=6x1" -frames:v 1 sheet.jpg`). ffmpeg is at `C:\ffmpeg\bin`.
3. Add a line to `heroGames` (§5) in the order the user wants.
4. Copy an existing Indie Originals card, change title, text, buttons, tags.
5. Update the CV too if the user wants (§8).

**Replace a video.** Overwrite `assets/<slug>.mp4` (same name); regenerate the poster if the opening changed.

**Video size.** The user has chosen to keep his own full-size videos (he replaced my compressed 720p versions). Current sizes 16–47 MB. GitHub warns on files over 50 MB and rejects over 100 MB. If a new file is >50 MB, tell him and offer compression: `ffmpeg -i in.mp4 -an -vf "scale=1280:-2" -c:v libx264 -preset slow -crf 27 -pix_fmt yuv420p -movflags +faststart out.mp4` (drop `-an` to keep audio). Don't overwrite his file without asking.

## 8. The CV

- **Source:** `C:\Users\ZBooK\Downloads\Faizan Ahmed - CV (Modern).html` — a one-page Letter-size HTML CV (dark sidebar, orange/amber accents, Sora + Inter fonts). It has editing comments inside.
- **PDF:** `C:\Users\ZBooK\Downloads\Faizan Ahmed - CV.pdf` (the user renamed it from "…(Modern).pdf"). The repo copy `assets/Faizan-Ahmed-CV.pdf` is byte-identical as of 2026-11-02.
- **Games block** ("Indie Games — Rift Scar Games, Oct 2025 – Present"): same four games and order. Buttons: `▶ Play Store` (class `play`, filled orange) and `● Play HTML5` (class `vid`, outlined). Aerial War, Brain Trio, Tower Tumble have both; Vanguard has Play Store only.
- **Regenerate the PDF** (headless Chrome, exact print settings are in the file's `@page`):

```bash
"C:/Program Files/Google/Chrome/Application/chrome.exe" --headless=new --disable-gpu --no-pdf-header-footer --virtual-time-budget=8000 --run-all-compositor-stages-before-draw --print-to-pdf="C:\Users\ZBooK\Downloads\Faizan Ahmed - CV.pdf" "file:///C:/Users/ZBooK/Downloads/Faizan%20Ahmed%20-%20CV%20(Modern).html"
```

  Then check it is 1 page, and copy it over `assets/Faizan-Ahmed-CV.pdf` so the website serves the new one.
- **Back it up before editing.** On 2026-10-26 the user accidentally deleted the CV HTML and PDF; it was rebuilt from a backup copy. Copy the HTML somewhere safe before changing it.

## 9. Open items / known issues

- **Vanguard has no HTML5 build** yet; add a Play HTML5 button when he gives a link.
- Hero buttons wrap to two rows at ~1024 px wide (fine at 1280+ and on phones).
- `vanguard.mp4` is 46.8 MB — close to GitHub's 50 MB warning.
- The `<title>` still says "… | AI Solutions Architect" although the AI zone is hidden — only change if asked.
- Claude's built-in browser pane: screenshots are often cropped/zoomed and phone-size screenshots can come out blank; when the pane is hidden the browser pauses muted videos by itself. Verify with JS measurements (`getBoundingClientRect`, `video.paused`, `currentSrc`) rather than trusting screenshots, and don't mistake that pause for a bug.

## 10. Working with this user

- Communicates briefly, sometimes with typos; often sends a screenshot of what's wrong. When asked for options, give a short list **with one recommendation**.
- **Commit and push only when he says so.** He sometimes wants to look first ("don't push first, I want to see") — then leave changes uncommitted and point him at the local preview.
- He also commits by himself between sessions (e.g. new videos), so always run `git status` / `git log` first and build on what's there.
- Commit style used so far: short imperative title + bullet list. Commits made by Claude end with the `Co-Authored-By` trailer the session's instructions specify.
- Don't delete content he didn't ask to remove; flag inconsistencies instead (e.g. names that don't match the video, missing links).
- Card/marketing text written by Claude must be shown to him for checking; facts come from the game's own README/HANDOFF in its Unity project.
