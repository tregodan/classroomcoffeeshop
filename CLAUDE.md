# CLAUDE.md — Corner Café

A classroom projector app: an illustrated modern coffee shop with a real photo out the window, weather and time-of-day effects, a "Right now" **board** showing the current **task** and timer, an editable **café sign**, an optional animated barista and DJ, and music from free café radio, SoundCloud, the Internet Archive, Spotify or local files. It's used by a teacher (the repo owner) on a classroom computer in Chrome.

- Live site: https://tregodan.github.io/classroomcoffeeshop/ (GitHub Pages, Source = GitHub Actions; every push to `main` deploys via `.github/workflows/pages.yml`).
- No build step, no dependencies. **Everything lives in `index.html`** (CSS, then HTML, then one IIFE `<script>`).

## Vocabulary (the owner uses these words — keep them in the UI)
- **Board**: the floating "Right now" panel (`#card` in code, for historical reasons). Shows task title, steps, countdown and progress bar. Styles: `pos-right`, `pos-left`, `pos-center`. "Hide board" = `S.card=false`.
- **Task**: an item in `S.timers` (code still says "timer"): `{id, name, min, steps[], style, hideTimer}`. A task shows no countdown if `hideTimer` is true or `min` is 0.
- **Café sign**: the SVG letter board on the wall (`#boardHead`, `#boardGoal`, `#boardDesc1/2`). Title = `S.signTitle || S.classLabel || 'Corner Café'`; goal = `S.signGoal || current task name`; context `S.signDesc` wraps to 2 lines of 28 characters.
- **View**: the place outside the window. **Time** = morning/noon/night/auto. **Outside** = clear/sun/rain/snow/fog.
- **Teacher controls**: the same page opened with `?control=1`.

## Architecture
- **Scene**: a fixed 1600×900 "world" (`#scene`), cover-scaled to the viewport by `fit()`. The window rect is x130–1010, y80–610. Layers, back to front: `#view` (fallback gradient, `#photo`, `#tint`, `#fog`, `#glare`, canvas `#wx` at 1.5×, `#nightGlass`, `#reflect`), then the SVG `#interior` (wall with a window hole via evenodd, frame, globes, shelf, café sign, counter, espresso machine, barista, DJ booth, plants), then `#shaft` and `#light` overlays.
- Colors per time of day come from `TOD_LOOK` and are set as CSS vars `--wall`, `--wall2`, `--tile`, `--brandInk` in `applyLook()`. Weather also adjusts `#photo` filters there.
- **UI layer** (`#ui`): brand + clock, the board (`#card`), music (`#spotWrap` Spotify embed or `#localPill`), bottom-right control bar `#bar2`, toast, the start gate `#gate` (one click to unlock audio and fullscreen). Bar and cursor auto-hide (`body.idle`).
- **Settings drawer** `#drawer`: the same markup is reused full-page in control mode.
- **State**: `S` is merged from defaults plus `localStorage['cornerCafe']`. Always call `save()` after changing `S`; it writes localStorage **and broadcasts** `{type:'state', S}`.
- **Sync** (BroadcastChannel `'cornerCafe'`, same browser only):
  - Both windows broadcast state on save; receivers run `applyRemoteState()` (the projector re-renders what changed; the controller refreshes the drawer UI and inputs, skipping focused fields).
  - Controller → projector: `{type:'cmd', cmd}` (`toggle|plus|reset|next|mplay|mtrack|spot`), `{type:'photo', file}`, `{type:'tracks', files}` (File objects, structured clone).
  - Projector → controller: `{type:'status', ...}` every 500 ms; the controller shows it in `#remote`.
  - Heavy features are skipped when `CONTROL` is true (weather canvas, rain audio, Spotify mount).
- **Photos**: `VIEWS[]` lists Pexels IDs and credits. URL is `https://images.pexels.com/photos/{id}/pexels-photo-{id}.jpeg?auto=compress&cs=tinysrgb&w=1920`. `loadPhoto()` tries each ID in turn, then falls back to a drawn gradient. Before adding a photo, verify it loads (PNG-only Pexels photos fail with this URL).
- **Music sources** (`S.src`): `radio` (default) · `soundcloud` · `archive` · `spotify` · `files` · `none`. `STATIONS[]` lists the ad-free radio streams (Radio Swiss Jazz/Classic, freeCodeCamp Code Radio, Radio Paradise; each with an `alt` fallback). SomaFM was tried and dropped: its servers answer 403 to streams embedded on other sites; `S.station` picks one, or `'custom'` with `S.radioUrl`. Radio and files share the one `audio` element, so both fade. SoundCloud uses the Widget API (`scWidget`, fade via `setVolume`). The Internet Archive embed has no API, so time-up just remounts it (`mountIA()`). SoundCloud and Archive share `#embedWrap`; `leaveSource()` stops everything when switching.
- **Audio**: WebAudio for the bell and generated rain; an `Audio()` element for local files and radio (5 s fade at time-up); the Spotify iFrame API controller (`spotCtl.pause()` at time-up; it can't fade, and full tracks need Premium plus a desktop browser).
- **Characters**: barista (`#barista`, `#baristaArm`, `#espresso` with a clipPath glass `#cupClip` and rising `#fill`) and DJ (`#dj`), toggled by `body.baristaOn` / `body.djOn`. CSS keyframe animations use `transform-box:view-box` with world-coordinate origins. All art is original flat SVG — don't copy known characters or brands.

## Conventions
- Keep it a single self-contained `index.html` that works from GitHub Pages and from `file://`. Only Google Fonts, Pexels images, and the music services (Radio Swiss, Code Radio and Radio Paradise streams, SoundCloud widget, archive.org embed, Spotify embed) are external. Music must stay ad-free: don't add YouTube or other ad-supported players.
- Plain words in the UI; the owner is a teacher, not a developer. Touch targets ≥ 44px.
- New settings: add a default in `S`, a control in the drawer, use `bindCheck()` for checkboxes, add it to `refreshInputs()` for sync, and handle it in `applyRemoteState()` if the projector must re-render.
- The control bar must stay on one line at 1440 px wide (it's right-aligned; the music player is on the left).
- Commit to `main` (that is what deploys). Use short, plain commit messages.

## Testing (headless Chrome via Playwright, available in the cloud workspace)
```python
from playwright.async_api import async_playwright
b = await p.chromium.launch(executable_path='/usr/bin/google-chrome', args=['--no-sandbox'])
ctx = await b.new_context()                     # same context = BroadcastChannel works between pages
proj = await ctx.new_page(); await proj.goto('file:///…/index.html'); await proj.click('#gateBtn')
ctl  = await ctx.new_page(); await ctl.goto('file:///…/index.html?control=1')
```
Seed state with `localStorage.setItem('cornerCafe', JSON.stringify({...}))` and reload. Read SVG text with `text_content`, not `inner_text`. Check `pageerror` events, take screenshots at 1440×900, 1024×768 (4:3 projector) and 1920×1080. The cloud sandbox usually can't reach Pexels or Spotify, so photos show the fallback gradient there; check the live site in a browser for those.

## History of owner requests (for context)
Ad-free YouTube café replacement → modern coffee-shop interior with morning/noon/night → real-photo window views with weather over them → Spotify embed → teacher controller window → barista (female, espresso fills a glass) and DJ → café share image and icon → editable café sign (title, goal, context) → "card" renamed **board**, "timers" renamed **tasks**, each task with an optional timer; main bar button hides the whole board → more free, ad-free, full-song music: café radio stations, SoundCloud, Internet Archive.
