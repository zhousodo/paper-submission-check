---
name: paper-submission-check
description: Pre-submission quality check for academic papers in LaTeX. Detects and removes AI-generated writing style (em dashes, focal words, -ing chains, negative parallelism, inflated significance), first-person pronoun overuse, symbol errors, capitalization inconsistencies, mixed Chinese/English punctuation, citation format issues, BIB file structural and formatting errors (entry types, title capitalization protection, journal name abbreviation consistency, DOI policies, author name formatting), content structure problems, table performance marking verification (bold/underline correctness for best/second-best values), reference content authenticity verification (web-based existence and summary accuracy checking for related work tables), and experimental discussion cross-section consistency (ablation grouping vs. discussion text alignment). Supports Elsevier, IEEE, Springer, ACM templates. Use when reviewing papers before journal or conference submission, removing AI writing traces, checking reference formatting, verifying table markings, auditing related work accuracy, checking experimental consistency, or when the user mentions paper checking, proofreading, submission preparation, quality review, AI style removal, table verification, reference verification, or ablation consistency.
---

# Paper Pre-Submission Quality Check

Systematic quality inspection for academic papers before journal/conference submission. Covers language, formatting, symbols, AI-style detection, LaTeX issues, citation correctness, BIB integrity, and content structure.

## Workflow

Track progress through all phases:

```
Pre-Submission Check Progress:
- [ ] Phase 0: Pre-Check (detect citation style, count authors, identify template)
- [ ] Phase 1: Pronoun & Subjectivity Check (with quantitative thresholds)
- [ ] Phase 2: AI-Generated Style Detection & Removal
- [ ] Phase 3: Symbol & Punctuation Errors
- [ ] Phase 4: Capitalization Consistency
- [ ] Phase 5: LaTeX-Specific Issues (citation-style-aware)
- [ ] Phase 6: Grammar & Language Quality
- [ ] Phase 7: Tables, Figures, Numbers & Equations (with cross-reference verification, performance marking audit, equation/algorithm verification)
- [ ] Phase 8: Content Structure & Completeness (abstract/introduction/paragraph/keywords/sections/experimental consistency)
- [ ] Phase 9: BIB File & Reference Format Check (with content authenticity verification)
- [ ] Phase 10: Final Pre-Submission Checklist
```

---

## Phase 0: Pre-Check

Before starting, determine these properties (they affect later phases):

### 1. Detect Citation Style

Search for `\bibliographystyle{}`:

| Style | Type | Citation Appearance | Sentence-Start Rule |
|-------|------|--------------------|--------------------|
| `elsarticle-num`, `IEEEtran`, `unsrt`, `plain` | **Numbered** | [1], [2] | Use `Author et al.~\cite{key}` or `The work in~\cite{key}` |
| `elsarticle-harv`, `apalike`, `natbib` | **Author-Year** | (Smith, 2024) | Can start with `\citet{key}` directly |

**CRITICAL for numbered style**: Do NOT write `the work of Author et al.~\cite{key}, who...` — this creates redundancy. Use one of:
- `Author et al.~\cite{key} proposed...` (author + number)
- `The work in~\cite{key} proposed...` (work-in-number reference)
- `In~\cite{key}, the authors proposed...` (in-number format)

### 2. Count Authors

Search for `\author{}`:
- **Single author**: "we/our" should be avoided, use passive or "this paper"
- **Two+ authors**: "we/our" is acceptable but should be used sparingly (see Phase 1 thresholds)

### 3. Identify Template & Journal

Search for `\documentclass{}` and `\journal{}`:
- Check if `\journal{}` still contains a **template default** value (e.g., "Nuclear Physics B" in elsarticle template — must be changed)
- Verify the document class matches the target journal

---

## Phase 1: Pronoun & Subjectivity Check

### Quantitative Thresholds

Count total occurrences of `\bwe\b` and `\bour\b` (excluding comments and code):

| Pronoun | Recommended Max (full paper) | Action if exceeded |
|---------|-----------------------------|--------------------|
| "we" | ≤15 | Replace excess with passive voice or "this paper" |
| "our" | ≤8 | Replace with "the proposed" / "the" |
| "us" | ≤3 | Replace with passive constructions |

### Section-Specific Rules

| Section | "we/our" Guidance |
|---------|-------------------|
| Abstract | Max 1-2; prefer "this paper proposes" |
| Introduction | Max 2-3 for contributions; "This paper proposes..." preferred for opening |
| Contributions list | "We propose..." is standard and acceptable |
| Method | Keep for formula derivations ("we compute..."); replace elsewhere with passive |
| Experiments | Prefer passive: "Experiments were conducted..." |
| Results/Discussion | Replace "our method" → "the proposed method" or method name |
| Conclusion | Prefer "This paper proposed..." or passive |
| Future Work | "we" is acceptable for describing future plans |

### The "our proposed X" Rule

**"our proposed X" is always redundant** — either "our X" or "the proposed X", never both. Search for and replace:

```
our proposed method   → the proposed method
our proposed approach → the proposed approach  
our proposed algorithm → the proposed algorithm
```

### Subjective Language (always replace)

| Subjective | Objective |
|------------|-----------|
| We believe that X | The results indicate X |
| In our opinion | Based on the experimental evidence |
| We feel this is important | This is significant because |
| Our method is the best | The proposed method outperforms |

---

## Phase 2: AI-Generated Style Detection & Removal

**This is the most critical phase.** Journal editors and reviewers increasingly detect AI-generated text. Major publishers (Elsevier, Springer Nature, IEEE) require disclosure of AI use, and undisclosed AI-generated text can lead to desk rejection or ethics investigation.

This phase has two parts: **Detection** (find AI patterns) and **Removal** (rewrite to sound human).

### Step 2a: Detect AI Patterns by Severity

Scan for patterns in order of severity (S1 = desk rejection risk, S2 = major revision risk):

**S1 — Desk Rejection Risk (fix every instance):**

| Pattern | What to Search | Threshold |
|---------|---------------|-----------|
| Em dash overuse | `—` or `---` | ≤3 total, 0 in abstract |
| 21 focal words | `delve`, `tapestry`, `landscape` (figurative), `nuanced`, `multifaceted`, `intricate`, `meticulous`, `pivotal`, `underscore`, `commendable` | 0 instances |
| Participial -ing chains | `, highlighting`, `, underscoring`, `, demonstrating`, `, showcasing`, `, reflecting`, `, contributing`, `, ensuring`, `, fostering` | ≤3 total |
| Tier-1 AI phrases | `garnered attention`, `pave the way`, `testament to`, `realm of`, `myriad of`, `plethora of`, `poised to revolutionize` | 0 instances |

**S2 — Major Revision Risk (fix all instances):**

| Pattern | What to Search | Threshold |
|---------|---------------|-----------|
| Negative parallelism | `not only...but also`, `not just...but`, `goes beyond...to` | ≤1 total |
| Rule of Three | Three abstract adjectives joined by "and" (e.g., "accuracy, efficiency, and robustness") | Replace with data |
| Inflated significance | `pivotal`, `crucial`, `vital`, `watershed`, `groundbreaking`, `transformative`, `paradigm shift` | Replace with specifics |
| Vague attributions | `researchers have shown`, `studies indicate`, `experts argue`, `it is widely recognized` | Replace with citations |
| Positivity bias | Missing limitations section, no failure cases discussed | Add honest limitations |

**S3 — Minor Revision (fix most):**

| Pattern | What to Search | Threshold |
|---------|---------------|-----------|
| Nominalizations | `utilization`, `implementation`, `optimization`, `facilitation` | Convert to verbs |
| Conjunctive saturation | `Furthermore`, `Moreover`, `Additionally` starting consecutive paragraphs | ≤3 each total |
| Formulaic transitions | "Having discussed X, we now turn to Y" | 0 instances |
| Correlative conjunctions | `whether...or`, `both...and`, `neither...nor` | ≤2 total |
| Hedge stacking | "could potentially suggest that it might" | ≤1 hedge per sentence |

### Step 2b: Rewrite with Human Style

Apply these golden rules when rewriting:

1. **Specific beats impressive**: Replace every adjective with a number
2. **Active beats nominal**: "X uses Y" not "the utilization of Y by X"
3. **Short beats long**: If a sentence has >30 words, split it
4. **Direct beats hedged**: "X improves Y" not "X could potentially improve Y"
5. **Cited beats claimed**: "Smith et al. showed X" not "researchers have shown X"
6. **Honest beats promotional**: Include real limitations, not vague future work
7. **Varied beats formulaic**: Don't start every paragraph with "Furthermore"
8. **Simple beats ornate**: "use" not "utilize", "show" not "underscore"

### Quick Before/After Reference

| AI-Generated | Human-Written |
|-------------|---------------|
| "This paper delves into the intricate challenges of anomaly detection" | "This paper addresses anomaly detection in streaming graphs" |
| "The method — leveraging sketch structures — achieves remarkable performance across diverse datasets" | "The method uses sketch structures and achieves 0.998 AUC on CICIDS2019" |
| "These findings not only validate the effectiveness but also underscore the potential, demonstrating superior adaptability" | "The method outperforms all baselines on three of four datasets (Table 3)" |
| "Furthermore, the comprehensive evaluation demonstrates both efficiency and robustness" | "The method processes 1M edges in 12 seconds with constant memory" |

For the complete AI phrase database, see [ai-phrases.md](ai-phrases.md).
For the detailed AI style removal guide with 13 patterns and rewriting strategies, see [ai-style-removal.md](ai-style-removal.md).

---

## Phase 3: Symbol & Punctuation Errors

### Mixed Chinese/English Punctuation (Critical for CJK authors)

Search for Chinese characters in English text:

| Chinese | English Correct | Search Pattern |
|---------|----------------|---------------|
| （ ） | ( ) | `\uff08`, `\uff09` |
| ， | , | `\uff0c` |
| 。 | . | `\u3002` |
| ； | ; | `\uff1b` |
| ： | : | `\uff1a` |
| " " | " " or `` '' (LaTeX) | `\u201c`, `\u201d` |
| ' ' | ' ' or ` ' (LaTeX) | `\u2018`, `\u2019` |
| 、 | , | `\u3001` |
| — | --- (LaTeX em-dash) | `\u2014` |

**All Chinese punctuation in English papers is an error. Zero tolerance.**

### Chinese Content Policy

| Location | Rule | Rationale |
|----------|------|-----------|
| LaTeX comments (`% ...`) | **Allowed** — Chinese comments are acceptable | Comments are invisible in PDF; they serve as author notes |
| `\zhpara{}` / `\zhcaption{}` | **Allowed** — if using bilingual comparison mode from `paper-english-polishing` | Controlled by `\showchinesetrue/false`; hidden in submission PDF |
| Body text (outside comments) | **Forbidden** — zero Chinese characters in submission PDF | Editors may desk-reject if Chinese appears in English paper |
| TikZ/figure labels | **Forbidden** — must be translated to English | Visible in PDF |
| Algorithm text | **Forbidden** — must be translated to English | Visible in PDF |
| Table cells | **Forbidden** — must be translated to English | Visible in PDF |
| `%====` section divider comments | **Allowed** — Chinese or English | Invisible in PDF |

**Verification**: Before submission, compile to PDF and search for any Chinese characters (Unicode range `\u4e00-\u9fff`). Only `\zhpara{}`-wrapped content (hidden by `\showchinesefalse`) and LaTeX `%` comments should remain.

### Dash Errors in LaTeX

| Usage | Correct LaTeX | Wrong |
|-------|--------------|-------|
| Hyphen (compound words) | `-` | |
| En-dash (number ranges) | `--` (e.g., pages 1--10) | `-` for ranges |
| Em-dash (parenthetical) | `---` | `--` for parenthetical |
| Minus sign (math) | `$-$` | `-` outside math mode |

### Space Errors

Search with regex for these patterns:

| Error | Correct | Regex |
|-------|---------|-------|
| Double space in text | Single space | `  +` (two or more spaces) |
| Missing space after period | `method. The` | `\.\w` |
| Space before comma | `method,` | ` ,` |
| No space after comma | `method, which` | `,[^ \\$~\n}]` |
| No space before `(` | `method (CMS)` | `\w\(` |

### Bracket Consistency

- All parentheses must be English `( )`, not Chinese `（ ）`
- Check for unmatched brackets
- Abbreviation pattern: full name + (ABBR) at first mention, then ABBR only

---

## Phase 4: Capitalization Consistency

### Section Titles

Check that ALL section/subsection titles follow ONE consistent style:

| Style | Example | Check |
|-------|---------|-------|
| Title Case | `\section{Related Work}` | First letter of major words capitalized |
| Sentence case | `\section{Related work}` | Only first word capitalized |
| ALL CAPS | `\section{EXPERIMENTS}` | Every letter capitalized |

**Error**: Mixing styles (e.g., "Related Work" + "EXPERIMENTS" + "method"). Pick one and apply consistently.

### Method/Model Names

- Consistency: same capitalization everywhere (e.g., "SBEAD" not "Sbead" or "sbead")
- First mention: full name + abbreviation
- After first mention: abbreviation only

### Acronyms & Terminology First-Use Protocol

**Core rule**: Every acronym, abbreviation, and domain-specific term must appear in FULL FORM at its first use, followed by the abbreviation in parentheses. All subsequent uses must use ONLY the abbreviation.

**Systematic verification protocol:**

1. **Build an acronym inventory**: Scan the entire paper for all uppercase sequences (2+ letters) and known abbreviations
2. **For each acronym, verify**:
   - [ ] First occurrence in **abstract**: full form + (ABBR) — abstract is self-contained
   - [ ] First occurrence in **body text**: full form + (ABBR) again — body is independent from abstract
   - [ ] All subsequent occurrences: abbreviation only (no re-expansion)
   - [ ] Never used before its definition in each context (abstract / body)
3. **Sentence-start rule**: Never start a sentence with an undefined acronym; never start with a bare number

**Common acronym errors:**

| Error | Example | Fix |
|-------|---------|-----|
| Used before definition | "APT attacks are dangerous. Advanced Persistent Threat (APT)..." | Move definition to first use |
| Defined but never used again | "Count-Min Sketch (CMS)" appears once, then always "Count-Min Sketch" | Use "CMS" consistently after definition |
| Defined only in abstract | "AUC" defined in abstract, used undefined in body | Re-define at first body occurrence |
| Inconsistent form | "GNN" in §3, "Graph Neural Network" in §5 | Use abbreviation after first definition |
| Overly abbreviated | Defining "HARP-GD-MIL-UCL" — too many nested acronyms | Limit nesting; spell out sub-components when possible |
| Well-known terms re-defined | Defining "CPU (Central Processing Unit)" in a CS paper | Skip definition for universally known terms (CPU, GPU, API, HTTP) |

**Concept and proper noun consistency:**

| Check | Rule |
|-------|------|
| Method names | Same capitalization and form everywhere: "HARP" not "Harp" or "harp" |
| Dataset names | Match the original publication exactly: "CICIDS2017" not "CIC-IDS-2017" |
| Metric names | Consistent form: "F1 score" or "F1-score" — pick one |
| Tool/library names | Official capitalization: "PyTorch" not "Pytorch" or "pytorch" |
| Venue names | Full official name at first use: "USENIX Security Symposium" not just "USENIX" |

---

## Phase 5: LaTeX-Specific Issues

### Non-Breaking Spaces Before Citations and References

```latex
% WRONG — line break may separate text from citation/reference
as shown in previous work \cite{smith2024}
as shown in Fig. \ref{fig:demo}
as shown in Table \ref{tab:results}

% CORRECT — non-breaking space prevents separation
as shown in previous work~\cite{smith2024}
as shown in Fig.~\ref{fig:demo}
as shown in Table~\ref{tab:results}
```

**Rule**: Always use `~` before `\cite{}`, `\ref{}`, `\eqref{}`.

Also check: `Table` or `Fig.` on one line with `\ref{}` on the next line → must be on same line with `~`.

### Citation at Sentence Beginning (Style-Dependent)

**For NUMBERED citation style** (`elsarticle-num`, `IEEEtran`):

```latex
% WRONG — renders as "[5] pioneered..."
\cite{lo2022graphsage} pioneered the application...

% CORRECT options for numbered style:
Lo et al.~\cite{lo2022graphsage} pioneered...     % Author + [number]
The work in~\cite{lo2022graphsage} pioneered...    % "work in [number]"
In~\cite{lo2022graphsage}, the authors pioneered...% "In [number],"

% WRONG — redundant construction
the work of Author et al.~\cite{key}, who employed... 
% "the work of Author [15], who" is awkward and redundant

% CORRECT
Author et al.~\cite{key} employed...
% OR
The work in~\cite{key} employed...
```

**For AUTHOR-YEAR style** (`elsarticle-harv`, `natbib`):

```latex
% Use \citet for textual citation at sentence start:
\citet{lo2022graphsage} pioneered...  % → "Lo et al. (2022) pioneered..."
```

### Redundant Formatting

Search for doubled commands:
```
\textbf{\textbf{...}}    → \textbf{...}
\textit{\textit{...}}    → \textit{...}
\emph{\emph{...}}        → \emph{...}
```

### Float Placement

- `[htbp]` is recommended for general use
- `[H]` (from float package) forces exact placement — use sparingly
- `[t]` is preferred for top-of-page figures in two-column layouts
- Check that ALL figures and tables have `\label` and are referenced in text

### Common LaTeX Errors

| Error | Fix |
|-------|-----|
| `\textit` for math variables | Use `$x$` not `\textit{x}` |
| Bare `%` in text | Use `\%` |
| Bare `_` in text | Use `\_` (e.g., `TON\_IoT`) |
| `\fontsize` without `\selectfont` | Always pair them |
| Missing `\label` after `\caption` | Add `\label` right after `\caption` |
| `\journal{}` still has template default | Change to target journal name |

---

## Phase 6: Grammar & Language Quality

### Contractions (Forbidden in Academic Writing)

```
don't → do not       can't → cannot        won't → will not
it's → it is         that's → that is      there's → there is
we've → we have      we're → we are        isn't → is not
```

### Duplicate Content Detection

Search for repeated phrases within the same paragraph or section:
- Repeated sentence openings within a paragraph
- Same phrase appearing twice in the same paragraph (e.g., "To formally define the problem... To formally define the problem...")
- Same idea restated in different words back-to-back

### Comma Splice Detection

A comma splice is two independent clauses joined only by a comma. Search for patterns like:
```
, When → . When  (or ; When)
, This → . This  (or ; This)  
, It   → . It    (or ; It)
, The  → . The   (if both clauses are complete sentences)
```

### Tense Consistency

| Section | Recommended Tense |
|---------|-------------------|
| Abstract | Past (what was done) + Present (what results show) |
| Introduction | Present (general facts) + Past (prior work) |
| Methods | Past (what was done) or Present (the algorithm does X) |
| Results | Past (experiments showed) |
| Discussion | Present (results suggest) + Future (future work will) |

### Redundant Phrases

| Verbose | Concise |
|---------|---------|
| in order to | to |
| due to the fact that | because |
| it can be seen that | (delete, start with the observation) |
| a large number of | many |
| in the case of | for / when |
| with respect to | for / regarding |
| on the other hand | however / alternatively |
| it is well-established that | (delete, state the fact directly) |
| it is important to note that | (delete, state the fact directly) |

### Common Grammar Errors in Non-Native English

| Error | Correct |
|-------|---------|
| "researches" (as plural noun) | "studies" or "research efforts" |
| "an anomaly detection" | "anomaly detection" (uncountable) |
| "datas" | "data" (already plural) |
| "informations" | "information" (uncountable) |
| "the Equation 1" | "Equation 1" or "Eq.~(1)" |
| "as shown in the Fig. 1" | "as shown in Fig.~1" |
| "in network environment" | "in the network environment" (missing article) |

---

## Phase 7: Tables, Figures & Numbers

### Tables

- Bold the best result in each column: `\textbf{0.9982}` — NOT double bold `\textbf{\textbf{...}}`
- Consistent decimal precision (e.g., all 4 decimal places in same table)
- Direction indicators: ↑ (higher is better), ↓ (lower is better)
- Use `booktabs` package (`\toprule`, `\midrule`, `\bottomrule`)
- Every table must be referenced in text

**Table caption and header length:**

| Check | Rule | Common Error |
|-------|------|--------------|
| Caption length | 1–3 sentences; self-contained but concise | 5+ sentence captions that replicate the main text |
| Column header length | ≤3 words per header; use abbreviations + footnote if needed | "The Average Precision Score of All Methods on Dataset X" as a single column header |
| Column header wrap | Headers should NOT wrap to 3+ lines; shorten or rotate | Multi-line headers that push table body below fold |
| Header abbreviation | Define abbreviation in table note if header is shortened | Header says "Prec." without footnote defining it |
| Table note length | ≤3 lines below table; for longer notes, move to main text | Full paragraph below table duplicating the discussion |

**Table caption position rule**: Caption ABOVE the table (`\caption{...}` before `\begin{tabular}`). Figure caption BELOW the figure.

### Figures

- Vector format (PDF/EPS) for plots, not PNG/JPG
- Captions should be self-contained (understandable without reading main text)
- Font size in figures should be readable (not too small after scaling)
- Every figure must be referenced in text
- Consistent color scheme across all figures

### Number Cross-Reference Verification

**CRITICAL**: Verify that every number mentioned in the text matches its source:

- Numbers in abstract must match numbers in tables/results
- Dataset statistics in text (e.g., "4,554,344 communications") must match Table values
- Claimed improvements in text ("49 percentage points higher") must be verifiable from tables
- Model names and their associated numbers must be consistent

| Check | Example Error |
|-------|---------------|
| Text vs Table mismatch | Text says "4,554,343" but Table says "4,554,344" |
| Abstract vs Results mismatch | Abstract says "0.998 AUC" but table shows "0.9982" |
| Percentage claim errors | "49 percentage points" from 0.51 to 0.998 = 0.488, not 0.49 |

### Number Formatting Rules

| Rule | Wrong | Correct |
|------|-------|---------|
| Spell out numbers < 10 at sentence start | "4 datasets were used" | "Four datasets were used" |
| Numbers with units together | "5 %" | "5\%" |
| Thousands separator | "4554344" | "4,554,344" |
| Decimal consistency | "0.997" and "0.9984" | Same decimal places in same table |

### Equation & Formula Verification

**CRITICAL**: Equations are where subtle errors hide — reviewers will verify them. Systematically check:

**Variable consistency:**

| Check | What to Verify | Common Error |
|-------|---------------|--------------|
| Cross-equation consistency | Same variable = same meaning in ALL equations | $d$ means "hash depth" in Eq.(1) but "dimension" in Eq.(5) |
| Text-equation match | Variables defined in text match equation usage | Text says "$H \in \mathbb{R}^{T \times 2h}$" but equation uses $H \in \mathbb{R}^{n \times d}$ |
| Subscript/superscript consistency | Index notation uniform | $x_i$ in one equation, $x^{(i)}$ in another for the same concept |
| Dimensionality match | Matrix/vector dimensions consistent across multiplications | $A \in \mathbb{R}^{n \times m}$ multiplied by $B \in \mathbb{R}^{k \times p}$ where $m \neq k$ |

**Formula correctness:**

- [ ] Every equation compiles without LaTeX errors
- [ ] Summation/product bounds are explicit: $\sum_{i=1}^{n}$ not $\sum_i$ (unless convention is clear)
- [ ] Fractions are readable: prefer `\frac{}{}` in display mode, inline use `/` or `\nicefrac`
- [ ] Parentheses match and use `\left( \right)` for large expressions
- [ ] Loss functions: all terms defined, weighting coefficients ($\lambda$, $\alpha$) introduced
- [ ] Gradient/optimization notation consistent: $\nabla$, $\partial$, $\arg\min$
- [ ] Units consistent where applicable (time in seconds/ms, memory in MB/GB)

**Equation numbering:**

- [ ] All equations referenced in text are numbered: `\begin{equation}` with `\label{eq:...}`
- [ ] Equations NOT referenced in text: consider unnumbered `\[...\]` or `equation*`
- [ ] No broken cross-references: search for "(??)" in compiled PDF
- [ ] Multi-line equations use `align` or `aligned` environment, NOT multiple `equation` blocks

**Algorithm pseudocode style:**

| Check | Rule | Example |
|-------|------|---------|
| Keywords | Use consistent keyword convention | `\textbf{Input:}`, `\textbf{Output:}`, `\textbf{for}`, `\textbf{while}`, `\textbf{return}` |
| Line numbering | All algorithm lines numbered | Use `algorithmic` with `\algsetup{linenosize=\small}` |
| Input/Output | Clearly specified at top of each algorithm block | `\REQUIRE` and `\ENSURE` in `algorithmic` |
| Variable naming | Same variable names as in equations and text | If equation uses $\theta$, pseudocode should use $\theta$ (not "params") |
| Comments | Brief, meaningful; in English for submission version | `\COMMENT{Update parameters}` |
| Indentation | Consistent; visually clear nesting | Auto-handled by `algorithmic` package |
| Caption | Descriptive; matches text references | `\caption{Training procedure of HARP}` |
| Cross-reference | Every algorithm referenced in text | `Algorithm~\ref{alg:...}` |
| Consistency with equations | Pseudocode operations match mathematical definitions | If Eq.(3) defines loss as $\mathcal{L}$, pseudocode should compute $\mathcal{L}$ |

### Table Performance Marking Verification Protocol

For every performance comparison table (main results, ablation, sensitivity analysis), systematically verify bold (best) and underline (second-best) markings:

**Step-by-step protocol:**

1. **Extract numerical values per column** — list all values for each metric column
2. **Rank values** — sort to identify true 1st (best) and 2nd (second-best)
3. **Cross-check markings** against actual ranking:
   - `\textbf{}` must be on the actual highest value (or lowest if lower-is-better)
   - `\underline{}` must be on the actual second-highest (or second-lowest)
4. **Verify every column independently** — a common error is ranking one column correctly but not another

**Tie handling:**
- Two values tied for best → both should be `\textbf{}`
- Two values tied for second-best → both should be `\underline{}`
- When best has a tie, the next distinct value is NOT second-best (it's third)

**Direction-aware columns:**
- Check for ↑ (higher-is-better) and ↓ (lower-is-better) indicators in headers
- Verify "best" means highest for ↑ metrics and lowest for ↓ metrics
- If no indicator, infer from metric semantics (F1↑, FPR↓, Latency↓, Parameters↓)

**Non-metric columns:**
- Columns that are deterministic properties of the configuration (e.g., "Parameter Count" as a function of hidden dimension, "Training Time" that scales deterministically) should generally NOT have bold/underline markings — they are reference information, not performance outcomes

**Common errors (ranked by frequency):**

| Error Type | Example | Fix |
|------------|---------|-----|
| Off-by-one ranking | Underline on 3rd-best, not 2nd-best | Re-rank all values in the column |
| Bold/underline swap | Best marked as underline, 2nd as bold | Swap the two markings |
| Direction confusion | Bolding highest parameter count when lower is better | Check ↑/↓ indicator and metric semantics |
| Missing tie handling | Only one of two identical best values bolded | Bold both tied values |
| Deterministic column marked | Parameter count bold/underlined | Remove markings from non-metric columns |
| Stale markings after data update | Values changed but markings not re-checked | Always re-verify after any data change |

---

## Phase 8: Content Structure & Completeness

### 8a. Abstract Quality

- [ ] Word count within journal limit (Elsevier: 150–300, IEEE: ≤200)
- [ ] Follows PRKS structure: Problem → Response → Key Results → Significance
- [ ] Problem section is ≤20% (not half the abstract)
- [ ] Contains specific numbers (AUC, improvement percentages, dataset names)
- [ ] No citations in abstract
- [ ] No undefined abbreviations (define in abstract even if defined in body)
- [ ] First sentence is NOT generic ("With the rapid development of...")
- [ ] Numbers in abstract match numbers in results tables

### 8b. Introduction Structure (CARS Model)

Verify the introduction follows the three-move structure:

- [ ] **Move 1 (Establish Territory)**: Field importance + key citations (1–2 paragraphs)
- [ ] **Move 2 (Establish Niche)**: Specific gap/limitation identified (1 paragraph)
- [ ] **Move 3 (Occupy Niche)**: "This paper proposes..." + contributions list + outline (1–2 paragraphs)
- [ ] Move 2 → Move 3 transition is natural (gap directly motivates the proposed method)
- [ ] Contributions: 3–5 numbered items, each specific and measurable
- [ ] Paper outline paragraph references all sections with `\ref{}`

### 8c. Paragraph & Sentence Structure

- [ ] No paragraphs >250 words (flag and suggest splitting)
- [ ] No single-sentence paragraphs (flag and suggest merging/expanding)
- [ ] Conclusion is multi-paragraph (not one giant block)
- [ ] No sentences >35 words (flag and suggest splitting)
- [ ] Sentence length varies naturally (not all the same length)

### 8d. Keywords

- [ ] Count: 4–6 (Elsevier) or 3–10 (IEEE)
- [ ] At least 1–2 keywords NOT from the title (for discoverability)
- [ ] Mix of broad (field) + specific (method/technique) terms
- [ ] No redundant keywords
- [ ] Standard terms used (not invented phrases)

### 8e. Section-Specific Structure

- [ ] **Related Work**: Organized thematically (not laundry list) + each theme has summary→limitations→citations + ends with gap summary paragraph that directly motivates the proposed method
- [ ] **Method**: All symbols defined at first use + notation table (if >10 symbols) + method overview figure + core intuition in plain language + algorithm pseudocode + design choice justifications + time/space complexity analysis
- [ ] **Experiments**: Baselines use published defaults (stated explicitly) + runs averaged with std + dataset statistics table + metrics mathematically defined + results interpreted (not just presented) + ablation study + fair comparison (same hardware, same splits, same parameters)
- [ ] **Discussion**: Result interpretation (not repetition) + comparison with ≥2 prior methods + mechanism explanation (WHY it works) + ≥3 specific limitations + broader implications
- [ ] **Limitations**: Present (in Discussion or Conclusion) — specific, not vague; each limitation names a concrete dataset, scenario, or parameter constraint
- [ ] **Conclusion**: Multi-paragraph (2–4) + strongest takeaway first + NOT a copy of the abstract + answers Introduction's research question + specific future work + no new information + no overclaiming

### 8f. Terminology Consistency

Pick one term and use throughout:

| Inconsistent | Pick One |
|-------------|----------|
| "anomaly detection" / "anomalous detection" / "AD" | Pick one form after definition |
| "graph stream" / "streaming graph" / "graph streams" | Pick one form |
| "dataset" / "data set" / "benchmark" | Be consistent |
| "TON_IoT" / "TON-IoT" / "TON\_IoT" | Match the original paper's name with `\_` in LaTeX |

### 8g. Cross-Section Consistency

- [ ] Abstract numbers = Table numbers
- [ ] Each numbered contribution maps to a specific section
- [ ] Introduction gap = Method solution
- [ ] Method symbols = Experiment parameters
- [ ] Conclusion claims ⊆ Results data

### 8h. Experimental Discussion Consistency

The Discussion/Analysis section of experiments is a frequent source of inconsistencies. Systematically verify:

**Ablation table vs. Discussion text grouping:**

When the ablation table organizes components into logical tiers or groups (e.g., "Cross-modal alignment", "Training strategy", "Model architecture"), verify that:

- [ ] The Discussion text references the SAME grouping as the table
- [ ] Each component is attributed to the CORRECT tier/group
- [ ] Delta values cited in text (e.g., "$\Delta$F1 = -3.67%") match the actual table data
- [ ] Component contribution descriptions match what the component actually does

| Error Type | Example | Impact |
|------------|---------|--------|
| Component miscategorized | Discussing "uncertainty-modulated loss" under "Model architecture" when it belongs under "Training strategy" | Misleads readers about design decisions |
| Delta value mismatch | Text says "$\Delta$F1 = -5.2%" but table shows -3.67% | Factual error; reviewer will catch |
| Contribution over-attribution | Claiming a component "contributes the largest gain" when another component has larger delta | Undermines credibility |

**Baseline description accuracy:**

- [ ] Each baseline's description matches its actual method (e.g., a method's acronym prefix meaning "Edge-feature" should not be described as "Efficient")
- [ ] Baseline evaluation scope differences are explicitly acknowledged (e.g., host-side methods evaluated at HBR granularity vs. session-level NFR methods)
- [ ] First-use acronym expansion is provided even in the experimental section (readers may jump directly to experiments)

**Discussion language rigor:**

- [ ] Causal claims are justified: "X caused improvement" only when ablation evidence supports it
- [ ] Comparative statements use correct grammar: "A与B均不同" not "A与B与C不同" (for Chinese); use parallel structure in English
- [ ] Hedging is appropriate: "suggests" vs "proves" vs "indicates" — matched to evidence strength

### Self-Citation Anonymization (for blind review)

- No author names in text if double-blind
- Replace "In our previous work [X]" with "In prior work [X]"
- Check PDF metadata (author field)

For the detailed structure and writing guide (CARS model, paragraph standards, keywords strategy, section rules), see [paper-structure-guide.md](paper-structure-guide.md).

---

## Phase 9: BIB File & Reference Format Check

**The reference list is where reviewers frequently find careless errors. Studies show manuscripts with citation errors face 40% higher rejection rates.**

### 9a. Structural Integrity

Check every BIB entry for:

1. **Matching braces**: Every `@type{key,` must have a closing `}`
2. **No nested entries**: One entry must be fully closed before the next begins
3. **Required fields present**: `title`, `author`, `year` at minimum
4. **No trailing comma after last field** (some BibTeX implementations fail)

### 9b. Entry Type Correctness

Verify each entry uses the correct `@type` — Google Scholar and digital libraries frequently assign wrong types:

| Check | Action |
|-------|--------|
| Conference paper as `@article` | Change to `@inproceedings`, add `booktitle` |
| arXiv preprint as `@inproceedings` | Change to `@misc` with `eprint` field |
| Book chapter as `@inbook` | Verify: `@incollection` if different author from book |
| Entry has `booktitle` but is `@article` | Change to `@inproceedings` |
| Entry has `journal` but is `@inproceedings` | Change to `@article` |

### 9c. Author Name Format

| Check | Correct | Wrong |
|-------|---------|-------|
| Spaces between initials | `J. D. Owens` | `J.D. Owens` (BibTeX treats as single first name) |
| Hyphenated names braced | `Wu-{chun} Feng` | `Wu-chun Feng` (may split incorrectly) |
| Special characters escaped | `M{\"u}ller` | `Müller` (encoding error risk) |
| Names match published paper | Use what's printed on the paper | Don't normalize or abbreviate arbitrarily |

### 9d. Title Capitalization Protection

**CRITICAL**: Protect proper nouns, acronyms, method names, and dataset names with individual `{}` braces. NEVER double-brace the entire title.

| Must Protect | Examples | BibTeX Syntax |
|-------------|---------|---------------|
| **Acronyms** | GPU, CNN, LSTM, IoT, CMS | `{GPU}`, `{CNN}`, `{LSTM}`, `{IoT}`, `{CMS}` |
| **Method/Model names** | BERT, ResNet, MIDAS, SBEAD, Adam | `{BERT}`, `{ResNet}`, `{MIDAS}`, `{SBEAD}` |
| **Dataset names** | ImageNet, CICIDS2017, DARPA, TON\_IoT | `{ImageNet}`, `{CICIDS2017}`, `{DARPA}` |
| **Proper nouns** | Gaussian, Bayesian, Markov, Fourier | `{G}aussian`, `{B}ayesian`, `{M}arkov` |
| **Tool names** | TensorFlow, PyTorch, Spark, Hadoop | `{TensorFlow}`, `{PyTorch}` |

```
% WRONG — entire title double-braced (prevents ALL case conversion)
title = {{Efficient GPU Computing Using CUDA for Deep Learning}}

% WRONG — acronyms not protected (may become lowercase)
title = {Efficient GPU computing using CUDA for deep learning}

% CORRECT — individual protection
title = {Efficient {GPU} Computing Using {CUDA} for Deep Learning}
```

### 9e. Journal Name Consistency (Publisher-Dependent)

| Publisher | Rule | Example |
|-----------|------|---------|
| **IEEE** | **Must abbreviate** per ISO 4 | `IEEE Trans. Knowl. Data Eng.` (NOT full name) |
| **Elsevier** | **Full name** | `Information Processing & Management` (NOT abbreviated) |
| **ACM** | **Full name** | `ACM Computing Surveys` (NOT abbreviated) |
| **Springer** | Check specific journal | Varies |

**Consistency rule**: ALL journal names in one bibliography must follow the SAME convention (all abbreviated OR all full). Never mix.

### 9f. Page Numbers, DOI, and Month

| Field | Correct | Wrong |
|-------|---------|-------|
| Pages | `pages = {35--49}` (en-dash) | `pages = {35-49}` (hyphen) |
| DOI | `doi = {10.1145/1234567}` (numbers only) | `doi = {https://doi.org/10.1145/1234567}` (full URL) |
| DOI + URL | Keep `doi` only, remove redundant `url` | Both `doi` and `url` pointing to same DOI |
| Month | `month = jun` (BibTeX macro, no quotes) | `month = {june}` or `month = {June}` |

**DOI display note**: `elsarticle-num.bst` does NOT render the `doi` field in output. Include it in .bib for your records, but it will not appear in the compiled PDF. ACM `acmart` REQUIRES DOI and does display it.

### 9g. Cross-Reference Check

- [ ] Every `\cite{key}` in .tex has a matching entry in .bib
- [ ] No "Citation undefined" warnings
- [ ] No duplicate citation keys in .bib
- [ ] Author names in text match BIB first author (e.g., "Lo et al." → BIB has `Lo` as first author)
- [ ] In-text `\cite{}` uses `~` non-breaking space before it
- [ ] Grouped citations sorted numerically: `\cite{a,b,c}` renders as [6, 8, 10] not [8, 6, 10]
- [ ] Digital library exports manually verified and corrected

### 9h. Reference Content Verification (Related Work Tables)

When the paper contains a comparison/summary table of related work methods, verify both existence and accuracy:

**Step 1: Verify paper existence** — For each cited paper, confirm via web search:

| Verification Method | Priority | When to Use |
|---------------------|----------|-------------|
| DOI resolution (doi.org/...) | Highest | When DOI is available |
| Google Scholar title search | High | Primary search method |
| DBLP / Semantic Scholar | High | Cross-verification |
| Publisher site (IEEE Xplore, ACM DL) | Medium | For publisher-specific papers |
| arXiv search | Medium | For preprints |

**Step 2: Verify bibliographic accuracy:**

- Author names match the actual published paper
- Venue/journal name is correct and complete
- Year matches the actual publication year
- Entry type (@article, @inproceedings, @inbook) matches the publication venue
- Vague author listings (e.g., "and others") should be replaced with complete author lists

**Step 3: Verify table summary accuracy** — For each paper's row in the comparison table:

| Field to Check | Verification Source | Common Error |
|----------------|--------------------|--------------| 
| Data Source | Paper abstract/experiments section | Overgeneralizing (e.g., "flow + packet" when paper only uses flows) |
| Core Technique | Paper abstract/method section | Mischaracterizing the technique (e.g., "Efficient X" when "E" stands for "Edge-feature") |
| Key Innovation | Paper abstract/contributions | Inventing innovations not claimed by the original authors |

**Red flags requiring immediate action:**

| Red Flag | Risk | Action |
|----------|------|--------|
| Paper not found in any database | May be fabricated or not yet published | Verify DOI; if unverifiable, flag to author |
| Author names don't match title | BIB entry may be wrong | Cross-check with publisher records |
| DOI resolves to different paper | Copy-paste error | Correct DOI |
| Table description contradicts paper abstract | Inaccurate summarization | Rewrite to match actual paper content |
| Very recent paper from small venue | May not be indexed yet | Not necessarily fake, but note verification difficulty |

**Example of common caught errors:**
- Table described a method's acronym prefix as "Efficient" → actual paper shows it stands for "Edge-feature" (misinterpreted abbreviation)
- Table listed a paper's author incorrectly → actual publisher record shows a different first author (auto-generated BIB entries are unreliable)

### 9i. Digital Library Export Verification

**NEVER trust auto-generated BibTeX** from Google Scholar, IEEE Xplore, ACM DL, or DBLP:

| Source | Common Errors |
|--------|-------------|
| **Google Scholar** | Wrong entry type (conference → `@article`), missing fields, wrong capitalization |
| **IEEE Xplore** | Scrambled venue names, no spaces between initials, month as string, page format wrong |
| **ACM DL** | Title case wrong, DOI as full URL, redundant metadata fields |
| **DBLP** | Generally better, but still verify capitalization and entry type |

For the complete reference format guide with examples, see [reference-format-guide.md](reference-format-guide.md).

---

## Phase 10: Final Pre-Submission Checklist

```
Final Submission Checklist:

Cross-References & Labels:
- [ ] All figures are referenced in text
- [ ] All tables are referenced in text  
- [ ] All equations are numbered (if referenced) and referenced
- [ ] All \label{} have corresponding \ref{} (and vice versa)

Terminology & Acronyms:
- [ ] All acronyms defined at first use (abstract + body independently)
- [ ] No acronym used before its definition
- [ ] Concept names consistent throughout (same capitalization, same form)
- [ ] Dataset names match original publication exactly
- [ ] Method/tool names use official capitalization

Equations & Algorithms:
- [ ] All equation variables defined in text before or immediately after equation
- [ ] Variable meaning consistent across all equations (no symbol reuse for different concepts)
- [ ] Matrix/vector dimensions consistent across multiplications
- [ ] Algorithm pseudocode: input/output specified, keywords consistent, variables match equations
- [ ] No "(??)" broken cross-references in compiled PDF

Tables:
- [ ] Table bold/underline markings verified (best/second-best per column)
- [ ] Table captions concise (1–3 sentences), positioned ABOVE table
- [ ] Column headers ≤3 words; abbreviated headers have footnotes
- [ ] Decimal places consistent within each column

Language & Style:
- [ ] Tense consistency per section (past for methods/results, present for facts)
- [ ] Zero Chinese characters in submission PDF body text
- [ ] Chinese LaTeX comments (% ...) are acceptable and may remain
- [ ] No orphan/widow lines (single line at top/bottom of page)
- [ ] No overfull hbox warnings (text overflowing margins)
- [ ] Spell check completed

Submission Metadata:
- [ ] \journal{} contains correct target journal name (not template default)
- [ ] Page limit satisfied
- [ ] Bibliography formatted per journal style
- [ ] Author information correct (for non-blind submissions)
- [ ] Author information removed (for blind submissions)
- [ ] Supplementary materials prepared if required
- [ ] Cover letter prepared if required
- [ ] Highlights / graphical abstract if required (Elsevier)
- [ ] CRediT author statement (Elsevier)
- [ ] Data availability statement
- [ ] Declaration of competing interests
- [ ] All co-authors have approved the manuscript
- [ ] LaTeX compiles with zero errors and minimal warnings
- [ ] BIB file compiles without errors

Content Verification:
- [ ] Related work table references verified for existence and summary accuracy
- [ ] Experimental discussion consistent with ablation table grouping
- [ ] All baseline descriptions match actual method names and techniques
```

---

## Report Format

After running all checks, produce a structured report:

```markdown
# Paper Pre-Submission Check Report

## Paper Info
- Template: [elsarticle / IEEEtran / ...]
- Citation Style: [numbered / author-year]
- Authors: [count]
- Target Journal: [name]

## Summary
- Total issues found: XX
- Critical (must fix): XX
- Warning (should fix): XX  
- Suggestion (optional): XX

## Pronoun Statistics
- "we" count: XX (threshold: ≤15) [OK/OVER]
- "our" count: XX (threshold: ≤8) [OK/OVER]

## Critical Issues
1. [Line XX] BIB entry missing closing brace → fix brace structure
2. [Line XX] Chinese bracket found: （ACCURACY）→ (ACCURACY)
3. [Line XX] AI-style phrase: "has garnered significant attention" → "has been widely studied"
4. ...

## Warnings
1. [Line XX] Inconsistent capitalization: "EXPERIMENTS" vs "Related Work"
2. [Line XX] Missing ~ before \cite: "work \cite{}" → "work~\cite{}"
3. [Line XX] Number mismatch: text says "4,554,343" but Table says "4,554,344"
4. ...

## Suggestions  
1. [Line XX] Consider replacing "novel" (used 4 times) with specific descriptors
2. [Line XX] Abstract opens with generic statement — consider starting with contribution
3. ...
```

## Additional Resources

- For the complete AI-generated phrase database, see [ai-phrases.md](ai-phrases.md)
- For the detailed AI style removal guide (13 patterns, severity levels, rewriting strategies), see [ai-style-removal.md](ai-style-removal.md)
- For the paper structure and writing quality guide (CARS model, paragraph/sentence standards, keywords, section rules), see [paper-structure-guide.md](paper-structure-guide.md)
- For the reference format and bibliography quality guide (entry types, title protection, journal abbreviations, DOI policies), see [reference-format-guide.md](reference-format-guide.md)
- For the detailed pre-submission checklist with per-publisher requirements, see [checklist.md](checklist.md)
