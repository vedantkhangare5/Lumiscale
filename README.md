# Lumiscale ✦

I built this because every image upscaler I tried either cost money, sent my photos to some random server, or just looked terrible. So I made one that runs entirely in your browser — no accounts, no uploads, no cost.

It's a single HTML file. That's it.

---

## What it does

You drop in a photo, pick how much you want to upscale it (2x, 4x, or 8x), hit Enhance, and drag the before/after slider to see the difference. When you're happy, download it. Your image never leaves your device.

Works on any photo — portraits, landscapes, product shots, old scanned pictures.

---

## How to use it

Just open `index.html` in your browser. No setup. No install. No terminal.

If you want to run it online, it's deployed at [lumiscale.vercel.app](https://lumiscale.vercel.app)

---

## How the upscaling works

There's no AI model being called. It's all canvas math running in your browser:

- Multi-pass bicubic interpolation (the number of passes depends on your chosen scale)
- Unsharp masking after each pass to bring back edge detail that interpolation tends to blur out
- A final sharpening pass tuned to the scale factor

It's not magic — a dedicated AI model would do better — but it's fast, free, and genuinely useful for making small images presentable.

---

## Stack

- Plain HTML, CSS, JavaScript — no frameworks
- Tailwind CSS (via CDN)
- Canvas 2D API for all the image processing
- Plus Jakarta Sans for the font
- Deploys as a static file on Vercel

---

## Run locally

```bash
git clone https://github.com/YOUR_USERNAME/lumiscale.git
cd lumiscale
open index.html
```

That's genuinely all you need to do.

---

## Why a single file?

Because I wanted anyone to be able to download it, send it to a friend, or host it anywhere without explaining dependencies or build steps. A single HTML file just works.

---

Made with ✦ by Vedant Khangare(https://github.com/vedantkhangare5)
