<p align="center"><img src="icon.png" width="96" alt="ScreenSplit icon"></p>

# ScreenSplit

A speedrun timer with an autosplitter that **watches the screen**. There are no game hooks, memory reading or scripts. You show it what to look for (a "Level clear" banner, a loading screen, a line of text) and it splits, starts, resets and removes loads by itself, in any game.

<p align="center">
  <img src="images/timer.png" width="220" alt="The ScreenSplit timer mid-run: splits with deltas, game time, info tiles including the world record, and a green status pill showing the autosplitter sees the game">
  &nbsp;
  <img src="images/editor-test.png" width="520" alt="The autosplit profile editor: a live preview of the game with the regions it watches, and a test run log of the start, splits and load removal">
</p>

## Download

**[⬇ Latest release](https://github.com/Ozmao/screensplit-releases/releases/latest)**: download `ScreenSplit.exe`.

- A single file with nothing to install. It needs Windows 10 (version 2004 or newer) or Windows 11, 64-bit.
- The exe isn't code-signed yet, so Windows SmartScreen may warn the first time. Click **More info → Run anyway**.
- Settings are kept in `%APPDATA%\ScreenSplit`, and your games and splits in `Documents\ScreenSplit`.

## What it does

- **Screen-based autosplitting:** match an image, read text (OCR) or detect a loading screen. Splits are timed from the frame where the change appeared.
- **Easy setup:** freeze the last 10 seconds and scrub back to grab the exact frame, get a suggested threshold, and use a test run that logs every split and why.
- **A full timer:** PB, golds, sum of best, attempt history, Real Time and Game Time (or both at once).
- **Shareable autosplitters:** one `.screensplit` file holds a game's whole autosplitter. Double-click it and it's ready to run.
- **Compare against** PB, best segments, average segments or your latest run, with info tiles under the timer (best possible time, current pace, possible time save and more).
- **Crash recovery:** if the PC or the timer crashes mid-run, continue the run on the next start.
- **Analysis:** averages, consistency and resets for every segment, charts of your runs, and a summary after each run.
- **OBS overlay you lay out yourself:** a browser source with a live preview window in ScreenSplit; drag the elements into place and right-click them for more.
- **speedrun.com:** compare against the world record and see the category's rules next to the game.
- **Works with LiveSplit:** import your `.lss` splits, or export back to `.lss`.
- **Hotkeys on keyboard or controller:** Xbox, PlayStation, Switch Pro and most others work while the game has focus.
- **Stream-ready:** the oz-ui themes, plus a chroma-key mode for OBS.

## A closer look

**Setting up an autosplit.** Drag a box over what to look for and capture it. The live score and a suggested threshold show whether it will work.

<p align="center"><img src="images/editor-image.png" width="760" alt="An image condition on a game's title logo: the region outlined on the live preview, its captured reference, threshold, live score 1.000 and the suggested threshold"></p>

**Reading text.** Text conditions use the OCR built into Windows, and show you what it reads.

<p align="center"><img src="images/editor-text.png" width="760" alt="A text condition on a LEVEL COMPLETE banner, with the cleaned-up image the OCR sees and its reading"></p>

**Your OBS overlay.** Lay it out in ScreenSplit and add it to OBS as a browser source.

<p align="center">
  <img src="images/overlay-editor.png" width="380" alt="The OBS overlay layout window with the elements placed on a 1920 by 1080 canvas">
  <img src="images/overlay.png" width="380" alt="The overlay on top of a game, as OBS shows it">
</p>

**Analysis.** Every segment's PB, gold, average and consistency, and charts of your runs.

<p align="center">
  <img src="images/analysis.png" width="380" alt="Analysis of each segment with a chart of every time one segment was run">
  <img src="images/analysis-runs.png" width="380" alt="Finished runs and the PB over time, and one run broken down segment by segment">
</p>

<sub>The game in the autosplit screenshots is a made-up demo, Block Quest. The run is a sample Celeste Any% file, with the world record live from speedrun.com.</sub>

## Getting started

Read **[HOW-TO-AUTOSPLIT.md](HOW-TO-AUTOSPLIT.md)** to set a game up from scratch, with a screenshot for each step. A copy is attached to every release too; read it here to see the pictures.

## Feedback

Found a bug or have an idea? [Open an issue](https://github.com/Ozmao/screensplit-releases/issues).

A Linux version is in progress.
