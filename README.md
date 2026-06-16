# wojackify

Super simple dummy web tool for putting a Wojak overlay on a user-selected image and downloading the result.

The intended product is deliberately tiny:

- user uploads an image
- app draws the image to a canvas
- app draws the Wojak overlay on top
- user adjusts overlay size/position
- user downloads the final PNG

No backend, no database, no cookies, no auth, no build step, no AI/LLM behavior.

## Current Stack

Everything is in `index.html`.

It uses:

- plain HTML
- inline CSS
- inline JavaScript
- browser Canvas API

The original overlay asset is `wojack.jpg`, but `index.html` currently embeds a pre-keyed transparent PNG version as a base64 `data:image/png` URL so the app can remain one-file and avoid local `file://` image/canvas security issues.

## Current State

The page has:

- image upload input
- size slider
- x position slider
- y position slider
- download button
- canvas preview

The embedded Wojak overlay is already transparent and cropped, so the browser only has to draw it onto the uploaded image.

## Debugging Note

The earlier runtime chroma-key implementation was removed because it was fragile under `file://` canvas constraints and could make the overlay disappear. The current implementation embeds a transparent PNG overlay directly.

## Browser Debugging Note

During the previous session, the bundled Codex Chrome plugin was installed/enabled:

```toml
[plugins."chrome@openai-bundled"]
enabled = true
```

But the active session still did not expose Chrome/browser control tools. The in-app browser/Chrome runtime failed with a Windows sandbox startup error:

```text
windows sandbox failed: runner error: CreateProcessAsUserW failed: 5
```

The next agent should start in a fresh Codex session and try:

```text
Use @chrome to open index.html and debug the Wojak overlay
```

If Chrome tools are available, visually verify:

1. Open `index.html`.
2. Upload a normal PNG/JPG first.
3. Confirm whether the overlay appears.
4. Then try the user's SVG example if available.
5. Inspect console errors.
6. Inspect canvas pixels or temporary offscreen canvas output if needed.

## Useful Files

- `index.html` - full app
- `wojack.jpg` - original green-screen Wojak source asset

## Current End State

- `index.html` only
- embedded transparent PNG overlay
- no runtime chroma-keying needed
