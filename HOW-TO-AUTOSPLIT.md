# Making a game fully autosplit with ScreenSplit

ScreenSplit never reads game memory. It watches **parts of the screen** and reacts when something
appears or disappears there. Making a game "fully autosplit" means answering four questions with
things you can see on screen:

| Question | Example answer |
|---|---|
| When does the run **start**? | The title screen logo disappears |
| When does each segment **split**? | A "LEVEL COMPLETE" banner appears |
| When should the run **reset**? | The title screen logo appears again |
| When is the game **loading**? (optional, for load removal) | The screen is black, or "Loading" is shown |

Every answer becomes a **condition** (something to look for) and a **trigger** (what to do when it
appears or disappears). Set them up once and save them as a **profile**, a folder you can zip up and
share with other runners.

---

## 0. Before you start

- **Run the game in windowed or borderless-windowed mode.** Exclusive fullscreen usually can't be
  captured.
- **Leave capture on Graphics Capture** (the default, *Settings → Screen capture*). It gets every
  frame the game draws, works with modern DirectX/Vulkan games, keeps working when other windows
  cover the game, and stamps every split with the exact frame time. The two older methods are only
  fallbacks for systems where Graphics Capture isn't available.
- **Use the same resolution and aspect ratio for setup and runs.** Regions are stored as fractions
  of the window, so changing the size is fine, but changing the aspect ratio (16:9 → 4:3) breaks them.
- **Text conditions use the OCR built into Windows.** It needs an OCR-capable language installed
  (*Windows Settings → Time & language → Language*). English is usually there already.

Hotkeys (change them in *Settings*):

| Key | Action |
|---|---|
| Numpad 1 | Start / Split |
| Numpad 3 | Reset |
| Numpad 8 | Undo split (your rescue button if a split fires by mistake) |
| Numpad 2 | Skip split |
| Numpad 5 | Pause |

Each action can also get a **controller button**: in *Settings → Hotkeys*, click the box in the
*Controller* column and press the button. Xbox, PlayStation, Switch Pro and most other controllers
work, while the game has focus. The game still sees the button, so pick one it doesn't use (the
touchpad, Share / Back, a stick click…).

Everything else is in the **right-click menu** on the timer.

---

## 1. Make your splits

1. Right-click the timer → **＋ New game or category…**
2. **Game:** pick it from the dropdown, or choose **＋ Add new game…** and type its name.
   **Category:** e.g. `Any%`. Add one row per segment (**Add**, **Insert above**, **Move up/down**).
   Leave the time columns empty; they fill in as you run.
3. Click **Save**. That's it: ScreenSplit files everything into your **game library** for you:

```
Documents\ScreenSplit\
  Celeste\
    Any%.splits.json          your splits, PB, golds, history
    Any% autosplitter\        your autosplit profile (made in step 2)
    icon.png                  optional: an icon for the header until the game has run
```

The **game's icon** in the timer's header comes from the game itself: while its autosplitter is loaded,
ScreenSplit shows the icon of the game process it found, and switches if that process changes. When
the game is closed it keeps showing the last one it saw. Until the game has run once, the header shows
an `icon.png` from the game's folder (shared autosplitters bring one), or else the game's initials.

Switch between games and categories from the **GAMES** list at the top of the right-click menu.
ScreenSplit reopens the last one on the next launch. Renaming the game or category in *Edit splits*
moves the file for you. The library location can be changed in *Settings*.

**Coming from LiveSplit?** Right-click → **Import from LiveSplit (.lss)…** brings over your segments,
PB, best segments, attempt count and attempt history, straight into the library.

**Going the other way** (LiveSplit, or uploading to splits.io): right-click → **Export to LiveSplit
(.lss)…** writes the same things back out, including every attempt's segment times.

**Someone already made an autosplitter for your game?** Double-click their `.screensplit` file (or
drop it on the timer, or right-click → Autosplitter → **Import autosplitter…**). Its checks,
reference images and segment names go straight into your library and it's ready to run. If you
already have splits for that category, your times are kept and only the autosplitter is replaced.

## 2. Create the autosplit profile

1. Right-click → **Autosplitter → New profile…**. The profile folder is created automatically next to
   the splits; if one already exists for this game and category, it's opened instead.
2. The **profile editor** opens. The autosplitter is paused while the editor is open, so nothing
   fires by accident.

## 3. Point it at the game

1. Start the game.
2. In the editor, pick it from **Game window** (click **↻** if it isn't in the list).
   The live preview appears on the left.
3. **Process** and **Title contains** fill in automatically. These are how the timer finds the game
   later:
   - **Process** is the exe name without `.exe`. It's the most reliable, so keep it.
   - **Title contains**: if the title changes (version numbers, FPS counters), shorten it to the
     part that never changes, or clear it and rely on Process.

## 4. Plan before you click

Play through the game once and note what is on screen at each moment you need. Good signals:

- **Look identical every time**: fixed banners, icons, logos, menu headers.
- **Sit in the same place on screen.**
- **Show up for at least a few frames.**

Avoid:

- **Changing content**: text over a moving background, timers, or anything that animates. For
  images, hide those parts with transparency (see §5).
- **Shared looks**: a signal that also appears somewhere else in the game, for example the same
  banner for a mid-level checkpoint. Use a per-segment override (§6) or a different signal.

Write yourself a small table:

| # | Moment | What's on screen | Type |
|---|---|---|---|
| start | Leave title screen | Title logo disappears | Image |
| 1–7 | Chapter done | "CHAPTER COMPLETE" text | Text |
| 8 | Final boss | Credits text | Text |
| reset | Back at title | Title logo appears | Image |
| loads | Loading | Black screen | Image |

One condition can serve several triggers. In this example the title logo does both start
(disappears) and reset (appears).

## 5. Add the conditions

### Image condition (most reliable)

1. Click **+ Image**.
2. **Name** it something readable, like *Title logo*. The **Id** is the internal name; letters,
   digits, `-` and `_` only.
3. **Drag a rectangle** over the preview around the thing to detect. **Keep it tight.** Large regions
   that are mostly dark background score high even when the thing isn't there.
4. Get the game to show the thing, then click **📷 Capture reference**. **Live score** should now
   read about **1.000**.
   - **Thing only shows for a moment?** Play normally, and right after it appears press **F8**. This
     works while you're in the game and doesn't take focus. The preview freezes; alt-tab to the
     editor, drag the slider under the preview back to the exact frame (up to ~10 s), and capture
     from there. **F8** again (or *Back to live*) resumes.
5. **Set the threshold.** Let the thing appear and disappear once or twice. The purple box under
   the live score watches the scores and says e.g. *Not matching ≤ 0.70, matching ≥ 0.99 → suggested
   0.845*. Click **Use**. If it says there's no clear split, the region is too loose or something in
   it moves; tighten it or erase the moving parts (next step). The dot turns green when it matches.
6. Optional, but strong for anything that changes: the reference is saved as
   `<profile>\images\<id>.png`. Open it in Paint.NET, GIMP or Photoshop and **erase** the parts that
   change, such as numbers, animated sparkles or a moving background. Transparent pixels are
   ignored. Then click **Load PNG…** and pick the edited file.

### Text condition (OCR)

1. Click **+ Text** and drag a rectangle around the text. Leave a little margin around the
   letters, and keep other text and busy scenery out of the box.
2. In **Look for**, type the words, e.g. `chapter complete`.
   - **contains** (default) is best, because OCR often adds stray punctuation.
   - **equals** must match the whole box. **regex** is for patterns like `chapter\s*\d`.
   - Matching ignores case unless you tick **Case sensitive**.
   - **Allow OCR mistakes** (on by default) ignores spaces and punctuation, treats look-alikes
     such as O/0, l/1/I and S/5 as the same, and lets one wrong letter through in words of 4–7
     letters (two in 8–13). Turn it off if two different screens only differ by one letter.
3. **Auto clean-up** (on by default) enlarges the text, boosts the contrast, makes it dark on light,
   adds a margin and tries a black/white, a grey and a colour version on every read. **What the
   OCR sees** shows the black/white version, and the text under it is what was read.
4. The dot turns green when the recognised text matches.

If it still reads nothing, turn **Auto clean-up** off and tune by hand: **Invert colors** for light
text on dark, **Upscale ×3–4** for small text, **Black/white at** for busy backgrounds (move the
slider until the letters are clean).

OCR is slower than image matching. It reads about 4 times per second by default, in the background,
so it never holds up image conditions. Text splits are stamped with the frame the text was read
from, so they land within ~¼ s; image splits land on the exact frame. Use images for anything
timing-critical, and text where the wording matters.

### Useful extras

- **Invert (true when NOT matching)** flips a condition, e.g. "HUD is not visible".
- **Duplicate** copies a condition, handy for the same banner at another position.
- Hover the conditions list: every condition shows its live score or text, so you can watch them
  all while you play.

## 6. Wire up the triggers

Under **2. Triggers**:

| Field | Meaning |
|---|---|
| **Start the timer when…** | Condition + *appears* or *disappears* |
| **Split when…** | The default split check, used for every segment without its own |
| **Reset when…** | Optional; resets the run, keeping golds and the attempt |
| **appears / disappears** | Fire when the condition becomes true, or becomes false |
| **hold (ms)** | It must stay that way this long first. Filters out flickers and one-frame flashes |
| **delay (ms)** | Wait this long after it fires, then act. Use it to line up with your game's official timing rules |

Rules to know:

- **A trigger only fires on a change.** If the banner is already on screen when a split becomes
  active, it has to disappear and come back first. Two splits on one long banner can't happen by
  accident.
- **After any split, start or reset**, whether automatic or by hotkey, nothing fires for a short
  pause (1000 ms by default, under *5. Fail-safes*).
- Every trigger shows a sentence under it saying what it does, e.g. *Splits every segment when
  "time SE" appears.*
- **Per-segment split checks** (expand the section) list every segment with what ends it. Pick a
  condition to give a segment its own check. Example: chapters 1–7 split on "CHAPTER COMPLETE", the
  last one on the credits. Leave the rest on *↳ use the default split check*.

### Fail-safes (no double splits)

Under **5. Fail-safes** in the editor. The defaults are on for every profile:

| Setting | Default | What it stops |
|---|---|---|
| **Must be gone before it can fire again** | 1000 ms | A check that blinks off for a moment (an OCR misread, a flicker, a fade) and comes back counting as a second appearance |
| **Same check can't split again within** | 3000 ms | The check that just split firing again right away |
| **Shortest possible segment** | 0 s (off) | Any automatic split this early into a segment. Set it a bit below your fastest segment |
| **Pause after any start/split/reset** | 1000 ms | Anything firing right after an action |

Every split a fail-safe ignores shows up in the event read-out with the reason.
- **Start with an empty reset trigger** until everything else works. An over-eager reset is the
  most annoying mistake. Undo can't bring back a reset run.

## 7. Load removal (optional)

1. Make a condition for "loading": a **black screen** image (capture the full or near-full window
   while it's black, threshold ~0.97), or the game's **Loading** text.
2. Pick it under **3. Load removal**.
3. On the timer, right-click → **Compare against → Game Time** if your category uses load-removed
   time.

Game time pauses for exactly as long as the condition is true. The timer shows the other time in a
pill under the method label, plus a **LOADING** pill while it's paused.

## 8. Test it without doing a real run

Under **4. Test run** in the editor, click **Start test run** and play through the game. A practice
timer runs your triggers against the live game and logs every action with the reason, e.g.:

```
   1.265  ✓ SPLIT 1 · Forest  ← Level clear appeared
   0.003  ▶ START  ← Title screen disappeared
```

Your real splits aren't touched. Changes to triggers apply to the test immediately, so you can fix a
trigger and keep playing. The log also shows every change the autosplitter sees on the checks it
is watching (`·  "time SE" appeared · reads "Time Elapsed 12:03"`) and every split a fail-safe
ignored (`⛔ ignored split ← "time SE": same check split 1.2 s ago`).

**During real runs**, right-click the timer → **Autosplitter → Event log…** for the same read-out.
It can stay open next to the timer, and **Save…** writes it to a text file you can share when
something split wrong.

- **No start trigger?** (You start runs by hand.) The test timer starts as soon as you click *Start
  test run*.
- While the editor is open, your normal **split / undo / skip / reset hotkeys and controller buttons** drive the test run (the
  split key even starts one), and there are **Start / Split · Undo · Reset** buttons.
- The line above the log always says what it's waiting for and what that condition sees right now,
  e.g. *Waiting to split: "Level clear" to appear (now: not matching, 0.62)*. If it says *"it already
  looks that way, so it must go away once first"*, that's the rule that stops double splits.

## 9. Save and run

1. Click **Save** (or Ctrl+S) and close the editor.
2. The status pill at the bottom of the timer should turn **green** and show the game and fps.
   - **Red "Waiting for …"**: the game window isn't found. Check Process / Title contains.
   - **Yellow with a message**: a condition has a problem, e.g. a missing reference image or OCR
     not available.
3. If a split ever fires early during a real run, press **Undo** and keep going, then fix that
   condition afterwards (right-click → **Edit profile…**).

**What you compare against.** Right-click → **Compare against**: your **Personal best**, **Best
segments** (your golds back to back), **Average segments** (each segment's average over your last 20
clean runs of it), your **Latest run** or the **World record** (below). The split times, deltas and
timer colour follow it; a new PB is still a new PB. You can also give **Switch comparison** a key or controller button in *Settings →
Hotkeys*.

**What's under the timer.** *Settings → Layout* picks up to six info tiles (previous segment, PB, sum
of best, best possible time, current pace, possible time save, current segment, finished runs, world
record), how
many splits the list shows (a fixed number makes the window fit them exactly, like a LiveSplit
layout) and whether the final split always stays visible at the bottom. It can also show the category's
rules in a small panel under the timer.

**World record and rules from speedrun.com.** Right-click → **Link category…** (or the
**speedrun.com and rules** box in *Edit splits*): search the game, pick the category and its
subcategories (e.g. *All Dungeons → Solo, ShB*). ScreenSplit then:
- fetches the **world record** (time and runner) and keeps it up to date, also offline from its cache.
  Compare against **World record** to see your pace against it: speedrun.com doesn't publish split
  times, so the record's time is spread over your segments the way your golds are (the final split
  is the record exactly). Add the **World record** info tile to see it under the timer.
- fills in the category's **rules**, which you can edit. Right-click → **Rules…** opens them in a
  window you can keep next to the game, *Settings → Layout* can show them under the timer, and the
  OBS overlay has a Rules box that scrolls through them.

ScreenSplit only reads from speedrun.com; it never needs an account.

**After each run** a short summary appears under the timer: reset in which segment (or the final
time), how far ahead or behind you were, your golds, and the segments where you gained and lost
the most. Click it to open that run in the **Analysis** (turn it off in *Settings → Layout*).

**Analysis** (right-click → **Analysis…**) looks at your whole history: attempts, finished runs,
playtime, and for every segment its PB, gold, average, median, consistency (±), how much your PB
loses to the gold and how often runs are reset there. Pick a segment to see a chart of every time
you ran it; the **Runs** tab charts your finished runs with your PB over time and breaks down any
run segment by segment.

**If ScreenSplit or the PC crashes mid-run**, the run isn't lost: it's saved every few seconds and
after every split. On the next start you can continue it (still counting, including the time it
was closed, or paused at the moment it closed), end it there and keep its golds, or discard it.
Unsaved changes to your splits (a new PB, golds) are kept the same way.

**How precise is it?** Every start, split and load change is stamped with the moment the deciding
frame was captured, not when ScreenSplit finished processing it. With Graphics Capture, splits
measured within ±10 ms of the screen change, roughly one frame at 60 fps.

**Showing it on stream.** Right-click the timer:
- **OBS overlay (browser source).** ScreenSplit serves an overlay at `http://localhost:16900/`
  (only this PC can reach it). In OBS: **Sources → + → Browser**, paste that URL and set the size to
  your canvas (1920 × 1080 unless you change it). Right-click the timer → **OBS overlay → Edit layout**
  opens the layout editor in its own window, a live preview of the browser source: drag the title, splits, timer, info tiles, world record,
  comparison and rules wherever you want them, resize them by their corner, pick which info tiles
  show, and turn the panels off for text straight over the game. Right-click an element to hide it,
  change its text size, toggle its panel or bring it to the front; **Preview** shows only what OBS
  will show. Changes show up in OBS at once. (**Edit in web browser…** opens the same editor in your
  browser.)
  The port can be changed (or the overlay turned off) in **Settings → OBS overlay**.
- **Show both Real and Game time** adds a second big timer for the other timing method, so viewers
  see RTA and load-removed time together.
- **Chroma key (for OBS)** turns the background into one flat colour. In **Settings → Display** pick
  the key colour (green, blue, magenta or any hex) and a style: **Panels** keeps the oz panels
  floating on the key colour (cleanest edges), **Text only** leaves just outlined text. In OBS add a **Window
  Capture** of ScreenSplit, then **Filters → Chroma Key** with the same colour; *Similarity* around
  400 and *Smoothness* around 80 is a good start. Pick the key colour your game uses least.

## 10. Troubleshooting

| Symptom | Fix |
|---|---|
| Never matches | Region not tight enough or wrong spot; recapture the reference while the thing is on screen; use the suggested threshold |
| Matches when it shouldn't | Use the suggested threshold, tighten the region, or mask changing pixels with transparency |
| Splits twice | Open the **event log** to see why; raise **Must be gone before it can fire again** or set a **Shortest possible segment** |
| Text split a bit late | Raise *OCR reads per second* under Advanced, or use an image condition instead |
| Preview is black | Make sure capture is *Graphics Capture*; use borderless windowed instead of exclusive fullscreen |
| OCR sees no text | Make the box a bit bigger than the text; keep **Auto clean-up** on; check an OCR language is installed |
| OCR reads garbage | Keep other text out of the box; keep **Allow OCR mistakes** on; or turn Auto clean-up off and tune by hand |
| Worked, then stopped | Game resolution or aspect ratio changed, or the UI scale changed in the game's settings |
| Wrong window found | Make **Process** more specific; clear **Title contains** |

## 11. Share it

Right-click → Autosplitter → **Share autosplitter (one file)…** saves a `.screensplit` file with your
checks, the reference images they use, your segment names and the game's icon. None of your times
are included. Send it to other runners: they double-click it (or drop it on ScreenSplit) and it's
installed in their library, ready to run. If their game window has a different name, they pick it
once in **Edit profile**. The same profile works in the Linux version. Text conditions may need
small tweaks there because Linux uses a different OCR engine.
