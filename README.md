# palette-lab

Colour palette candidates, rendered side by side over **one identical component skeleton** — so what you compare is the colour, not the content.

Live: **https://pugtox.github.io/palette-lab/**

## What's in here

| | |
|---|---|
| **Template** | The copy-and-paste skeleton for adding a candidate. Four places to change; missing one breaks something. |
| **Type scale** | Six steps, independent of colour, shared by every candidate. |
| **A / B / C** | Two modes each (light + dark). |
| **D / E / F** | One mode each (light or dark). Example of the single-mode shape. |

## Source of truth

`index.html` here is a **copy**. The original lives at `web-gzliu/palettes/index.html`
in the local workspace, alongside the workflow documents.

That workspace is **not** published: it holds rates, readiness notes and client
material. Only this one page is safe to make public.

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
anything but the one page.

## Rules the page follows

- **No real names, brands or prices** in the previews. The mockup copy is generic on purpose —
  nothing to scrub when the file is reused, and identical content keeps the comparison honest.
- **Every candidate's colours must be transcribed into `contrast-check.mjs`**, verbatim.
  If they drift apart, the all-green contrast result is meaningless.
- Body text and secondary text must both clear **4.5:1**.
