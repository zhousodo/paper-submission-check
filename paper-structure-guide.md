# Paper Structure & Writing Quality Guide

Structural and writing quality standards for academic papers targeting top-tier journals. Covers abstract construction, introduction structure (CARS model), paragraph and sentence length, keyword optimization, section-specific rules, and conclusion requirements. Based on Swales' CARS model, Elsevier/IEEE/Springer author guidelines, and empirical analysis of 9,830+ research papers.

---

## 1. Abstract Writing Standards

The abstract is the first thing editors read. A poorly written abstract can result in desk rejection before the paper reaches reviewers.

### Word Count Requirements by Publisher

| Publisher | Article Type | Abstract Limit | Notes |
|-----------|-------------|---------------|-------|
| **Elsevier** | Research Article | 150–300 words (varies by journal) | Check specific journal's Guide for Authors |
| **Elsevier** | Short Communication | 100–150 words | |
| **IEEE** | Transactions/Access | ≤200 words | Strict limit |
| **IEEE** | Conference | 150–200 words | |
| **Springer** | Research Article | 150–250 words | Varies by journal |
| **ACM** | Conference | 150–250 words | |

**How to count**: In LaTeX, extract the abstract text and use any word counter. Exclude LaTeX commands from the count.

### Abstract Structure (the "PRKS" Framework)

A strong abstract follows four parts in order:

| Part | Content | Approximate Share |
|------|---------|-------------------|
| **P — Problem** | What problem does this paper address? | 15–20% |
| **R — Response** | What does this paper propose/do? | 25–30% |
| **K — Key Results** | What are the main quantitative results? | 35–40% |
| **S — Significance** | Why do these results matter? | 10–15% |

**Common errors**:

| Error | Example | Fix |
|-------|---------|-----|
| Problem section too long (>50%) | 5 sentences on background before mentioning the method | Compress to 1–2 sentences |
| Missing specific numbers | "achieves superior performance" | "achieves 0.998 AUC on CICIDS2019" |
| Undefined abbreviations | "AUC" used without expansion | "Area Under the Curve (AUC)" at first use |
| Generic opening sentence | "With the exponential growth of..." | Start with the specific problem or contribution |
| Citations in abstract | "\cite{xxx}" appearing in abstract | Remove — abstracts should be self-contained |

### Abstract Opening Sentence

The opening sentence is critical. Editors immediately recognize generic AI-style openings.

| Bad Opening (Generic) | Good Opening (Specific) |
|-----------------------|------------------------|
| "With the rapid development of..." | "Detecting anomalous edges in streaming graphs requires O(1) per-edge methods" |
| "In recent years, X has attracted significant attention" | "This paper proposes SBEAD, a sketch-based method for real-time graph anomaly detection" |
| "The exponential growth of data has made X increasingly important" | "Existing graph-based anomaly detection methods require complete graph structures, limiting real-time applicability" |

**Rule**: If the opening sentence could apply to any paper in the field, it is too generic. Start with your specific problem or contribution.

### Abstract Self-Containment Rule

The abstract must be understandable without reading the paper. This means:
- Every abbreviation must be defined in the abstract, even if defined again in the body
- No references to figures, tables, equations, or sections
- No citations (most publishers prohibit this)
- All claimed numbers must be verifiable from the paper's results

---

## 2. Introduction Structure: Swales' CARS Model

The **CARS (Create A Research Space)** model, developed by linguist John Swales (1990), is the standard framework for writing research introductions. Editors and reviewers unconsciously expect this structure.

### The Three Moves

| Move | Purpose | What to Write | Typical Length |
|------|---------|--------------|---------------|
| **Move 1: Establish Territory** | Show the research area is important and active | General background + importance + cite key works | 1–2 paragraphs |
| **Move 2: Establish Niche** | Identify the gap/problem in existing research | Limitations of current methods + what is missing | 1 paragraph |
| **Move 3: Occupy Niche** | State what this paper does to fill the gap | Proposed approach + contributions + paper outline | 1–2 paragraphs + contributions list |

### Move 1: Establish Territory (3 optional steps)

| Step | Example |
|------|---------|
| **1a. Claim centrality** | "Graph-based anomaly detection has been extensively studied..." |
| **1b. Make topic generalizations** | "Graph data structures capture rich structural information while preserving content-related details" |
| **1c. Review previous work** | "Approaches include deep learning-based [refs] and structure-based methods [refs]" |

### Move 2: Establish Niche (4 strategies, pick one)

| Strategy | Signal Phrases | Example |
|----------|---------------|---------|
| **Counter-claiming** | "However, ...", "Despite this, ..." | "However, most methods focus on static graphs, which cannot capture temporal dynamics" |
| **Indicating a gap** | "No study has...", "Little attention has been paid to..." | "Few methods address real-time detection in streaming graphs with bounded memory" |
| **Question-raising** | "The question remains...", "It is unclear whether..." | "It remains unclear whether sketch-based methods can achieve both accuracy and efficiency" |
| **Continuing a tradition** | "Building upon...", "Extending..." | "Building upon sketch-based streaming methods, this paper aims to enhance..." |

**Critical**: Move 2 is where many papers fail. The gap statement must be specific and directly motivate Move 3.

### Move 3: Occupy Niche (3 components)

| Component | Format | Notes |
|-----------|--------|-------|
| **State purpose** | "This paper proposes X and Y" | 1 sentence, clear and direct |
| **List contributions** | Numbered \begin{enumerate} list | 3–5 items, each measurable and specific |
| **Paper outline** | "The remainder of this paper..." | 1 paragraph mapping sections to content |

### Introduction Checklist

```
Introduction Checklist:
- [ ] Move 1 present: field importance established with citations
- [ ] Move 2 present: specific gap/limitation identified (not vague)
- [ ] Move 3 present: clear statement of what this paper does
- [ ] Contributions: 3–5 numbered items, each specific and measurable
- [ ] Paper outline: all sections referenced with \ref{}
- [ ] Length: 1.5–2.5 pages (including contributions and figure, if any)
- [ ] Flow: Move 1 → Move 2 → Move 3 transition is natural
- [ ] No "orphan" paragraphs (unrelated to the three moves)
```

---

## 3. Paragraph Structure & Length

### Empirical Standards (from analysis of 9,830 research papers)

| Metric | Median | Healthy Range | Problematic |
|--------|--------|---------------|-------------|
| Words per paragraph | 109 | 65–200 | <30 or >250 |
| Sentences per paragraph | 4 | 3–6 | 1 or >8 |
| By section — Methods | 92 words | 60–150 | >200 |
| By section — Discussion | 131 words | 80–200 | >250 |
| By section — Introduction | 120 words | 80–200 | >250 |

### Paragraph Rules

**Rule 1: One idea per paragraph**

Each paragraph should make one clear point. If a paragraph covers two distinct ideas, split it.

**Rule 2: Topic sentence first**

The first sentence of each paragraph should state the paragraph's main point. The remaining sentences provide evidence, elaboration, or examples.

| Weak Opening | Strong Opening |
|-------------|---------------|
| "There are many methods..." | "Sketch-based methods reduce memory from O(n) to O(d×b)" |
| "It is important to consider..." | "CMS maintains d independent hash rows for robustness" |

**Rule 3: No single-sentence paragraphs**

A paragraph with only one sentence indicates an underdeveloped idea. Either expand it or merge it with an adjacent paragraph.

Exception: The paper outline paragraph ("The remainder of this paper...") is often a single long sentence and is acceptable.

**Rule 4: No giant paragraphs**

Paragraphs exceeding 250 words should be split. Common locations for giant paragraphs:
- The first Introduction paragraph (trying to cover too much background)
- Dataset descriptions (listing all features in one block)
- The Conclusion (restating everything in one paragraph)

**Rule 5: Conclusion must be multi-paragraph**

For papers >6 pages, the conclusion should contain 2–4 short paragraphs:
1. Summary of contributions and key findings
2. Main significance or implications
3. Limitations (if not in Discussion)
4. Future work directions

### Detection Method

Count words per paragraph by searching for blank lines between `\par` or double newlines. Flag:
- Paragraphs with >250 words → suggest split
- Paragraphs with only 1 sentence → suggest merge or expand
- 3+ consecutive short paragraphs (<50 words each) → suggest consolidation

---

## 4. Sentence Length Standards

### Ideal Ranges

| Range | Assessment | Action |
|-------|-----------|--------|
| 8–14 words | Too short — may sound simplistic | Combine with adjacent sentence if appropriate |
| **15–25 words** | **Ideal range** | No action needed |
| 25–35 words | Acceptable but watch clarity | Review for readability |
| **>35 words** | **Too long** — comprehension drops significantly | Must split into 2 sentences |

### Sentence Length Variation

Good academic writing varies sentence length naturally. If 5+ consecutive sentences are all the same length (±3 words), the prose feels mechanical (an AI marker).

| Monotonous (AI-like) | Varied (Human-like) |
|----------------------|-------------------|
| "The method processes edges. The method assigns scores. The method detects anomalies. The method uses CMS." (all ~5-7 words) | "The method processes edges in O(1) time. For each edge, it queries d hash rows and computes the IQR-based boundary. Edges exceeding this boundary receive high anomaly scores." (7, 15, 9 words) |

### Long Sentence Splitting Strategies

| Long Sentence | Split Version |
|--------------|--------------|
| "The proposed method uses CMS to maintain count statistics for each node and edge, and then applies an IQR-based boundary detection mechanism that adapts to varying traffic patterns while maintaining sensitivity to sudden anomalies." (35 words) | "The proposed method uses CMS to maintain count statistics for each node and edge. An IQR-based boundary detection mechanism then adapts to varying traffic patterns while remaining sensitive to sudden anomalies." (17 + 16 words) |

---

## 5. Keywords Optimization

### Number Requirements by Publisher

| Publisher | Required | Recommended |
|-----------|---------|-------------|
| **Elsevier** | Varies by journal, typically max 6 | 4–6 |
| **IEEE** | Min 3, Max 10 | 5–7 |
| **Springer** | Varies by journal | 4–6 |
| **ACM** | Uses CCS concepts (separate system) | 3–5 keywords + CCS |

### Keyword Quality Checklist

| Check | Rule | Example |
|-------|------|---------|
| **Count** | At least 4, no more than 6 (Elsevier) or 10 (IEEE) | 4 keywords → consider adding 1–2 more |
| **Not all from title** | At least 1–2 keywords should NOT appear in the title | Title has "Anomaly Detection" and "Graph Stream" → add a keyword like "Count-Min Sketch" that isn't in the title |
| **Broad + Specific mix** | Include both general field terms and specific method/technique terms | Broad: "Network Security"; Specific: "Count-Min Sketch" |
| **No redundancy** | Each keyword should add unique discoverability | "Anomaly Detection" and "Anomalous Behavior Detection" are redundant |
| **Standard terms** | Use established terms from the field, not invented ones | "Graph Stream" ✓; "Stream-Graph Analysis" ✗ (non-standard) |
| **Separator** | Elsevier uses `\sep`; IEEE uses commas; check journal guide | |

### Keyword Discoverability Strategy

Think: "What would a researcher search for to find this paper?"

| Level | Purpose | Examples |
|-------|---------|---------|
| **Field** | Broad field terms for general discoverability | Network Security, Data Mining |
| **Topic** | Specific research topic | Anomaly Detection, Graph Stream |
| **Method** | Specific technique used | Count-Min Sketch, Sketch Data Structure |
| **Application** | Domain where it's applied | Intrusion Detection, Edge Stream |

Aim for 2 topic + 1–2 method + 1–2 field/application keywords.

---

## 6. Related Work: How to Write Well

**Purpose**: Build a persuasive case for why your work is needed. This is NOT a reading list — it is an argument.

### Organization: Thematic, Not Chronological

Top journals prefer **thematic organization** — grouping papers by research approach or theme, not by publication date. Treat it as a "synthesis matrix": imagine inviting all cited authors to a dinner party where they debate, rather than each giving a monologue.

| Bad (Laundry List) | Good (Thematic Synthesis) |
|--------------------|--------------------------|
| "Smith (2019) proposed X. Jones (2020) proposed Y. Lee (2021) proposed Z." | "Deep learning-based methods [Smith 2019, Lee 2021] achieve high accuracy but require training data. In contrast, structure-based methods [Jones 2020] operate unsupervised but..." |

### Recommended Structure

```
Related Work Structure:
├── Opening: scope statement ("This section reviews X, organized into A and B")
├── Theme A: [e.g., Deep learning-based graph AD]
│   ├── Core idea of this approach category (1–2 sentences)
│   ├── Representative works synthesized together (cite multiple, compare)
│   └── Limitations of this category (1–2 sentences)
├── Theme B: [e.g., Structure-based / sketch-based AD]
│   ├── Core idea (1–2 sentences)
│   ├── Representative works synthesized (cite multiple, compare)
│   └── Limitations of this category (1–2 sentences)
└── Gap Summary Paragraph:
    "Despite advances in both A and B, existing methods still face [X] and [Y].
     To address these limitations, this paper proposes..."
```

### Writing Rules

| Rule | Explanation |
|------|-----------|
| **Synthesize, don't summarize** | Group papers by themes, not one-paragraph-per-paper |
| **Be critical but respectful** | State limitations objectively; never dismiss other work |
| **End with the gap** | Final paragraph MUST synthesize what's missing and connect to your method |
| **Include recent work** | At least 3–5 references from the past 2 years |
| **Sufficient breadth** | 15–30 citations for a full journal paper |

### Common Errors That Cause Rejection

| Error | Severity | Reviewer's Reaction |
|-------|----------|-------------------|
| "Laundry list" — one paragraph per paper, no synthesis | High | "The author can read but cannot synthesize" |
| No critical analysis — just describes what papers do | High | "This is a reading report, not a literature review" |
| No gap summary — jumps directly to method | High | "I don't understand why this work is necessary" |
| Coverage too narrow — misses important competing methods | High | "The author seems unaware of [important work]" |
| No recent references | Medium | "The literature is outdated" |
| Dismissive of competing work | Medium | "Academic writing should be objective" |
| Too long — exhaustive review of the entire field | Medium | "This is a survey paper, not a research article" |

### Checklist

```
Related Work Checklist:
- [ ] Organized thematically (not one-paragraph-per-paper)
- [ ] Each theme group: summary + comparison + limitations + citations
- [ ] Final paragraph synthesizes the gap across all groups
- [ ] Gap directly connects to and motivates the proposed method
- [ ] Sufficient breadth (15–30 citations for a full paper)
- [ ] Recent references included (at least 3–5 from the past 2 years)
- [ ] Respectful tone toward competing methods
- [ ] No theme group with only 1 citation (shows thin coverage)
```

---

## 7. Method / Algorithm: How to Write Well

**Purpose**: Describe the proposed method with sufficient detail for a skilled researcher to reproduce it. The method section is the technical core of the paper.

### Multi-Level Presentation Strategy

Present the algorithm at multiple levels of abstraction, from intuition to formalism:

| Level | Content | Format |
|-------|---------|--------|
| **1. High-level intuition** | What is the core idea? Why should it work? | 1 paragraph of prose |
| **2. System overview** | How do the components fit together? | Flowchart or architecture diagram |
| **3. Problem formulation** | Formal definition: input, output, assumptions | Definitions + notation table |
| **4. Detailed algorithm** | Step-by-step description with mathematical derivation | Equations + prose |
| **5. Pseudocode** | Implementable specification | Algorithm block |
| **6. Design justification** | Why this approach and not alternatives? | Prose (1–2 paragraphs) |
| **7. Complexity analysis** | Time and space complexity | Theorem or informal proof |

### Recommended Structure

```
Method Structure:
├── 3.1 Problem Formulation
│   ├── Definition 1: Graph stream
│   ├── Definition 2: Anomaly score
│   ├── Input/Output specification
│   └── Notation table (if many symbols)
├── 3.2 Method Overview
│   └── Architecture diagram (Fig. X) + 1 paragraph explaining the pipeline
├── 3.3 Algorithm A (e.g., SBEAD)
│   ├── Core idea and intuition (1 paragraph, no math)
│   ├── Detailed steps with equations
│   ├── Algorithm 1 (pseudocode block)
│   └── Design choice justification: "CMS is chosen over Bloom filters because..."
├── 3.4 Algorithm B (e.g., SP-SBEAD)
│   ├── How it extends Algorithm A
│   ├── Additional components with equations
│   ├── Algorithm 2 (pseudocode block)
│   └── Key difference from Algorithm A
└── 3.5 Complexity Analysis
    ├── Time complexity: per-edge and total
    └── Space complexity: per-structure and total
```

### Writing Rules

| Rule | Explanation |
|------|-----------|
| **Every symbol defined at first use** | Never use a symbol before defining it. If many symbols, provide a notation table |
| **Pseudocode is mandatory** | Text alone is insufficient; pseudocode must be clear enough for a CS student to implement |
| **Explain WHY, not just WHAT** | "We use CMS because it provides sub-linear space complexity" — not just "We use CMS" |
| **Don't over-explain standard methods** | If CMS is well-known, cite the original paper and give a 2–3 sentence summary, not a full re-derivation |
| **Number all key equations** | Equations referenced later must be numbered. Inline equations that are not referenced need not be numbered |
| **Include a method overview figure** | A single figure showing the complete pipeline significantly improves readability |

### Common Errors That Cause Rejection

| Error | Severity | Reviewer's Reaction |
|-------|----------|-------------------|
| Insufficient detail to reproduce | Very High | "I cannot verify these results" (the Reproducibility Project reduced targets from 50 to 18 papers for this reason) |
| Undefined or inconsistent symbols | High | "d means hash count in Section 3 but depth in Section 5" |
| No pseudocode or algorithm block | High | "How am I supposed to implement this?" |
| No complexity analysis | Medium | "What is the computational cost? Is this practical?" |
| Over-explaining standard methods | Medium | "I don't need a full derivation of CMS; cite the original paper" |
| No design justification | Medium | "Why this approach and not [alternative]?" |
| Method overview figure missing | Medium | "I had to read the section three times to understand the pipeline" |
| Equations not numbered | Low | "I need to reference Eq. (X) but it has no number" |

### Checklist

```
Method Checklist:
- [ ] Problem formally defined with input/output specification
- [ ] All symbols defined at first use
- [ ] Notation table provided (if >10 symbols)
- [ ] Method overview figure/diagram included
- [ ] Core intuition explained in plain language before math
- [ ] Algorithm pseudocode provided (Algorithm block)
- [ ] Key equations numbered and referenced in text
- [ ] Design choices justified (why this approach?)
- [ ] Standard methods cited, not re-derived
- [ ] Complexity analysis: time and space
- [ ] Method is reproducible from description alone
```

---

## 8. Experiments: How to Write Well

**Purpose**: Convince reviewers that results are valid, reproducible, and fairly obtained. Reviewers scrutinize this section most carefully.

### Recommended Subsection Order

| Order | Subsection | Content | Required? |
|-------|-----------|---------|-----------|
| 1 | **Datasets** | Description, statistics table, preprocessing, why these datasets | Yes |
| 2 | **Baselines** | Which methods, why selected, parameter settings | Yes |
| 3 | **Experimental Settings** | Hardware, software, hyperparameters, number of runs | Yes |
| 4 | **Evaluation Metrics** | Mathematical definition of each metric | Yes |
| 5 | **Main Results** | Comparison tables/figures with interpretation | Yes |
| 6 | **Ablation Study** | Component-by-component analysis | Strongly recommended |
| 7 | **Parameter Sensitivity** | Key parameter variation effects | Recommended |
| 8 | **Efficiency Analysis** | Time/memory comparison | Recommended |

### Writing Rules for Each Subsection

**Datasets**:
- Provide a summary statistics table (nodes, edges, timestamps, attack ratio)
- Explain WHY these datasets were chosen (not just "widely used")
- Describe preprocessing steps so others can reproduce

**Baselines**:
- State explicitly: "All baselines use the default parameters published by the respective authors"
- For each baseline: 1–2 sentences on core idea + citation. Do NOT re-explain the full method
- Justify selection: why these baselines and not others?

**Main Results**:
- **Lead with your strongest finding** — put the most compelling result first
- **Interpret, don't just present** — after each table, explain: What stands out? Why does method X perform better on dataset Y?
- **Discuss weak results honestly** — if you underperform on one dataset, explain why
- **Bold the best values** in each column of comparison tables

**Ablation Study** (increasingly expected by top venues):
- Remove or replace each key component one at a time
- Show which components contribute most to performance
- Present as a table with clear component labels

**Parameter Sensitivity**:
- Vary one parameter at a time while fixing others
- Show results as line plots (parameter on x-axis, metric on y-axis)
- Discuss the optimal range and sensitivity

### Fair Comparison: The #1 Reviewer Priority

ICLR reviewer guides state that **fairness of comparison is the first thing reviewers check**, above raw numbers. Unfair comparisons are the fastest path to rejection.

| Fair Practice | Unfair Practice (will be caught) |
|--------------|--------------------------------|
| Use published default parameters for all baselines | Re-tune your method but not baselines |
| Run all methods on the same hardware | Compare your GPU results with baseline CPU results |
| Use the same data splits for all methods | Give your method more training data |
| Report all datasets, including where you don't win | Only report datasets where you outperform |
| Report mean ± std over multiple runs | Report only the best single run |

### Statistical Rigor

| Practice | When Required | How |
|----------|--------------|-----|
| Multiple runs | Always | "Averaged over 5 runs" with standard deviation |
| Error bars | When showing plots | Mean ± std as error bars or shaded regions |
| Significance test | When improvements are small (<2%) | Paired bootstrap test or Wilcoxon signed-rank test |
| Confidence intervals | For key metrics | Report 95% CI when possible |

### Common Errors That Cause Rejection

| Error | Severity | Reviewer's Reaction |
|-------|----------|-------------------|
| Unfair baseline comparison | Very High | "The comparison is rigged" — this is what reviewers check FIRST |
| No statistical rigor — single run, no error bars | High | "A 1% improvement could be noise" |
| No ablation study | High | "Which component actually helps? I have no idea" |
| Only presenting numbers without interpretation | High | "The Results section should not be a text version of the table" |
| Selective reporting — only showing favorable results | Very High | "What is the author hiding?" |
| Missing implementation details | Medium | "I cannot reproduce this" |
| No dataset statistics table | Medium | "How large are these datasets? What's the class imbalance?" |
| Metrics not defined | Medium | "What exactly is 'Accuracy' here — balanced or unbalanced?" |

### Checklist

```
Experiments Checklist:
- [ ] Dataset statistics table provided (nodes, edges, labels, etc.)
- [ ] Reason for choosing these datasets explained
- [ ] All baselines described (1–2 sentences each) with citations
- [ ] "Default parameters from original papers" explicitly stated
- [ ] Hardware and software environment specified
- [ ] Number of runs stated (e.g., "averaged over 5 runs")
- [ ] Evaluation metrics mathematically defined
- [ ] Main comparison table: best results bolded
- [ ] Results interpreted, not just presented
- [ ] Weak results discussed honestly
- [ ] Ablation study included (or justified why not)
- [ ] Parameter sensitivity analysis included (or justified)
- [ ] Efficiency comparison table (time and memory)
- [ ] Statistical significance reported for close results
```

---

## 9. Discussion: How to Write Well

**Purpose**: Interpret results, compare with prior work, explain mechanisms, and acknowledge weaknesses. The Discussion is where you demonstrate that you truly understand your own work.

### Recommended Structure

```
Discussion Structure:
├── 9.1 Interpretation of Main Findings
│   └── What do the numbers mean? Why does your method outperform baselines?
├── 9.2 Comparison with Prior Work
│   └── How do your findings align with, extend, or contradict previous studies?
├── 9.3 Why It Works (Mechanism)
│   └── Technical explanation of why the proposed approach is effective
├── 9.4 Limitations
│   ├── Limitation 1: Dataset scope / generalizability
│   ├── Limitation 2: Parameter sensitivity / assumptions
│   └── Limitation 3: Scenarios where the method may fail
└── 9.5 Broader Implications
    └── What does this mean for the field? What new questions arise?
```

### Writing Rules

| Rule | Explanation |
|------|-----------|
| **Interpret, don't repeat** | "SP-SBEAD achieves 0.998 AUC" is Results; "This high AUC is due to the separate tracking of source and destination patterns, which captures directional anomalies that edge-only methods miss" is Discussion |
| **Compare with prior work explicitly** | Name specific methods: "Unlike MIDAS, which uses a single CMS, SP-SBEAD maintains three separate structures, enabling node-level analysis" |
| **Address contradictions** | If your results contradict prior findings, discuss why. Silence is a red flag |
| **Limitations must be specific** | Not "has limitations" but "tested only on 4 datasets; encrypted traffic scenarios are not evaluated" |
| **Don't write a second literature review** | Every cited paper in Discussion must connect to YOUR findings |
| **Limit speculation** | Only speculate when grounded in your data; label speculation clearly |

### Limitations: What Editors Expect

A 2024 SAGE editorial states: "Limitations sections are required by ALL major scientific reporting guidelines (CONSORT, STROBE, PRISMA)." Papers without limitations signal either carelessness or an attempt to hide weaknesses.

| Vague Limitation (Triggers Suspicion) | Specific Limitation (Builds Credibility) |
|--------------------------------------|----------------------------------------|
| "The method has some limitations" | "The method was evaluated on four network traffic datasets; generalizability to social network or financial transaction graphs is untested" |
| "Future work will address limitations" | "CMS hash collisions may cause false positives for low-volume attack patterns (e.g., slow-rate DDoS), as evidenced by the lower precision on the DARPA dataset" |
| "The approach may not work in all cases" | "SP-SBEAD's weighted aggregation parameters (a=0.4, b=0.3, c=0.3) were tuned on CICIDS2017; optimal weights may vary for other traffic distributions" |
| "More experiments are needed" | "The evaluation uses offline replay of captured traffic; real-time deployment with live traffic at line rate (10 Gbps+) has not been tested" |

### Common Errors That Cause Rejection

| Error | Severity | Reviewer's Reaction |
|-------|----------|-------------------|
| Just repeating numbers from Results | High | "The Discussion is supposed to INTERPRET, not REPEAT" |
| No comparison with prior work | High | "How does this relate to existing findings?" |
| No explanation of WHY the method works | High | "The author doesn't understand their own method" |
| Missing or vague limitations | High | "No limitations acknowledged = not credible" |
| Writing a second literature review | Medium | "Every reference must connect to YOUR results" |
| Excessive speculation without data | Medium | "This claim has no evidence" |

### Checklist

```
Discussion Checklist:
- [ ] Main findings interpreted (not just numbers repeated)
- [ ] Compared with at least 2–3 specific prior methods
- [ ] Contradictions with prior work addressed (if any)
- [ ] Technical mechanism explained: WHY does the method work?
- [ ] Limitations section present with ≥3 specific limitations
- [ ] Each limitation is concrete (dataset, scenario, parameter)
- [ ] Broader implications discussed
- [ ] Speculation clearly labeled and grounded in data
- [ ] Not a second literature review
```

---

## 10. Conclusion: How to Write Well

**Purpose**: Synthesize the paper's contribution, state significance, acknowledge limitations, and point toward future work. The conclusion is for readers who have finished the paper — it should feel like a thoughtful wrap-up, not a copy of the abstract.

### Conclusion ≠ Abstract

| Aspect | Abstract | Conclusion |
|--------|----------|-----------|
| **Reader** | Has NOT read the paper | Has read the entire paper |
| **Content** | What, How, Results (standalone) | What it means, why it matters, what's next |
| **Background** | Includes problem context | No background needed |
| **Tone** | Factual, quantitative | More reflective, synthesizing |
| **New ideas** | No — just summarize | No — but can reflect on implications |

### Recommended Structure (2–4 paragraphs)

```
Conclusion Structure:
├── Paragraph 1: Summary of Contributions (2–3 sentences)
│   "This paper proposed X and Y for [problem]. Experiments on N datasets
│    demonstrated that [main quantitative result]."
├── Paragraph 2: Key Findings and Significance
│   "The most notable finding is [X], which suggests [Y].
│    This result confirms that [approach] is effective for [scenario]."
├── Paragraph 3: Limitations (if not in Discussion)
│   "However, the evaluation is limited to [scope].
│    [Specific limitation 1]. [Specific limitation 2]."
└── Paragraph 4: Future Work (specific, not vague)
    "Future work will focus on [specific direction 1] and [specific direction 2].
     In particular, [concrete next step grounded in findings]."
```

### Writing Rules

| Rule | Explanation |
|------|-----------|
| **Strongest message first** | Busy reviewers often skim the conclusion; put the most important takeaway in the first 2 sentences |
| **Don't copy the abstract** | Rephrase and synthesize; if >50% of sentences match the abstract, it's too similar |
| **Answer the Introduction's question** | The gap stated in Introduction Move 2 must find its resolution here |
| **No new information** | Everything in the conclusion must have been discussed in the paper |
| **Specific future work** | "Extend to encrypted traffic detection" not "explore future directions" |
| **Multi-paragraph** | For papers >6 pages, a single-paragraph conclusion looks rushed |
| **Appropriate length** | 5–7% of total word count (400–560 words for an 8,000-word paper) |

### Common Errors That Cause Rejection

| Error | Severity | Reviewer's Reaction |
|-------|----------|-------------------|
| Nearly copies the abstract | High | "The conclusion should synthesize, not summarize" |
| Single giant paragraph | Medium | "This feels unstructured and rushed" |
| Introduces new claims not in the paper | High | "Where did this come from? Why wasn't it discussed earlier?" |
| Overclaiming ("transforms the field") | High | "Extraordinary claims require extraordinary evidence" |
| Vague future work ("paving the way") | Medium | "This is empty; what specifically will you do next?" |
| No limitations (if Discussion also has none) | High | "The paper has zero self-criticism" |
| Too long (>10% of paper) | Medium | "The conclusion is bloated" |

### Checklist

```
Conclusion Checklist:
- [ ] Multi-paragraph structure (2–4 paragraphs for full papers)
- [ ] First 2 sentences contain the strongest takeaway
- [ ] NOT a copy of the abstract (>50% different)
- [ ] Answers the research question from the Introduction
- [ ] No new information introduced
- [ ] Limitations mentioned (here or in Discussion)
- [ ] Future work is specific and grounded in findings
- [ ] Length: 5–7% of total paper word count
- [ ] No overclaiming or AI-style superlatives
```

---

## 11. Cross-Section Consistency Checks

| Check | What to Verify |
|-------|---------------|
| Abstract numbers = Table numbers | Every number in the abstract must match a table or figure |
| Contribution list = Paper content | Each numbered contribution must map to a specific section |
| Introduction gap = Method solution | The gap identified in Move 2 must be directly addressed by the proposed method |
| Related Work gap = Contribution novelty | The novelty claimed must not be already done by cited papers |
| Method symbols = Experiment parameters | Mathematical symbols (d, b, w) must match parameter names in experiments |
| Discussion = Results interpretation | Discussion should interpret results, not repeat the numbers |
| Conclusion claims ⊆ Results data | No claim in the conclusion that isn't supported by the experiments |
| Keywords ⊆ Paper content | Every keyword should appear in the paper's actual content |

---

## 12. Quick Reference: Section Length Guidelines

For a typical 8,000–10,000 word research paper:

| Section | Approximate Share | Pages (single-column) |
|---------|------------------|-----------------------|
| Abstract | 2–3% (150–300 words) | — |
| Introduction | 12–15% | 1.5–2 |
| Related Work | 15–20% | 2–2.5 |
| Problem Definition | 5–8% | 0.5–1 |
| Method | 20–25% | 2.5–3 |
| Experiments | 25–30% | 3–4 |
| Discussion | 5–10% | 0.5–1 |
| Conclusion | 5–7% | 0.5–1 |

These are guidelines, not rigid rules. The key is that Method + Experiments should be the longest combined section (the "meat" of the paper).
