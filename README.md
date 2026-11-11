# Corner Café ☕ — a classroom coffee-shop screen

An ad-free "jazz café" scene for the classroom projector. You're inside a modern coffee shop looking out the window at a real place. A card shows students what they should be working on right now, with a countdown.

## What it does
- **Out the window:** real photos of seven places (urban, modern city, castle, country, village, beach, mountains), or your own photo.
- **Time of day:** morning, noon, or night (or follow the clock). The café lighting changes to match.
- **Outside:** clear, sun, rain, snow, or fog, shown over the photo, with optional rain sounds.
- **Class timers:** add your own, with a name, minutes, steps, and card style. At zero, the music stops and a soft bell rings.
- **Music:** paste a Spotify playlist link, or use your own MP3s (these fade out gently).
- Settings save in your browser.

## Use it
Open the GitHub Pages link in Chrome on the classroom computer and click **Open the café**.

**Teacher controls:** click **Teacher controls** and drag that window to your own screen. The café stays full screen on the projector. Use *Extend* display (Windows: `Win + P`), not *Duplicate*, so students don't see the controls. Everything you change there updates the projector live: timers, view, weather, music, and the card.

Shortcuts: `Space` start/pause · `F` full screen · `S` settings · `→` next view · `N` next timer

## Turn on the website (one time)
Repo **Settings → Pages → Build and deployment → Source: GitHub Actions**. This takes effect immediately, with no Save button. Every push publishes the site automatically.
After a minute it's live at `https://tregodan.github.io/classroomcoffeeshop/`.

## Spotify notes
Press play inside the Spotify player. Full songs need Spotify Premium signed in to Chrome on a desktop computer; otherwise Spotify plays 30-second previews. Spotify pauses when time is up (it can't fade).

Photos: [Pexels](https://www.pexels.com) (free to use). The whole app is a single file, `index.html`.
