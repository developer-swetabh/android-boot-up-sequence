# AOSP/AAOS Boot & Image Architecture — Phase 2

Phase 2 adds deep technical content, interactive visuals, and accessible callouts to every diagram block.

## What's New in Phase 2

### Every block has a tooltip + deep dive callout
- All 13 boot chain blocks (PBL, XBL, ABL, AVB, TZ, Kernel, Ramdisk, DT, FSI, SSI, Zygote, SystemServer, Launcher) have clickable/focusable tooltips
- Each tooltip shows a 2–3 sentence summary, an optional "Read more →" tab link, and a **"Show deep dive ▼"** toggle that expands rich inline content (code, explanation, architecture notes)
- Keyboard accessible: Tab to focus, Enter/Space to open, Esc to dismiss

### Kernel → Init — dm-linear & dm-verity Animated Pipeline
- Visual 5-layer pipeline: VFS → dm-verity → dm-linear → super → eMMC
- Each layer is hoverable/focusable with descriptions
- **6-step interactive stepper** walking through:
  1. super partition layout + `lpdump` output
  2. LpMetadata extent map + C struct reference
  3. dm-linear sector translation + `dmsetup table` + `fstab.qcom`
  4. dm-verity hash-checked layer + `dmsetup status`
  5. Merkle hash tree visual (new PNG diagram) + build pseudo-code
  6. Failure modes, `dmesg` output, init response

### New Diagrams (300 DPI PNGs)
- **merkle-tree.png** — SHA-256 leaf/parent/root structure, verification path highlighted, O(log N) annotation
- **bcb-visual.png** — misc partition layout, BCB fields shown as colored strips with byte offsets and descriptions

### Kernel → Init — init.rc Deep Dive
- Annotated code snippets for: early-init, init trigger, class core (servicemanager + logd), class hal (display + audio), class main (zygote)
- Two adb debug commands with sample output per section
- Sequence diagram showing class_start ordering guarantee

### AVB — Improved Layout + Interactive Verification Flow
- Chain of Trust block diagram (larger fonts, better spacing)
- Merkle tree PNG embedded in both AVB and Kernel→Init tabs
- libavb verification pseudo-code walkthrough (6-step with RPMB logic)
- avbtool example commands with realistic sample output
- Boot state table (GREEN / YELLOW / ORANGE / RED)
- Collapsible vbmeta structure + avbtool sign/verify commands

### Recovery — BCB Visual + system/bin/recovery Deep Dive
- **bcb-visual.png** — misc partition offset strip diagram (new)
- Interactive BCB field reference table with hex offsets
- Read/write/verify adb commands with hexdump samples
- Full lifecycle sequence diagram
- system/bin/recovery capabilities: OTA install, wipe, sideload, RecoveryUI (framebuffer-only)
- Example recovery.log output with realistic timestamps

### General Polish
- All diagrams: 300 DPI, consistent large fonts
- No chipset-specific references
- Lazy Mermaid rendering per tab (render only when activated)
- Mermaid error fallback: styled error + raw source in expandable `<details>`
- Reduce Motion toggle respects `prefers-reduced-motion`
- All interactive elements have ARIA labels, tabindex, focus-visible outlines
- Captions under every diagram
- Copy-to-clipboard on all code blocks

## File Structure

```
aosp-fix2/
├── index.html                    (all CSS + JS inline, ~125 KB)
├── README.md
└── assets/
    ├── diagrams/
    │   ├── boot-flow-block.png       (300 DPI — larger fonts)
    │   ├── boot-flow-sequence.png    (300 DPI)
    │   ├── kernel-init-block.png     (300 DPI — larger fonts)
    │   ├── avb-block.png             (300 DPI — improved layout)
    │   ├── recovery-block.png        (300 DPI — larger fonts)
    │   ├── hlos-block.png            (300 DPI — larger fonts)
    │   ├── images-block.png          (300 DPI)
    │   ├── merkle-tree.png           (300 DPI — NEW)
    │   └── bcb-visual.png            (300 DPI — NEW)
    └── lottie/
        └── boot-pulse.json
```

## How to Open

```bash
# Recommended — local HTTP server (required for Lottie assets and font loading)
cd aosp-fix2/
python3 -m http.server 8080
# → open http://localhost:8080

# Direct file open also works (most features except Lottie)
open index.html
```

## Testing Checklist — Phase 2 Acceptance Criteria

### 1. Every block has an accessible tooltip + deep dive
- Boot Flow tab → click **PBL** → tooltip shows title + body + "Show deep dive ▼"
- Click "Show deep dive ▼" → inline content expands with architecture notes
- Click **XBL**, **ABL**, **AVB**, **TrustZone** → each shows different content
- Tab to any node → Enter opens tooltip → Esc closes
- Click ✕ button → tooltip closes

### 2. Kernel → Init — dm-linear + dm-verity
- Kernel→Init tab → 5-layer pipeline visible (VFS → eMMC)
- 6-step stepper: click Next/Prev → content changes, progress bar updates
- Step 5 shows Merkle tree PNG diagram inline

### 3. Kernel → Init — init.rc deep dive
- init.rc section shows 5 stage badges: early-init, init, core, hal, main
- Each has annotated code snippet with Copy button
- adb debug commands present with sample output
- Sequence diagram Fig 2.4 renders

### 4. AVB — chain of trust + Merkle + avbtool
- AVB tab → block diagram visible + zoomable
- Merkle tree PNG shown in Fig 3.2
- Collapsible "How ABL Verifies vbmeta" → libavb pseudo-code
- Collapsible "vbmeta Structure" → avbtool commands with sample output
- Boot state table (GREEN/YELLOW/ORANGE/RED) visible

### 5. Recovery — BCB visual + system/bin/recovery
- Recovery tab → bcb-visual.png shown (colorful strip diagram)
- BCB field table with hex offsets
- adb read/write/verify commands
- Collapsible recovery capabilities with example log output

### 6. Fonts readable, all diagrams fit
- All block diagrams legible at 1280×800 without zooming
- Zoom +/- buttons work on block diagrams

### 7. Accessibility
- All interactive elements reachable via keyboard
- Esc closes tooltip from any focused state
- Copy buttons confirm with "✓ Copied"
- Mermaid diagrams show styled error if CDN unavailable
