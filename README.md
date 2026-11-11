# For Doydoy — 22nd Birthday

A single-page romantic site. No build step, no dependencies.

```
index.html          the whole site (HTML + CSS + JS in one file)
assets/pretty.png   her photo (polaroid on chapter one + the right-hand banner)
assets/doydoy.mp4   video 1  (chapter two)
assets/doydoy2.mp4  video 2  (chapter four)
assets/doy-bg.mp4   the background clip — a small silent copy of doy.mp4
assets/doy.mp4      your original of that clip (not used by the page, see below)
```

## The experience

Behind every slide, two banners wave like flags in the wind: `doy.mp4` plays
silently down the **left**, and her photo hangs down the **right**, mirrored.
Each is anchored at its outer edge and dissolves toward the middle, so the text
sits in clear space between them. Reduced-motion visitors get a still frame of
the photo and no video.

1. **Open it** — "Just because you have a cute smile", floating hearts
2. **Photo** — her picture as a polaroid
3. **Video 1** — portrait frame, tap 🔇 for sound
4. **Reasons** — seven cards she taps to flip
5. **Video 2**
6. **Cake** — she taps out 3 candles to make a wish
7. **Finale** — "22", confetti, replay — and after ~2s a little bubble pops up:
   *"You still owe me a lollipop tho 😂"* (tap it to dismiss)

Navigate by tapping **Next**, swiping, or arrow keys. Tapping anywhere spawns a heart.

## Customising the words

Open `index.html` and look for the `<!-- EDIT ME -->` comments. Change:

- her name on the finale slide (`Happy Birthday,<br>Doydoy`)
- the `<p class="line">` message under each chapter
- the seven card backs (`<i class="b">your laugh and smile</i>`)
- the joke bubble at the end (search for `id="joke"`)
- the page `<title>`

### The waving background

Two dials in the CSS control how loud each side is:

- `#flag { opacity: .36 }` — the clip on the left
- `#flagR { opacity: .38 }` — the photo on the right

Raise them for a bolder background, drop toward `.25` for more of a whisper.
They use different blend modes on purpose: the clip is brightly lit so it
blends normally, while the photo is dark and uses `screen`, which lifts it out
of the background instead of muddying it. Swap a dark clip in and you'll likely
want `screen` on both.

Everything else lives in the `// her clip (left) and her photo (right), waving
like flags` block near the bottom of the script:

- `amp` — how far the cloth swings
- the `-t*0.0017` term — the wind speed
- `dh` and `cap` — how large each banner is; `cap` holds them to half the
  screen each so they meet in the middle without ever overlapping
- `side` — `1` hangs a banner from the left edge, `-1` mirrors it to the right
- `CROP` — your clip is letterboxed (black bars top and bottom), so the page
  crops to the picture inside it. If you swap in a different clip, either
  re-measure those fractions or set it to `{x:0, y:0, w:1, h:1}` for a clip
  with no bars.

**About the two copies.** The background version is re-encoded to 406×720,
silent, at 729 KB — down from the original's 9.2 MB. At the size it actually
renders, nobody can tell, and it saves her phone ~9 MB on load. Your original
`doy.mp4` is untouched in `assets/`; nothing links to it, so visitors never
download it. Delete it before deploying if you want a leaner upload, or
regenerate the background copy with:

```bash
ffmpeg -i assets/doy.mp4 -an -vf "scale=-2:720,fps=24" \
  -c:v libx264 -profile:v main -crf 30 -preset medium \
  -movflags +faststart assets/doy-bg.mp4
```

## Preview locally

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

## Deploy

**Netlify (easiest)** — go to https://app.netlify.com/drop and drag this whole
folder into the page. You get a live URL in seconds.

**Vercel** —
```bash
npx vercel --prod
```
Accept the defaults; it's a static site, so no framework or build command.

Either host serves `index.html` automatically. Nothing else to configure.

## Note on the videos

The two chapter videos total ~12 MB, which is fine for both hosts' free tiers
but can be slow on a weak mobile connection. If you want it snappier:

```bash
ffmpeg -i assets/doydoy.mp4 -vf scale=480:-2 -c:v libx264 -crf 28 -c:a aac -b:a 96k assets/doydoy_small.mp4
```
then point the `<video src>` at the smaller file.
