# SVG Editor · SVG 矢量编辑器

> A small vector editor in one HTML file: rectangles, ellipses, lines, freehand paths and text — export clean, editable SVG source.

**English** | [简体中文](README.zh-CN.md)

<a href="https://bushimashen.github.io/svg-editor/"><b>▶ Live demo</b></a>

## Features

- **5 drawing tools** — rectangle, ellipse, line, freehand brush, text (plus image import, embedded as a data URI)
- **Shift constraints** — square, circle, and 45°-locked lines
- **Select / move / resize** — 8 handles, arrow-key nudge (1 px, or 10 px with Shift)
- **Per-shape properties** — fill, stroke, stroke width, opacity, rotation, horizontal/vertical flip
- **Layer list** — top-most first, click to select, eye icon to hide, bring-forward / send-backward / to-front / to-back
- **Undo / redo** — Ctrl+Z / Ctrl+Shift+Z, up to 60 steps
- **Zoom** — Ctrl+wheel or the −/+ buttons; zooming the view never changes the exported size
- **Export** — download `drawing.svg`, or copy the source to the clipboard

## Why the export is worth trusting

Shapes are stored as plain data and rendered through one `el(shape)` function that emits real SVG elements.
Export calls **the same function** for every shape with the selection overlay excluded — so what you download
is exactly what you see, not a screenshot and not a re-implementation.

Because the output is real SVG (not canvas pixels), it opens in Figma, Illustrator, Inkscape or a browser and
every shape stays editable, with `transform` attributes carrying rotation and flips.

## A bug worth documenting

The layer label used to be written as an object literal keyed by shape kind:

```js
// broken: every value is evaluated, so s.text.slice() runs for rects too
return { rect: '矩形', text: '文字「' + s.text.slice(0, 8) + '」' }[s.kind] + ' #' + s.id
```

`s.text` is `undefined` on a rectangle, so the whole literal throws — and because that call sits inside the
inspector render, **the entire right panel silently stopped rendering** for non-text shapes. Switched to an
`if / else if` chain that only touches `s.text` when the shape is actually text.

## Keyboard

| Key | Action |
|---|---|
| `V` `R` `O` `L` `P` `T` | select / rect / ellipse / line / brush / text |
| `Shift` while drawing | square, circle, 45° line |
| Arrows | nudge 1 px (`Shift` = 10 px) |
| `Delete` / `Backspace` | delete selection |
| `Ctrl+Z` / `Ctrl+Shift+Z` | undo / redo |
| `Ctrl`+wheel | zoom the view |

## Usage

Download `index.html` and double-click it. No install, no build, no network.

## Limits

- Shapes are axis-aligned plus a rotation angle; there's no node/path editing after you draw.
- Freehand strokes are stored as raw point lists — no smoothing or simplification.
- No multi-select, grouping or snapping to other objects.
- Document size is fixed at 960 × 600.

## License

MIT — see [LICENSE](LICENSE).
