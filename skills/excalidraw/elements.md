# Excalidraw Element Templates

All elements go inside the `elements` array. Each element needs a unique `id` and an `index` (sequential: `"a0"`, `"a1"`, `"a2"`, ...).

## Root file structure

```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://marketplace.visualstudio.com/items?itemName=pomdtr.excalidraw-editor",
  "elements": [],
  "appState": { "gridSize": null, "viewBackgroundColor": "#ffffff" },
  "files": {}
}
```

---

## Rectangle with label (standard box)

Text inside a shape requires two linked elements: the shape and a contained text element. Always use this pattern — never a standalone text element inside a shape. Always center the text inside the shape.

```json
{
  "id": "box-1", "type": "rectangle",
  "x": 280, "y": 60, "width": 400, "height": 65,
  "angle": 0, "strokeColor": "#1e1e1e", "backgroundColor": "transparent",
  "fillStyle": "solid", "strokeWidth": 2, "strokeStyle": "solid",
  "roughness": 1, "opacity": 100, "groupIds": [], "frameId": null,
  "index": "a0", "roundness": {"type": 3}, "seed": 1, "version": 1, "versionNonce": 1,
  "isDeleted": false, "boundElements": [{"type": "text", "id": "box-1-label"}],
  "updated": 1, "link": null, "locked": false
},
{
  "id": "box-1-label", "type": "text",
  "x": 280, "y": 76, "width": 400, "height": 33,
  "angle": 0, "strokeColor": "#1e1e1e", "backgroundColor": "transparent",
  "fillStyle": "solid", "strokeWidth": 1, "strokeStyle": "solid",
  "roughness": 1, "opacity": 100, "groupIds": [], "frameId": null,
  "index": "a1", "roundness": null, "seed": 2, "version": 1, "versionNonce": 2,
  "isDeleted": false, "boundElements": null, "updated": 1, "link": null, "locked": false,
  "text": "Label text", "fontSize": 16, "fontFamily": 1,
  "textAlign": "center", "verticalAlign": "middle",
  "containerId": "box-1", "originalText": "Label text",
  "autoResize": true, "lineHeight": 1.25
}
```

The label's `x`/`y` should match the shape's `x`/`y`, and `width`/`height` should match the shape's dimensions.

---

## Diamond with label (decision node)

Same two-element pattern as rectangle:

```json
{
  "id": "decision-1", "type": "diamond",
  "x": 280, "y": 200, "width": 200, "height": 100,
  "angle": 0, "strokeColor": "#1e1e1e", "backgroundColor": "transparent",
  "fillStyle": "solid", "strokeWidth": 2, "strokeStyle": "solid",
  "roughness": 1, "opacity": 100, "groupIds": [], "frameId": null,
  "index": "a2", "roundness": {"type": 2}, "seed": 3, "version": 1, "versionNonce": 3,
  "isDeleted": false, "boundElements": [{"type": "text", "id": "decision-1-label"}],
  "updated": 1, "link": null, "locked": false
},
{
  "id": "decision-1-label", "type": "text",
  "x": 280, "y": 200, "width": 200, "height": 100,
  "angle": 0, "strokeColor": "#1e1e1e", "backgroundColor": "transparent",
  "fillStyle": "solid", "strokeWidth": 1, "strokeStyle": "solid",
  "roughness": 1, "opacity": 100, "groupIds": [], "frameId": null,
  "index": "a3", "roundness": null, "seed": 4, "version": 1, "versionNonce": 4,
  "isDeleted": false, "boundElements": null, "updated": 1, "link": null, "locked": false,
  "text": "Decision?", "fontSize": 16, "fontFamily": 1,
  "textAlign": "center", "verticalAlign": "middle",
  "containerId": "decision-1", "originalText": "Decision?",
  "autoResize": true, "lineHeight": 1.25
}
```

---

## Arrow

Arrows are positioned manually — do not use `startBinding`/`endBinding`. Set `points` relative to the arrow's own `x`/`y` origin.

```json
{
  "id": "arrow-1", "type": "arrow",
  "x": 480, "y": 125, "width": 0, "height": 60,
  "angle": 0, "strokeColor": "#1e1e1e", "backgroundColor": "transparent",
  "fillStyle": "solid", "strokeWidth": 2, "strokeStyle": "solid",
  "roughness": 1, "opacity": 100, "groupIds": [], "frameId": null,
  "index": "a4", "roundness": {"type": 2}, "seed": 5, "version": 1, "versionNonce": 5,
  "isDeleted": false, "boundElements": null, "updated": 1, "link": null, "locked": false,
  "points": [[0, 0], [0, 60]], "lastCommittedPoint": null,
  "startBinding": null, "endBinding": null,
  "startArrowhead": null, "endArrowhead": "arrow", "elbowed": false
}
```

For a vertical arrow going down: `points: [[0,0],[0,60]]`, `width: 0`, `height: 60`.
For a horizontal arrow going right: `points: [[0,0],[80,0]]`, `width: 80`, `height: 0`.

---

## Standalone text (titles, annotations)

Use only for labels that float outside shapes (e.g. diagram title, step annotations):

```json
{
  "id": "title", "type": "text",
  "x": 280, "y": 20, "width": 400, "height": 22,
  "angle": 0, "strokeColor": "#1e1e1e", "backgroundColor": "transparent",
  "fillStyle": "solid", "strokeWidth": 1, "strokeStyle": "solid",
  "roughness": 1, "opacity": 100, "groupIds": [], "frameId": null,
  "index": "a5", "roundness": null, "seed": 6, "version": 1, "versionNonce": 6,
  "isDeleted": false, "boundElements": null, "updated": 1, "link": null, "locked": false,
  "text": "Diagram Title", "fontSize": 20, "fontFamily": 1,
  "textAlign": "center", "verticalAlign": "middle",
  "containerId": null, "originalText": "Diagram Title",
  "autoResize": true, "lineHeight": 1.25
}
```

---

## Notes

- `index` must be sequential across all elements: `"a0"`, `"a1"`, `"a2"`, ...
- Every `id` must be unique — use descriptive strings like `"parse-step"`, `"parse-step-label"`
- Arrow `points` are relative to the arrow's own `x`/`y` — not absolute coordinates
- `seed` and `versionNonce` can be any integer — use sequential integers for simplicity
- Always use `"fontFamily": 1` — this is Excalifont, the default Excalidraw handwritten font
- Font sizes: Small = 16 (labels inside shapes), Medium = 20 (titles, standalone text), Large = 28, Extra Large = 36
