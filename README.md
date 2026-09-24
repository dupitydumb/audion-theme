# Audion Themes Directory & Development Guide

Welcome to the community theme repository for [Audion](https://github.com/dupitydumb/Audion). This repository serves as the official gallery of themes discoverable and installable directly inside the app, as well as via the web browser at [dupitydumb.github.io/audion-theme](https://dupitydumb.github.io/audion-theme).

---

## 🎨 Theme Architecture

An Audion theme is packaged as a `.audiotheme` JSON file conforming to format version `1`.

### Theme Structure Overview
```json
{
  "version": 1,
  "name": "Theme Name",
  "author": "YourName",
  "description": "Short summary of your theme aesthetic.",
  "accentColor": "#38bdf8",
  "mode": "dark",
  "customColors": {
    "bgBase": "#0b0f19",
    "bgElevated": "#111827",
    "bgSurface": "#1f2937",
    "bgHighlight": "#374151",
    "textPrimary": "#f9fafb",
    "textSecondary": "#9ca3af",
    "textSubdued": "#64748b",
    "borderColor": "#1f2937",
    "sidebarBg": "#090d16",
    "playerBg": "#0d1322"
  },
  "background": {
    "type": "gradient",
    "value": "linear-gradient(180deg, #0b0f19 0%, #0d1527 50%, #080c14 100%)",
    "opacity": 1,
    "blur": 0,
    "fixed": true
  },
  "animation": {
    "reducedMotion": false,
    "pageTransition": "fade",
    "playerVisualization": "bars",
    "hoverScale": true,
    "accentPulse": true,
    "transitionSpeed": "normal"
  },
  "customJs": "// Optional JS overlay animation (see below)"
}
```

---

## ⚡ Custom JavaScript Overlay Effects (`customJs`)

Audion provides full visual canvas customization! Themes can supply a `customJs` property containing JavaScript that renders directly onto an overlay `<canvas id="audion-effect-layer">`.

> ⚠️ **User Safety First:** Custom JavaScript executions are disabled by default in Audion. Users can opt-in to execute theme scripts via **Settings > Appearance > "Enable theme custom effects (custom JavaScript)"**.

### Execution Context & Parameters
The script body is evaluated inside an execution function receiving three arguments:
- `canvas`: `HTMLCanvasElement` sized to the full browser viewport with `pointer-events: none`.
- `ctx`: `CanvasRenderingContext2D` of the canvas.
- `accent`: The current accent color hex code (e.g. `"#38bdf8"`).

### Lifecycle & Cleanup Function
**Crucial:** Your script **must return a cleanup function** (`() => void`). Audion executes this cleanup function whenever:
- The user switches themes.
- The theme or effect is disabled.
- The window unmounts.

```javascript
let animId;

function render() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  // ... your animation logic ...
  animId = requestAnimationFrame(render);
}

render();

// Clean up animation loops or event listeners:
return () => {
  cancelAnimationFrame(animId);
};
```

---

## 🛠️ Showcase Examples

Check out our reference implementations in `/themes`:

1. **Midnight Rain (`themes/midnight-rain.audiotheme`)**: Simulates gentle falling raindrops with variable speed, length, and opacity.
2. **Winter Frost (`themes/winter-frost.audiotheme`)**: Renders drifting, swaying snowflakes with sine-wave motion.
3. **Cyber Matrix (`themes/cyber-matrix.audiotheme`)**: Generates classic green terminal Katakana/matrix rain streams.
4. **Liquid Glass (`themes/liquid-glass.audiotheme`)**: Soft radial gradient orbs gliding smoothly across the dark background.

---

## 🚀 Submitting Your Theme

1. **Test your theme:** Use **Settings > Appearance > Theme Package > Import** inside Audion to verify your theme locally.
2. **Submit via Web:** Visit [Theme Submission Page](https://dupitydumb.github.io/audion-theme/submit.html) to validate your JSON and automatically generate a prefilled GitHub Pull Request.
3. **Submit via PR:**
   - Add your `<theme-name>.audiotheme` file into the `themes/` folder.
   - Register it inside `index.json`.
   - Open a pull request!
