# Corner Café ☕ — a classroom coffee-shop screen

**Live:** https://tregodan.github.io/classroomcoffeeshop/

An ad-free "jazz café" scene for the classroom projector, built to replace YouTube café videos. You're inside an illustrated modern coffee shop looking out the window at a real place. A **board** tells students the task they're working on right now, with an optional countdown.

## Features
- **Out the window:** real photos (from Pexels) of seven places: urban, modern city, castle, country, village, beach, mountains. You can also paste a photo link or use a photo from your computer.
- **Time of day:** morning, noon, night, or follow the clock. The café's walls, lamps, and the view all change.
- **Outside:** clear, sun, rain, snow, or fog over the photo, with optional generated rain sound.
- **The board:** the "Right now" panel. It shows the task, up to 5 steps, and the countdown. It can sit on the right, on the left, or big and centered.
- **Tasks:** your own list (Welcome in, Do now, Work time…). Each has steps, minutes, an optional timer (Show timer on/off), and a board style. At zero, the board pulses, a soft bell rings, and the music stops.
- **Café sign:** the letter board on the wall. It shows a title (or your class name, or "Corner Café"), **Today**, the goal of the day, and a line of context.
- **Barista and DJ:** optional animated staff. She pulls espresso into a glass, and he mixes on turntables.
- **Music:** a Spotify playlist embed, or your own MP3/M4A files (these fade out at time-up).
- **Teacher controls:** a second window that controls the projector live.
- Settings save in the browser (localStorage).

## Using it in class
1. Open the live link in **Chrome** on the classroom computer and click **Open the café**. That goes full screen and lets the sound play.
2. Click **Teacher controls** in the bottom bar and drag that window to your own screen.
3. Set the display to **Extend** (Windows: `Win + P`), not Duplicate, so students only see the café.
4. Use the controls to start or pause tasks, hide the board, change the view or weather, and edit the sign. The projector updates instantly.

**Main bar:** Start timer · +1 min · Reset · Hide board · View · Time · Outside · Teacher controls · Full screen · Settings. The bar and the mouse pointer hide when the mouse is still.

**Shortcuts:** `Space` start/pause · `N` next task · `→` next view · `F` full screen · `S` settings · `Esc` close settings

## Spotify notes
Paste a playlist, album, or song link under **Music**, then press play *inside* the Spotify player on the café screen once. Full songs need Spotify Premium signed in to Chrome on a desktop computer; otherwise Spotify plays 30-second previews. Spotify pauses at time-up (it can't fade).

## Publishing
GitHub Pages is set to **Source: GitHub Actions**. Every push to `main` publishes automatically through `.github/workflows/pages.yml`. The build warnings about Node 20 and Ubuntu versions are harmless.

## Files
| File | What it is |
|---|---|
| `index.html` | The whole app: HTML, CSS, and JS in one file |
| `icon.svg`, `apple-touch-icon.png` | Tab and home-screen icon (mini café) |
| `og-image.png` | 1200×630 link-preview image |
| `.github/workflows/pages.yml` | Publishes to GitHub Pages |
| `CLAUDE.md` | Notes for Claude when editing this project |

Photos: [Pexels](https://www.pexels.com) (free to use; photographers are credited in the window).
