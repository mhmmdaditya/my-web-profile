# my-web-profile

Personal profile site for **Muhammad Aditya** — Software Engineer and Technical Lead based in
Jakarta, Indonesia. Technical lead for JAGRA (an integrated HR and security platform) and technical
expert for drone-based computer vision inspection at RA ATAP, Telkom University.

The whole site is **one self-contained `index.html`**. No build step, no framework, no bundler —
open the file and it runs.

---

## Live

Once GitHub Pages is enabled (see [Deploying](#deploying)):

**https://mhmmdaditya.github.io/my-web-profile/**

---

## What's in it

| | |
|---|---|
| **WebGL fluid hero** | A full-bleed GPU fluid simulation paints cyan → blue → violet → magenta ink on a near-black ground. It bursts on load, then an invisible auto-cursor orbits the centre forever; the real mouse or finger stirs it too. |
| **Bilingual — EN / ID** | A switch in the header translates 131 elements across the page. The choice is remembered in `localStorage`, and the page opens in Indonesian automatically for `id-*` browsers. `<html lang>` follows the switch. |
| **The full CV** | Profile, six capability domains, a dated experience timeline, six selected projects, education, certifications, and community work — all sourced from the PDF in `assets/`. |
| **CV download** | The PDF is embedded in the page as a data URI, so the download works even from a single file with nothing beside it. |
| **Contact** | Copy-to-clipboard email bar, plus email, LinkedIn, and WhatsApp links. No backend, nothing collected. |

### Behaviour worth knowing about

- **Responsive rem grid.** The root font-size is driven by viewport width, so the layout scales
  proportionally instead of jumping between breakpoints. Every branch is floored at 14px so body
  copy stays readable on a long page.
- **`prefers-reduced-motion` is respected.** The fluid simulation, the marquee, and every entrance
  animation are skipped; a static ink gradient stands in for the hero.
- **Graceful WebGL fallback.** Same static gradient if the browser has no WebGL context.
- **The simulation pauses** when the hero scrolls out of view and when the tab is hidden, so it
  costs nothing while you read.
- **Accessibility.** Semantic landmarks, a skip link, visible focus rings, `aria-pressed` on the
  language switch, keyboard-dismissable mobile menu, and live-region feedback on copy.
- **Nothing is parked invisible.** Scroll reveals are opt-in via JavaScript, with a 3-second
  failsafe that shows everything regardless.

---

## Running it locally

Open `index.html` in a browser. That's genuinely it.

If you'd rather serve it over HTTP (Lenis and the Google Fonts stylesheet both load fine from
`file://`, but a server is closer to production):

```bash
python -m http.server 8000     # then open http://localhost:8000
# or
npx serve .
```

---

## Deploying

### GitHub Pages

The repository is already shaped for it — `index.html` sits at the root and `.nojekyll` keeps
Jekyll from touching anything.

1. Go to **Settings → Pages**.
2. Under **Build and deployment → Source**, pick **Deploy from a branch**.
3. Branch **`main`**, folder **`/ (root)`**. Save.
4. Wait a minute, then open **https://mhmmdaditya.github.io/my-web-profile/**.

### Anywhere else

Netlify, Vercel, Cloudflare Pages, or any static host: point it at the repository root with no
build command and no output directory. Or just upload `index.html` on its own — it carries the
photo and the CV inside it.

---

## Project structure

```
.
├── index.html                       # the entire site (~440 KB, assets embedded)
├── assets/
│   ├── profile-photo.jpg            # source photo, embedded in index.html
│   └── muhammad-aditya-cv.pdf       # source CV, embedded in index.html
├── .nojekyll                        # GitHub Pages: serve files as-is
├── .gitignore
└── README.md
```

`assets/` holds the **originals**. The page does not fetch them at runtime — it carries its own
base64 copies — so they are here for regeneration, not for serving.

---

## Built with

- **[Onest](https://fonts.google.com/specimen/Onest)** for text and **[JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono)** for data labels, via Google Fonts.
- **[Lenis](https://github.com/darkroomengineering/lenis) 1.3.19** for smooth scrolling, loaded from jsDelivr. If the CDN is unreachable the page falls back to native smooth scrolling.
- **[WebGL Fluid Simulation](https://github.com/PavelDoGreat/WebGL-Fluid-Simulation)** by Pavel Dobryakov (MIT) — the solver behind the hero, tuned here for an oily, marbled look on near-black, with a colour band restricted to cyan through magenta and an auto-cursor that keeps it alive with no input.
- Plain DOM and vanilla JavaScript for everything else.

---

## Editing the content

**Text.** All copy lives in `index.html`. English is the markup's own content; the Indonesian
counterpart rides along in a `data-id` attribute on the same element:

```html
<h3 data-id="Keamanan &amp; Jaringan">Security &amp; Networking</h3>
```

Edit both halves together and the switch stays in sync. Only `<strong>` and `<b>` are used inside
translation attributes — keep it that way, and escape `&` as `&amp;`.

**Photo or CV.** Replace the file in `assets/`, then re-embed it. From the repository root:

```bash
# photo
node -e "const fs=require('fs');const b=fs.readFileSync('assets/profile-photo.jpg').toString('base64');const f='index.html';let d=fs.readFileSync(f,'utf8');d=d.replace(/src=\"data:image\/jpeg;base64,[^\"]*\"/,'src=\"data:image/jpeg;base64,'+b+'\"');fs.writeFileSync(f,d)"

# CV
node -e "const fs=require('fs');const b=fs.readFileSync('assets/muhammad-aditya-cv.pdf').toString('base64');const f='index.html';let d=fs.readFileSync(f,'utf8');d=d.replace(/var CV_B64 = '[^']*'/,'var CV_B64 = '+JSON.stringify(b));fs.writeFileSync(f,d)"
```

**Colours and spacing.** Every colour and the type scale are CSS custom properties in the `:root`
block at the top of the file. The page commits to a single dark theme deliberately, so each colour
is painted explicitly rather than inherited.

---

## Ringkasan (Bahasa Indonesia)

Situs profil pribadi Muhammad Aditya, dibuat sebagai **satu berkas `index.html` mandiri** — tanpa
framework, tanpa proses build. Fitur utamanya: hero dengan simulasi fluida WebGL, **pengalih bahasa
EN/ID** yang menerjemahkan seluruh halaman dan mengingat pilihan pengunjung, isi CV lengkap, serta
tombol unduh CV yang berfungsi tanpa berkas pendamping karena PDF-nya sudah tertanam di dalam
halaman.

Untuk menjalankan: buka `index.html` di peramban. Untuk publikasi: aktifkan GitHub Pages lewat
**Settings → Pages**, pilih branch `main` dan folder `/ (root)`.

Menyunting teks: bahasa Inggris ada di isi elemen, bahasa Indonesia di atribut `data-id` pada
elemen yang sama — ubah keduanya bersamaan.

---

## Licence

The fluid simulation is MIT-licensed, © Pavel Dobryakov. The fonts are under the SIL Open Font
License. The site's design, code, written content, CV, and photograph are © 2026 Muhammad Aditya —
please don't reuse the personal content or the likeness.
