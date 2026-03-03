# Lumiscale — Product Requirements Document

> **Tagline:** Illuminate every pixel  
> **Type:** Single-page web app (HTML + CSS + Vanilla JS)  
> **Deployment Target:** Vercel (static)  
> **Cost:** Zero — no APIs, no backend, fully client-side  

---

## 1. Overview

Lumiscale is a browser-based AI image upscaling web app. Users drag and drop an image, choose an upscale factor (2x, 4x, 8x), and the app processes it entirely on the client using canvas-based multi-pass bicubic interpolation and unsharp mask sharpening. No image ever leaves the user's device.

---

## 2. Tech Stack

| Layer | Technology |
|---|---|
| Markup | HTML5 |
| Styling | Tailwind CSS (CDN) + custom CSS |
| Font | Plus Jakarta Sans (Google Fonts) |
| Icons | Material Symbols Outlined (Google Fonts) |
| Logic | Vanilla JavaScript (ES2020+) |
| Canvas | Native browser Canvas 2D API |
| Deploy | Vercel (static file drop or GitHub import) |

**No npm. No frameworks. No API keys. Single HTML file output.**

---

## 3. Brand Identity

| Token | Value |
|---|---|
| App Name | Lumiscale |
| Tagline | Illuminate every pixel |
| Primary | `#a689fa` (soft violet) |
| Accent Blue | `#38bdf8` (sky blue) |
| Lumi Gold | `#f0c060` (warm gold) |
| Background | `#080810` (near black) |
| Card | `#0f0f1a` |
| Font | Plus Jakarta Sans |

---

## 4. Pages & Sections

### 4.1 Navbar (sticky)
- Left: flare icon + "Lumiscale" wordmark
- Center (desktop only): italic tagline "Illuminate every pixel"
- Right: "Free Forever" pill badge with violet glow

### 4.2 Hero Section
- Headline: `Your images, luminously sharp` — gradient text on "luminously sharp"
- Subtext: `Upscale up to 8x — fully private, runs in your browser.`

### 4.3 Upload Zone (default state)
- Large rounded card with dashed border
- Cloud upload icon in violet-tinted square
- Heading: "Drop your masterpiece here"
- Subtext: "Supports JPG, PNG, WEBP up to 20MB"
- "Select File" CTA button (gradient violet → blue)
- Drag & drop support with active highlight state
- Hidden `<input type="file" accept="image/*">`

### 4.4 Workspace (shown after upload, hidden before)
**Left panel (2/3 width) — Before/After Comparison Slider:**
- `aspect-video` container
- Bottom layer: original image (full width)
- Top layer: upscaled image clipped to slider position
- White vertical drag handle with circular grip icon
- Mouse and touch drag support
- Labels: "Original" bottom-left, "Luminescent Nx" bottom-right (shown after processing)
- Below: "Processing locally — never leaves your device" note + image dimensions

**Right panel (1/3 width) — Controls:**
- Scale selector: 2x / 4x / 8x pill buttons (4x active by default)
- Progress section (hidden until Enhance is clicked):
  - Label: "Enhancement Progress"
  - Gradient progress bar with shimmer animation
  - Status text below (e.g. "Analysing pixels… 65%")
- "✦ Enhance Details" button — gradient, pulse glow animation
- "Download Result" button — gold border, disabled/faded until processing complete, confetti on enable
- "↩ Upload New Image" text button to reset

### 4.5 Trust Badges
- Three pill badges: "100% Private" · "No Server Upload" · "Zero Cost"
- Divider line above

### 4.6 Footer
- Faded Lumiscale logo
- "Illuminate every pixel © 2025. Localized AI Power."
- Links: Terms · Privacy · GitHub

---

## 5. Upscaling Engine (client-side)

### Algorithm: Multi-pass Bicubic + Unsharp Mask

**Step 1 — Multi-pass upscale:**
- 2x → 1 pass at full scale
- 4x → 2 passes at √4 = 2x each
- 8x → 3 passes at 2x each
- Each pass uses `ctx.imageSmoothingQuality = 'high'`

**Step 2 — Unsharp mask sharpening:**
- Apply 3×3 Gaussian blur kernel `[1,2,1, 2,4,2, 1,2,1]` (sum=16)
- For each pixel: `output = original + amount × (original − blurred)`
- Amount: `0.4` for 2x, `0.6` for 4x, `0.8` for 8x
- Clamp all values to 0–255

**Step 3 — Final render:**
- Draw sharpened canvas to final output canvas at exact target dimensions
- Export as PNG via `canvas.toDataURL('image/png')`

### Progress Steps
| % | Message |
|---|---|
| 0 | Loading image… |
| 15 | Analysing pixel structure… |
| 30 | Upscaling with high-quality interpolation… |
| 30–75 | Enhancing pass N/N… |
| 75 | Applying luminance sharpening… |
| 90 | Polishing output… |
| 100 | ✦ Enhanced Nx — WxHpx |

---

## 6. Interactions & Animations

| Interaction | Behaviour |
|---|---|
| Drag over upload zone | Border highlight, background tint |
| File selected | Upload zone hides, workspace fades in |
| Scale button click | Active pill style switches |
| Enhance click | Progress bar animates with shimmer, status updates |
| Processing complete | Download button enables, confetti burst |
| Download click | Save PNG as `[filename]_lumiscale_Nx.png` |
| Reset click | Workspace hides, upload zone reappears |
| Slider drag | Before/after panels update in real time |

---

## 7. Visual Effects

- **Background:** radial gradients (violet top-center, blue bottom-left) + SVG noise grain overlay (fixed, z-50, opacity 0.025)
- **Aurora glow:** `box-shadow` on buttons and comparison handle
- **Gradient text:** CSS `-webkit-background-clip: text` on hero headline
- **Shimmer bar:** pseudo-element sliding across progress bar
- **Confetti:** 18 colored `div` particles with CSS keyframe fall animation on download enable
- **Fade-up:** workspace section animates in on image load
- **Pulse glow:** Enhance button has repeating glow keyframe

---

## 8. File Structure

```
lumiscale/
└── index.html   ← entire app (HTML + CSS + JS, single file)
```

---

## 9. Deployment (Vercel)

1. Create a GitHub repo, add `index.html`
2. Import repo on vercel.com
3. Framework preset: **Other** (static)
4. Output directory: leave blank (root)
5. Deploy → live in ~30 seconds

Every `git push` auto-redeploys.

---

## 10. Non-Goals

- No backend / server
- No API keys
- No npm / build step
- No frameworks (React, Vue, etc.)
- No user accounts
- No image storage
