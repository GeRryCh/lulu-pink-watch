# Lulu's Pink Watch

A telling-time game for young kids: a real kid's wristwatch, Lulu the bunny, and 10 levels (numbers, the two hands, o'clock, half past, story levels).

Open `index.html` in a browser to play. It works straight from disk; no web server needed.

## Files

```
index.html                 the whole game
img/watch.webp             photo of a real kid's watch (hands removed), drawn behind the game's hands
audio/voice/<key>.mp3      Lulu's recorded lines (ElevenLabs)
audio/music.mp3            background music (ElevenLabs)
audio/manifest.json        maps each line key to its recording
audio/pack.js              all recordings bundled for file:// use (generated)
tools/build-audio-pack.py  rebuilds audio/pack.js from audio/manifest.json
```

## How the levels teach

- Each level adds one idea: numbers, the two hands, o'clock (heart hand only), reading o'clock, a story with o'clock times (still heart hand only), then half past.
- Both hands move for the first time in level 7, where the intro shows how to set half past.
- The half-past levels each mix in one o'clock round, so the child keeps checking the long hand.
- A round with a mistake comes back once at the end of the level, at most two per level.
- A hint that Lulu gives by herself, after several tries, does not cost a star. A wrong answer does.
- Lulu reads every balloon aloud, so a child who can't read yet can still choose. Half-past balloons always include the next hour (3:30 next to 4:30), the most common mix-up.
- After a one-star level, "Play again" is the main button.

## Voice and music

Lulu's lines and the music are ElevenLabs recordings. `manifest.json` maps each line key from `LINES` in `index.html` to its file:

```json
{
  "music": "audio/music.mp3",
  "voice": {
    "hello": "audio/voice/hello.mp3",
    "t_7_00": "audio/voice/t_7_00.mp3"
  }
}
```

The game loads `audio/pack.js` first. Browsers block `fetch()` for pages opened from disk, so the pack carries the recordings as base64. Without the pack, the game fetches `manifest.json` instead, which needs a web server (`python3 -m http.server`).

After you add or replace a recording, update `manifest.json` and rebuild the pack:

```
python3 tools/build-audio-pack.py
```

If you change the text of a line in `LINES`, record that line again. Any key without a recording falls back to the browser voice, and the built-in music-box tune plays if there is no music file.

## The watch

The watch art is a photo of a real kid's watch, cut out, straightened and with its real hands, centre pin and glare removed. The game draws its own heart (hour) and star (minute) hands on top, and redraws a numeral in a bubble when it lights one up.

The image is 936×1240 px at 2 px per unit and fills the clock's viewBox (`-24 -50 468 620`). In the image, the dial centre is at (448, 620) px and the numeral centres lie on a circle of radius 232 px, with 12 straight up. That matches `CX = 200`, `CY = 260` and `NUM_R = 116` in the code. To use a different photo, keep that geometry.
