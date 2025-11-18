# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-file birthday gift website with an animated envelope opening experience. The entire project is contained in `index.html` - a self-contained HTML file with embedded CSS and JavaScript.

## Architecture

**Single-File Structure:**
- `index.html` - Contains all HTML markup, CSS styles, and JavaScript logic
- No build process, dependencies, or external files required
- Can be opened directly in a browser

**Key Components:**

1. **Envelope Screen** (`.envelope-screen`)
   - Initial landing screen with greeting text and animated envelope
   - Envelope consists of multiple layered elements:
     - `.envelope-back` - Bottom triangular flap
     - `.envelope-body` - Main rectangular body with side triangular decorations
     - `.letter` - Hidden letter inside the envelope
     - `.envelope-flap` - Top triangular flap (uses clip-path)
     - `.envelope-seal` - Decorative heart seal on the flap

2. **Z-Index Layering System**
   - Critical for realistic envelope opening animation
   - Different z-index values for closed vs opening states:
     - Closed: back(0), letter(2), body(3), flap(4), seal(5)
     - Opening: flap(0), seal(1), body(5), letter(4)
   - The envelope body overlaps the letter during opening to create realistic slide-out effect

3. **Confetti Effect**
   - Triggered 600ms after envelope click
   - Two-phase animation: explosion from center, then falling
   - Uses CSS custom properties (--x-target, --y-target, --rotation) for dynamic animations
   - Multiple shapes: circles, squares, rectangles, and heart emojis

4. **Main Content** (`.main-content`)
   - Hidden initially (opacity: 0, visibility: hidden)
   - Revealed after 2s delay when envelope opens
   - Contains birthday message sections with cards

## Color Palette

**Preserve this exact color scheme:**
- Primary pinks: #ff6b9d, #ff8fab, #ffa6c1, #ffb6c1, #ffc0cb
- Light pinks: #ffe6f0, #fff0f5, #ffeef8
- Dark pink: #c44569
- Accent (sparingly): #d4af37, #ffd700

## Development Workflow

**No build process required** - Simply open `index.html` in a browser to view changes.

**Testing:**
```bash
# Option 1: Open directly
open index.html

# Option 2: Use a local server (if needed for testing)
python3 -m http.server 8000
# Then visit: http://localhost:8000
```

## Important Technical Notes

1. **Envelope Positioning:**
   - The `.envelope-flap` top position is carefully calibrated to `70px` with `height: 100px`
   - The `.envelope-seal` is positioned at `top: 105px` to sit on the connection point
   - These values have been adjusted multiple times for pixel-perfect alignment

2. **Animation Timing:**
   - Envelope opening animation: 0.8s with cubic-bezier(0.68, -0.55, 0.265, 1.55)
   - Confetti explosion: 1.2s for initial burst
   - Confetti fall: 2-4s randomized
   - Screen transition: 2s delay before showing main content

3. **Russian Language:**
   - All user-facing text is in Russian
   - Preserve the language when making changes to text content

4. **Responsive Design:**
   - Mobile breakpoint at 768px
   - Envelope scales down to 280x180px on mobile
   - All proportions and positions adjust accordingly

## Common Modifications

**Changing envelope appearance:**
- Modify envelope colors in `.envelope-body`, `.envelope-flap` gradients
- Adjust size by changing `.envelope` width/height and recalculating child dimensions
- Be careful with z-index changes - they control layer visibility during animations

**Adjusting confetti:**
- Change `confettiCount` in `createConfetti()` function (currently 200)
- Modify `colors` array to add/remove colors (stay within pink palette)
- Adjust explosion `velocity` range (currently 200-600px)

**Modifying timing:**
- Confetti trigger delay: line ~730 `setTimeout(() => createConfetti(), 600)`
- Screen transition: line ~735 `setTimeout(() => {...}, 2000)`
- Animation durations: search for `transition:` and `animation:` in CSS

## Font Dependencies

Uses Google Fonts:
- Playfair Display (headings)
- Cormorant Garamond (body text)

Loaded via CDN in `<head>` section. Internet connection required for fonts to load properly.
