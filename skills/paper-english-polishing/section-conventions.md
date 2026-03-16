# Section-Specific Writing Conventions

Detailed per-section writing patterns for top-tier venue papers. Reference from SKILL.md during polishing.

## Contents

- [Abstract](#abstract)
- [Introduction](#introduction)
- [Related Work](#related-work)
- [Method](#method)
- [Experiments](#experiments)
- [Discussion / Limitations](#discussion--limitations)
- [Conclusion](#conclusion)

---

## Abstract

### Structure: PRKS (4 components, 150-300 words)

| Component | Budget | What to Write |
|-----------|--------|---------------|
| **P**roblem | 15–20% | Specific problem, NOT "With the rapid development of..." |
| **R**esponse | 25–30% | "This paper proposes X, which..." — one clear sentence |
| **K**ey Results | 35–40% | Concrete numbers: AUC, F1, improvement margins, dataset names |
| **S**ignificance | 10–15% | Why it matters; what's unique about the approach |

### Abstract Rules

- First sentence: NEVER start with generic statements ("In recent years...", "With the development of...")
- First sentence: State the specific problem or challenge directly
- Include at least 2-3 concrete numbers from results
- Define abbreviations independently from body text
- No citations in abstract
- No "our proposed" — use "the proposed" or method name
- Tense: past for what was done, present for what results show

### Abstract Templates

**Template A (problem-first)**:
> [Specific problem statement]. [Why existing solutions fall short — 1 sentence]. This paper proposes [METHOD], which [core mechanism — 1 sentence]. [METHOD] [key design choice]. On [DATASET1] and [DATASET2], [METHOD] achieves [METRIC1] of [VALUE1] and [METRIC2] of [VALUE2], [comparison with baselines]. [Broader implication or unique capability].

**Template B (challenge-first)**:
> [Challenge description with concrete scope]. Existing approaches [specific limitation]. [METHOD] addresses this by [mechanism]. [Design detail that enables the contribution]. Experiments on [datasets] demonstrate [specific numbers]. [What this enables that was not possible before].

---

## Introduction

### Structure: CARS Model (Create A Research Space)

| Move | Paragraphs | Purpose | Key Phrases |
|------|------------|---------|-------------|
| **Move 1**: Establish Territory | 1-2 | Field importance + background | "X is a critical challenge in...", "Recent advances in..." |
| **Move 2**: Establish Niche | 1 | Identify the specific gap | "However, existing methods...", "A key limitation is..." |
| **Move 3**: Occupy Niche | 1-2 | Propose solution + contributions | "This paper proposes...", "The main contributions are:" |

### Move 1 Rules
- Start with a **concrete, specific** motivating example or statistic (NOT "In recent years, with the development of...")
- Cite 5-10 key works to establish the research landscape
- End with what the field has achieved (setting up the gap)

### Move 2 Rules
- Identify a **specific, falsifiable** gap (not "there is room for improvement")
- The gap must DIRECTLY motivate the proposed method
- Bad: "Few works have studied X" (vague)
- Good: "Existing methods detect anomalies but cannot simultaneously identify which specific events constitute the evidence, leaving analysts without actionable attribution" (specific)

### Move 3 Rules
- "This paper proposes [METHOD]" — clear, direct
- Contributions list: 3-5 numbered items
- Each contribution: **verb-led**, **specific**, **measurable**

### Contribution Templates

| Type | Template |
|------|----------|
| Algorithmic | "We propose [NAME], a [SHORT DESCRIPTION] that [KEY MECHANISM]. Unlike [COMPARISON], [NAME] [SPECIFIC DIFFERENCE]." |
| Theoretical | "We provide theoretical guarantees showing that [PROPERTY], formalized as [WHAT] (Proposition X)." |
| Empirical | "We evaluate [NAME] on [N] datasets against [N] baselines, reporting [SPECIFIC METRIC IMPROVEMENTS]." |

---

## Related Work

### Organization: Thematic, NOT Chronological

Structure each theme as:

```
\subsection{Theme Name}

[Theme overview — what this line of work does, 1-2 sentences]

[Work 1 description. Work 2 description. Work 3 description.]

[Summary of achievements + specific limitations relevant to THIS paper.]
```

### Rules

- Each paragraph discusses 3-5 related works under one theme
- End each theme with limitations that motivate the proposed work
- Final paragraph: synthesize across all themes, state what's missing
- Use PAST tense for what prior work did ("proposed", "introduced", "demonstrated")
- Use PRESENT tense for still-true facts ("GNNs are computationally expensive")
- For numbered citation style: `Author et al.~\cite{key} proposed...` or `The work in~\cite{key} proposed...`
- NEVER: `\cite{key} proposed...` (renders as "[5] proposed...")

### Comparison Phrases

| Purpose | Phrases |
|---------|---------|
| Grouping | "A line of work focuses on...", "Several approaches tackle..." |
| Contrast | "In contrast to [X], [Y] ...", "While [X] addresses ..., it does not ..." |
| Limitation | "However, these methods [specific limitation]", "A common limitation is..." |
| Positioning | "HARP differs from these approaches in that...", "Unlike [X], HARP..." |

---

## Method

### Structure Template

```
\section{Method}

[1-2 sentence overview + figure reference]

\subsection{Problem Formulation}
[Formal definition, notation table]

\subsection{Component 1}
[Description with equations]

\subsection{Component 2}
[Description with equations]

\subsection{Training}
[Loss function, optimization, implementation details]

\subsection{Theoretical Analysis} (if applicable)
[Propositions, proofs]
```

### Rules

- Define EVERY symbol at first use: "$H \in \mathbb{R}^{T \times 2h}$, where $T$ is the sequence length and $h$ is the hidden dimension"
- Use notation table if >10 symbols
- Include method overview figure with caption that is self-contained
- Provide design justifications: "We choose GRU over Transformer because..."
- Algorithm pseudocode for the training procedure
- Complexity analysis (time and space)

### Equation Introduction Patterns

| Pattern | Example |
|---------|---------|
| Direct | "The hidden states are computed as:" |
| Motivated | "To capture bidirectional dependencies, we encode the sequence as:" |
| Named | "The sufficiency loss measures the prediction gap:" |

**After equation**: Always explain the variables and the intuition, not just the math.

---

## Experiments

### Standard Structure

```
\subsection{Experimental Setup}
  - Datasets (with statistics table)
  - Baselines (with brief justification)
  - Evaluation Metrics (defined)
  - Implementation Details

\subsection{Main Results}
  - Table + interpretation (not just "Table X shows the results")

\subsection{Ablation Study}
  - Systematic removal/replacement of components

\subsection{Sensitivity Analysis / Further Analysis}
  - Hyperparameter sensitivity, case studies, etc.
```

### Rules

- Dataset statistics in a table (samples, features, class distribution, split sizes)
- State baseline implementations: "We use the official implementation" or "We re-implement following [citation]"
- Results tables: bold best, underline second-best, consistent decimal places
- **Interpret, don't just present**: "HARP outperforms SeqGRU by 0.36 percentage points in AP, indicating that the attribution module provides an inductive bias that benefits ranking quality"
- Report confidence intervals or standard deviations for key results
- Negative results are valuable: report and explain them honestly

### Results Discussion Phrases

| Purpose | Phrases |
|---------|---------|
| Highlight best | "[METHOD] achieves the highest [METRIC] of [VALUE]" |
| Compare | "[METHOD] outperforms [BASELINE] by [MARGIN] in [METRIC]" |
| Explain | "This improvement can be attributed to..." |
| Qualify | "The difference is not statistically significant due to..." |
| Negative | "Contrary to expectation, [X] does not improve [Y], likely because..." |

---

## Discussion / Limitations

### Rules

- Be specific: "The 128-event sequence truncation may miss early signals in month-long APT campaigns" (not "the method has limitations")
- Each limitation: what, why it matters, potential mitigation
- Separate "Discussion" (interpret results) from "Limitations" (honest shortcomings)
- At least 3 concrete limitations

---

## Conclusion

### Structure: 3-4 Paragraphs

| Paragraph | Content |
|-----------|---------|
| 1 | Strongest contribution + how it works (1-2 sentences) |
| 2 | Key experimental findings with numbers |
| 3 | Limitations summary (brief, referencing Limitations section) |
| 4 | Future work directions (specific, not vague) |

### Rules

- NOT a copy of the abstract
- Past tense for what was done, future for what will be done
- No new information or results not in the paper
- End on a forward-looking note
- Do not overclaim: "demonstrates" not "proves"
