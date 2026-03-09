# Color Palette Reference

Detailed color specifications for academic paper figures. Referenced by [SKILL.md](SKILL.md).

## Contents

- [1. Okabe-Ito Palette](#1-okabe-ito-palette-primary-recommendation)
- [2. Paul Tol Qualitative Palette](#2-paul-tol-qualitative-palette-alternative)
- [3. Sequential Colormaps](#3-sequential-colormaps-for-heatmaps--continuous-data)
- [4. Diverging Colormaps](#4-diverging-colormaps-for-data-centered-at-zero)
- [5. Cybersecurity Domain Color Conventions](#5-cybersecurity-domain-color-conventions)
- [6. Color Assignment Strategy](#6-color-assignment-strategy)
- [7. Supplementary Colors for Figures](#7-supplementary-colors-for-figures)
- [8. CMYK Conversion Notes](#8-cmyk-conversion-notes)
- [9. Color in Tables](#9-color-in-tables)

---

## 1. Okabe-Ito Palette (Primary Recommendation)

A widely used colorblind-safe palette for scientific figures, especially for categorical comparisons.

### Full Specification

| Name | HEX | RGB | Approximate CMYK | LaTeX Name | Visual Role |
|------|-----|-----|-------------------|------------|-------------|
| Blue | #0072B2 | (0, 114, 178) | (100, 36, 0, 30) | `acBlue` | Proposed method / Primary |
| Orange | #E69F00 | (230, 159, 0) | (0, 31, 100, 10) | `acOrange` | Baseline / Warning |
| Bluish Green | #009E73 | (0, 158, 115) | (100, 0, 27, 38) | `acGreen` | Benign / Success |
| Vermillion | #D55E00 | (213, 94, 0) | (0, 56, 100, 16) | `acVermillion` | Attack / Malicious / Alert |
| Reddish Purple | #CC79A7 | (204, 121, 167) | (0, 41, 18, 20) | `acPurple` | Auxiliary |
| Sky Blue | #56B4E9 | (86, 180, 233) | (63, 23, 0, 9) | `acSky` | Secondary / Background |
| Yellow | #F0E442 | (240, 228, 66) | (0, 5, 73, 6) | `acYellow` | Caution: low contrast on white |
| Black | #000000 | (0, 0, 0) | (0, 0, 0, 100) | `acBlack` | Reference / Axis / Text |

### LaTeX Definition Block

```latex
% Okabe-Ito colorblind-safe palette
\definecolor{acBlue}{HTML}{0072B2}
\definecolor{acOrange}{HTML}{E69F00}
\definecolor{acGreen}{HTML}{009E73}
\definecolor{acVermillion}{HTML}{D55E00}
\definecolor{acPurple}{HTML}{CC79A7}
\definecolor{acSky}{HTML}{56B4E9}
\definecolor{acYellow}{HTML}{F0E442}
\definecolor{acBlack}{HTML}{000000}
```

### Python (matplotlib) Definition

```python
OKABE_ITO = {
    'blue':       '#0072B2',
    'orange':     '#E69F00',
    'green':      '#009E73',
    'vermillion': '#D55E00',
    'purple':     '#CC79A7',
    'sky':        '#56B4E9',
    'yellow':     '#F0E442',
    'black':      '#000000',
}
OKABE_ITO_CYCLE = list(OKABE_ITO.values())

import matplotlib as mpl
mpl.rcParams['axes.prop_cycle'] = mpl.cycler(color=OKABE_ITO_CYCLE)
```

### pgfplots Cycle List

```latex
\pgfplotscreateplotcyclelist{okabe-ito}{
  {acBlue, mark=*},
  {acVermillion, mark=square*},
  {acGreen, mark=triangle*},
  {acOrange, mark=diamond*, dashed},
  {acPurple, mark=otimes*, dashed},
  {acSky, mark=star, dotted},
  {acBlack, mark=+, dashdotted},
}
\pgfplotsset{cycle list name=okabe-ito}
```

---

## 2. Paul Tol Qualitative Palette (Alternative)

For papers needing more than 7 colors or a different aesthetic. Verified colorblind-safe.

### Bright Variant (up to 7)

| Color | HEX | LaTeX |
|-------|-----|-------|
| Blue | #4477AA | `\definecolor{tolBlue}{HTML}{4477AA}` |
| Cyan | #66CCEE | `\definecolor{tolCyan}{HTML}{66CCEE}` |
| Green | #228833 | `\definecolor{tolGreen}{HTML}{228833}` |
| Yellow | #CCBB44 | `\definecolor{tolYellow}{HTML}{CCBB44}` |
| Red | #EE6677 | `\definecolor{tolRed}{HTML}{EE6677}` |
| Purple | #AA3377 | `\definecolor{tolPurple}{HTML}{AA3377}` |
| Grey | #BBBBBB | `\definecolor{tolGrey}{HTML}{BBBBBB}` |

### Muted Variant (up to 10)

| Color | HEX |
|-------|-----|
| Indigo | #332288 |
| Cyan | #88CCEE |
| Teal | #44AA99 |
| Green | #117733 |
| Olive | #999933 |
| Sand | #DDCC77 |
| Rose | #CC6677 |
| Wine | #882255 |
| Purple | #AA4499 |
| Pale Grey | #DDDDDD |

---

## 3. Sequential Colormaps (for Heatmaps & Continuous Data)

### Recommended (in order of preference)

| Colormap | When to Use | Colorblind Safe | LaTeX/pgfplots |
|----------|-------------|-----------------|----------------|
| **viridis** | Default for all sequential data | Yes | `colormap/viridis` |
| **cividis** | Strongest colorblind safety | Yes (designed for CVD) | `colormap/cividis` |
| **inferno** | High contrast; dark background emphasis | Yes | `colormap/hot2` (approx) |
| **plasma** | Alternative to viridis | Yes | `colormap/plasma` (custom) |

### Avoid for Quantitative Data

| Colormap | Problem |
|----------|---------|
| jet / rainbow | Non-uniform luminance; can create false visual boundaries |
| hot | Often poor perceptual uniformity for fine-grained comparisons |
| autumn / spring | Low distinction; not perceptually uniform |
| Red-green only encodings | Often inaccessible for readers with red-green CVD |

### pgfplots Custom Viridis Definition

```latex
\pgfplotsset{
  colormap={viridis}{
    rgb255(0cm)=(68,1,84)
    rgb255(1cm)=(72,36,117)
    rgb255(2cm)=(65,68,135)
    rgb255(3cm)=(53,95,141)
    rgb255(4cm)=(42,120,142)
    rgb255(5cm)=(33,145,140)
    rgb255(6cm)=(34,168,132)
    rgb255(7cm)=(68,191,112)
    rgb255(8cm)=(122,209,81)
    rgb255(9cm)=(189,223,38)
    rgb255(10cm)=(253,231,37)
  },
}
```

---

## 4. Diverging Colormaps (for Data Centered at Zero)

| Colormap | When to Use | Center Color |
|----------|-------------|-------------|
| **RdBu** (Red-Blue) | Correlation matrices, positive/negative change | White |
| **PiYG** (Pink-Yellow-Green) | Alternative diverging | White |
| **PRGn** (Purple-Green) | Colorblind-safe diverging | White |

**Rule**: Always set the center point at 0 (or the neutral value). Asymmetric ranges must still be visually centered.

---

## 5. Cybersecurity Domain Color Conventions

Standard semantic color assignments for security papers:

| Concept | Color | HEX | Rationale |
|---------|-------|-----|-----------|
| **Malicious / Attack** | Vermillion or Red | #D55E00 | Universal danger signal |
| **Benign / Normal** | Blue or Green | #0072B2 / #009E73 | Safety/trust association |
| **Suspicious / Anomalous** | Orange | #E69F00 | Warning level |
| **Unknown / Unlabeled** | Grey | #999999 | Neutral / no information |
| **Highlighted / Proposed** | Blue | #0072B2 | Primary emphasis |
| **C2 Communication** | Purple | #CC79A7 | Convention in kill-chain diagrams |
| **Lateral Movement** | Orange | #E69F00 | Internal propagation |
| **Data Exfiltration** | Vermillion | #D55E00 | High severity |
| **Reconnaissance** | Sky Blue | #56B4E9 | Early-stage, lower severity |

### Attack Kill Chain Color Progression

```
Recon (#56B4E9) → Weaponize (#E69F00) → Deliver (#CC79A7) → 
Exploit (#D55E00) → Install (#AA3377) → C2 (#882255) → Exfil (#000000)
```

Use gradient intensity: lighter shades for early stages, darker for later stages.

---

## 6. Color Assignment Strategy

### For Method Comparison Papers

1. **Your method**: Always Slot 1 (Blue, solid, circle) — most visually prominent
2. **Strongest competitor**: Slot 2 (Vermillion) — contrasts with Blue
3. **Other baselines**: Slots 3–6 in order of relevance
4. **Random/oracle reference**: Slot 7 (Black, dash-dot)

### For Ablation Studies

| Variant | Color Strategy |
|---------|---------------|
| Full model | Blue (same as method comparison) |
| w/o Component A | Vermillion (biggest drop highlighted) |
| w/o Component B | Orange |
| w/o Component C | Green |
| Other variants | Remaining palette colors |

### For Multi-Dataset Comparison

Option A: Same method = same color across datasets (preferred)
Option B: Same dataset = same color, different line style per method

**Rule**: Be consistent. Document your mapping and apply it to ALL figures.

---

## 7. Supplementary Colors for Figures

### Bar Chart Fill Colors

For bar charts where you need fill + border:

```latex
\definecolor{acBarFill}{HTML}{4E79A7}   % steel blue, good for bars
\definecolor{acParamLine}{HTML}{E15759} % red accent for overlay lines
```

### Shaded Region Colors

For confidence intervals or error bands:

```latex
% Use palette color with low opacity
fill=acBlue, fill opacity=0.15
fill=acVermillion, fill opacity=0.15
```

### Background / Grid Colors

```latex
\definecolor{acGridGray}{HTML}{D0D0D0}  % major grid
\definecolor{acBgGray}{HTML}{F8F8F8}    % plot background (optional)
```

---

## 8. CMYK Conversion Notes

Some publishers (especially for print journals) require CMYK color space.

**Key issues when converting RGB → CMYK**:
- Bright blues become duller
- Vivid greens shift toward teal
- Screen-bright colors will appear muted in print

**Mitigation**:
1. Test with `\usepackage[cmyk]{xcolor}` (converts all colors)
2. Verify in printed proof, not just screen
3. Okabe-Ito palette was designed with print in mind; conversion is acceptable
4. If using Elsevier / IEEE online-only journal → RGB is fine

---

## 9. Color in Tables

Tables should generally NOT use heavy coloring. Acceptable uses:

| Use Case | Implementation | Caution |
|----------|---------------|---------|
| Row striping (alternating) | `\rowcolors{2}{gray!5}{white}` | Very light; disable for print |
| Header row background | `\cellcolor{gray!15}` | Light gray only |
| Heatmap-style cells | Gradient fill based on value | Must include numeric values too |
| Highlight rows | `\rowcolor{acSky!15}` | Use sparingly for 1–2 key rows |

**Rule**: If the paper will be printed, avoid colored table backgrounds entirely. Numbers + bold/underline are sufficient.

---

## 10. Fill Opacity Guidelines

Opacity (transparency) is critical for overlapping visual elements. Using wrong opacity values is one of the most common figure quality issues.

### By Visual Context

| Context | Opacity Range | Default | LaTeX Code |
|---------|--------------|---------|------------|
| Confidence/error band | 0.10–0.20 | 0.15 | `fill opacity=0.15` |
| Radar chart fill (proposed) | 0.15–0.25 | 0.20 | `fill opacity=0.20` |
| Radar chart fill (baselines) | 0.08–0.15 | 0.10 | `fill opacity=0.10` |
| Bar chart fill | 0.70–1.0 | 0.75 | `fill=acBlue!75` |
| Box plot fill | 0.25–0.35 | 0.30 | `fill=acBlue!30` |
| Scatter plot points | 0.40–0.70 | 0.50 | `opacity=0.5` |
| Stacked area (bottom layer) | 0.70–0.80 | 0.75 | `fill opacity=0.75` |
| Stacked area (top layer) | 0.50–0.65 | 0.60 | `fill opacity=0.60` |
| Legend background (when overlapping data) | 0.80–0.90 | 0.85 | `fill=white, fill opacity=0.85` |
| Zoom region indicator (dashed rect) | border only | — | `draw=acBlack!50, fill=none` |
| Heatmap cells | 1.0 (opaque) | 1.0 | — |

### Overlap Rules

When multiple transparent elements overlap:

| Scenario | Guidance |
|----------|----------|
| 2 overlapping bands | Use opacity ≤ 0.15; combined region should remain readable |
| 3+ overlapping bands | Use opacity ≤ 0.10; consider using only top/bottom fill |
| Scatter with 1000+ points | Opacity 0.3–0.4; rely on density for pattern visibility |
| Scatter with < 200 points | Opacity 0.6–0.8; individual points should be distinguishable |

### pgfplots Opacity Implementation

```latex
% Fill opacity (transparent fill, opaque border)
\addplot[fill=acBlue, fill opacity=0.15, draw=acBlue, line width=1pt] ...;

% Full opacity control (both fill and draw)
\addplot[acBlue, opacity=0.5] ...;  % affects everything

% Scatter with per-point opacity
\addplot[only marks, mark=*, acBlue, opacity=0.5, mark size=1pt] ...;
```

### matplotlib Opacity

```python
# Fill between (confidence band)
ax.fill_between(x, y_lower, y_upper, color='#0072B2', alpha=0.15)

# Scatter
ax.scatter(x, y, c='#0072B2', alpha=0.5, s=10, edgecolors='none')

# Bar
ax.bar(x, y, color='#0072B2', alpha=0.75, edgecolor='#0060A0')
```

---

## 11. Contrast Requirements for Figure Elements

Minimum contrast ratios based on WCAG 2.1 guidelines, adapted for academic figures.

### Text Contrast

| Element | Min Contrast Ratio | Target |
|---------|-------------------|--------|
| Axis labels (dark on white) | 4.5:1 | 7:1 (AAA) |
| Tick labels | 4.5:1 | 7:1 |
| Annotations/callouts | 4.5:1 | 7:1 |
| Legend text | 4.5:1 | 7:1 |
| Heatmap cell text (on dark bg) | 4.5:1 | White text: `#FFFFFF` on dark cells |
| Heatmap cell text (on light bg) | 4.5:1 | Black text: `#000000` on light cells |

### Graphical Object Contrast

| Element | Min Contrast Ratio | Notes |
|---------|-------------------|-------|
| Data lines against background | 3:1 | Thicker lines (1.0+ pt) help |
| Markers against background | 3:1 | Filled markers preferred over hollow |
| Bar fills against background | 3:1 | Use border lines for extra distinction |
| Adjacent bars (different methods) | 3:1 between each pair | Test in grayscale |
| Grid lines vs background | 1.5:1 | Grid should be subtle; too much contrast is distracting |

### Checking Contrast

Tools for verifying contrast ratios:
- **WebAIM Contrast Checker**: https://webaim.org/resources/contrastchecker/
- **Color Oracle** (desktop app): Simulates CVD types
- **Coblis**: https://www.color-blindness.com/coblis-color-blindness-simulator/
- **Manual**: Convert PDF to grayscale; ensure all series remain distinguishable

### Problem Colors on White Background

| Color | HEX | Contrast vs White | Safe? |
|-------|-----|-------------------|-------|
| acBlue | #0072B2 | 5.3:1 | Yes |
| acOrange | #E69F00 | 2.6:1 | **Marginal** — use with thick lines or filled markers |
| acGreen | #009E73 | 3.8:1 | Yes |
| acVermillion | #D55E00 | 3.9:1 | Yes |
| acPurple | #CC79A7 | 3.0:1 | **Marginal** — avoid for thin lines |
| acSky | #56B4E9 | 2.5:1 | **Low** — use filled markers; avoid for thin lines on white |
| acYellow | #F0E442 | 1.3:1 | **Fail** — never use alone on white background |

### Mitigation for Low-Contrast Colors

| Problem Color | Fix |
|---------------|-----|
| acYellow on white | Darken to `#BFA800` or use only as fill with dark border |
| acSky on white | Use filled markers (`mark=*`) not hollow; increase line width to 1.2pt |
| acOrange on white | Use with solid line + filled marker; avoid dashed |
| Any color thin line | Increase line width (1.0→1.2pt) to compensate for reduced contrast |
| Any color on gray bg | Test explicitly; many colors lose contrast on gray |

---

## 12. Color for Statistical Annotations

| Element | Color Rule | Example |
|---------|-----------|---------|
| Error bars | Same color as data series | Blue error bars on blue bars |
| Significance brackets | Black (neutral) | `\draw[thin, black]` |
| Significance text (\*, \*\*) | Black (neutral) | `\node[font=\scriptsize]` |
| Confidence bands | Same color as line, at 15% opacity | `fill=acBlue, fill opacity=0.15` |
| Reference lines (threshold, baseline) | Gray or black, dashed | `acBlack!40, dashed, thin` |
| Callout arrows | Gray or dark, not colored | `draw=black!60, thin` |

**Rule**: Statistical annotations should be visually subordinate to data. Use neutral colors (black/gray) for brackets and text. Only the data itself should carry the palette colors.
