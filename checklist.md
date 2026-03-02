# Detailed Pre-Submission Checklist

## Per-Journal/Conference Requirements

Before submission, verify these journal-specific items:

### Elsevier Journals (e.g., IPM, COMNET, JSA, KBS, ESWA)

- [ ] Use `elsarticle` document class
- [ ] `\journal{}` changed from template default (e.g., NOT "Nuclear Physics B")
- [ ] Correct bibliography style: `elsarticle-num` (numbered) or `elsarticle-harv` (author-year)
- [ ] Highlights file (3-5 bullet points, max 85 chars each)
- [ ] Graphical abstract (if required)
- [ ] Keywords: 4-6 keywords separated by `\sep`
- [ ] CRediT author statement
- [ ] Data availability statement
- [ ] Declaration of competing interests
- [ ] Acknowledgments (funding info)
- [ ] Cover letter
- [ ] Numbered citation style: verify sentence-start citations use author names
- [ ] No `\usepackage{natbib}` unless using `elsarticle-harv`

### IEEE Journals/Conferences

- [ ] Use `IEEEtran` document class
- [ ] `\IEEEoverridecommandlockouts` if needed
- [ ] Bibliography style: `IEEEtran`
- [ ] Abstract: max 200 words
- [ ] Index terms (not "keywords")
- [ ] No page numbers in submission
- [ ] Citation format: [1] numbered style, use author names at sentence start

### Springer Journals (e.g., APIN, TOIT)

- [ ] Use `svjour3` or `llncs` document class
- [ ] Bibliography style per journal
- [ ] Author contributions section
- [ ] Conflict of interest statement
- [ ] Data availability statement
- [ ] Numbered citation: [1] or Author-year per journal

### ACM Conferences

- [ ] Use `acmart` document class with correct format
- [ ] CCS concepts
- [ ] ACM Reference Format on first page
- [ ] Rights/permissions block

### General (All Publishers)

- [ ] Paper title matches submission system
- [ ] Author order matches all co-authors' agreement
- [ ] Email addresses are institutional (not personal gmail/qq)
- [ ] ORCID IDs included if required
- [ ] Funding acknowledgment matches funder requirements
- [ ] Ethical approval statement if human subjects involved

---

## Systematic Search Commands

Use these regex patterns to find issues. Run with the Grep tool on the `.tex` file:

### Phase 0: Pre-Check

```bash
rg '\\bibliographystyle\{' paper.tex           # Detect citation style
rg '\\author' paper.tex                         # Count authors
rg '\\documentclass' paper.tex                  # Identify template
rg '\\journal\{' paper.tex                      # Check journal name (look for template defaults)
```

### Phase 1: Pronouns

```bash
rg -in '\bwe\b' paper.tex                       # Count and evaluate all "we"
rg -in '\bour\b' paper.tex                      # Count and evaluate all "our"
rg -in '\bus\b' paper.tex                        # Count "us" (watch for false positives like "focus")
rg -in '\b(I|my|me)\b' paper.tex                # Should not appear in multi-author papers
rg -in 'we (believe|feel|think|assume)' paper.tex   # Subjective language
rg -in 'in our (opinion|view|experience)' paper.tex # Subjective language
rg -in 'our proposed' paper.tex                  # Always redundant → "the proposed"
```

### Phase 2: AI Style

```bash
rg -in 'delve' paper.tex
rg -in 'worth noting|should be noted|important to note' paper.tex
rg -in 'realm of|landscape of' paper.tex
rg -in 'myriad|plethora|multitude' paper.tex
rg -in 'crucial role|pivotal role|vital role' paper.tex
rg -in 'garnered.*attention' paper.tex
rg -in 'pave.*way|paves.*way' paper.tex
rg -in 'intricate.*nature' paper.tex
rg -in 'remarkable|exceptional|compelling|paramount' paper.tex
rg -in 'not only.*but also' paper.tex
rg -in 'comprehensive' paper.tex
rg -in 'novel' paper.tex
rg -in 'cutting-edge|state-of-the-art' paper.tex
rg -in 'leverage|utilize' paper.tex
rg -in 'notably|remarkably|significantly' paper.tex
rg -in 'underscores?' paper.tex
rg -in 'testament' paper.tex
rg -in 'effectively address' paper.tex
rg -in 'innovative' paper.tex
rg -in 'robust.*across.*' paper.tex
rg -in 'superior.*capabilities' paper.tex
rg -in 'stands as' paper.tex
rg -in 'sheds? (new )?light' paper.tex
rg -in 'revolutionize|game.changer' paper.tex
rg -in 'ever-evolving|rapidly evolving' paper.tex
rg -in 'immense potential|great potential|holds? promise' paper.tex
rg -in 'in conclusion' paper.tex
rg -in 'furthermore.*moreover' paper.tex
rg -in 'demonstrates? (remarkable|exceptional|superior)' paper.tex
rg -in 'validates? the' paper.tex
rg -in 'underscores? (the|their|its)' paper.tex
rg -in 'promising avenue' paper.tex
```

### Phase 3: Symbols

```bash
rg '[\uff08\uff09\uff0c\u3002\uff1b\uff1a\u201c\u201d\u2018\u2019\u3001]' paper.tex  # Chinese punctuation
rg '(?<!\-)\-\-(?!\-)' paper.tex   # En-dashes (verify each is a number range)
rg '\.\w' paper.tex                 # Missing space after period
rg '  +' paper.tex                  # Double spaces
rg ' [,\.\;\:]' paper.tex          # Space before punctuation
rg ',[^ \\$~\n}]' paper.tex       # Missing space after comma
```

### Phase 4: Capitalization

```bash
rg '\\section\{' paper.tex          # List all section titles
rg '\\subsection\{' paper.tex       # List all subsection titles
rg '\\subsubsection\{' paper.tex    # List all subsubsection titles
```

### Phase 5: LaTeX

```bash
rg '(?<!~)\\cite\{' paper.tex       # Missing ~ before \cite
rg '(?<!~)\\ref\{' paper.tex        # Missing ~ before \ref
rg '\\textbf\{\\textbf' paper.tex   # Double \textbf
rg '\\textit\{\\textit' paper.tex   # Double \textit
rg '^\\cite\{' paper.tex            # Sentence starting with bare \cite
rg '\\journal\{Nuclear Physics' paper.tex  # Template default journal name
```

### Phase 6: Grammar

```bash
rg -in "(don't|can't|won't|it's|that's|there's|we've|we're|isn't|aren't|wasn't|weren't|couldn't|wouldn't|shouldn't|hasn't|haven't|hadn't)" paper.tex
rg -in 'in order to' paper.tex
rg -in 'due to the fact' paper.tex
rg -in 'it can be seen' paper.tex
rg -in 'a large number of' paper.tex
rg -in 'datas\b|informations\b|researches\b' paper.tex
rg -in 'the (Equation|Fig\.|Figure|Table) ' paper.tex
rg -in ', When|, This is|, It is|, The ' paper.tex   # Potential comma splices
```

### Phase 7: Numbers

```bash
rg '\d{4,}(?![\d,])' paper.tex      # Large numbers without thousands separator
rg '0\.\d+' paper.tex               # Decimal numbers (check precision consistency)
```

### Phase 8: Content Structure

```bash
# ── Abstract ──
rg '\\begin\{abstract\}' paper.tex   # Locate abstract
rg '\\cite' paper.tex | head         # Check if citations appear in abstract (should not)
rg -in 'with the (rapid|exponential|increasing)' paper.tex  # Generic AI-style abstract opening
rg -in 'in recent years' paper.tex   # Generic abstract opening

# ── Introduction (CARS Model) ──
rg '\\section\{Introduction' paper.tex    # Locate introduction
rg -in 'however|despite|although|while' paper.tex  # Move 2 gap indicators
rg -in 'this paper proposes|this paper presents|this paper introduces' paper.tex  # Move 3 statement
rg '\\begin\{enumerate\}' paper.tex       # Locate contributions list
rg 'remainder of this paper' paper.tex    # Locate paper outline paragraph
rg -in 'the main contributions' paper.tex # Locate contribution preamble

# ── Related Work ──
rg '\\section\{Related' paper.tex     # Locate Related Work
rg '\\subsection\{' paper.tex         # Check thematic organization (subsections = themes)
rg -in 'gap|limitation|however|lack' paper.tex  # Check for gap summary at end

# ── Method / Algorithm ──
rg '\\section\{.*[Mm]ethod\|\\section\{.*[Aa]lgorithm\|\\section\{.*[Pp]roposed' paper.tex
rg '\\begin\{algorithm\}' paper.tex   # Locate pseudocode blocks
rg '\\begin\{definition\}' paper.tex  # Locate formal definitions
rg -in 'definition \d' paper.tex      # Check definition numbering
rg -in 'theorem|lemma|proposition' paper.tex  # Locate proofs
rg -in 'time complexity|space complexity' paper.tex  # Check complexity analysis

# ── Experiments ──
rg '\\section\{[Ee]xperiment' paper.tex   # Locate experiments
rg -in 'default.*parameter\|published.*setting\|original.*parameter' paper.tex  # Fair comparison statement
rg -in 'averaged over|mean of|across.*runs' paper.tex  # Multiple runs statement
rg -in 'ablation' paper.tex               # Check ablation study presence
rg -in 'sensitivity|parameter.*analysis' paper.tex  # Parameter sensitivity
rg -in 'hardware|GPU|CPU|memory' paper.tex # Hardware info
rg -in 'reproduce|reproducib' paper.tex   # Reproducibility statement

# ── Discussion ──
rg '\\section\{[Dd]iscussion' paper.tex   # Locate discussion
rg -in 'outperforms because\|reason.*is\|explains\|due to\|attributed to' paper.tex  # Mechanism explanation
rg -in 'prior work\|previous.*study\|compared (with|to).*existing' paper.tex  # Comparison with prior work

# ── Limitations ──
rg -in 'limitation' paper.tex        # Check if limitations are discussed
rg -in 'future work' paper.tex       # Check future work mentions
rg -in 'however.*only.*dataset\|limited to\|not.*tested\|unverified' paper.tex  # Specific limitations

# ── Conclusion ──
rg '\\section\{[Cc]onclusion' paper.tex  # Locate conclusion
rg -in 'paving the way\|paves the way\|future advancements' paper.tex  # Vague future work
rg -in 'transform\|revolution\|groundbreak\|paradigm' paper.tex  # Overclaiming

# ── Keywords ──
rg '\\begin\{keyword\}' paper.tex    # Locate keywords block
rg '\\sep' paper.tex                 # Count keyword separators (count+1 = number of keywords)

# ── Cross-Section Consistency ──
rg '\\label\{' paper.tex             # List all labels
rg '\\ref\{' paper.tex               # List all references (every label should be referenced)
```

### Phase 9: BIB File

```bash
rg '@\w+\{' paper.bib               # List all entry types and keys
rg '^\}' paper.bib                   # Count closing braces (should match entry count)
```

---

## Severity Classification

| Severity | Description | Examples |
|----------|-------------|---------|
| **Critical** | Will cause rejection or compilation failure | BIB entry missing closing brace, Chinese punctuation, undefined references, page limit exceeded, anonymization breach |
| **Major** | Significantly impacts paper quality | AI-style phrases (reviewer suspicion), inconsistent capitalization, citation formatting errors, number mismatches between text and tables |
| **Minor** | Should fix but won't cause rejection alone | Missing `~` before `\cite`, minor hedging overuse, redundant phrases, "our proposed" |
| **Suggestion** | Nice to have | Decimal precision alignment, figure font size optimization, abstract opening improvement |

---

## Quick Reference: Common Real-World Errors Found in Papers

Based on analysis of actual submissions, these are the most frequently occurring errors:

1. **BIB file structural errors** — missing closing braces causing entry nesting
2. **Template default values left unchanged** — `\journal{Nuclear Physics B}` in elsarticle
3. **Inconsistent section capitalization** — "Related Work" + "EXPERIMENTS" + "method"
4. **Starting sentences with `\cite{}`** instead of author names (numbered style)
5. **Redundant "the work of Author et al.~\cite{key}, who"** — redundant for numbered citations
6. **Chinese brackets in English text** — common when typing on Chinese IME
7. **Redundant `\textbf{\textbf{}}`** — from copy-paste
8. **AI-style superlatives without numbers** — "remarkable performance" without data
9. **Missing `~` before `\cite` and `\ref`** — line-break issues
10. **"our proposed method"** — always redundant, use "the proposed method" instead
11. **Number mismatch between text and tables** — "4,554,343" vs "4,554,344"
12. **`--` used for em-dash** — should be `---`
13. **Inconsistent decimal precision** in result tables — 0.997 vs 0.9984
14. **Undefined acronyms** used before first definition
15. **Generic abstract opening** — "With the rapid development of..."
16. **Duplicate phrases** in same paragraph — copy-paste remnants
17. **Comma splices** — ", When" should be ". When"
18. **Missing articles** — "in network environment" → "in the network environment"
19. **Excessive "we/our"** — especially "our" which can usually be replaced with "the"
20. **AUC or other acronyms undefined in abstract** — abstract must be self-contained
21. **Abstract problem section too long** — >50% of abstract is background, not enough on results
22. **Introduction missing CARS Move 2** — no clear gap statement between background and proposal
23. **Conclusion is one giant paragraph** — should be 2–4 paragraphs for papers >6 pages
24. **Missing limitations** — no Limitations subsection in Discussion or Conclusion
25. **Keywords only from title** — all keywords already appear in title, no additional discoverability
26. **Conclusion ≈ Abstract repetition** — conclusion just restates abstract in different words
27. **Sentences >35 words** — comprehension drops, reviewers flag readability
28. **Paragraphs >250 words** — wall of text, should be split
29. **Related Work ends without gap summary** — no synthesis linking to proposed method
30. **Related Work is a "laundry list"** — one paragraph per paper with no thematic synthesis
31. **Method missing pseudocode** — text description alone is insufficient for reproduction
32. **Method symbols inconsistent** — "d" means hash count in Method but depth in Experiments
33. **Method missing overview figure** — no visual pipeline/architecture diagram
34. **Experiments no multiple runs** — single run result with no error bars or std
35. **Experiments unfair comparison** — your method re-tuned but baselines use suboptimal settings
36. **Experiments no ablation study** — reviewer cannot tell which component helps
37. **Experiments numbers without interpretation** — results table exists but text just restates numbers
38. **Discussion just repeats Results** — "SP-SBEAD achieves 0.998" without explaining WHY
39. **Discussion no mechanism explanation** — no analysis of why the method works
40. **Conclusion introduces new claims** — information that doesn't appear earlier in the paper
41. **Conclusion overclaims** — "transforms the field" without extraordinary evidence
42. **Conclusion no specific future work** — "explore future directions" without concrete plans
