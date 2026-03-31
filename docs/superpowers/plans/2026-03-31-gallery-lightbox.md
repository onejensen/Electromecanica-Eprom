# Gallery Lightbox Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace placeholder Unsplash images in the gallery page with the 7 real workshop photos, and add a vanilla JS lightbox for full-screen viewing.

**Architecture:** Single-file change to `src/pages/galeria.astro`. The `imagenes` array is updated with local paths; a hidden overlay `<div>` is injected once into the DOM via an inline `<script>` tag. Clicking a gallery card populates the overlay and shows it.

**Tech Stack:** Astro 6, vanilla JS (no dependencies), inline CSS

---

### Task 1: Replace placeholder images with real photos

**Files:**
- Modify: `src/pages/galeria.astro` (lines 3–41)

- [ ] **Step 1: Update the `imagenes` array**

Replace the entire `imagenes` array (lines 3–41) with:

```js
const imagenes = [
  { src: '/galeria/FRS_9654.jpg',  alt: 'Taller Electromecánica Eprom — vista general' },
  { src: '/galeria/FRS_9697.jpg',  alt: 'Trabajos de mecánica en Eprom Palma' },
  { src: '/galeria/FRS_9708.jpg',  alt: 'Diagnóstico electrónico en Eprom' },
  { src: '/galeria/FRS_9712.jpg',  alt: 'Reparación de vehículos en Mallorca' },
  { src: '/galeria/FRS_9714.jpg',  alt: 'Instalaciones del taller Eprom' },
  { src: '/galeria/FRS_9726.jpg',  alt: 'Mecánicos trabajando en Eprom' },
  { src: '/galeria/FRS_9801.jpg',  alt: 'Servicio de electromecánica en Palma' },
];
```

- [ ] **Step 2: Remove the "fotos de referencia" notice**

Delete lines 73–75 (the `<div>` with the grey disclaimer text):

```html
<!-- DELETE this block -->
<div style="margin-top:48px;text-align:center;">
  <p style="color:#666;font-size:0.85rem;font-family:'Barlow Condensed',sans-serif;">Estas imágenes son de referencia. Pronto añadiremos fotografías reales de nuestro taller.</p>
</div>
```

- [ ] **Step 3: Add `data-index` to each gallery card**

In the `imagenes.map(...)` JSX, add `data-index={i}` and `data-src={img.src}` to the outer `<div>` and a `cursor:pointer` style, so the lightbox script can identify which image was clicked:

```astro
{imagenes.map((img, i) => (
  <div
    class="gallery-item"
    data-index={i}
    data-src={img.src}
    style={`position:relative;aspect-ratio:4/3;overflow:hidden;border-radius:2px;border:1px solid #333;animation:fadeInUp 0.6s ease-out ${(i*0.08)+0.1}s both;cursor:pointer;`}
  >
    <img
      src={img.src}
      alt={img.alt}
      style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;transition:transform 0.5s ease;"
      loading="lazy"
    />
    <div style="position:absolute;bottom:0;left:0;right:0;background:linear-gradient(to top,rgba(0,0,0,0.8) 0%,transparent 100%);padding:20px 16px 16px;">
      <p style="color:#F0F0F0;font-size:0.85rem;font-family:'Barlow Condensed',sans-serif;font-weight:500;">{img.alt}</p>
    </div>
  </div>
))}
```

- [ ] **Step 4: Verify in browser**

```bash
npm run dev
```

Open `http://localhost:4321/Electromecanica-Eprom/galeria` — the 7 real photos should appear in the grid (no Unsplash images, no disclaimer). No lightbox yet.

- [ ] **Step 5: Commit**

```bash
git add src/pages/galeria.astro
git commit -m "feat: replace placeholder images with real workshop photos"
```

---

### Task 2: Add vanilla JS lightbox

**Files:**
- Modify: `src/pages/galeria.astro` (append `<script>` block and overlay markup)

- [ ] **Step 1: Add the lightbox overlay markup**

Before the closing `</Layout>` tag, add:

```html
<!-- Lightbox overlay -->
<div id="lb-overlay" style="display:none;position:fixed;inset:0;z-index:9999;background:rgba(0,0,0,0.92);align-items:center;justify-content:center;">
  <button id="lb-close" aria-label="Cerrar" style="position:absolute;top:20px;right:28px;background:none;border:none;color:#F5A623;font-size:2.5rem;line-height:1;cursor:pointer;font-family:'Bebas Neue',sans-serif;">×</button>
  <button id="lb-prev" aria-label="Anterior" style="position:absolute;left:16px;top:50%;transform:translateY(-50%);background:none;border:none;color:#F5A623;font-size:3rem;line-height:1;cursor:pointer;font-family:'Bebas Neue',sans-serif;">‹</button>
  <img id="lb-img" src="" alt="" style="max-width:90vw;max-height:90vh;object-fit:contain;display:block;border:2px solid #333;" />
  <button id="lb-next" aria-label="Siguiente" style="position:absolute;right:16px;top:50%;transform:translateY(-50%);background:none;border:none;color:#F5A623;font-size:3rem;line-height:1;cursor:pointer;font-family:'Bebas Neue',sans-serif;">›</button>
</div>
```

- [ ] **Step 2: Add the lightbox script**

After the overlay markup (still before `</Layout>`), add:

```html
<script>
  const images = [
    { src: '/galeria/FRS_9654.jpg',  alt: 'Taller Electromecánica Eprom — vista general' },
    { src: '/galeria/FRS_9697.jpg',  alt: 'Trabajos de mecánica en Eprom Palma' },
    { src: '/galeria/FRS_9708.jpg',  alt: 'Diagnóstico electrónico en Eprom' },
    { src: '/galeria/FRS_9712.jpg',  alt: 'Reparación de vehículos en Mallorca' },
    { src: '/galeria/FRS_9714.jpg',  alt: 'Instalaciones del taller Eprom' },
    { src: '/galeria/FRS_9726.jpg',  alt: 'Mecánicos trabajando en Eprom' },
    { src: '/galeria/FRS_9801.jpg',  alt: 'Servicio de electromecánica en Palma' },
  ];

  let current = 0;

  const overlay = document.getElementById('lb-overlay');
  const lbImg   = document.getElementById('lb-img');
  const btnClose = document.getElementById('lb-close');
  const btnPrev  = document.getElementById('lb-prev');
  const btnNext  = document.getElementById('lb-next');

  function openLightbox(index) {
    current = index;
    lbImg.src = images[current].src;
    lbImg.alt = images[current].alt;
    overlay.style.display = 'flex';
    document.body.style.overflow = 'hidden';
  }

  function closeLightbox() {
    overlay.style.display = 'none';
    document.body.style.overflow = '';
  }

  function showPrev() {
    current = (current - 1 + images.length) % images.length;
    lbImg.src = images[current].src;
    lbImg.alt = images[current].alt;
  }

  function showNext() {
    current = (current + 1) % images.length;
    lbImg.src = images[current].src;
    lbImg.alt = images[current].alt;
  }

  // Open on card click
  document.querySelectorAll('.gallery-item').forEach((card) => {
    card.addEventListener('click', () => {
      const index = parseInt(card.dataset.index, 10);
      openLightbox(index);
    });
  });

  // Controls
  btnClose.addEventListener('click', closeLightbox);
  btnPrev.addEventListener('click', (e) => { e.stopPropagation(); showPrev(); });
  btnNext.addEventListener('click', (e) => { e.stopPropagation(); showNext(); });

  // Click outside image closes
  overlay.addEventListener('click', (e) => {
    if (e.target === overlay) closeLightbox();
  });

  // Escape key closes
  document.addEventListener('keydown', (e) => {
    if (overlay.style.display === 'none') return;
    if (e.key === 'Escape') closeLightbox();
    if (e.key === 'ArrowLeft') showPrev();
    if (e.key === 'ArrowRight') showNext();
  });
</script>
```

- [ ] **Step 3: Verify lightbox in browser**

```bash
npm run dev
```

Open `http://localhost:4321/Electromecanica-Eprom/galeria` and verify:
- Clicking a photo opens the lightbox overlay
- × button closes it
- ‹ / › arrows navigate between photos (wrapping around)
- Clicking outside the image closes it
- Escape key closes it
- Arrow keys navigate

- [ ] **Step 4: Commit**

```bash
git add src/pages/galeria.astro
git commit -m "feat: add vanilla JS lightbox to gallery page"
```
