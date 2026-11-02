# Portfolio site

One page, hand-written HTML and CSS. No build step, no dependencies, no JavaScript.

```
site/
  index.html                          the page
  styles.css                          tokens, type scale, layout
  assets/fonts/instrument-sans-latin.woff2   self-hosted, SIL Open Font License
  assets/icon.svg                     favicon, light appearance
  assets/icon-dark.svg                favicon, dark appearance
```

## Preview

```bash
cd site && python3 -m http.server 8000
```

Open <http://localhost:8000>. Opening `index.html` directly also works, except that the
self-hosted font may be blocked by `file://` rules in some browsers.

## Publish

GitHub Pages, from this folder:

1. Push the repository.
2. Settings → Pages → Source: **Deploy from a branch**, folder **/site**.
3. Add a custom domain later if you want one; nothing in the markup needs to change.

For a user site (`atharva-potdar.github.io`), copy the contents of `site/` to the root of that
repository instead. Paths are relative, so they keep working.

## Editing

- **Colour** lives in the `:root` block of `styles.css` and is redefined once for
  `prefers-color-scheme: dark`. Every value in the file references those tokens.
- **Type** uses one family, `Instrument Sans`, self-hosted as a variable font, plus the system
  monospace stack for data and labels. Body copy is 17 px; the display sizes are `--step-2` to
  `--step-4`.
- **Spacing** is a multiple of 8 px. Section rhythm comes from `clamp()` values that stay on the
  grid at both ends.
- **A project** is one `<li class="entry">`: number, title link, stack line, description, one
  measured note, and a definition list of facts. Copy the block and change the contents.
- **Motion** is three transitions, all on `transform` or `opacity` or `border-color`, and all
  disabled under `prefers-reduced-motion`.

## Checks run on this build

Lighthouse 13.6 on Chromium, both presets, run against a local static server:

| | Performance | Accessibility | Best practices | SEO |
| --- | --- | --- | --- | --- |
| Mobile | 100 | 100 | 100 | 100 |
| Desktop | 100 | 100 | 100 | 100 |

Mobile moved to LCP in 0.8 s on Lighthouse's simulated slow 4G, desktop in 0.2 s, with a total
blocking time of 0 ms and a cumulative layout shift of 0. Total page weight is 52 KiB across four
requests: the document, one stylesheet, one font, and the icon.

The remaining advisory items, and why they are left alone:

- **Minify CSS**, 3.8 KiB estimated. `styles.css` gzips from 11.8 KiB to 2.9 KiB, so the real
  saving is under a kilobyte. Minification needs a build step, and this site deliberately has none.
- **Render-blocking request**, the stylesheet, 151 ms. Inlining it would remove one request and
  forfeit caching on repeat visits. At a 0.8 s mobile LCP it is not worth the trade.
- **Efficient cache lifetimes** and **document request latency** are properties of the host. The
  local `python3 -m http.server` sends no cache headers; GitHub Pages sets its own. Neither is
  affected by the markup.

Also verified by script rather than by Lighthouse:

- No horizontal overflow at 320, 390, 560, 768, 1024, 1280 and 1440 px, nor at 200% text size.
- Body text 17.1:1, secondary text 7.72:1, accent 5.7:1 on the light paper; 15.73:1, 8.57:1 and
  6.89:1 in dark. All above 4.5:1, and secondary text clears Apple's 7:1 target for custom colors.
- Every link and control is at least 44 px tall; focus is always visible at 5.7:1 (6.89:1 in dark).
- One `<h1>`, no skipped heading levels, no duplicate IDs, every section labelled, every list keeps
  its list role, and every link names its destination.
- Zero scripts, two weights, one font file, no images.
