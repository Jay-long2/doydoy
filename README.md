# For Doydoy — 22nd Birthday

A single-page romantic site. No build step, no dependencies.

```
index.html          the whole site (HTML + CSS + JS in one file)
assets/pretty.png   her photo
assets/doydoy.mp4   video 1
assets/doydoy2.mp4  video 2
```

## The experience

1. **Open it** — intro card with floating hearts
2. **Photo** — her picture as a polaroid
3. **Video 1** — portrait frame, tap 🔇 for sound
4. **Reasons** — six cards she taps to flip
5. **Video 2**
6. **Cake** — she taps out 3 candles to make a wish
7. **Finale** — "22", confetti, replay button

Navigate by tapping **Next**, swiping, or arrow keys. Tapping anywhere spawns a heart.

## Customising the words

Open `index.html` and look for the `<!-- EDIT ME -->` comments. Change:

- her name on the finale slide (`Happy Birthday,<br>Doydoy`)
- the `<p class="line">` message under each chapter
- the six card backs (`<i class="b">your laugh</i>`)
- the page `<title>`

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

They total ~12 MB, which is fine for both hosts' free tiers but can be slow on a
weak mobile connection. If you want it snappier:

```bash
ffmpeg -i assets/doydoy.mp4 -vf scale=480:-2 -c:v libx264 -crf 28 -c:a aac -b:a 96k assets/doydoy_small.mp4
```
then point the `<video src>` at the smaller file.
