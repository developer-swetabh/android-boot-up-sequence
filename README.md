# AOSP / AAOS Boot & Image Architecture — Interactive Reference

A single-page interactive learning resource covering Android's complete boot chain, partition layout, Android Verified Boot, HLOS/Non-HLOS subsystems, kernel-to-init flow, and recovery mode — from junior developer to architect level.

---

## File Structure

```
aosp-docs/
│
├── index.html              ← Main page (self-contained, all CSS + JS inline)
│
├── assets/
│   ├── boot-pulse.json     ← Lottie animation: hero section pulsing boot indicator
│   ├── verified-check.json ← Lottie animation: AVB verified checkmark
│   └── partition-load.json ← Lottie animation: partition loading progress bar
│
└── README.md               ← This file
```

---

## How to Open Locally

### Option 1 — Direct file open (no server needed for most features)

```bash
# macOS
open index.html

# Linux
xdg-open index.html

# Windows
start index.html
```

> ⚠️ **Lottie animations require a local server** (browser security policy blocks `fetch()` from `file://` for JSON assets). All other features (Mermaid diagrams, SVG block diagrams, interactivity) work without a server.

### Option 2 — Local HTTP server (recommended, enables Lottie)

```bash
# Python 3 (built-in)
cd aosp-docs/
python3 -m http.server 8080
# Then open: http://localhost:8080

# Node.js (if installed)
npx serve .
# Then open: http://localhost:3000

# VS Code Live Server extension
# Right-click index.html → "Open with Live Server"
```

---

## Sections Covered

| # | Section | Block Diagram | Sequence Diagram |
|---|---------|--------------|-----------------|
| 1 | Boot-up Flow (PBL → Launcher) | ✅ Inline SVG | ✅ Mermaid |
| 2 | Kernel to Init Flow (populate_rootfs → two-stage init) | ✅ Inline SVG | ✅ Mermaid |
| 3 | Android Verified Boot (AVB 2.0 + RPMB + dm-verity) | ✅ Inline SVG | ✅ Mermaid |
| 4 | Recovery Flow (BCB + VAB recovery) | ✅ Inline SVG | ✅ Mermaid |
| 5 | HLOS & Non-HLOS (SA8155P subsystems + PIL) | ✅ Inline SVG | ✅ Mermaid |
| 6 | Android Images & Partitions (all partitions, interactive) | ✅ Animated map | ✅ Mermaid |
| FAQ | 5 Common Questions | — | — |

---

## Exporting Diagrams

Every diagram has a **⬇ SVG** button in its header toolbar that downloads the diagram as a `.svg` file. You can batch-export all diagrams using the **"⬇ Download All Diagrams"** button in the hero section.

### Converting SVG → PNG at 300 DPI

```bash
# Using Inkscape (recommended)
inkscape --export-png=boot-block-diagram.png --export-dpi=300 boot-block-diagram.svg

# Using rsvg-convert
rsvg-convert -d 300 -p 300 boot-block-diagram.svg -o boot-block-diagram.png

# Using ImageMagick
convert -density 300 boot-block-diagram.svg boot-block-diagram.png
```

---

## Diagram Naming Convention

| Filename | Content |
|----------|---------|
| `boot-block-diagram.svg` | Boot chain layered block diagram (Section 1) |
| `boot-sequence-diagram.svg` | Boot stage handoff sequence (Section 1) |
| `kernel-init-block.svg` | populate_rootfs + two-stage init block (Section 2) |
| `kernel-init-sequence.svg` | Kernel to init detailed sequence (Section 2) |
| `avb-block-diagram.svg` | AVB chain of trust block (Section 3) |
| `avb-sequence.svg` | AVB verification + boot decision sequence (Section 3) |
| `recovery-block-diagram.svg` | Recovery decision architecture block (Section 4) |
| `recovery-sequence.svg` | adb reboot recovery complete sequence (Section 4) |
| `hlos-block-diagram.svg` | Qualcomm SA8155P SoC subsystem layout (Section 5) |
| `hlos-sequence.svg` | NON-HLOS image loading via PIL (Section 5) |
| `partitions-sequence.svg` | Partition load/verification order (Section 6) |

---

## Interactive Features

| Feature | How to Use |
|---------|-----------|
| **Section navigation** | Sticky top nav with section anchors. Active section highlighted automatically. |
| **Diagram zoom** | `＋` / `－` / `⊡` buttons in each diagram header. Click-drag to pan. |
| **Code copy** | `Copy` button on every code block. Shows `✓ Copied` confirmation. |
| **Collapsible sections** | Click any `▶` summary row to expand deep-dive content. |
| **Partition detail** | Click any partition card to expand a full detail panel with debug commands. |
| **Tooltips** | Hover over any <u>amber underlined</u> term or partition timeline block for a tooltip. |
| **Partition timeline** | Scroll to Section 6 — blocks animate in staggered. Hover for description. |
| **FAQ accordion** | Click any FAQ question to expand the answer. |
| **Keyboard navigation** | All interactive controls are focusable via `Tab`. `Enter` activates them. |

---

## Platform Context

All content is based on:
- **Chipset**: Qualcomm SA8155P (Snapdragon Automotive)
- **Android versions**: Android 11–14 (AAOS / AOSP)
- **Architecture**: ARM64 (Cortex-A55/A78), Hexagon QDSP6
- **Boot scheme**: A/B Virtual A/B with `update_engine`
- **Security**: AVB 2.0, dm-verity, RPMB, TrustZone/QSEE
- **GKI**: Generic Kernel Image with stable KMI
- **HAL**: HIDL + AIDL, VNDK/VNDKSP boundaries

---

## Dependencies (CDN — no install required)

| Library | Version | Purpose |
|---------|---------|---------|
| [Mermaid.js](https://mermaid.js.org/) | 10.x | Sequence + flowchart diagrams |
| [Lottie Web](https://airbnb.io/lottie/) | 5.12.2 | Micro-animations (JSON-based) |
| [Google Fonts](https://fonts.google.com/) | — | Orbitron, JetBrains Mono, Outfit |

All loaded from CDN. Works offline if CDN resources are cached. For fully offline use, download these libraries and update the `<script>` / `<link>` src attributes to local paths.

---

## Accessibility

- Semantic HTML5 (`<nav>`, `<section>`, `<footer>`, `<table>`)
- All interactive elements are keyboard-navigable (`tabindex`, `onkeydown`)
- `aria-label`, `aria-expanded`, `aria-live`, `role` attributes on interactive regions
- SVG diagrams have `role="img"` and `aria-label` descriptions
- `.sr-only` class available for screen-reader-only text
- Focus styles via `:focus-visible` (amber outline)

---

## Content Source

All technical content is derived from an in-depth AOSP/AAOS architecture discussion covering:
- Qualcomm boot chain specifics (PBL → XBL → ABL)
- `populate_rootfs()` kernel source analysis
- AVB 2.0 vbmeta structure and Merkle tree verification
- RPMB physical architecture and TrustZone key custody
- BCB (Bootloader Control Block) recovery mechanism
- PIL (Peripheral Image Loader) NON-HLOS subsystem loading
- GKI/GSI partition separation rationale
- Automotive-specific concerns (ISO 26262, <2s camera boot)
