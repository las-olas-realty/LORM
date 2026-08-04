# LasOlasRealtyMGMT.com

Single-page marketing site for **14061 SW 54th Street, Miramar, Florida 33027** — a six-bedroom executive home with a private residential elevator in a gated, full-amenity community.

Published by **Las Olas Realty and Management LLC**.

Static HTML. No build step, no framework, no dependencies. Clone it, edit it, upload it.

---

## Two rules that must not be broken

1. **No price appears anywhere.** Not a sale price, not a rent price, not a range. This includes the FAQ structured data, which deliberately answers the pricing question by directing people to message you.
2. **No phone number is displayed.** The number `19176079012` exists only inside `wa.me` link URLs so WhatsApp can route the message. Every visible button reads *Message Me*, *WhatsApp me to tour*, or *Request a showing time*.

If you edit the copy, re-check both before pushing.

---

## Files

```
.
├── index.html          The entire site — markup, CSS and JS in one file
├── 404.html            On-brand not-found page with a WhatsApp CTA
├── robots.txt          Crawler permissions + sitemap pointer
├── sitemap.xml         URL, image and video sitemap for search engines
├── llms.txt            Plain-text property summary for AI assistants
├── CNAME               Custom domain for GitHub Pages
├── .nojekyll           Tells GitHub Pages to serve files as-is
├── .gitignore          Keeps camera originals and OS junk out of the repo
└── assets/
    ├── logo.png        Las Olas Realty wordmark (transparent background)
    ├── home.jpg        Front elevation with turret — also the social share image
    ├── exterior.jpg    Three-car garage and driveway
    ├── kitchen.jpg     Open kitchen with gas range
    ├── elevator.jpg    The private elevator
    ├── life.mp4        Lifestyle reel (hero video, autoplays muted)
    └── life-poster.jpg First frame, shown while the video loads
```

Total repo weight is about 3.9 MB, most of it the video.

---

## Do you need any other crawler files?

**No. You already have everything that matters.** Here is what each one does and why nothing else is required:

| File | Purpose |
|---|---|
| `robots.txt` | Grants access to standard search crawlers **and** explicitly allows the AI crawlers by name — GPTBot, OAI-SearchBot, ChatGPT-User, ClaudeBot, Claude-Web, anthropic-ai, Google-Extended, PerplexityBot, Applebot-Extended, Bingbot. Also points to the sitemap. |
| `sitemap.xml` | Lists the page plus every photo and the video, so images and the reel can surface in image and video search. |
| `llms.txt` | An emerging convention: a clean plain-text brief that AI assistants can read instead of parsing the page. It states the facts, states that no price is published, and states that contact happens through WhatsApp. |
| JSON-LD in `index.html` | Four schema.org blocks — `RealEstateAgent`, `SingleFamilyResidence`, `VideoObject` and `FAQPage`. This is what produces rich results and what answer engines quote from. |
| Meta tags in `index.html` | Description, keywords, geo coordinates, Open Graph, Twitter cards, and per-bot indexing directives. |

Things you do **not** need and should not add:

- `ads.txt` — you are not selling ad inventory.
- `security.txt` — no vulnerability disclosure program to point at.
- `humans.txt` — decorative, ignored by every crawler.
- A `manifest.json` / service worker — this is a one-page brochure site, not an installable app.
- Any keyword-stuffed hidden `<div>`. The keyword-rich paragraph in the footer is visible to humans on purpose. Hidden text is cloaking and gets sites penalized.

The one thing still worth doing is **submitting the sitemap manually** — see the checklist below.

---

## Deploying

Everything is relative-path except the canonical and Open Graph URLs, which are absolute to `https://lasolasrealtymgmt.com/`. Serve the repo root as the web root and it works.

### GitHub Pages

1. Repo → **Settings → Pages**
2. Source: **Deploy from a branch**, branch `main`, folder `/ (root)`
3. Custom domain: `lasolasrealtymgmt.com` (the `CNAME` file already sets this)
4. Tick **Enforce HTTPS** once the certificate provisions — usually a few minutes

At your DNS registrar, point the apex domain at GitHub's IPs:

```
A     @      185.199.108.153
A     @      185.199.109.153
A     @      185.199.110.153
A     @      185.199.111.153
CNAME www    <your-github-username>.github.io.
```

### Netlify

Connect the repo. Build command: leave empty. Publish directory: `.` — that's it. Add the custom domain in **Domain settings**.

### Vercel

Import the repo, choose **Other** as the framework preset, leave build and output settings empty.

### Traditional host (cPanel, Plesk, FTP)

Upload the contents of this folder — not the folder itself — into `public_html`. Confirm `index.html` sits at the root, alongside the `assets/` directory. Delete `CNAME` and `.nojekyll`; they do nothing outside GitHub Pages and are harmless if left.

---

## After it goes live

- [ ] Load the site and confirm the hero video plays and all four photos appear
- [ ] Tap every **Message Me** button and confirm WhatsApp opens with the message pre-filled
- [ ] Test on a real phone, not just a narrow browser window
- [ ] Verify the domain in [Google Search Console](https://search.google.com/search-console) and submit `sitemap.xml`
- [ ] Do the same in [Bing Webmaster Tools](https://www.bing.com/webmasters) — this also feeds ChatGPT search
- [ ] Confirm Google Analytics (`G-C8MNG7PNT5`) is recording traffic in Realtime
- [ ] Run the page through the [Rich Results Test](https://search.google.com/test/rich-results) to confirm the FAQ and property schema parse
- [ ] Paste the URL into Facebook's [Sharing Debugger](https://developers.facebook.com/tools/debug/) so the preview card caches correctly

---

## Making changes

### Swap the WhatsApp number

It appears in `index.html` and `404.html`. Find and replace every instance of `19176079012` — country code first, no `+`, no punctuation. Nine links in `index.html`, one in `404.html`.

Also update the number in `llms.txt` if you change how people reach you.

### Change the Google Analytics property

Replace both instances of `G-C8MNG7PNT5` in `index.html`, and both in `404.html`.

### Add or replace a photo

Optimize before committing — do not drop a 12 MB camera file into `assets/`. Target roughly 1,500 px on the long edge and under 500 KB.

```bash
# macOS / Linux, using ImageMagick
magick input.JPG -auto-orient -resize 1500x -quality 78 -strip assets/newphoto.jpg
```

Then add a `<figure class="shot ...">` block in the gallery section of `index.html`, copying the structure of an existing one. Give it real alt text describing the room — it is read by screen readers and it feeds image search. Add the file to `sitemap.xml` under `<image:image>`.

### Edit the sales copy

The English and Spanish pitches live side by side in the `#pitch` section, in `<article id="pitch-en">` and `<article id="pitch-es">`. They are independent — **if you edit one, edit the other**, or Spanish-speaking buyers get a stale version.

### Adjust drive times

They live in the `<ul class="dist">` block in the `#location` section, and again in `llms.txt` and the FAQ schema near the bottom of `index.html`. Keep all three in sync.

### Reuse this for a different listing

The layout is property-agnostic. Replace the photos, the six numbers in the `.facts` band, the address strings, the gallery captions, both pitches, and the JSON-LD `SingleFamilyResidence` block. The Marrakech styling, arch treatment and CTA structure carry over unchanged.

---

## Design notes

- **Palette:** Matisse blue `#0E4C9A` against spa butter yellow `#F6E3A8`, with a Marrakech terracotta accent `#C8512C` pulled from the Las Olas logo. Background is a zellige eight-point star tile rendered as an inline SVG — no image request.
- **Type:** Marcellus for display, Jost for body. Loaded from Google Fonts.
- **Signature element:** the Moorish arch, repeated at every scale — the hero video, each gallery photo, the icon tiles, and the list bullets.
- **Accessibility:** skip link, visible keyboard focus rings, `aria` labels on the video sound toggle and language tabs, `prefers-reduced-motion` respected, no horizontal scroll from 320 px to 1440 px.
- The hero video autoplays muted and looping. Sound is off until the visitor taps the speaker button, which is what browsers require and what visitors expect.

---

## Before you publish — verify these

The footer carries an accuracy disclaimer, but these are worth confirming against the source yourself:

- Square footage, bedroom and bathroom counts
- HOA dues, rules and what the community fee actually covers
- Current school assignments through Broward County Public Schools
- Which amenities the community operates today
- Drive times, which are approximate and traffic-dependent

Equal Housing Opportunity.

---

© Las Olas Realty and Management LLC
