---
name: paper-figure-styling
description: Comprehensive figure styling, color scheme, typography, axis/grid design, aspect ratio, legend system, statistical visualization, layout, table design, caption formatting, cross-reference audit, cross-figure consistency, export quality pipeline, hyperlink color policy, and cross-publisher adaptation (Elsevier, IEEE, Springer, ACM, USENIX). Covers colorblind-safe palettes, triple visual encoding (color+line+marker), all figure types (line, bar, heatmap, confusion matrix, ROC/PR, box/violin, radar, scatter, architecture, network graph, Sankey, timeline, waterfall, area), error bars (SD/SEM/CI), significance markers, shared legends for multi-panel figures, broken axis, inset zoom, and end-to-end export pipeline from code to camera-ready PDF. Use when creating figures, styling plots, choosing colors, designing legends, formatting tables, checking figure/table references, preparing visual elements for journal or conference submission, 配色, 排版, 图表, 图例, 字体, 颜色, 坐标轴, 误差线, 导出.
---

# Paper Figure Styling & Visual Design

Systematic quality guide for all visual elements in academic papers. Applies to cybersecurity venues (S&P, CCS, USENIX Security, NDSS, TDSC, TIFS, Computers & Security) and general CS journals.

## Execution Safety [CRITICAL]

Before applying style changes:

- Do not change reported values, statistical results, labels, or ranking order.
- Do not alter figure semantics to "look better" (for example, axis range tricks that hide variance).
- Keep figure/table numbering and cross-references stable unless user explicitly requests renumbering.
- Preserve reproducibility: any style adjustment should be scriptable (LaTeX/pgfplots/matplotlib config), not manual one-off edits.

## Workflow

```
Figure Styling Progress:
- [ ] Phase 1: Color System Setup (palette + semantic mapping + fill opacity)
- [ ] Phase 2: Typography Audit (figure fonts, table fonts, sizes, math mode)
- [ ] Phase 3: Axis, Grid & Proportion System (ticks, grid, aspect ratio, data-ink)
- [ ] Phase 4: Visual Encoding System (color + line style + marker)
- [ ] Phase 5: Per-Figure-Type Review (with aspect ratio per type)
- [ ] Phase 6: Legend Design System (position, styling, shared legend, decision tree)
- [ ] Phase 7: Statistical Visualization (error bars, significance markers, CI bands)
- [ ] Phase 8: Layout, Float Placement & Page Density
- [ ] Phase 9: Table Visual Design
- [ ] Phase 10: Caption & Label Formatting
- [ ] Phase 11: Cross-Reference & Hyperlink Audit
- [ ] Phase 12: Cross-Figure Consistency Audit
- [ ] Phase 13: Export Quality Pipeline
- [ ] Phase 14: Grayscale & Accessibility Test
- [ ] Phase 15: Publisher Compliance Final Check
```

---

## Phase 1: Color System

### Default Palette: Okabe-Ito (Wong)

```latex
\definecolor{acBlue}{HTML}{0072B2}       % proposed method / primary
\definecolor{acOrange}{HTML}{E69F00}      % baseline 3
\definecolor{acGreen}{HTML}{009E73}       % baseline 2 / benign class
\definecolor{acVermillion}{HTML}{D55E00}  % strongest baseline / attack class
\definecolor{acPurple}{HTML}{CC79A7}      % baseline 4
\definecolor{acSky}{HTML}{56B4E9}         % baseline 5 / auxiliary
\definecolor{acYellow}{HTML}{F0E442}      % caution: low contrast on white
\definecolor{acBlack}{HTML}{000000}       % reference / oracle / axis
```

### Rules

1. **Semantic lock**: Same entity = same color in ALL figures (e.g., F1 is always acBlue)
2. **Max 7 colors** per figure; split into subfigures if more needed
3. **Security domain**: vermillion/red → malicious/attack; blue/green → benign/normal
4. **Sequential data** (heatmaps): viridis, cividis, or inferno — NEVER jet/rainbow
5. **Diverging data**: RdBu or PiYG (centered at 0)
6. **Your method** always gets the most prominent color (acBlue) + solid line

### Fill Opacity Guidelines

| Context | Opacity | Example |
|---------|---------|---------|
| Confidence/error band | 0.10–0.20 | `fill opacity=0.15` |
| Radar chart fill | 0.15–0.25 | Proposed: 0.20; baselines: 0.10 |
| Bar chart fill (solid) | 0.75–1.0 | `fill=acBlue!75` with darker border |
| Box plot fill | 0.25–0.35 | `fill=acBlue!30` |
| Scatter plot points | 0.40–0.70 | Dense data: lower; sparse: higher |
| Stacked area | 0.60–0.80 | Bottom layer highest; top lowest |
| Legend background (if needed) | 0.80–0.90 | `fill=white, fill opacity=0.85` |

**Rule**: When multiple semi-transparent fills overlap, reduce opacity so combined regions remain readable. Test with 3+ overlapping bands at chosen opacity.

### Hyperlink & Cross-Reference Colors

```latex
\usepackage[colorlinks=true,
  linkcolor=acBlue!80!black,   % internal refs (Fig., Table, Eq.)
  citecolor=acGreen!70!black,  % citation numbers/author-year
  urlcolor=acPurple!80!black,  % URLs
]{hyperref}
```

**Rule**: Colored links are often useful in review drafts. For camera-ready, follow venue policy exactly; some pipelines (for example IEEE PDF eXpress) require no active links/bookmarks. See [publisher-specs.md](publisher-specs.md) for per-publisher policy.

For detailed palettes (Paul Tol, sequential, diverging, CMYK), see [color-reference.md](color-reference.md).

---

## Phase 2: Typography

### Figure Text — Font Family

Prefer clean, highly legible figure fonts (often sans-serif). If the venue template or style guide requires matching the document font, follow the venue rule.

```latex
\pgfplotsset{
  every axis/.append style={
    font=\sffamily\small,
    label style={font=\sffamily\small},
    tick label style={font=\sffamily\footnotesize},
    legend style={font=\sffamily\scriptsize},
    title style={font=\sffamily\small\bfseries},
  },
}
```

For TikZ architecture diagrams:
```latex
\begin{tikzpicture}[font=\sffamily\footnotesize, ...]
```

### Font Size Constraints

| Element | Minimum | Recommended | Context |
|---------|---------|-------------|---------|
| Axis labels | 7 pt | 8–9 pt | `\small` or `\footnotesize` |
| Tick labels | 6 pt | 7–8 pt | `\footnotesize` or `\scriptsize` |
| Legend text | 6 pt | 7–8 pt | `\scriptsize` |
| Panel labels (a)(b) | 8 pt bold | 8–10 pt bold | `\small\bfseries` |
| Annotations in figure | 6 pt | 7 pt | `\scriptsize` |
| Data value labels | 6 pt | 6–7 pt | `\scriptsize` or `\tiny` (edge case) |

**CRITICAL**: Avoid `\tiny` unless the venue explicitly permits very small labels. A practical target is >= 7pt at final print size. Nature caps figure text at 5–7pt; most CS venues recommend >= 7pt.

### Table Text — Font Requirements

| Element | Font | Size | Style |
|---------|------|------|-------|
| Column headers | Same as body or sans-serif | `\small` | `\textbf{}` (bold) |
| Body text | Document main font | `\small` or `\footnotesize` | regular |
| Table notes | Document main font | `\footnotesize` or `\scriptsize` | regular |
| Best result | Document main font | same as body | `\textbf{}` |
| Second-best | Document main font | same as body | `\underline{}` (optional) |

**Rule**: Tables use the document's main font (serif for most templates). Figures use sans-serif. This is the standard convention.

### Font Consistency Across Paper

| Check | Rule |
|-------|------|
| All figures use same family | Sans-serif throughout (or as venue requires) |
| All figures use same size scale | Axis labels all `\small`, ticks all `\footnotesize` |
| Math mode in figures | Use `$...$` for Greek/math; renders with document math font |
| No mixed fonts | Do not mix Arial in one figure and Helvetica in another |
| Matplotlib + LaTeX match | Use PGF backend or set `font.family` to match template |

### Font Embedding

XeLaTeX/LuaLaTeX: fonts auto-embedded. pdfLaTeX: add to preamble:
```latex
\usepackage[T1]{fontenc}  % ensures font embedding
```
For matplotlib exports: `plt.rcParams['pdf.fonttype'] = 42`

---

## Phase 3: Axis, Grid & Proportion System

### 3a. Axis Line Styling

| Element | Default | pgfplots Code |
|---------|---------|---------------|
| Axis line width | 0.5 pt | `axis line style={line width=0.5pt}` |
| Axis color | Black | `axis line style={black}` |
| Two-sided frame (box) | Full box | Default pgfplots behavior |
| L-shaped frame (Nature/Science style) | Left + bottom only | `axis x line=bottom, axis y line=left` |

**Venue guidance**:
- Nature/Science: L-shaped frame (no top/right border) is preferred
- IEEE/ACM: Full box is acceptable; L-shaped also welcome
- When in doubt, use L-shaped — it reduces non-data ink

```latex
% Nature-style clean axes
axis x line=bottom,
axis y line=left,
axis line style={line width=0.5pt},
```

### 3b. Tick Mark Design

| Property | Academic Standard | pgfplots Code |
|----------|------------------|---------------|
| Direction | **Outward** (Nature/Science default) | `tick align=outside` |
| Major tick length | 3–4 pt | `major tick length=4pt` |
| Minor tick length | 2 pt | `minor tick length=2pt` |
| Minor ticks | 4 between majors (when needed) | `minor tick num=4` |
| Tick width | Same as axis line | inherits from axis style |

```latex
tick align=outside,
major tick length=4pt,
minor tick length=2pt,
```

**When to use minor ticks**: Only when readers need to read precise intermediate values. Omit for categorical axes and when grid lines serve the purpose.

### 3c. Grid Design

| Scenario | Use Grid? | Style |
|----------|-----------|-------|
| Line chart (method comparison) | YES — major only | `dashed, gray!25, line width=0.3pt` |
| Scatter plot | YES — major only | Same as above |
| Bar chart | Y-axis major only | `ymajorgrids=true, xmajorgrids=false` |
| ROC / PR curve | YES — major only | Subtle for value reading |
| Confusion matrix / heatmap | NO | Grid implied by cell borders |
| Architecture / pipeline diagram | NO | Not a data plot |
| Nature papers | **NO** | Nature guidelines: "avoid background gridlines" |

```latex
% Standard academic grid
grid=major,
grid style={dashed, gray!25, line width=0.3pt},

% Bar chart: Y-grid only
ymajorgrids=true,
xmajorgrids=false,
grid style={dashed, gray!15, line width=0.3pt},

% Nature-style: no grid
grid=none,
```

### 3d. Aspect Ratio Guidelines

| Chart Type | Recommended Ratio (W:H) | pgfplots | Rationale |
|------------|------------------------|----------|-----------|
| Line chart (default) | ~1.6:1 (golden) | `height=0.618\columnwidth` | Balanced; good for trend visualization |
| ROC / PR curve | 1:1 (square) | `height=\columnwidth` | Both axes share [0,1] domain |
| Confusion matrix | 1:1 (square) | `height=0.8\columnwidth` | Symmetric matrix |
| Bar chart | 1.6:1 to 2:1 | `height=0.5\columnwidth` | Wider emphasizes comparison |
| Radar / spider | 1:1 | `height=0.8\columnwidth` | Radial symmetry |
| Architecture diagram | Content-driven | Flexible | Fits the pipeline flow |
| Scatter / t-SNE | 1:1 or data-driven | Match axis ranges | Preserve distance relationships |
| Timeline | 3:1 to 4:1 | `height=0.3\columnwidth` | Emphasizes temporal extent |
| Dual-axis | ~1.6:1 | `height=0.6\columnwidth` | Same as line chart |
| Box / violin | 1.4:1 to 1.8:1 | `height=0.55\columnwidth` | Depends on number of categories |

```latex
% Golden ratio (default for most charts)
width=\columnwidth, height=0.618\columnwidth,

% Square (ROC, confusion matrix, scatter)
width=0.8\columnwidth, height=0.8\columnwidth,

% Wide (bar chart, timeline)
width=\columnwidth, height=0.5\columnwidth,
```

### 3e. Data-Ink Ratio Principles (Tufte)

Maximize the proportion of ink devoted to data. Remove non-data elements that do not aid comprehension.

| Action | Before | After |
|--------|--------|-------|
| Remove frame box | Full 4-sided frame | L-shaped (left + bottom only) |
| Lighten grid | Dark solid grid | Light dashed gray or no grid |
| Remove background | Gray/colored background | White background |
| Simplify ticks | Ticks on all 4 sides | Ticks on left + bottom only |
| Avoid 3D effects | 3D bar chart | **Always** use 2D for 2D data |
| Remove redundant labels | "%" on each tick + "(%)" in axis label | Unit in axis label only |

### 3f. Axis Range & Truncation

| Rule | Line Chart | Bar Chart |
|------|-----------|-----------|
| Y-axis start at 0? | NO — truncate to data range (e.g., 85–100%) | **YES** — truncating exaggerates differences |
| Truncated bars | — | Use broken/discontinuous axis (see [figure-types.md](figure-types.md) §21) |
| X-axis | Natural domain of parameter | Categorical — auto |
| Padding | 2–5% beyond data extremes | Auto from pgfplots `enlarge x limits` |

**Broken axis**: When bar chart data has a large gap (e.g., one method at 50%, others at 90–99%), use a discontinuous Y-axis with a visual break marker. See template in [figure-types.md](figure-types.md).

### 3g. Axis Label Format

| Variable Type | Label Format | Example |
|---------------|-------------|---------|
| Continuous with unit | Name (Unit) | `F1 Score (\%)` |
| Categorical | Name only | `Method` |
| Dimensionless ratio | Name | `Compression Ratio` |
| Log-scale axis | Name (log scale) | `$\lambda$ (log scale)` |
| Time | Name (unit) | `Training Time (s)` |
| Count | Name | `Number of Edges` |

**Rule**: Parentheses for units: `(%)`, `(s)`, `(MB)`. Do not use brackets `[%]` unless the venue convention requires it (some physics journals use brackets).

---

## Phase 4: Triple Encoding — Color + Line Style + Marker

**Rule**: Use at least two independent channels (for example color + marker, or color + line style). For high-stakes comparison plots, use triple encoding (color + line style + marker).

### Method Comparison Encoding Table

| Slot | Role | Color | Line | Marker | pgfplots |
|------|------|-------|------|--------|----------|
| 1 | **Your method** | acBlue | solid | ● `*` | `mark=*, acBlue, line width=1.2pt` |
| 2 | Baseline A | acVermillion | solid | ■ `square*` | `mark=square*, acVermillion` |
| 3 | Baseline B | acGreen | solid | ▲ `triangle*` | `mark=triangle*, acGreen` |
| 4 | Baseline C | acOrange | dashed | ◆ `diamond*` | `mark=diamond*, acOrange, dashed` |
| 5 | Baseline D | acPurple | dashed | ⊗ `otimes*` | `mark=otimes*, acPurple, dashed` |
| 6 | Baseline E | acSky | dotted | ★ `star` | `mark=star, acSky, dotted` |
| 7 | Reference | acBlack | dash-dot | + `+` | `mark=+, acBlack, dashdotted` |

### Metric Comparison Encoding Table (Same-Method, Multiple Metrics)

| Metric Type | Line | Marker | Semantic |
|-------------|------|--------|----------|
| Classification (F1, Prec, Recall, Acc) | solid | filled shapes | Direct decision metrics |
| Ranking (AUROC, AUPRC) | dashed | hollow or special shapes | Threshold-independent |
| Correlation (MCC, Kappa) | densely dashed | cross marks | Agreement metrics |

### Dimensions

| Element | Value | Range |
|---------|-------|-------|
| Data line width | 1.0–1.2 pt | 0.8–1.5 pt |
| Marker size | 2.0–2.5 pt | 1.5–3.0 pt |
| Reference line | 0.4 pt | 0.3–0.5 pt |
| Axis line | 0.5 pt | 0.4–0.6 pt |
| Grid line | 0.3 pt | 0.2–0.4 pt |

---

## Phase 5: Per-Figure-Type Quick Checklist

### Aspect Ratio Quick Reference (Per Type)

| Type | W:H | pgfplots height |
|------|-----|----------------|
| Line / Sensitivity | 1.6:1 | `0.618\columnwidth` |
| Bar / Ablation | 1.8:1 | `0.55\columnwidth` |
| ROC / PR | 1:1 | `\columnwidth` (square) |
| Heatmap / Confusion | 1:1 | `0.8\columnwidth` |
| Radar | 1:1 | `0.8\columnwidth` |
| Box / Violin | 1.5:1 | `0.6\columnwidth` |
| Scatter / t-SNE | 1:1 | `\columnwidth` |
| Dual-axis | 1.6:1 | `0.6\columnwidth` |
| Architecture | flexible | content-driven |

### Line Charts (Sensitivity / Convergence / ROC / PR)
- [ ] Triple encoding applied
- [ ] Y-axis starts from a meaningful value (not always 0 — truncate if data is 85–100%)
- [ ] Grid: major only, `dashed, gray!30`
- [ ] X-axis log scale if values span orders of magnitude
- [ ] Zoomed inset if curves cluster in a small region
- [ ] Tick marks outward; L-shaped axis frame for Nature-style

### Bar Charts (Comparison / Ablation / Grouped / Stacked)
- [ ] Your method highlighted (most prominent color or outlined)
- [ ] Value labels present if ≤ 8 bars; omit if cluttered
- [ ] Error bars (std or SEM) if reporting averaged results
- [ ] Hatching patterns for grayscale fallback in grouped bars
- [ ] Consistent bar width across all bar-chart figures
- [ ] Horizontal bars if category labels are long
- [ ] Y-axis starts at 0 (or use broken axis if necessary)

### Heatmaps & Confusion Matrices
- [ ] Colormap: viridis (default) / cividis (strongest colorblind safety)
- [ ] Cell annotations: white text on dark, black on light (auto-threshold)
- [ ] Color bar present with label
- [ ] Axis labels = class names, not indices
- [ ] Normalized confusion matrix (row-normalized or total-normalized)

### ROC Curves
- [ ] **Square** aspect ratio (mandatory)
- [ ] Diagonal reference line (dashed gray, label: "Random")
- [ ] AUC value in legend: `YourMethod (AUC = 0.995)`
- [ ] Both axes [0, 1]
- [ ] Zoomed inset near (0, 1) corner if curves overlap

### Precision-Recall Curves
- [ ] **Square** aspect ratio
- [ ] Baseline: horizontal line at class prevalence
- [ ] AUPRC value in legend
- [ ] Optional: iso-F1 contour lines

### Box Plots & Violin Plots
- [ ] Median line clearly visible (thick or colored)
- [ ] Individual points overlaid (jittered) if n ≤ 30
- [ ] Color by method (consistent with color system)
- [ ] Whiskers: 1.5×IQR standard

### Scatter Plots (t-SNE / UMAP / Feature Space)
- [ ] **Square** aspect ratio
- [ ] Different marker shapes per class (not just color)
- [ ] Alpha transparency 0.4–0.7 for overlap
- [ ] No axis ticks/labels for t-SNE/UMAP (dimensionless)
- [ ] Legend: class name + count

### Radar / Spider Charts
- [ ] **Square** frame (1:1)
- [ ] 5–8 axes maximum
- [ ] All axes same scale [0, 100] or [0, 1]
- [ ] Fill alpha = 0.15–0.25; outline solid for proposed, dashed for baselines
- [ ] Axis labels outside the polygon

### Architecture / Pipeline Diagrams (TikZ)
- [ ] Max 2–3 accent colors + neutral (gray/black)
- [ ] Consistent box style: same `rounded corners`, same `line width`
- [ ] Arrow direction = data flow (left→right or top→bottom)
- [ ] All text in English (NO hard-coded Chinese in submission)
- [ ] Font: sans-serif, minimum 7pt
- [ ] Width fits column constraint

### Network / Graph Visualizations
- [ ] Node color by type (legend required)
- [ ] Node size by degree or importance
- [ ] Edge thickness by weight or confidence
- [ ] Force-directed or hierarchical layout (not random)

### Timeline / Attack Chain Diagrams
- [ ] Time axis clearly labeled with units
- [ ] Attack phases color-coded (recon→exploit→C2→exfil)
- [ ] Arrows show causal relationships
- [ ] Duration represented by segment length (proportional)

### Waterfall / Incremental Contribution Charts
- [ ] Positive increments one color; negative decrements another
- [ ] Connecting lines between bars
- [ ] Total bar visually distinct (darker or outlined)
- [ ] Labels on each segment showing delta value

### Stacked Area Charts
- [ ] Layer order: largest at bottom
- [ ] Semi-transparent fills (alpha 0.6–0.8)
- [ ] Legend order matches visual stacking order

For per-type pgfplots/matplotlib templates, see [figure-types.md](figure-types.md).

---

## Phase 6: Legend Design System

### 6a. Legend Position Decision Tree

```
Is the plot area spacious with a clear empty corner?
├── YES → Place legend INSIDE the plot
│         ├── Data dense at top-right → at={(0.03,0.97)}, anchor=north west
│         ├── Data dense at top-left → at={(0.97,0.97)}, anchor=north east
│         ├── Data spans full area → Move legend OUTSIDE (see below)
│         └── Default (most cases) → at={(0.97,0.03)}, anchor=south east
├── NO → Place legend OUTSIDE the plot
│         ├── Single plot, ≤5 items → Below: at={(0.5,-0.22)}, anchor=north
│         ├── Single plot, 6–8 items → Below, multi-row: at={(0.5,-0.30)}, columns=3
│         ├── Multi-panel figure → Shared legend (see §6d)
│         └── Tall narrow plot → Right side: at={(1.05,0.5)}, anchor=west
└── SPECIAL CASES:
    ├── ≤ 2 series → Direct labels on lines (no legend box)
    ├── 1 series → State in caption; no legend needed
    ├── ROC curves → Inside, near bottom-right (curves cluster top-left)
    └── PR curves → Inside, near bottom-left or top-right
```

### 6b. Legend Styling System

| Property | Academic Default | pgfplots Code |
|----------|-----------------|---------------|
| Background | Transparent | `fill=none` |
| Border | None (modern convention) | `draw=none` |
| Internal padding | 2–3 pt | `inner sep=2pt` |
| Corner radius | None | `rounded corners=0pt` |
| Shadow | **Never** in academic figures | — |
| Column count | Auto from item count | `legend columns=N` |
| Column separation | ≥ 4 pt (6 pt preferred) | `column sep=6pt` |
| Row separation | 2–3 pt | `row sep=2pt` |
| Horizontal entry spacing | Even | `/tikz/every even column/.append style={column sep=8pt}` |

**When legend MUST overlap data** (no external space available):

```latex
legend style={
    fill=white, fill opacity=0.85,
    text opacity=1,
    draw=none,
    inner sep=3pt,
}
```

**CRITICAL**: If using semi-transparent background, verify that the underlying data lines are still visible through the legend box.

### 6c. Legend Marker & Line Rendering

| Element | In Legend | In Plot | Rule |
|---------|----------|---------|------|
| Marker shape | Same | Same | MUST match exactly |
| Marker size | Same as plot (2–2.5 pt) | 2–2.5 pt | MUST match; never enlarge for legend |
| Line width | Same as plot | 1.0–1.2 pt | MUST match |
| Line style (dash pattern) | Same | Same | Legend line must be long enough to show pattern |
| Fill color | Same | Same | MUST match |
| Fill opacity | Same | Same | MUST match |

**Problem**: pgfplots default legend line sample is often too short to distinguish solid from dashed.

**Fix**: Extend legend line length:
```latex
legend style={
    /pgfplots/every legend image post/.append style={sharp plot},
},
legend image code/.code={
    \draw[##1, line width=1pt] plot coordinates {(0cm,0cm) (0.6cm,0cm)};%
},
```

### 6d. Shared Legend for Multi-Panel Figures

**Method 1: `legend to name` (Recommended for pgfplots)**

```latex
\begin{figure*}[!t]
\centering
\begin{minipage}[b]{0.48\textwidth}\centering
\begin{tikzpicture}
\begin{axis}[
    width=\textwidth, height=0.618\textwidth,
    legend to name=sharedlegend,
    legend style={legend columns=4, font=\sffamily\scriptsize,
                  draw=none, column sep=6pt},
    % ... other axis options ...
]
\addplot[mark=*, acBlue, line width=1.0pt] coordinates {...};
\addlegendentry{YourMethod}
\addplot[mark=square*, acVermillion] coordinates {...};
\addlegendentry{Baseline A}
\addplot[mark=triangle*, acGreen] coordinates {...};
\addlegendentry{Baseline B}
\end{axis}
\end{tikzpicture}
\par\smallskip{\small\bfseries (a)} {\small Dataset-A}
\end{minipage}%
\hfill
\begin{minipage}[b]{0.48\textwidth}\centering
\begin{tikzpicture}
\begin{axis}[
    width=\textwidth, height=0.618\textwidth,
    % NO legend here — uses shared legend from panel (a)
]
\addplot[mark=*, acBlue, line width=1.0pt] coordinates {...};
\addplot[mark=square*, acVermillion] coordinates {...};
\addplot[mark=triangle*, acGreen] coordinates {...};
\end{axis}
\end{tikzpicture}
\par\smallskip{\small\bfseries (b)} {\small Dataset-B}
\end{minipage}

\vspace{4pt}
\ref{sharedlegend}

\caption{Performance comparison across two datasets.}
\label{fig:shared_legend_example}
\end{figure*}
```

**Method 2: Manual TikZ Legend Strip** (for non-pgfplots or mixed content)

```latex
\vspace{4pt}
\centering
\begin{tikzpicture}[font=\sffamily\scriptsize]
  \draw[acBlue, line width=1pt, mark=*] plot coordinates {(0,0)(0.5,0)};
  \node[right] at (0.55,0) {YourMethod};
  \draw[acVermillion, line width=1pt, mark=square*] plot coordinates {(2.5,0)(3.0,0)};
  \node[right] at (3.05,0) {Baseline A};
  \draw[acGreen, line width=1pt, mark=triangle*] plot coordinates {(5.0,0)(5.5,0)};
  \node[right] at (5.55,0) {Baseline B};
\end{tikzpicture}
```

### 6e. Legend Entry Ordering

| Chart Type | Ordering Rule | Rationale |
|------------|---------------|-----------|
| Line chart (multi-method) | Top-to-bottom matches visual order at right edge | Readers scan legend then look right |
| Bar chart (grouped) | Left-to-right matches bar group order | Visual consistency |
| Stacked area/bar | Bottom-to-top matches stacking order | Natural mapping |
| All types | **Proposed method always first** | Emphasis on contribution |
| Ablation | Full model first, then variants by performance | Most important first |

### 6f. Legend vs. Direct Label Decision Guide

| Condition | Recommendation |
|-----------|---------------|
| ≤ 2 series, lines well-separated | **Direct label** at line endpoint (no legend) |
| 3 series, some separation | Direct label if space allows; otherwise legend |
| 4–7 series | **Legend** (direct labels too cluttered) |
| Lines cross frequently | **Legend** (direct labels ambiguous at crossings) |
| Single series | State identity in **caption** only |
| Bar chart | Legend below plot; value labels on bars (optional) |
| Pie/donut chart | Direct labels on segments (no external legend) |

**Direct label technique** (pgfplots):
```latex
\addplot[acBlue, mark=*] coordinates {...}
    node[right, font=\sffamily\scriptsize, acBlue] at (axis cs:0.9,97) {YourMethod};
```

### 6g. Legend for Special Figure Types

| Figure Type | Legend Style | Placement |
|-------------|-------------|-----------|
| ROC curve | Text with AUC: `Method (AUC=0.99)` | Inside, bottom-right |
| PR curve | Text with AUPRC | Inside, bottom-left or top-right |
| Confusion matrix | Colorbar (not text legend) | Right side of matrix |
| Heatmap | Colorbar with label | Right side |
| Architecture diagram | Separate mini-legend box for node/edge types | Corner or below |
| Network graph | Node type + edge type legend | Below or beside |

### 6h. Common Legend Mistakes

| Mistake | Fix |
|---------|-----|
| Legend overlaps data points | Move outside plot or add semi-transparent background |
| Legend border visible | `draw=none` (modern convention is borderless) |
| Legend marker size differs from plot | Ensure legend inherits plot marker settings |
| Legend line too short for dash pattern | Extend via `legend image code` |
| Different legend positions across panels | Standardize: all bottom, all top-right, or shared |
| Legend font larger than tick labels | Legend font ≤ tick label font (one step smaller is ideal) |
| Legend entries in wrong order | Match visual order in plot; proposed method first |
| Redundant legend (1 series) | Remove legend; state in caption |

---

## Phase 7: Statistical Visualization

### 7a. Error Bar Type Selection

| Type | When to Use | Formula | Caption Text |
|------|-------------|---------|-------------|
| **SD** (Standard Deviation) | Showing data spread of individual values | σ = √(Σ(xᵢ−μ)²/n) | "Error bars: mean ± SD (n=10 runs)" |
| **SEM** (Standard Error of Mean) | Comparing group means | σ/√n | "Error bars: mean ± SEM (n=10 runs)" |
| **95% CI** (Confidence Interval) | Estimating population parameter | μ ± 1.96×SEM | "Error bars: 95% CI (n=10 runs)" |
| **Min–Max** | Small sample, showing full range | [min, max] | "Error bars: min–max range" |
| **IQR** (box plot whiskers) | Non-normal distributions | Q1, Q3, ±1.5×IQR | "Box: IQR; whiskers: 1.5×IQR" |

**CRITICAL**: Always state in the figure caption or legend which type of error bar is shown. Omitting this is a frequent reviewer complaint and a violation of Nature/Cell guidelines.

### 7b. Error Bar Styling

| Element | Style | Code |
|---------|-------|------|
| Cap width | 4–6 pt (proportional to bar width) | `error bar style={line width=0.5pt, line cap=round}` |
| Bar line width | Same as axis line (0.5 pt) | inherits |
| Color | Same as corresponding data series | match data color |
| For bar charts | Centered on bar top, one direction (upward) | `error bars/y dir=plus, y explicit` |
| For line charts | Bidirectional at data points | `error bars/y dir=both, y explicit` |

**Confidence band** (preferred over discrete error bars for dense line charts):
```latex
\addplot[name path=upper, draw=none] coordinates {...};
\addplot[name path=lower, draw=none] coordinates {...};
\addplot[fill=acBlue, fill opacity=0.15, draw=none] fill between[of=upper and lower];
\addplot[mark=*, acBlue, line width=1.0pt] coordinates {...};
```

### 7c. Significance Markers

For pairwise statistical comparisons (t-test, Wilcoxon, etc.):

| Symbol | Meaning | p-value Range |
|--------|---------|---------------|
| ns | Not significant | p ≥ 0.05 |
| \* | Significant | p < 0.05 |
| \*\* | Highly significant | p < 0.01 |
| \*\*\* | Very highly significant | p < 0.001 |
| \*\*\*\* | — | p < 0.0001 (some fields) |

**Placement rules**:
1. Horizontal bracket connecting the two compared bars/groups
2. Marker (\*) centered above the bracket
3. Bracket starts 2–3% above the taller bar
4. Stagger brackets vertically for multiple comparisons (outermost bracket highest)
5. Use thin lines (0.4–0.5 pt) for brackets

```latex
% Significance bracket between bars at x=1 and x=2
\draw[thin] (axis cs:1,96) -- (axis cs:1,97) -- (axis cs:2,97) -- (axis cs:2,95);
\node[font=\sffamily\scriptsize] at (axis cs:1.5,97.5) {**};
```

### 7d. Uncertainty Visualization

| Method | When | Visual |
|--------|------|--------|
| Error bars (discrete) | ≤ 15 data points per series | T-bars at each point |
| Confidence band (continuous) | Dense data (> 15 points) | Shaded region around line |
| Box plot | Distribution comparison | Median + IQR + whiskers |
| Violin plot | Distribution shape matters | Kernel density + box overlay |
| Posterior distribution | Bayesian methods | Shaded density curve |

For per-type code templates, see [figure-types.md](figure-types.md) §18–20.

---

## Phase 8: Layout, Float Placement & Page Density

### 8a. Float Placement — The #1 Source of Layout Disasters

**Placement specifier rules** (the `[!t]` in `\begin{figure}[!t]`):

| Specifier | Meaning | When to Use |
|-----------|---------|-------------|
| `[!t]` | Top of page, override restrictions | **Default for most figures/tables** |
| `[!b]` | Bottom of page | Secondary; use for less important floats |
| `[!ht]` | Here first, then top | When figure must stay near reference text |
| `[!htbp]` | Try all positions | Last resort; gives LaTeX maximum freedom |
| `[H]` | Force HERE (requires `float` pkg) | **Avoid** — breaks float queue; use only for debugging |

**CRITICAL rules**:
1. Prefer `!` when floats drift too far from references; do not add it blindly to every float.
2. Avoid bare `[h]`; prefer `[!ht]` or `[!htbp]`.
3. For `figure*` / `table*`, default to `[!t]`/`[!p]`; `[b]` needs `stfloats`/`dblfloatfix` and class compatibility.
4. Order preservation: LaTeX processes floats in order per type — a stuck `figure` blocks subsequent figures.

### 8b. Page Density Tuning — Eliminating Wasted Space

LaTeX defaults are **extremely conservative**. Add this block to preamble (see [layout-placement.md](layout-placement.md) §3 for full parameter reference):

```latex
\renewcommand{\topfraction}{0.9}    \renewcommand{\bottomfraction}{0.8}
\renewcommand{\textfraction}{0.1}   \renewcommand{\floatpagefraction}{0.8}
\renewcommand{\dbltopfraction}{0.9} \renewcommand{\dblfloatpagefraction}{0.8}
\setcounter{topnumber}{3}  \setcounter{bottomnumber}{2}
\setcounter{totalnumber}{5}\setcounter{dbltopnumber}{2}
\setlength{\textfloatsep}{10pt plus 2pt minus 4pt}
\setlength{\floatsep}{8pt plus 2pt minus 2pt}
\setlength{\intextsep}{8pt plus 2pt minus 2pt}
\setlength{\dbltextfloatsep}{10pt plus 2pt minus 4pt}
```

### 8c. Single Figure/Table Placement

| Scenario | Best Practice |
|----------|--------------|
| Single-column figure | `\begin{figure}[!t]` + `width=\columnwidth` |
| Full-width figure (double-col paper) | `\begin{figure*}[!t]` + `width=\textwidth` |
| Single-column table | `\begin{table}[!t]` + `\resizebox{\columnwidth}{!}{...}` if wide |
| Full-width table | `\begin{table*}[!t]` |
| Figure near its first reference | `\begin{figure}[!ht]` — "here" first, then top |
| Keep float in its section | Add `\usepackage[section]{placeins}` → auto `\FloatBarrier` at sections |

### 8d. Multi-Figure Arrangements

| Layout | Panel Width | Template |
|--------|-----------|----------|
| 1×2 (most common) | `0.48\textwidth` | `minipage[b]` + `\hfill` |
| 1×3 | `0.32\textwidth` | `minipage[b]` + `\hfill` |
| 2×2 grid | `0.48\textwidth`, `\\[6pt]` between rows | Two rows of 1×2 |
| 2×3 grid (max) | `0.32\textwidth` | Two rows of 1×3 |
| 1 large + 2 small | `0.55` + `0.43\textwidth` (nested) | Asymmetric layout |

**KEY**: Always add `%` after `\end{minipage}` to prevent unwanted space between panels.

For complete code templates of all layouts, see [layout-placement.md](layout-placement.md) §5.

### 8e. Common Layout Failures (Top 5)

| Symptom | Fix |
|---------|-----|
| Float appears pages after reference | Add `!` to specifier; tune density params |
| Whole page whitespace + one float | `\floatpagefraction` → 0.8 |
| Table overflows column | `\resizebox{\columnwidth}{!}{...}` or `\footnotesize` + `\tabcolsep=3pt` |
| Gap between side-by-side panels | Add `%` after `\end{minipage}` |
| `figure*` ignores `[b]` | Use `[!t]` or load `stfloats` / `dblfloatfix` |

For the complete 10-item failure table and troubleshooting flowchart, see [layout-placement.md](layout-placement.md) §7.

### 8f. Sizing & Subfigure Rules

- Use **relative** sizing: `width=\columnwidth` / `0.48\textwidth` — never hardcode cm/mm
- Labels **(a)**, **(b)**, **(c)**: bold, 8–10pt, centered below
- Max **6 subfigures** per figure; all comparison panels share **same Y-axis range**

For publisher-specific dimensions, see [publisher-specs.md](publisher-specs.md).

---

## Phase 9: Table Visual Design

### Mandatory Style Rules

1. **booktabs** package: `\toprule`, `\midrule`, `\bottomrule` only
2. **NO vertical lines** — never use `|` in column spec
3. **NO `\hline`** — use `\cmidrule(lr){a-b}` for partial separators
4. **Best result** per column: `\textbf{96.5\%}`
5. **Second best** (optional): `\underline{98.54\%}`
6. **Direction indicators** in header: Acc↑, FPR↓, Latency↓
7. Row grouping via `\cmidrule(lr)` + indentation or `\multirow`
8. Table notes via `threeparttable` + `tablenotes`

### Table Font & Spacing

| Element | Rule |
|---------|------|
| Whole table | `\small` or `\footnotesize` for dense tables |
| Column header | `\textbf{}`, same font as body |
| Line spacing | `\renewcommand{\arraystretch}{1.15}` for breathing room |
| Column padding | `\setlength{\tabcolsep}{4pt}` to prevent overflow |

### Table Width & Decimal Alignment

- Overflow fix: `\resizebox{\columnwidth}{!}{...}` or `tabularx`
- **Decimal consistency**: same column = same decimal places (99.10%, 98.54%, 97.32%)

### Table Caption & Header Length

| Element | Guideline | Action if Violated |
|---------|-----------|-------------------|
| Caption | 1–3 sentences; self-contained but concise | Trim to essential info; move details to main text |
| Column header | ≤3 words; abbreviate with footnote | Shorten header, add `\tnote{a}` with `threeparttable` |
| Header wrapping | Max 2 lines; rotate if necessary for narrow columns | Use `\rotatebox{90}{Header}` or abbreviate |
| Table note | ≤3 lines; explain abbreviations and special markings | If longer, move explanation to main text |

**Caption position**: Table caption ABOVE (`\caption{}` before `\begin{tabular}`); figure caption BELOW.

---

## Phase 10: Caption & Label Formatting

### Caption Rules

1. **Self-contained (P1 — reviewers read captions BEFORE body text)**: Every caption must be independently understandable. Include: (a) what the figure/table shows, (b) brief experimental setup, (c) key takeaway/finding, (d) definition of any non-obvious abbreviation. A reviewer who reads ONLY the caption should understand the figure's message.
2. **End with period**; figure caption below, table caption above
3. **`\label` immediately after `\caption`** on consecutive lines
4. **Panel references in caption**: `(a) Dataset-A; (b) Dataset-B` — in text: `Fig.~\ref{fig:x}(a)`
5. Caption format auto-handled by document class (Fig./TABLE/Figure:) — do NOT manually format; see [publisher-specs.md](publisher-specs.md) for per-publisher styles
6. **Performance table captions**: Must state the marking convention — "Bold: best result per column; underline: second-best"

### Panel Label Style Per Venue

| Venue Type | Style | Example | Position |
|------------|-------|---------|----------|
| Nature/Science | Lowercase bold, no parentheses | **a**, **b**, **c** | Top-left of panel |
| IEEE / Elsevier | Parenthesized | **(a)**, **(b)**, **(c)** | Below or top-left |
| ACM | Parenthesized | **(a)**, **(b)**, **(c)** | Below panel |
| USENIX | Either convention | Consistent within paper | Below panel |

**Rule**: Pick ONE style and apply it consistently to ALL multi-panel figures in the paper.

---

## Phase 11: Cross-Reference & Hyperlink Audit

### Cross-Reference Checks

1. Every `\label{fig:*}` / `\label{tab:*}` must have a matching `\ref{}` in text
2. Every `\ref{}` must have a matching `\label{}`; check for "[?]" in compiled PDF
3. **Always** use `~` (non-breaking space): `Fig.~\ref{}`, `Table~\ref{}`, `Eq.~\eqref{}`
4. In-text: `Fig.~\ref{fig:x}` / `Table~\ref{tab:x}` / `Eq.~\eqref{eq:x}`
5. At sentence start: spell out fully — `Figure~\ref{}`, `Table~\ref{}`, `Equation~\eqref{}`

### Hyperlink Color Audit

1. Internal links (Fig/Table/Eq): subdued color, NOT bright red/neon blue
2. Citations: different hue from internal links
3. Camera-ready: follow venue rules (some require hidden links; IEEE PDF eXpress requires no active links/bookmarks)
4. All links of same type = same color throughout

```latex
% Review draft: subtle colored links
\hypersetup{
  colorlinks=true,
  linkcolor=black!70!blue,
  citecolor=black!60!green,
  urlcolor=black!60!purple
}
```

```latex
% Camera-ready when venue requires no active link styling
\hypersetup{hidelinks}
```

---

## Phase 12: Cross-Figure Consistency Audit

Review ALL figures in the paper as a set, checking for visual coherence.

### Color Consistency

- [ ] Same entity (method/dataset/class) uses the SAME color in every figure
- [ ] Color assignments match the semantic lock table from Phase 1
- [ ] No ad-hoc colors introduced in individual figures
- [ ] If multiple heatmaps exist, all use the same colormap
- [ ] Attack/benign color convention consistent (red=attack, blue/green=benign)

### Typography Consistency

- [ ] All figures use the same font family (all sans-serif)
- [ ] Same font size for equivalent elements across figures (axis labels, ticks, legends)
- [ ] Math symbols render identically ($\lambda$ looks the same everywhere)
- [ ] No figure accidentally uses a different font (common when mixing matplotlib + pgfplots)

### Encoding Consistency

- [ ] Same method uses the same marker + line style in ALL line charts
- [ ] Same method uses the same bar fill/pattern in ALL bar charts
- [ ] Legend entries use identical wording (not "YourMethod" in Fig.3 and "Ours" in Fig.5)

### Axis Consistency

- [ ] Same metric uses the same axis range where comparable (F1 always 80–100% or always 0–100%)
- [ ] Tick direction consistent across all figures (all outward or all inward)
- [ ] Grid style consistent (all dashed gray, all none, or all same)
- [ ] Axis frame consistent (all L-shaped or all boxed)
- [ ] Axis label format consistent ("F1 Score (%)" not "F1(%)" in some figures)

### Proportion Consistency

- [ ] Figures of the same type have the same aspect ratio
- [ ] Multi-panel figures have uniform panel sizes
- [ ] Line widths and marker sizes consistent across figures

### Legend Consistency

- [ ] Legend position consistent across similar figure types
- [ ] Legend styling identical (all borderless, same font size, same column layout)
- [ ] Legend entry order consistent (proposed method always first)

### Panel Label Consistency

- [ ] All multi-panel figures use the same label style: all (a)(b) or all **a**, **b**
- [ ] Panel label position consistent (all centered below or all top-left)
- [ ] Panel label font and size identical

---

## Phase 13: Export Quality Pipeline

### Step-by-Step Workflow

```
Step 1: Source Format Selection
├── pgfplots / TikZ → Compiled directly in LaTeX (best quality, fonts auto-match)
├── matplotlib → Export as PDF (vector) or PGF (LaTeX-native)
├── R / ggplot2 → Export as PDF (vector)
└── Other tools → Export as high-resolution PDF

Step 2: Resolution Verification
├── Vector (PDF/EPS/SVG) → Resolution-independent; always crisp
├── Raster line art → ≥ 600 DPI (Nature requires 1000 DPI)
├── Photographs → ≥ 300 DPI
└── Mixed (line + photo) → ≥ 600 DPI

Step 3: Font Embedding Check
├── pdfLaTeX → \usepackage[T1]{fontenc}
├── XeLaTeX / LuaLaTeX → Fonts auto-embedded
├── matplotlib → plt.rcParams['pdf.fonttype'] = 42
└── Verify: run 'pdffonts output.pdf' → all entries show 'yes' for embedded

Step 4: File Format for Submission
├── Elsevier → EPS or PDF (vector); TIFF (1000 DPI) for raster
├── IEEE → EPS or PDF; may require separate figure files
├── Nature/Science → TIFF (1000 DPI) or PDF/EPS for vector
├── ACM → PDF preferred
└── USENIX → PDF preferred

Step 5: Quality Verification
├── Zoom to 400% in PDF viewer → text sharp, no pixelation
├── Print at actual size → all text ≥ 6pt is readable
├── Convert to grayscale → all series still distinguishable
├── Check file size → vector PDF typically < 2MB per figure
└── Open in Illustrator/Inkscape → text is editable (not outlines)
```

### matplotlib Export Configuration

```python
import matplotlib.pyplot as plt

plt.rcParams.update({
    'font.family': 'sans-serif',
    'font.sans-serif': ['Arial', 'Helvetica'],
    'font.size': 8,
    'axes.labelsize': 9,
    'xtick.labelsize': 7,
    'ytick.labelsize': 7,
    'legend.fontsize': 7,
    'figure.figsize': (3.5, 2.16),  # golden ratio for single column
    'figure.dpi': 300,
    'savefig.dpi': 600,
    'savefig.bbox': 'tight',
    'savefig.pad_inches': 0.02,
    'pdf.fonttype': 42,
    'ps.fonttype': 42,
    'lines.linewidth': 1.0,
    'lines.markersize': 4,
    'axes.linewidth': 0.5,
    'grid.linewidth': 0.3,
    'grid.alpha': 0.3,
})

fig.savefig('figure.pdf', format='pdf')
```

### Common Export Pitfalls

| Problem | Cause | Fix |
|---------|-------|-----|
| Blurry text in figure | Raster export at low DPI | Use vector PDF; if raster, ≥ 600 DPI |
| Font mismatch with document | matplotlib uses system font | Use PGF backend or configure `font.family` |
| Huge PDF file size | Dense scatter plots with thousands of points | Rasterize scatter layer only: `ax.set_rasterized(True)` |
| White border / excess padding | Default matplotlib margins | `bbox_inches='tight', pad_inches=0.02` |
| Colors differ in printed PDF | RGB vs CMYK conversion | Test with `\usepackage[cmyk]{xcolor}` |
| Text cut off at edges | Tight layout not applied | `fig.set_constrained_layout(True)` |
| Labels overlap in export | Missing layout engine | `constrained_layout=True` (do NOT combine with `tight_layout`) |

---

## Phase 14: Accessibility Testing

1. **Grayscale**: Convert PDF to grayscale; every series must be distinguishable by line style + marker alone
2. **Colorblind**: Test with Color Oracle or Coblis (Protanopia + Deuteranopia + Tritanopia)
3. **Contrast**: follow WCAG targets as baseline: normal text >= 4.5:1, graphical objects >= 3:1
4. **ACM accessibility**: Every figure MUST have `\Description{...}` in ACM submissions

---

## Phase 15: Pre-Submission Figure Checklist

```
Complete Figure/Table Quality Checklist:

Color & Encoding:
- [ ] All figures use the SAME Okabe-Ito (or chosen) palette
- [ ] Same entity = same color/marker in every figure
- [ ] Triple encoding (color + line + marker) on all multi-series plots
- [ ] Grayscale test passed; colorblind simulation passed
- [ ] Fill opacity appropriate per context (bands 0.15, bars 0.75, scatter 0.5)

Typography:
- [ ] Figure/text fonts follow venue template; labels remain legible (recommended >= 7pt)
- [ ] Any text below 6pt is explicitly allowed by the target venue
- [ ] All fonts embedded in PDF (verified with pdffonts)
- [ ] Font family consistent across ALL figures in the paper
- [ ] Math mode symbols render correctly in figure labels

Axis & Grid:
- [ ] Tick direction consistent (outward for Nature; inward or outward for others)
- [ ] Grid style consistent across figures (or deliberately varied by type)
- [ ] Axis range appropriate (line charts truncated; bar charts start at 0)
- [ ] Axis labels include units in parentheses
- [ ] No 3D effects on 2D data
- [ ] Aspect ratio appropriate per chart type (ROC/PR = square; lines = golden ratio)

Legend:
- [ ] Legend position does NOT overlap data
- [ ] Legend border: none (draw=none)
- [ ] Legend marker/line matches plot exactly (size, style, color)
- [ ] Legend entry order matches visual order; proposed method first
- [ ] Multi-panel figures use shared legend (not duplicated per panel)
- [ ] Legend font ≤ tick label font size
- [ ] ≤ 2 series: consider direct labels instead of legend
- [ ] Legend line sample long enough to show dash patterns

Statistical Annotations:
- [ ] Error bar type stated in caption (SD, SEM, 95% CI, or min-max)
- [ ] Significance markers use standard notation (ns, *, **, ***)
- [ ] Error bars use same color as corresponding data series
- [ ] Confidence bands have appropriate opacity (0.10–0.20)

Float Placement & Page Density:
- [ ] Float specifiers are intentional (avoid bare [h]; use ! when drift occurs)
- [ ] Page density params tuned (topfraction=0.9, textfraction=0.1, etc.)
- [ ] No page with only one small float + large whitespace
- [ ] No float appears more than 1 page away from its first \ref
- [ ] figure* / table* placement matches class behavior ([!t] default)
- [ ] No "Too many unprocessed floats" errors
- [ ] % after every \end{minipage} in side-by-side layouts

Content Quality:
- [ ] Plots are vector (pgfplots/tikz/PDF); raster images ≥ 300 DPI
- [ ] Every figure/table has \label and is \ref'd in text
- [ ] All \ref use ~ non-breaking space: Fig.~\ref, Table~\ref
- [ ] Captions are self-contained sentences ending with period
- [ ] ACM submissions: each figure has a meaningful \Description{...}
- [ ] No Chinese/non-English text in any figure (submission version)
- [ ] Panel labels (a)(b)(c) present, bold, consistent style across paper
- [ ] Figure width uses relative sizing (\columnwidth / \textwidth)

Tables:
- [ ] booktabs only, no vertical lines, no \hline
- [ ] Best result bolded per column; decimal places consistent
- [ ] Direction indicators (↑/↓) in headers
- [ ] No table overflows column width

Cross-Figure Consistency:
- [ ] Color/marker/line encoding identical for same entity across all figures
- [ ] Font family and size identical across all figures
- [ ] Axis range, tick direction, grid style consistent
- [ ] Legend style and position consistent across similar figures
- [ ] Panel label style consistent (all (a)(b) or all a, b)
- [ ] Wording in legend entries identical across figures

Cross-References & Links:
- [ ] Hyperlinks follow venue policy (review vs camera-ready)
- [ ] Internal ref color ≠ citation color ≠ URL color
- [ ] No overfull hbox warnings from figures/tables

Export Quality:
- [ ] All figures are vector format (PDF/EPS) or ≥ 600 DPI raster
- [ ] Fonts embedded (pdffonts verification)
- [ ] File sizes reasonable (< 2MB per vector figure)
- [ ] 400% zoom test: text remains crisp
- [ ] Grayscale print test: all content readable
```

---

## Additional Resources

- [color-reference.md](color-reference.md) — Full palette specs, LaTeX/Python definitions, CMYK notes, fill opacity guidelines, contrast requirements
- [figure-types.md](figure-types.md) — Per-type pgfplots/TikZ/matplotlib templates with code, including shared legend, error bars, significance markers, broken axis
- [publisher-specs.md](publisher-specs.md) — Detailed per-publisher specs, caption styles, file format requirements
- [layout-placement.md](layout-placement.md) — Float mechanics deep dive, multi-figure templates, page density tuning, troubleshooting guide
