# palette-lab

Colour palette candidates, rendered side by side over **one identical component skeleton** — so what you compare is the colour, not the content.

Live: **https://pugtox.github.io/palette-lab/**

## What's on the page

| | |
|---|---|
| **Type scale** | Six steps, independent of colour, shared by every candidate. |
| **A / B / C** | Two modes each (light + dark). Kept for comparison. |
| **D / E / F** | One mode each (light or dark). Marked **Shortlisted**. |

**The shortlist is a marker, not a property of the palette.** To move it, put `data-pick` on a
different `<section class="case">` and add `<span class="pick">Shortlisted</span>` to its heading.
Leave no more than three marked — a reader choosing between six balanced trade-offs has
been given no advice at all.

## Source of truth

`index.html` here is a **copy**. The original lives at `web-gzliu/palettes/index.html`
in the local workspace, alongside the workflow documents.

That workspace is **not** published — it holds private material. Only this one page
is safe to make public.

## Syncing

From the workspace root:

```powershell
Copy-Item web-gzliu\palettes\index.html palette-lab\index.html -Force
node web-gzliu\contrast-check.mjs      # must exit 0
git -C palette-lab add -A
git -C palette-lab commit -m "sync palette page"
git -C palette-lab push
```

Keep this a **copy, not a submodule**. This repository must never contain
anything but the one page plus this README.

## Adding a candidate

Four places to change. Miss one and something breaks.

### 1. Variable block — inside `<style>`

```css
[data-palette='x'][data-mode='light'] {
  --accent: #......;   --accent-soft: rgba(..., 0.10);
  --page: #......;     --panel: #......;       --panel-hover: #......;
  --line: #......;     --line-strong: #......;
  --fg: #......;       --muted: #......;       --grid: rgba(..., 0.05);
}
```

A single-mode candidate keeps only `light` or `dark`, never both.

### 2. Preview section — after the last `</section>`, before `</div>`

```html
<section class="case" id="x">
  <h2><span class="letter">X</span> Name <small>one-line character</small></h2>
  <p class="note">Why this one — write the trade-off, not adjectives.</p>
  <div class="row">
    <figure>
      <div class="phone" data-palette="x" data-mode="light">
        <!-- copy a whole topbar + body from an existing preview and keep it generic -->
      </div>
      <figcaption>light</figcaption>
    </figure>
  </div>
  <pre class="tokens"><b>light</b>
...exactly the same variables as (1)...
  </pre>
</section>
```

### 3. TOC — one line inside `<ul class="toc">`

```html
<li><a href="#x">X · Name</a></li>
```

### 4. Contrast checker — one entry in `schemes` (`web-gzliu/contrast-check.mjs`)

```js
x: { name: 'X — Name', light: { fg: '#......', muted: '#......', page: '#......', panel: '#......', accent: '#......' } },
```

**These values must match (1) character for character.** They are transcribed by hand,
and if they drift the contrast verdict is void.

Then run `node web-gzliu/contrast-check.mjs`. Body and secondary text must both clear
**4.5:1** and the exit code must be **0**.

The checker rejects anything that is not exactly `#rrggbb`. That guard matters: a
malformed value can otherwise produce a plausible-looking but wrong number — the
5-digit `#0f766` computes as 5.81:1 where the true value is 5.47:1.

## Rules the page follows

- **No real names, brands or prices** in the previews. The mockup copy is generic on
  purpose — nothing to scrub when the file is reused, and identical content keeps the
  comparison honest. This is also a **publication rule**: whatever goes in the page
  becomes public on the next sync.
- **One `<h1>` per page.** The mockup headline is a `<div class="mock-h1">`, not a
  heading — nine simulated headlines must not compete with the real one.
- Body text and secondary text both clear **4.5:1**.
