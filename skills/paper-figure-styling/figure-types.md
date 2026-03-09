# Figure Type Templates & Guidelines

Per-type code templates and design rules for academic papers. Referenced by [SKILL.md](SKILL.md).

All templates assume the Okabe-Ito palette is defined (see [color-reference.md](color-reference.md)).

## Contents

- [1. Line Chart](#1-line-chart--method-comparison--sensitivity-analysis)
- [2. Bar Chart](#2-bar-chart--performance-comparison--ablation)
- [3. Dual-Axis Chart](#3-dual-axis-chart--performance-vs-cost-tradeoff)
- [4. ROC Curve](#4-roc-curve)
- [5. Precision-Recall Curve](#5-precision-recall-curve)
- [6. Heatmap / Confusion Matrix](#6-heatmap--confusion-matrix)
- [7. Box Plot / Violin Plot](#7-box-plot--violin-plot)
- [8. Scatter Plot](#8-scatter-plot--t-sne--umap--feature-visualization)
- [9. Radar / Spider Chart](#9-radar--spider-chart)
- [10. Architecture / Pipeline Diagram](#10-architecture--pipeline-diagram-tikz)
- [11. Network / Graph Visualization](#11-network--graph-visualization)
- [12. Timeline / Attack Progression Diagram](#12-timeline--attack-progression-diagram)
- [13. Waterfall / Incremental Contribution Chart](#13-waterfall--incremental-contribution-chart)

---

## 1. Line Chart — Method Comparison / Sensitivity Analysis

### When to Use
- Hyperparameter sensitivity (x = param value, y = metric)
- Convergence curves (x = epoch, y = loss/metric)
- Threshold sweeps (x = threshold, y = metric)

### pgfplots Template

```latex
\begin{tikzpicture}
\begin{axis}[
    width=\columnwidth,
    height=0.65\columnwidth,
    xlabel={Hyperparameter $\lambda$},
    ylabel={F1 Score (\%)},
    xmin=0, xmax=1,
    ymin=85, ymax=100,
    grid=major,
    grid style={dashed, gray!30},
    legend style={at={(0.5,-0.25)}, anchor=north, legend columns=3,
                  font=\sffamily\scriptsize, draw=none, column sep=4pt},
    every axis plot/.append style={line width=1.0pt, mark size=2.0pt},
    font=\sffamily\small,
    label style={font=\sffamily\small},
    tick label style={font=\sffamily\footnotesize},
]
\addplot[mark=*, acBlue] coordinates {(0.1,88) (0.3,93) (0.5,97) (0.7,95) (0.9,91)};
\addplot[mark=square*, acVermillion] coordinates {(0.1,85) (0.3,89) (0.5,92) (0.7,90) (0.9,87)};
\addplot[mark=triangle*, acGreen] coordinates {(0.1,82) (0.3,86) (0.5,90) (0.7,88) (0.9,84)};
\legend{YourMethod, Baseline A, Baseline B}
\end{axis}
\end{tikzpicture}
```

### Design Rules
- Y-axis range: truncate to data range (e.g., 85–100 not 0–100) for visibility
- X-axis: use log scale (`xmode=log`) if values span orders of magnitude
- At most 7 lines per plot; split into subfigures if more
- Solid lines for proposed + primary baselines; dashed for secondary
- Error band: `\addplot[fill=acBlue, fill opacity=0.15, draw=none] ... \closedcycle;`

### With Error Bands (Confidence Interval)

```latex
\addplot[name path=upper, draw=none] coordinates {(0.1,90) (0.5,99) (0.9,93)};
\addplot[name path=lower, draw=none] coordinates {(0.1,86) (0.5,95) (0.9,89)};
\addplot[fill=acBlue, fill opacity=0.15] fill between[of=upper and lower];
\addplot[mark=*, acBlue, line width=1.0pt] coordinates {(0.1,88) (0.5,97) (0.9,91)};
```

---

## 2. Bar Chart — Performance Comparison / Ablation

### When to Use
- Comparing methods on a single metric across datasets
- Ablation study (full model vs. variants)
- Category-level performance breakdown

### Grouped Bar Template

```latex
\begin{tikzpicture}
\begin{axis}[
    width=\columnwidth,
    height=0.6\columnwidth,
    ybar=2pt,
    bar width=8pt,
    ylabel={F1 Score (\%)},
    ymin=70, ymax=100,
    symbolic x coords={Dataset-A, Dataset-B, Dataset-C},
    xtick=data,
    x tick label style={font=\sffamily\footnotesize},
    legend style={at={(0.5,-0.22)}, anchor=north, legend columns=3,
                  font=\sffamily\scriptsize, draw=none, column sep=4pt},
    nodes near coords,
    nodes near coords style={font=\sffamily\scriptsize, color=black},
    every node near coord/.append style={yshift=2pt},
    font=\sffamily\small,
    grid=major,
    grid style={dashed, gray!15},
    ymajorgrids=true,
    xmajorgrids=false,
]
\addplot[fill=acBlue, draw=acBlue!80!black] coordinates
    {(Dataset-A, 96.5) (Dataset-B, 95.3) (Dataset-C, 97.5)};
\addplot[fill=acVermillion!75, draw=acVermillion!80!black] coordinates
    {(Dataset-A, 92.3) (Dataset-B, 93.7) (Dataset-C, 89.2)};
\addplot[fill=acGreen!75, draw=acGreen!80!black] coordinates
    {(Dataset-A, 88.1) (Dataset-B, 87.4) (Dataset-C, 85.9)};
\legend{YourMethod, Baseline A, Baseline B}
\end{axis}
\end{tikzpicture}
```

### With Hatching (Grayscale Fallback)

```latex
\pgfplotsset{
    /pgfplots/bar cycle list/.style={/pgfplots/cycle list={
        {fill=acBlue, draw=acBlue!80!black},
        {fill=acVermillion!75, draw=acVermillion!80!black,
         postaction={pattern=north east lines, pattern color=white}},
        {fill=acGreen!75, draw=acGreen!80!black,
         postaction={pattern=dots, pattern color=white}},
    }},
}
```

### Stacked Bar Template

```latex
\begin{axis}[
    ybar stacked,
    bar width=12pt,
    symbolic x coords={Method A, Method B, Method C},
    xtick=data,
    legend style={at={(0.5,-0.22)}, anchor=north, legend columns=3,
                  font=\sffamily\scriptsize, draw=none},
    ylabel={Execution Time (s)},
]
\addplot[fill=acBlue!70] coordinates {(Method A,12) (Method B,15) (Method C,20)};
\addplot[fill=acOrange!70] coordinates {(Method A,8) (Method B,5) (Method C,3)};
\addplot[fill=acGreen!70] coordinates {(Method A,3) (Method B,4) (Method C,2)};
\legend{Preprocessing, Training, Inference}
\end{axis}
```

### Design Rules
- Bar width: 8–14pt depending on number of groups
- Gap between groups ≥ bar width
- Value labels: omit if bars are crowded or if exact values are in a table
- Sort bars by value (descending) unless categorical order is meaningful
- Your method: always leftmost or otherwise first in the group

---

## 3. Dual-Axis Chart — Performance vs. Cost Tradeoff

### When to Use
- F1 vs. Parameters (model complexity analysis)
- Accuracy vs. Inference time
- Any two metrics with different scales

### Template

```latex
\begin{tikzpicture}
\begin{axis}[
    width=\columnwidth,
    height=0.6\columnwidth,
    xlabel={Hidden Dimension $d$},
    ylabel={F1 Score (\%)},
    ymin=80, ymax=100,
    xtick={64, 128, 256, 512},
    axis y line*=left,
    ylabel style={color=acBlue!80!black},
    ybar, bar width=14pt,
    font=\sffamily\small,
    nodes near coords,
    nodes near coords style={font=\sffamily\scriptsize},
    legend style={at={(0.5,-0.25)}, anchor=north, legend columns=2,
                  font=\sffamily\scriptsize, draw=none, column sep=6pt},
]
\addplot[fill=acBlue!75, draw=acBlue!80!black]
    coordinates {(64, 93.09) (128, 83.77) (256, 94.39) (512, 95.69)};
\addlegendentry{F1}
\addlegendimage{mark=diamond*, acVermillion, line width=1.2pt, dashed}
\addlegendentry{Params (M)}
\end{axis}

\begin{axis}[
    width=\columnwidth,
    height=0.6\columnwidth,
    ylabel={Parameters (M)},
    ymin=0, ymax=50,
    xtick={64, 128, 256, 512},
    axis y line*=right,
    axis x line=none,
    ylabel style={color=acVermillion!80!black},
]
\addplot[mark=diamond*, acVermillion, line width=1.2pt, dashed, mark size=2.5pt]
    coordinates {(64, 0.70) (128, 2.74) (256, 10.87) (512, 43.31)};
\end{axis}
\end{tikzpicture}
```

### Design Rules
- Left Y-axis color matches bars; right Y-axis color matches line
- Always include both Y-axis labels with units
- Legend must explain both scales

---

## 4. ROC Curve

### When to Use
- Binary classification performance at various thresholds
- Comparing multiple detectors

### Template

```latex
\begin{axis}[
    width=\columnwidth,
    height=\columnwidth,  % square for ROC
    xlabel={False Positive Rate},
    ylabel={True Positive Rate},
    xmin=0, xmax=1, ymin=0, ymax=1,
    grid=major, grid style={dashed, gray!20},
    legend style={at={(0.98,0.02)}, anchor=south east,
                  font=\sffamily\scriptsize, draw=none},
    font=\sffamily\small,
]
% Random classifier baseline
\addplot[acBlack!40, dashed, thin] coordinates {(0,0) (1,1)};
\addlegendentry{Random}

% Methods
\addplot[acBlue, mark=none, line width=1.2pt]
    coordinates {(0,0) (0.001,0.85) (0.01,0.95) (0.05,0.98) (0.1,0.99) (1,1)};
\addlegendentry{YourMethod (AUC=0.995)}

\addplot[acVermillion, mark=none, line width=1.0pt, dashed]
    coordinates {(0,0) (0.005,0.70) (0.02,0.88) (0.1,0.95) (0.3,0.98) (1,1)};
\addlegendentry{Baseline A (AUC=0.9534)}
\end{axis}
```

### Design Rules
- **Square** aspect ratio (width = height)
- Include diagonal baseline (dashed gray)
- AUC value in legend: `Method (AUC = X.XXXX)`
- No markers on smooth curves (too many points)
- If all curves cluster near (0,1), add a **zoomed inset**

### Zoomed Inset

```latex
\begin{axis}[
    % ... main ROC axis ...
]
% Main curves here
\end{axis}

% Inset
\begin{axis}[
    at={(0.35\columnwidth, 0.08\columnwidth)},
    width=0.45\columnwidth, height=0.45\columnwidth,
    xmin=0, xmax=0.05, ymin=0.9, ymax=1,
    xlabel={}, ylabel={},
    font=\sffamily\tiny,
    axis background/.style={fill=white},
    axis line style={thin},
]
% Same curves, zoomed range
\end{axis}
```

---

## 5. Precision-Recall Curve

### When to Use
- Imbalanced datasets (cybersecurity: attack ≪ normal)
- More informative than ROC when positive class is rare

### Template

```latex
\begin{axis}[
    width=\columnwidth,
    height=\columnwidth,
    xlabel={Recall},
    ylabel={Precision},
    xmin=0, xmax=1, ymin=0, ymax=1,
    grid=major, grid style={dashed, gray!20},
    legend style={at={(0.02,0.02)}, anchor=south west,
                  font=\sffamily\scriptsize, draw=none},
    font=\sffamily\small,
]
% Prevalence baseline (e.g., 1% positive rate)
\addplot[acBlack!30, dashed, thin] coordinates {(0,0.01) (1,0.01)};
\addlegendentry{Prevalence}

\addplot[acBlue, mark=none, line width=1.2pt]
    coordinates {(0.5,1.0) (0.8,0.98) (0.95,0.95) (0.99,0.90) (1.0,0.85)};
\addlegendentry{YourMethod (AUPRC=0.985)}
\end{axis}
```

### Design Rules
- Legend at **bottom-left** (opposite to ROC)
- Include prevalence baseline
- AUPRC value in legend
- Iso-F1 contour lines optional (shows F1-score levels)

---

## 6. Heatmap / Confusion Matrix

### When to Use
- Confusion matrix (predicted vs. actual classes)
- Correlation matrix (feature correlations)
- Attention weight visualization
- Hyperparameter grid search results

### pgfplots Confusion Matrix Template

```latex
\begin{axis}[
    width=0.8\columnwidth,
    height=0.8\columnwidth,
    xlabel={Predicted Label},
    ylabel={True Label},
    xticklabels={Normal, Attack},
    yticklabels={Attack, Normal},
    xtick={0, 1},
    ytick={0, 1},
    colormap/viridis,
    colorbar,
    colorbar style={ylabel={Count}},
    point meta min=0,
    point meta max=5000,
    font=\sffamily\small,
    tick label style={font=\sffamily\footnotesize},
]
\addplot[matrix plot, mesh/cols=2, mesh/rows=2,
         point meta=explicit] coordinates {
    (0,0) [4850]  (1,0) [12]
    (0,1) [3]     (1,1) [135]
};

% Cell annotations (white on dark, black on light)
\node[font=\sffamily\small, white] at (axis cs:0,0) {4850};
\node[font=\sffamily\small, white] at (axis cs:1,1) {135};
\node[font=\sffamily\small] at (axis cs:1,0) {12};
\node[font=\sffamily\small] at (axis cs:0,1) {3};
\end{axis}
```

### Design Rules
- Colormap: viridis (default) or cividis (strongest CVD safety)
- Cell annotations: **white text** on dark cells, **black text** on light cells
- Auto-threshold: annotation color flips at ~50% of max value
- Colorbar always present with label
- Axis labels: class names (not 0/1 indices)
- For normalized confusion matrix: show percentages with 1 decimal

---

## 7. Box Plot / Violin Plot

### When to Use
- Distribution of results across multiple runs
- Comparing variance across methods
- Showing robustness (low variance = robust)

### pgfplots Box Plot Template

```latex
\begin{axis}[
    width=\columnwidth,
    height=0.55\columnwidth,
    ylabel={F1 Score (\%)},
    boxplot/draw direction=y,
    xtick={1, 2, 3},
    xticklabels={YourMethod, Baseline A, Baseline B},
    x tick label style={font=\sffamily\footnotesize},
    font=\sffamily\small,
]
\addplot+[boxplot, fill=acBlue!30, draw=acBlue,
          boxplot prepared={
    median=98.9, upper quartile=99.2, lower quartile=98.5,
    upper whisker=99.5, lower whisker=97.8}]
    coordinates {};

\addplot+[boxplot, fill=acVermillion!30, draw=acVermillion,
          boxplot prepared={
    median=92.3, upper quartile=93.1, lower quartile=91.0,
    upper whisker=94.2, lower whisker=89.5}]
    coordinates {};
\end{axis}
```

### Design Rules
- Fill color: light (30% opacity) version of the method color
- Border color: full-strength method color
- Median line: thick, full color
- Individual data points overlaid if n ≤ 30 (jitter horizontally)
- Violin plot preferred when showing distribution shape matters

---

## 8. Scatter Plot — t-SNE / UMAP / Feature Visualization

### When to Use
- Embedding visualization (t-SNE, UMAP)
- Feature space separation quality
- Showing cluster structure

### pgfplots Template

```latex
\begin{axis}[
    width=\columnwidth,
    height=\columnwidth,
    xlabel={}, ylabel={},  % no labels for t-SNE/UMAP
    xtick=\empty, ytick=\empty,  % no ticks
    legend style={at={(0.02,0.98)}, anchor=north west,
                  font=\sffamily\scriptsize, draw=none},
    font=\sffamily\small,
    clip=false,
]
\addplot[only marks, mark=*, mark size=1pt, acBlue, opacity=0.5]
    coordinates {(1,2) (1.5,2.3) ...};
\addlegendentry{Normal (n=9500)}

\addplot[only marks, mark=triangle*, mark size=1.5pt, acVermillion, opacity=0.7]
    coordinates {(5,6) (5.2,5.8) ...};
\addlegendentry{Attack (n=500)}
\end{axis}
```

### Design Rules
- **No axis ticks or labels** for t-SNE/UMAP (dimensions are meaningless)
- Different **marker shapes** per class (not just color)
- Transparency: alpha=0.4–0.7 for overlapping points
- Attack class: larger markers (1.5×) and higher opacity for visibility
- Legend: class name + sample count
- Mark size: 0.8–1.5pt for large n (>1000); 2–3pt for small n (<200)

---

## 9. Radar / Spider Chart

### When to Use
- Multi-metric comparison across methods (5–8 metrics)
- Showing method strengths/weaknesses at a glance

### pgfplots Template (requires `pgfplots-radar` or manual polar)

```latex
% Manual approach using polar coordinates
\begin{polaraxis}[
    width=0.8\columnwidth,
    xtick={0, 60, 120, 180, 240, 300},
    xticklabels={F1, Precision, Recall, AUROC, AUPRC, MCC},
    x tick label style={font=\sffamily\footnotesize, anchor=south},
    ymin=80, ymax=100,
    ytick={80, 85, 90, 95, 100},
    y tick label style={font=\sffamily\tiny},
    legend style={at={(1.15,0.5)}, font=\sffamily\scriptsize, draw=none},
]
\addplot[acBlue, fill=acBlue, fill opacity=0.15, line width=1.0pt]
    coordinates {(0,99.1) (60,98.2) (120,100) (180,99.1) (240,100) (300,99.1) (360,99.1)};
\addlegendentry{YourMethod}

\addplot[acVermillion, fill=acVermillion, fill opacity=0.1, line width=0.8pt, dashed]
    coordinates {(0,92.3) (60,90.5) (120,94.1) (180,95.2) (240,93.8) (300,92.0) (360,92.3)};
\addlegendentry{Baseline}
\end{polaraxis}
```

### Design Rules
- **5–8 axes** maximum (more becomes unreadable)
- All axes same scale: [0,100] or normalize to [0,1]
- Your method: solid fill, higher opacity (0.15); baselines: lower (0.10), dashed
- Axis labels outside the polygon
- Grid rings: 3–5 concentric levels

---

## 10. Architecture / Pipeline Diagram (TikZ)

### When to Use
- System/model overview (Fig. 1 of most papers)
- Data processing pipeline
- Module interaction diagram

### Style Template

```latex
\begin{tikzpicture}[
    font=\sffamily\footnotesize,
    >=Latex,
    box/.style={draw=black!70, rounded corners=2pt, line width=0.6pt,
                fill=black!3, minimum height=10mm, minimum width=22mm,
                inner sep=3pt},
    accent/.style={box, fill=acBlue!8, draw=acBlue!60},
    arrow/.style={-Latex, line width=0.6pt, draw=black!60},
    label/.style={font=\sffamily\scriptsize, text=black!70},
    node distance=8mm and 8mm,
]

\node[box] (input) {Input Data};
\node[accent, right=of input] (proc) {Processing};
\node[accent, right=of proc] (model) {GNN Model};
\node[box, right=of model] (output) {Detection};

\draw[arrow] (input) -- (proc);
\draw[arrow] (proc) -- (model);
\draw[arrow] (model) -- (output);

\end{tikzpicture}
```

### Design Rules
- **2–3 accent colors max**; rest neutral gray/black
- Use `fill=acBlue!8` (very light) for key modules, not saturated fills
- Consistent box dimensions: all boxes same height unless semantically different
- Arrow style: uniform across entire diagram
- Font: sans-serif, ≥ 7pt
- **No Chinese text** in submission version
- Fit within column width (single or double)
- Left-to-right (Western reading) or top-to-bottom flow
- Group related modules with a dashed bounding box or background shade

---

## 11. Network / Graph Visualization

### When to Use
- Attack graph / provenance graph examples
- Heterogeneous graph structure illustration
- Network topology

### TikZ Template

```latex
\begin{tikzpicture}[
    font=\sffamily\scriptsize,
    hps/.style={circle, draw=acBlue, fill=acBlue!15, minimum size=8mm, line width=0.5pt},
    nfr/.style={diamond, draw=acVermillion, fill=acVermillion!15, minimum size=8mm, line width=0.5pt},
    subj/.style={-Latex, acBlue, line width=0.7pt},
    obj/.style={-Latex, acOrange, dashed, line width=0.7pt},
    chain/.style={-Latex, acPurple, dotted, line width=0.7pt},
]

\node[hps] (h1) at (0,0) {$h_1$};
\node[nfr] (n1) at (2,0) {$n_1$};
\node[hps] (h2) at (4,0) {$h_2$};

\draw[subj] (h1) -- node[above, font=\sffamily\tiny] {subj} (n1);
\draw[obj] (n1) -- node[above, font=\sffamily\tiny] {obj} (h2);
\draw[chain] (h1) to[bend left=30] node[above, font=\sffamily\tiny] {chain} (h2);
\end{tikzpicture}
```

### Design Rules
- Node shape encodes node type (circle, diamond, rectangle, etc.)
- Node color from the paper's palette
- Edge style (solid/dashed/dotted) encodes relation type
- Edge thickness or color intensity encodes weight/confidence
- Legend box explaining node types and edge types (mandatory)
- Force-directed layout for organic graphs; hierarchical for DAGs

---

## 12. Timeline / Attack Progression Diagram

### When to Use
- Kill chain / attack phase visualization
- Temporal sequence of events
- Multi-stage attack illustration

### TikZ Template

```latex
\begin{tikzpicture}[
    font=\sffamily\footnotesize,
    phase/.style={draw=black!50, rounded corners=1pt, minimum height=7mm,
                  minimum width=18mm, inner sep=2pt, line width=0.5pt},
]
\node[phase, fill=acSky!30] (recon) at (0,0) {Recon};
\node[phase, fill=acOrange!30, right=4mm of recon] (exploit) {Exploit};
\node[phase, fill=acPurple!30, right=4mm of exploit] (c2) {C2};
\node[phase, fill=acVermillion!30, right=4mm of c2] (exfil) {Exfil};

\draw[-Latex, thick] (recon) -- (exploit);
\draw[-Latex, thick] (exploit) -- (c2);
\draw[-Latex, thick] (c2) -- (exfil);

% Time axis
\draw[->] ([yshift=-8mm]recon.south west) -- ([yshift=-8mm]exfil.south east)
    node[right, font=\sffamily\scriptsize] {Time};
\end{tikzpicture}
```

### Design Rules
- Color intensity increases with severity/stage progression
- Time axis always present with arrow and label
- Duration proportional to segment length (if data available)
- Causal arrows between phases

---

## 13. Waterfall / Incremental Contribution Chart

### When to Use
- Showing how each component contributes to final performance
- Ablation study visualization (alternative to table)

### Design Rules
- Start bar: full model performance
- Subsequent bars: show delta (positive = add, negative = remove)
- Positive delta: acGreen fill; Negative delta: acVermillion fill
- Connecting lines between bar tops
- Final bar: acBlue (the model's actual performance)
- Value labels on each bar showing the delta

---

## 14. Stacked Area Chart

### When to Use
- Showing composition over time (e.g., traffic type breakdown)
- Temporal trends of multiple components

### Design Rules
- Largest component at bottom for stability
- Semi-transparent fills (alpha 0.6–0.8)
- Legend order matches visual stacking order (bottom to top)
- Include total line on top (optional, solid black)
- X-axis: time with appropriate resolution

---

## 15. Bubble Chart / Multi-Dimensional Comparison

### When to Use
- Comparing methods on 3+ dimensions simultaneously
- X = Metric A, Y = Metric B, Size = Metric C (e.g., F1 vs Speed vs Params)

### Design Rules
- Bubble color by method (consistent with palette)
- Bubble size proportional to third variable (include size legend)
- Add method name labels near bubbles
- Grid for reference; logarithmic axes if ranges are wide
- Your method: slightly larger outline or glow effect for emphasis

---

## 16. Parallel Coordinates Plot

### When to Use
- Comparing methods across many metrics simultaneously (>5)
- Alternative to radar chart for higher dimensions

### Design Rules
- Each vertical axis = one metric, normalized to [0,1] or [min,max]
- One polyline per method
- Your method: thicker line (1.5pt) + full opacity
- Baselines: thinner (0.8pt) + reduced opacity (0.5)
- Axis labels at top; values at bottom
- Colorblind-safe line colors from palette

---

## 17. Matplotlib Export Settings (for Python-Generated Figures)

When generating figures with matplotlib for LaTeX inclusion:

```python
import matplotlib.pyplot as plt
import matplotlib as mpl

# Publication-quality settings
plt.rcParams.update({
    'font.family': 'sans-serif',
    'font.sans-serif': ['Arial', 'Helvetica'],
    'font.size': 8,
    'axes.labelsize': 9,
    'axes.titlesize': 9,
    'xtick.labelsize': 7,
    'ytick.labelsize': 7,
    'legend.fontsize': 7,
    'figure.figsize': (3.5, 2.5),  # single column width in inches
    'figure.dpi': 300,
    'savefig.dpi': 600,
    'savefig.bbox': 'tight',
    'savefig.pad_inches': 0.02,
    'pdf.fonttype': 42,  # embed fonts as TrueType
    'ps.fonttype': 42,
    'lines.linewidth': 1.0,
    'lines.markersize': 4,
    'axes.linewidth': 0.5,
    'grid.linewidth': 0.3,
    'grid.alpha': 0.3,
})

# Save as PDF (vector) for LaTeX
fig.savefig('figure.pdf', format='pdf')
```

### Double-Column Figure

```python
plt.rcParams['figure.figsize'] = (7.0, 3.0)  # double-column width
```

---

## 18. Shared Legend for Multi-Panel Figures

### When to Use
- Two or more subfigures share the same series/methods
- Avoids redundant legends in each panel

### Method 1: `legend to name` (pgfplots — Recommended)

```latex
\begin{figure*}[!t]
\centering
\begin{minipage}[b]{0.48\textwidth}\centering
\begin{tikzpicture}
\begin{axis}[
    width=\textwidth, height=0.618\textwidth,
    xlabel={Hyperparameter $\lambda$},
    ylabel={F1 Score (\%)},
    ymin=85, ymax=100,
    grid=major, grid style={dashed, gray!25},
    legend to name=sharedlegend,
    legend style={legend columns=4, font=\sffamily\scriptsize,
                  draw=none, column sep=6pt},
    every axis plot/.append style={line width=1.0pt, mark size=2.0pt},
    font=\sffamily\small,
    tick label style={font=\sffamily\footnotesize},
    tick align=outside,
]
\addplot[mark=*, acBlue] coordinates {(0.1,88) (0.3,93) (0.5,98) (0.7,96) (0.9,92)};
\addlegendentry{YourMethod}
\addplot[mark=square*, acVermillion] coordinates {(0.1,85) (0.3,89) (0.5,93) (0.7,91) (0.9,87)};
\addlegendentry{Baseline A}
\addplot[mark=triangle*, acGreen] coordinates {(0.1,82) (0.3,86) (0.5,91) (0.7,89) (0.9,84)};
\addlegendentry{Baseline B}
\addplot[mark=diamond*, acOrange, dashed] coordinates {(0.1,80) (0.3,84) (0.5,88) (0.7,86) (0.9,81)};
\addlegendentry{Baseline C}
\end{axis}
\end{tikzpicture}
\par\smallskip{\small\bfseries (a)} {\small Dataset-A}
\end{minipage}%
\hfill
\begin{minipage}[b]{0.48\textwidth}\centering
\begin{tikzpicture}
\begin{axis}[
    width=\textwidth, height=0.618\textwidth,
    xlabel={Hyperparameter $\lambda$},
    ylabel={F1 Score (\%)},
    ymin=85, ymax=100,
    grid=major, grid style={dashed, gray!25},
    every axis plot/.append style={line width=1.0pt, mark size=2.0pt},
    font=\sffamily\small,
    tick label style={font=\sffamily\footnotesize},
    tick align=outside,
]
\addplot[mark=*, acBlue] coordinates {(0.1,90) (0.3,94) (0.5,99) (0.7,97) (0.9,93)};
\addplot[mark=square*, acVermillion] coordinates {(0.1,86) (0.3,90) (0.5,94) (0.7,92) (0.9,88)};
\addplot[mark=triangle*, acGreen] coordinates {(0.1,83) (0.3,87) (0.5,92) (0.7,90) (0.9,85)};
\addplot[mark=diamond*, acOrange, dashed] coordinates {(0.1,81) (0.3,85) (0.5,89) (0.7,87) (0.9,82)};
\end{axis}
\end{tikzpicture}
\par\smallskip{\small\bfseries (b)} {\small Dataset-B}
\end{minipage}

\vspace{4pt}
\ref{sharedlegend}

\caption{Hyperparameter sensitivity analysis across two datasets.}
\label{fig:shared_legend}
\end{figure*}
```

### Method 2: Manual TikZ Legend Strip

```latex
% Place after all subfigure panels, before \caption
\vspace{4pt}
\centering
\begin{tikzpicture}[font=\sffamily\scriptsize]
  \draw[acBlue, line width=1pt] plot[mark=*] coordinates {(0,0)(0.5,0)};
  \node[right, inner sep=1pt] at (0.55,0) {YourMethod};
  %
  \draw[acVermillion, line width=1pt] plot[mark=square*] coordinates {(2.8,0)(3.3,0)};
  \node[right, inner sep=1pt] at (3.35,0) {Baseline A};
  %
  \draw[acGreen, line width=1pt] plot[mark=triangle*] coordinates {(5.6,0)(6.1,0)};
  \node[right, inner sep=1pt] at (6.15,0) {Baseline B};
  %
  \draw[acOrange, dashed, line width=1pt] plot[mark=diamond*] coordinates {(8.4,0)(8.9,0)};
  \node[right, inner sep=1pt] at (8.95,0) {Baseline C};
\end{tikzpicture}
```

### Method 3: matplotlib Shared Legend

```python
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(7.0, 2.5))

# Plot on both axes (same methods, different data)
for ax, data, title in [(ax1, data1, '(a) Dataset-A'), (ax2, data2, '(b) Dataset-B')]:
    ax.plot(x, data['ours'], '-o', color='#0072B2', label='YourMethod')
    ax.plot(x, data['bl_a'], '-s', color='#D55E00', label='Baseline A')
    ax.plot(x, data['bl_b'], '-^', color='#009E73', label='Baseline B')
    ax.set_title(title, fontsize=9)

# Shared legend below both axes
handles, labels = ax1.get_legend_handles_labels()
fig.legend(handles, labels, loc='lower center', ncol=3,
           fontsize=7, frameon=False, bbox_to_anchor=(0.5, -0.02))

fig.savefig('figure.pdf', bbox_inches='tight', pad_inches=0.02)
```

### Design Rules
- Generate legend entries in only ONE axis; reference elsewhere
- `legend columns=-1` makes all entries fit in one row
- Shared legend saves ~3–5mm vertical space per panel
- Verify legend line sample is long enough to distinguish dash patterns

---

## 19. Error Bar Templates

### 19a. Error Bars on Bar Chart (pgfplots)

```latex
\begin{axis}[
    width=\columnwidth,
    height=0.55\columnwidth,
    ybar=2pt,
    bar width=10pt,
    ylabel={F1 Score (\%)},
    ymin=70, ymax=100,
    symbolic x coords={Dataset-A, Dataset-B, Dataset-C},
    xtick=data,
    x tick label style={font=\sffamily\footnotesize},
    legend style={at={(0.5,-0.25)}, anchor=north, legend columns=3,
                  font=\sffamily\scriptsize, draw=none, column sep=6pt},
    font=\sffamily\small,
    ymajorgrids=true,
    grid style={dashed, gray!15},
]
% error bars: y dir=plus means one-sided (upward from bar top)
\addplot[fill=acBlue, draw=acBlue!80!black,
         error bars/.cd, y dir=both, y explicit,
         error bar style={line width=0.5pt, acBlue!70!black},
         error mark options={rotate=90, mark size=3pt, line width=0.5pt, acBlue!70!black},
] coordinates {
    (Dataset-A, 96.5) +- (0, 0.35)
    (Dataset-B, 95.3) +- (0, 0.42)
    (Dataset-C, 97.50) +- (0, 0.68)
};
\addlegendentry{YourMethod}

\addplot[fill=acVermillion!75, draw=acVermillion!80!black,
         error bars/.cd, y dir=both, y explicit,
         error bar style={line width=0.5pt, acVermillion!70!black},
         error mark options={rotate=90, mark size=3pt, line width=0.5pt},
] coordinates {
    (Dataset-A, 92.30) +- (0, 1.2)
    (Dataset-B, 93.70) +- (0, 1.5)
    (Dataset-C, 89.20) +- (0, 2.1)
};
\addlegendentry{Baseline A}
\end{axis}
```

### 19b. Confidence Band on Line Chart (pgfplots)

```latex
\begin{axis}[
    width=\columnwidth,
    height=0.618\columnwidth,
    xlabel={Epoch},
    ylabel={Validation Loss},
    grid=major, grid style={dashed, gray!25},
    font=\sffamily\small,
    tick align=outside,
    legend style={at={(0.97,0.97)}, anchor=north east,
                  font=\sffamily\scriptsize, draw=none},
]
% Upper bound
\addplot[name path=upper, draw=none, forget plot]
    coordinates {(1,0.85) (5,0.60) (10,0.42) (20,0.30) (30,0.25) (50,0.22)};
% Lower bound
\addplot[name path=lower, draw=none, forget plot]
    coordinates {(1,0.75) (5,0.48) (10,0.34) (20,0.24) (30,0.19) (50,0.16)};
% Shaded band
\addplot[fill=acBlue, fill opacity=0.15, draw=none, forget plot]
    fill between[of=upper and lower];
% Mean line
\addplot[acBlue, mark=*, mark size=1.5pt, line width=1.0pt]
    coordinates {(1,0.80) (5,0.54) (10,0.38) (20,0.27) (30,0.22) (50,0.19)};
\addlegendentry{YourMethod (mean $\pm$ SD)}

% Repeat for baselines with different colors...
\end{axis}
```

### 19c. Error Bars on Line Chart (pgfplots)

```latex
\addplot[acBlue, mark=*, line width=1.0pt,
         error bars/.cd, y dir=both, y explicit,
         error bar style={line width=0.4pt},
         error mark options={rotate=90, mark size=2.5pt, line width=0.4pt},
] coordinates {
    (64, 93.09) +- (0, 1.2)
    (128, 95.50) +- (0, 0.8)
    (256, 95.3) +- (0, 0.4)
    (512, 98.95) +- (0, 0.3)
};
```

### 19d. matplotlib Error Bars

```python
# Bar chart with error bars
methods = ['YourMethod', 'Baseline A', 'Baseline B']
means = [96.5, 92.30, 88.10]
stds = [0.35, 1.20, 1.80]
colors = ['#0072B2', '#D55E00', '#009E73']

bars = ax.bar(methods, means, color=colors, edgecolor=[c+'CC' for c in colors],
              width=0.6, zorder=3)
ax.errorbar(methods, means, yerr=stds, fmt='none', ecolor='black',
            elinewidth=0.5, capsize=4, capthick=0.5, zorder=4)
ax.set_ylabel('F1 Score (%)', fontsize=9)
ax.set_ylim(70, 100)
```

### Design Rules for Error Bars
- Cap width: proportional to bar width (roughly 40–60% of bar width)
- Line width: same as axis line (0.4–0.5 pt)
- Color: same as data series (or black for all, if cleaner)
- **Always state error bar type in caption** (SD, SEM, 95% CI)
- For dense line charts (>15 data points), prefer confidence bands over discrete error bars

---

## 20. Statistical Significance Markers

### 20a. Bracket with Stars (pgfplots/TikZ)

```latex
\begin{axis}[
    width=\columnwidth,
    height=0.6\columnwidth,
    ybar=2pt, bar width=14pt,
    ylabel={F1 Score (\%)},
    ymin=70, ymax=105,  % extra headroom for brackets
    symbolic x coords={Method A, Method B, Method C},
    xtick=data,
    font=\sffamily\small,
    clip=false,  % allow annotations outside axis box
]
\addplot[fill=acBlue, draw=acBlue!80!black]
    coordinates {(Method A, 99.1) (Method B, 92.3) (Method C, 88.5)};

% Significance bracket: Method A vs Method B
\draw[thin, black] (axis cs:Method A, 100) -- ++(0, 1.5pt)
    -- (axis cs:Method B, 101.5pt) -- ++(0, -1.5pt);
\node[font=\sffamily\scriptsize] at (axis cs:{Method A}, 102.5) [xshift=20pt] {**};

% Significance bracket: Method A vs Method C (taller bracket)
\draw[thin, black] (axis cs:Method A, 103) -- ++(0, 1.5pt)
    -- (axis cs:Method C, 104.5pt) -- ++(0, -1.5pt);
\node[font=\sffamily\scriptsize] at (axis cs:{Method B}, 105.5) {***};
\end{axis}
```

### 20b. matplotlib Significance Markers

```python
def add_significance(ax, x1, x2, y, h, text):
    """Add significance bracket between two bars."""
    ax.plot([x1, x1, x2, x2], [y, y+h, y+h, y], lw=0.5, c='black')
    ax.text((x1+x2)/2, y+h, text, ha='center', va='bottom', fontsize=7)

# Usage after plotting bars:
add_significance(ax, 0, 1, 100, 1.5, '**')   # bars at x=0 and x=1
add_significance(ax, 0, 2, 103, 1.5, '***')  # bars at x=0 and x=2
```

### Design Rules for Significance Markers
- Bracket line: thin (0.4–0.5 pt), black
- Text: centered above bracket, same font as tick labels
- Stagger brackets vertically for multiple comparisons
- Innermost comparison closest to bars; outermost highest
- Leave 2–3% headroom above tallest bar for annotations
- Use `clip=false` in pgfplots to allow annotations outside axis box
- Standard notation: ns (p≥0.05), \* (p<0.05), \*\* (p<0.01), \*\*\* (p<0.001)

---

## 21. Broken / Discontinuous Axis

### When to Use
- Bar chart where one value is much smaller/larger than others
- Y-axis must start at 0 (bar chart convention) but data clusters in narrow range (e.g., 85–100%)
- Avoids misleading truncation while keeping detail visible

### pgfplots Approach (Two Stacked Axes)

```latex
\begin{tikzpicture}
% Bottom axis (0 to break point)
\begin{axis}[
    name=bottom,
    width=\columnwidth, height=0.15\columnwidth,
    ybar, bar width=12pt,
    ymin=0, ymax=20,
    ytick={0, 10, 20},
    symbolic x coords={Ours, BL-A, BL-B, BL-C},
    xtick=data,
    font=\sffamily\small,
    tick label style={font=\sffamily\footnotesize},
    axis x line=bottom,
    axis y line=left,
    xticklabels={,,,,},  % hide x labels on bottom
]
\addplot[fill=acBlue!75] coordinates {(Ours,0) (BL-A,0) (BL-B,0) (BL-C,15)};
\end{axis}

% Top axis (break point to max)
\begin{axis}[
    at={(bottom.north west)}, anchor=south west,
    width=\columnwidth, height=0.45\columnwidth,
    ybar, bar width=12pt,
    ymin=80, ymax=100,
    ytick={80, 85, 90, 95, 100},
    ylabel={F1 Score (\%)},
    symbolic x coords={Ours, BL-A, BL-B, BL-C},
    xtick=data,
    x tick label style={font=\sffamily\footnotesize},
    font=\sffamily\small,
    axis x line=top,
    axis y line=left,
    ymajorgrids=true,
    grid style={dashed, gray!15},
]
\addplot[fill=acBlue!75, draw=acBlue!80!black] coordinates
    {(Ours,99.1) (BL-A,92.3) (BL-B,88.5) (BL-C,85.2)};
\end{axis}

% Break marks (diagonal lines)
\draw[thick, white] ([xshift=-2mm]bottom.north west) -- ++(4mm,0);
\draw ($(bottom.north west)+(-1mm,-1.5mm)$) -- ++(2mm,3mm);
\draw ($(bottom.north west)+(0.5mm,-1.5mm)$) -- ++(2mm,3mm);
\end{tikzpicture}
```

### Design Rules for Broken Axis
- Break marks: two short diagonal lines on the Y-axis at the discontinuity
- Bottom segment: small, showing 0 baseline
- Top segment: larger, showing the data range of interest
- Both segments share the same X-axis categories
- Alternative: use a single axis with `ymin=80` and note "Y-axis starts at 80" in caption (acceptable for line charts, not bar charts)

---

## 22. Enhanced Inset / Zoom Panel

### When to Use
- ROC curves clustering near (0, 1) corner
- Multiple methods with similar performance in a narrow range
- Convergence curves with interesting behavior in early epochs

### pgfplots Template with Connector

```latex
\begin{tikzpicture}
\begin{axis}[
    name=main,
    width=\columnwidth, height=\columnwidth,
    xlabel={False Positive Rate}, ylabel={True Positive Rate},
    xmin=0, xmax=1, ymin=0, ymax=1,
    grid=major, grid style={dashed, gray!20},
    legend style={at={(0.98,0.02)}, anchor=south east,
                  font=\sffamily\scriptsize, draw=none},
    font=\sffamily\small,
    tick align=outside,
]
% Main ROC curves
\addplot[acBlue, line width=1.2pt, mark=none] coordinates {
    (0,0) (0.001,0.85) (0.01,0.95) (0.05,0.98) (0.1,0.99) (1,1)};
\addlegendentry{YourMethod (AUC=0.995)}
\addplot[acVermillion, dashed, line width=1.0pt, mark=none] coordinates {
    (0,0) (0.005,0.70) (0.02,0.88) (0.1,0.95) (0.3,0.98) (1,1)};
\addlegendentry{Baseline A (AUC=0.9534)}
\addplot[acBlack!40, dashed, thin, mark=none] coordinates {(0,0) (1,1)};
\addlegendentry{Random}

% Zoom region indicator
\draw[acBlack!50, thin, dashed]
    (axis cs:0,0.85) rectangle (axis cs:0.15,1.0);
\end{axis}

% Inset axis (zoomed view)
\begin{axis}[
    at={($(main.north west)+(0.25\columnwidth,-0.05\columnwidth)$)},
    anchor=north west,
    width=0.45\columnwidth, height=0.45\columnwidth,
    xmin=0, xmax=0.15, ymin=0.85, ymax=1.0,
    xlabel={}, ylabel={},
    font=\sffamily\tiny,
    tick label style={font=\sffamily\tiny},
    tick align=outside,
    axis background/.style={fill=white},
    axis line style={thin},
    grid=major, grid style={dashed, gray!15},
]
\addplot[acBlue, line width=1.0pt, mark=none] coordinates {
    (0.001,0.85) (0.005,0.92) (0.01,0.95) (0.05,0.98) (0.1,0.99) (0.15,0.995)};
\addplot[acVermillion, dashed, line width=0.8pt, mark=none] coordinates {
    (0.005,0.70) (0.01,0.80) (0.02,0.88) (0.05,0.92) (0.1,0.95) (0.15,0.96)};
\end{axis}

% Connector lines from zoom region to inset
\draw[acBlack!30, thin]
    ($(main.north west)+(0.0\columnwidth,-0.15\columnwidth)$) --
    ($(main.north west)+(0.25\columnwidth,-0.05\columnwidth)$);
\draw[acBlack!30, thin]
    ($(main.north west)+(0.15\columnwidth,0)$) --
    ($(main.north west)+(0.7\columnwidth,-0.05\columnwidth)$);
\end{tikzpicture}
```

### Design Rules for Inset Panels
- Inset size: 35–50% of main plot width/height
- Position: typically in the empty region of the main plot (bottom-right for ROC)
- Border: thin solid line around inset axis
- Background: white (opaque, to cover main plot grid)
- Connect: thin dashed lines from zoom rectangle corners to inset corners
- Font: one step smaller than main plot (`\tiny` or `\scriptsize`)
- Include grid in inset for precise reading
- No legend in inset (inherits from main plot)
