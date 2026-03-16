# Reference Format & Bibliography Quality Guide

Comprehensive reference formatting standards for academic papers targeting top-tier journals. Covers BibTeX entry types, author name formatting, title capitalization protection, journal name abbreviation rules, field completeness, DOI policies, and cross-reference consistency. Based on Elsevier/IEEE/Springer/ACM official author guidelines, John Owens' Common Errors in Bibliographies, and TeX StackExchange community best practices.

---

## 0. Reference Template Selector [READ THIS FIRST]

**This is the most important section.** Selecting the wrong reference template or mixing conventions from different publishers is a top cause of desk rejection. Determine your target publisher FIRST, then follow the corresponding rules throughout.

### Step 1: Identify Your Target Publisher

Detect from `\documentclass{}` and `\bibliographystyle{}` in the `.tex` file:

| If you see... | Publisher | Template Family |
|---------------|-----------|----------------|
| `\documentclass{elsarticle}` | **Elsevier** | elsarticle |
| `\documentclass{IEEEtran}` | **IEEE** | IEEEtran |
| `\documentclass{acmart}` | **ACM** | acmart |
| `\documentclass{svjour3}` or `\documentclass{llncs}` | **Springer** | svjour3 / LNCS |

### Step 2: Select the Correct Bibliography Configuration

#### Elsevier — Complete LaTeX Configuration

```latex
\documentclass[preprint,12pt]{elsarticle}
% For author-year style, use: \documentclass[preprint,12pt,authoryear]{elsarticle}

% Elsevier uses natbib internally — do NOT add \usepackage{natbib}
% unless using elsarticle-harv (author-year style)

\begin{document}
% ... paper content ...
\bibliographystyle{elsarticle-num}    % Numbered style [1], [2], [3]
% OR
% \bibliographystyle{elsarticle-harv}  % Author-year style (Smith, 2024)
% OR
% \bibliographystyle{elsarticle-num-names} % Numbered + author names: Smith et al. [1]
\bibliography{refs}
\end{document}
```

**Elsevier style variants:**

| Style File | Citation Appearance | Sorting | When to Use |
|-----------|-------------------|---------|-------------|
| `elsarticle-num` | [1], [2], [3] | Citation order | Most Elsevier journals (default) |
| `elsarticle-harv` | (Smith, 2024) | Alphabetical | Journals requiring author-year |
| `elsarticle-num-names` | Smith et al. [1] | Citation order | Journals wanting names + numbers |

**Which Elsevier style?** Check your journal's **Guide for Authors** page (click "Guide for Authors" in left menu on journal homepage). If no preference stated, use `elsarticle-num`.

#### IEEE — Complete LaTeX Configuration

```latex
\documentclass[journal]{IEEEtran}
% For conference: \documentclass[conference]{IEEEtran}

\begin{document}
% ... paper content ...
\bibliographystyle{IEEEtran}  % THE official IEEE BibTeX style
\bibliography{refs}
\end{document}
```

**IEEE style variants:**

| Style File | Sorting | Notes |
|-----------|---------|-------|
| `IEEEtran` | Citation order (unsorted) | **Recommended for all IEEE submissions** |
| `IEEEtranS` | Alphabetical | Some IEEE journals; check Guide for Authors |
| `IEEEtranN` | Citation order + full names | Rarely used |
| `IEEEtranSN` | Alphabetical + full names | Rarely used |

### Step 3: Apply Publisher-Specific Rules (Critical Differences)

**The following table shows EVERY difference that matters. Getting any of these wrong can cause rejection.**

| Rule | Elsevier | IEEE | Mismatch = Rejection Risk |
|------|----------|------|--------------------------|
| **Journal names** | **Full name** (`Information Processing & Management`) | **Abbreviated per ISO 4** (`Inf. Process. Manag.`) | **HIGH** — Reviewers/editors notice immediately |
| **Author initials** | `A.B. Author` (no space between initials OK) | `A. B. Author` (**space required** between initials) | Medium — BibTeX style usually handles |
| **Month field** | Optional | **Required** — use bare BibTeX macros: `month = jun` | Medium — Missing months flagged by IEEE |
| **DOI display** | `elsarticle-num.bst` does **NOT** render DOI | `IEEEtran.bst` renders DOI (if field present) | Low — Include DOI in .bib regardless |
| **Article title** | Normal text | **Quoted** (handled by style automatically) | Low — Style handles |
| **et al. threshold** | 3+ authors → et al. | **6+ authors** → et al. | Low — Style handles |
| **Page range** | `pages = {35--49}` (en-dash) | `pages = {35--49}` (en-dash) | Same for both |
| **Citation at sentence start** | `Author et al.~\cite{key}` or `The work in~\cite{key}` | `Author et al.~\cite{key}` or `In~\cite{key},` | **HIGH** — Never start with bare `\cite{key}` in numbered style |
| **`\usepackage{natbib}`** | **Do NOT add** (elsarticle loads it internally) | Not used | Medium — Duplicate package error |
| **`\usepackage{cite}`** | Compatible | **Recommended** for auto-sorting `[1,3,5]→[1,3,5]` | Low — Cosmetic |
| **Conference name format** | Full name in booktitle | Full name in booktitle | Same — but digital library exports are ALWAYS wrong |
| **Bibliography title** | Auto: "References" | Auto: "References" | — |

### Step 4: Switching Between Publishers

When resubmitting a paper from one publisher to another (e.g., Elsevier → IEEE after rejection), change ALL of the following:

**Elsevier → IEEE Migration Checklist:**

```
- [ ] \documentclass{elsarticle} → \documentclass[journal]{IEEEtran}
- [ ] \bibliographystyle{elsarticle-num} → \bibliographystyle{IEEEtran}
- [ ] Remove elsarticle-specific: \journal{}, \begin{frontmatter}...\end{frontmatter}
- [ ] Add IEEE-specific: \IEEEoverridecommandlockouts (if needed)
- [ ] ALL journal names in .bib: full → abbreviated (ISO 4)
      Example: "Information Processing & Management" → "Inf. Process. Manag."
- [ ] Add month field to ALL .bib entries (IEEE requires it)
      Format: month = jun (bare macro, no braces/quotes)
- [ ] Author initials: add spaces ("J.D." → "J. D.")
- [ ] Keywords: \begin{keyword}...\end{keyword} → \begin{IEEEkeywords}...\end{IEEEkeywords}
- [ ] Abstract word limit: ≤200 words (IEEE strict)
- [ ] Recompile and verify: no natbib conflicts, no missing fields
```

**IEEE → Elsevier Migration Checklist:**

```
- [ ] \documentclass[journal]{IEEEtran} → \documentclass[preprint,12pt]{elsarticle}
- [ ] \bibliographystyle{IEEEtran} → \bibliographystyle{elsarticle-num}
- [ ] Add elsarticle-specific: \journal{Your Target Journal}, \begin{frontmatter}
- [ ] Remove IEEE-specific: \IEEEoverridecommandlockouts, \IEEEkeywords
- [ ] ALL journal names in .bib: abbreviated → full
      Example: "IEEE Trans. Knowl. Data Eng." → "IEEE Transactions on Knowledge and Data Engineering"
- [ ] \journal{} MUST be changed from template default (NOT "Nuclear Physics B")
- [ ] Prepare: Highlights, Graphical Abstract, CRediT statement, Cover Letter
- [ ] Keywords: \begin{IEEEkeywords} → \begin{keyword} with \sep separators
- [ ] Remove \usepackage{cite} if conflicts with natbib
- [ ] Recompile and verify: no package conflicts, journal name rendered correctly
```

### Quick BIB Entry Comparison (Same Paper, Two Formats)

**Elsevier format (.bib):**

```bibtex
@article{smith2024anomaly,
  author  = {J. Smith and A.B. Jones and C. Lee},
  title   = {Anomaly Detection in Streaming Graphs Using {CMS}},
  journal = {Information Processing & Management},
  volume  = {61},
  number  = {3},
  pages   = {103--120},
  year    = {2024},
  doi     = {10.1016/j.ipm.2024.103120},
}
```

**IEEE format (.bib) — same paper, different conventions:**

```bibtex
@article{smith2024anomaly,
  author  = {J. Smith and A. B. Jones and C. Lee},
  title   = {Anomaly Detection in Streaming Graphs Using {CMS}},
  journal = {Inf. Process. Manag.},
  volume  = {61},
  number  = {3},
  pages   = {103--120},
  month   = mar,
  year    = {2024},
  doi     = {10.1016/j.ipm.2024.103120},
}
```

**Key differences highlighted:**
1. `journal`: Full name vs abbreviated
2. `author`: `A.B.` vs `A. B.` (space between initials)
3. `month`: Absent vs present (bare macro)

---

## 1. Entry Type Correctness

The most fundamental error: using the wrong `@type`. Google Scholar, Mendeley, and publisher digital libraries frequently assign incorrect entry types.

### Entry Type Selection Rules

| Source | Correct BibTeX Type | Common Error |
|--------|---------------------|-------------|
| Journal article | `@article` | — |
| Conference paper (in proceedings) | `@inproceedings` | Google Scholar often uses `@article` |
| Book (entire) | `@book` | — |
| Book chapter | `@incollection` | Often confused with `@inbook` |
| Book section (by same author as book) | `@inbook` | Often confused with `@incollection` |
| arXiv preprint | `@misc` or `@article` | Should NOT be `@inproceedings` |
| PhD/Master thesis | `@phdthesis` / `@mastersthesis` | Sometimes `@misc` |
| Technical report | `@techreport` | Sometimes `@article` |
| Website / online resource | `@misc` with `howpublished` and `note` | — |

### How to Verify

For each BibTeX entry, check:
1. Does it have a `journal` field? → Should be `@article`
2. Does it have a `booktitle` field (conference proceedings)? → Should be `@inproceedings`
3. Is it from arXiv? → Should be `@misc` with `eprint` field, or `@article` with `journal = {arXiv preprint arXiv:XXXX.XXXXX}`
4. Is there a mismatch between entry type and fields? (e.g., `@article` with `booktitle` → wrong type)

### Required Fields by Entry Type

| Entry Type | Required Fields | Optional but Recommended |
|-----------|----------------|--------------------------|
| `@article` | `author`, `title`, `journal`, `year` | `volume`, `number`, `pages`, `doi`, `month` |
| `@inproceedings` | `author`, `title`, `booktitle`, `year` | `pages`, `doi`, `address`, `publisher`, `month` |
| `@book` | `author` or `editor`, `title`, `publisher`, `year` | `edition`, `address`, `isbn` |
| `@incollection` | `author`, `title`, `booktitle`, `publisher`, `year` | `editor`, `pages`, `chapter` |
| `@misc` | `author`, `title`, `year` | `howpublished`, `note`, `eprint`, `url` |
| `@phdthesis` | `author`, `title`, `school`, `year` | `address`, `month` |
| `@techreport` | `author`, `title`, `institution`, `year` | `number`, `type`, `address` |

---

## 2. Author Name Formatting

### Space Between Initials

BibTeX interprets author names based on spacing. Missing spaces cause BibTeX to treat multiple initials as a single first name.

| Wrong | Correct | Why |
|-------|---------|-----|
| `J.D. Owens` | `J. D. Owens` | BibTeX reads `J.D.` as one first name; no middle initial extracted |
| `A.B.C. Smith` | `A. B. C. Smith` | Same problem — three initials treated as one |

### Hyphenated Names

For names where the second part after the hyphen is lowercase, protect with braces:

| Wrong | Correct | Why |
|-------|---------|-----|
| `Wu-chun Feng` | `Wu-{chun} Feng` | Without braces, BibTeX may split incorrectly |
| `Wen-mei Hwu` | `Wen{-mei} Hwu` | Brace placement protects hyphen handling |

### Special Characters

Non-ASCII characters must use LaTeX escape sequences:

| Wrong | Correct | Character |
|-------|---------|-----------|
| `Müller` | `M{\"u}ller` | ü (umlaut) |
| `Özçelik` | `{\"{O}}z{\c{c}}elik` | Ö, ç |
| `Ñoño` | `{\~{N}}o{\~{n}}o` | Ñ, ñ |
| `Ångström` | `{\\AA}ngstr{\"o}m` | Å, ö |
| `Pérez` | `P{\'{e}}rez` | é (acute accent) |

### Author Name Matching

The author names in BibTeX entries must match what is printed on the published paper:
- If the paper shows "John D. Owens", use `John D. Owens`
- If the paper shows "J. D. Owens", use `J. D. Owens`
- Do NOT normalize names — reproduce what appears on the original paper

### Publisher-Specific Author Format

| Publisher | Author Format in Bibliography | Notes |
|-----------|------------------------------|-------|
| **Elsevier** (`elsarticle-num`) | Initials + Surname: `A.B. Author` | BibTeX style handles conversion |
| **IEEE** (`IEEEtran`) | Initials + Surname: `J. D. Owens` | Spaces between initials required |
| **Springer** | Initials + Surname or full first name | Varies by journal |
| **ACM** (`acmart`) | **Full names**: `John D. Owens` | ACM requires full first names, not initials |

---

## 3. Title Capitalization and Protection

This is the most misunderstood BibTeX feature. BibTeX styles may convert titles to sentence case (lowercase). You must protect words that MUST remain capitalized.

### The Rules

1. **Store titles in title case** (capitalize major words) — match the published paper exactly
2. **Protect proper nouns, acronyms, and method names** with individual braces `{}`
3. **NEVER double-brace the entire title** `{{Like This}}` — this prevents ALL style-based case conversion

### What Must Be Protected

| Category | Examples | BibTeX Syntax |
|----------|---------|---------------|
| **Acronyms** | GPU, CNN, LSTM, IoT, TCP, HTTP | `{GPU}`, `{CNN}`, `{LSTM}`, `{IoT}` |
| **Method/Model names** | BERT, ResNet, Adam, MIDAS, SBEAD | `{BERT}`, `{ResNet}`, `{Adam}`, `{MIDAS}`, `{SBEAD}` |
| **Dataset names** | ImageNet, CIFAR, CICIDS, MNIST | `{ImageNet}`, `{CIFAR}`, `{CICIDS}` |
| **Proper nouns** | Gaussian, Bayesian, Markov, Fourier | `{G}aussian`, `{B}ayesian`, `{M}arkov`, `{F}ourier` |
| **Tool/System names** | TensorFlow, PyTorch, Linux, Apache | `{TensorFlow}`, `{PyTorch}`, `{Linux}` |
| **Language names** | Python, Java, SQL, LaTeX | `{Python}`, `{Java}`, `{SQL}`, `{LaTeX}` |
| **Organization names** | IEEE, ACM, NIST, DARPA | `{IEEE}`, `{ACM}`, `{NIST}` |
| **Chemical/Math symbols** | CO₂, pH, NP-hard | `{CO$_2$}`, `p{H}`, `{NP}-hard` |

### Examples

```bibtex
% WRONG — no protection, "GPU" and "CUDA" may become lowercase
title = {Efficient GPU Computing Using CUDA for Deep Learning},

% WRONG — double-braced entire title, prevents ALL case conversion
title = {{Efficient GPU Computing Using CUDA for Deep Learning}},

% CORRECT — individual protection for acronyms and proper nouns
title = {Efficient {GPU} Computing Using {CUDA} for Deep Learning},

% WRONG — method name not protected
title = {BERT: pre-training of deep bidirectional transformers},

% CORRECT — method name and proper noun protected
title = {{BERT}: Pre-training of Deep Bidirectional Transformers},

% WRONG — dataset name not protected
title = {Anomaly detection on the CICIDS2017 dataset},

% CORRECT — dataset name protected
title = {Anomaly Detection on the {CICIDS2017} Dataset},
```

### The Double-Brace Anti-Pattern

**NEVER do this:**

```bibtex
title = {{My Entire Title With All Words Capitalized}}
```

Why it's wrong:
- Most BibTeX styles apply their own capitalization rules (title case or sentence case)
- Double braces tell BibTeX "do not touch this at all"
- If the journal uses sentence case, your title will stick out with incorrect capitalization
- If the style changes in revision, all double-braced titles remain wrong

**Instead:** Single-brace or quote the title, protect only individual words:

```bibtex
title = {My entire title with {GPU} and {BERT} protected}
```

---

## 4. Journal Name Rules

### The Core Rule: Consistency Within One Paper

All journal names in the bibliography must follow ONE convention:
- **All abbreviated** (IEEE requires this)
- **All full names** (Elsevier and ACM typically use this)
- **NEVER mix** abbreviated and full names in the same bibliography

### Publisher-Specific Rules

| Publisher | Journal Name Requirement | Standard | Example |
|-----------|------------------------|----------|---------|
| **IEEE** | **Must abbreviate** per ISO 4 / IEEE standard | IEEE abbreviation list | `IEEE Trans. Pattern Anal. Mach. Intell.` |
| **Elsevier** | **Full name** (most journals) | Journal's official name | `Information Processing & Management` |
| **ACM** | **Full name** | ACM publication list | `ACM Computing Surveys` |
| **Springer** | Varies by journal — check Guide for Authors | ISO 4 or full | Depends on specific journal |

### IEEE Journal Abbreviation Rules (ISO 4)

IEEE requires abbreviated journal names. Common abbreviation patterns:

| Full Word | Abbreviation | Full Word | Abbreviation |
|-----------|-------------|-----------|-------------|
| Transactions | Trans. | International | Int. |
| Journal | J. | Conference | Conf. |
| Proceedings | Proc. | Computing | Comput. |
| Communications | Commun. | Information | Inf. |
| Engineering | Eng. | Intelligence | Intell. |
| Processing | Process. | Management | Manag. |
| Networks | Netw. | Analysis | Anal. |
| Systems | Syst. | Applications | Appl. |
| Science | Sci. | Technology | Technol. |
| Security | Secur. | Software | Softw. |
| Knowledge | Knowl. | Learning | Learn. |
| Pattern | Pattern | Machine | Mach. |
| Artificial | Artif. | Detection | Detect. |

### Common IEEE Journal Abbreviation Examples

| Full Name | IEEE Abbreviation |
|-----------|-------------------|
| IEEE Transactions on Pattern Analysis and Machine Intelligence | `IEEE Trans. Pattern Anal. Mach. Intell.` |
| IEEE Transactions on Knowledge and Data Engineering | `IEEE Trans. Knowl. Data Eng.` |
| IEEE Transactions on Information Forensics and Security | `IEEE Trans. Inf. Forensics Secur.` |
| IEEE Transactions on Neural Networks and Learning Systems | `IEEE Trans. Neural Netw. Learn. Syst.` |
| IEEE Access | `IEEE Access` (not abbreviated — single-word titles stay) |
| IEEE Internet of Things Journal | `IEEE Internet Things J.` |

### How to Find Correct Abbreviations

1. **IEEE**: Use `IEEEabrv.bib` bundled with `IEEEtran` package, or search IEEE Xplore
2. **General**: Use the ISSN LTWA (List of Title Word Abbreviations) at https://www.issn.org/services/online-services/access-to-the-ltwa/
3. **Verification**: CASSI (Chemical Abstracts Service Source Index) search tool

### Conference Name (booktitle) Rules

Conference proceedings names in BibTeX are also frequently wrong:

| Wrong (from IEEE digital library) | Correct |
|-----------------------------------|---------|
| `booktitle={Intelligent Vehicles Symposium (IV), 2011 IEEE}` | `booktitle={Proceedings of the 2011 IEEE Intelligent Vehicles Symposium}` |
| `booktitle={CVPR}` | `booktitle={Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition}` |
| `booktitle={NeurIPS}` | `booktitle={Advances in Neural Information Processing Systems}` |

**Rule**: ALWAYS verify and correct conference names from digital library exports. Both ACM and IEEE export incorrectly formatted conference names.

---

## 5. DOI Policy by Publisher

DOI (Digital Object Identifier) requirements vary by publisher. Some require DOI, some recommend it, and some bibliography styles don't even display it.

### DOI Requirement Matrix

| Publisher | DOI Required? | BibTeX Style Displays DOI? | Action |
|-----------|--------------|---------------------------|--------|
| **Elsevier** (`elsarticle-num`) | Recommended | **No** — `elsarticle-num.bst` does NOT render the `doi` field | Include `doi` in .bib for your records; it won't appear in output |
| **Elsevier** (`elsarticle-harv`) | Recommended | **No** | Same as above |
| **IEEE** (`IEEEtran`) | Recommended | Depends on style options | Check compiled output; add `doi` field regardless |
| **Springer** | Recommended | Varies by journal | Check Guide for Authors |
| **ACM** (`acmart`) | **Required** | **Yes** | Must include `doi` for every entry that has one |

### DOI Format in BibTeX

| Wrong | Correct | Why |
|-------|---------|-----|
| `doi = {https://doi.org/10.1145/1234567}` | `doi = {10.1145/1234567}` | Store numbers only, not the full URL |
| `doi = {http://dx.doi.org/10.1145/1234567}` | `doi = {10.1145/1234567}` | `dx.doi.org` is deprecated; store numbers only |
| `url = {https://doi.org/10.1145/1234567}` (with `doi` field also present) | Remove `url`, keep `doi` only | Redundant — DOI and URL pointing to same resource |

### DOI Consistency Rule

Within one bibliography:
- If MOST entries have DOI → all entries that have DOIs should include them
- If the BibTeX style does NOT display DOI → still include them in .bib for archival
- NEVER have some entries with DOI URL in `url` field and others with DOI in `doi` field — pick one convention

---

## 6. Page Number and Volume Formatting

### Page Range Format

| Wrong | Correct | Why |
|-------|---------|-----|
| `pages = {35-49}` | `pages = {35--49}` | En-dash (`--`) required for ranges |
| `pages = {35 - 49}` | `pages = {35--49}` | No spaces around dash |
| `pages = {35—49}` | `pages = {35--49}` | Em-dash is wrong; use en-dash |
| `pages = {1}` (but paper has page range) | `pages = {1--10}` | Include full range |

### Article Number Format (for journals without page numbers)

Some journals use article numbers instead of page numbers:

```bibtex
% For journals that use article numbers (e.g., IEEE Access, many Elsevier journals)
pages = {12345},          % Single article number
% OR
pages = {12:1--12:10},    % Paper number : page range (common in ACM)
```

### Duplicate Page Numbers

If a digital library lists `pages = {1}` for multiple papers from the same proceedings, the page numbers are wrong (some conferences like IPDPS 2009 failed to assign unique page numbers). In this case:
- Try to find the correct paper number and use `n:1--n:K` format
- If unable to determine, leave `pages` out entirely — it's better than incorrect data

### Volume and Number Fields

| Entry Type | `volume` | `number` |
|-----------|----------|----------|
| Journal article | Volume number (e.g., `42`) | Issue number (e.g., `3`) |
| Conference proceedings | Usually absent | Usually absent |
| arXiv preprint | Absent | Absent |

---

## 7. Month Field Formatting

### BibTeX Built-in Month Abbreviations

BibTeX has built-in month macros. These are NOT strings — they have no quotes or braces:

| Wrong | Correct | Why |
|-------|---------|-----|
| `month = {june}` | `month = jun` | IEEE digital library format is wrong; use BibTeX macro |
| `month = {June}` | `month = jun` | No braces or quotes for month macros |
| `month = "6"` | `month = jun` | Not a number — use the macro |
| `month = {Jan.}` | `month = jan` | Abbreviated string is wrong; use macro |

### All BibTeX Month Macros

```
jan  feb  mar  apr  may  jun  jul  aug  sep  oct  nov  dec
```

### Month Ranges

```bibtex
month = jun # "/" # jul,           % June/July
month = "18~" # dec,               % December 18
```

### When to Include Month

- **Recommended**: Always include for your own records (helps determine publication priority)
- **Required by IEEE**: IEEE expects months in references
- **Optional for Elsevier/Springer/ACM**: Check specific journal

---

## 8. URL Formatting

### Rules

1. Wrap URLs in `\url{}` (requires `\usepackage{url}` in preamble) — enables line breaking
2. If a `doi` field is present, do NOT also include the DOI as a `url` — this is redundant
3. Use `https://` not `http://` when available
4. Include `note = {Accessed: 2025-01-15}` for web resources to record access date

### Examples

```bibtex
% CORRECT — url wrapped properly
howpublished = {\url{https://github.com/example/repo}},

% WRONG — url not wrapped
howpublished = {https://github.com/example/repo},

% WRONG — DOI duplicated as both doi and url
doi = {10.1145/1234567},
url = {https://doi.org/10.1145/1234567},

% CORRECT — DOI only
doi = {10.1145/1234567},
```

---

## 9. Cross-Reference Consistency

### Citation-Bibliography Matching

| Check | How to Detect | Action |
|-------|-------------|--------|
| `\cite{key}` in .tex but no entry in .bib | LaTeX warning: "Citation `key` undefined" | Add missing entry or fix key typo |
| Entry in .bib but never `\cite{}`d | Compile with `--verbose`; or grep .bib keys against .tex | Remove unused entries (cleanup) |
| Duplicate citation keys | Two `@type{samekey,` in .bib | Rename one key |
| Author name mismatch | Text says "Lo et al." but .bib first author is not "Lo" | Fix .bib author or text |

### In-Text Citation Format Rules

| Rule | Wrong | Correct |
|------|-------|---------|
| Citations as words (numbered style) | "A method is described in [15]." | "A method is described by Author et al. [15]." |
| Non-breaking space before cite | `work \cite{key}` | `work~\cite{key}` |
| Grouped citations sorted | `[8, 6, 10]` | `[6, 8, 10]` (use `\usepackage{cite}` for auto-sorting) |
| Multiple citations | `\cite{a}\cite{b}` | `\cite{a,b}` |

---

## 10. Publisher-Specific Reference Format Summary

### Elsevier (`elsarticle-num`)

```
Format: [#] A.B. Author, C.D. Author, Title, Journal Name Volume (Year) pages.
```

| Item | Requirement |
|------|-------------|
| Journal name | **Full name** (not abbreviated) |
| Author format | Initials + Surname |
| DOI | Recommended; `elsarticle-num.bst` does NOT render it in output |
| Title | Included |
| Pages | `pages` field with `--` |
| Month | Optional |

### IEEE (`IEEEtran`)

```
Format: [#] A. B. Author, "Title," Abbreviated Journal Name, vol. V, no. N, pp. X–Y, Mon. Year.
```

| Item | Requirement |
|------|-------------|
| Journal name | **Must abbreviate** per ISO 4 / IEEE standard |
| Author format | Initials (with spaces) + Surname |
| DOI | Recommended |
| Title | In quotation marks (style handles this) |
| Article title | **Quotation marks** added by style |
| Journal title | **Italics** added by style |
| Pages | `pp. X--Y` |
| Month | **Required**, use BibTeX macros (`jan`, `feb`, ...) |
| et al. | When ≥6 authors |

### Springer

```
Format varies by journal — check specific Guide for Authors.
```

| Item | Requirement |
|------|-------------|
| Journal name | Some abbreviated (ISO 4), some full — check specific journal |
| Author format | Varies by journal |
| DOI | Recommended |
| Pages | With or without `pp.` — depends on style |

### ACM (`acmart`)

```
Format: [#] Author Full Name. Year. Title. Journal Full Name Volume, Issue (Month Year), pages. https://doi.org/DOI
```

| Item | Requirement |
|------|-------------|
| Journal name | **Full name** |
| Author format | **Full names** (not initials) |
| DOI | **Required** — must include for every entry |
| Title | Included |
| Year | Appears early in the reference |

---

## 11. Common Errors from Digital Library Exports

**Golden rule**: NEVER trust auto-generated BibTeX from Google Scholar, ACM DL, IEEE Xplore, or DBLP without manual verification.

### Google Scholar Errors

| Error | Frequency | Example |
|-------|-----------|---------|
| Wrong entry type | Very common | Conference paper exported as `@article` |
| Missing fields | Common | No `pages`, no `volume`, no `doi` |
| Incorrect capitalization | Common | All-lowercase or all-uppercase titles |
| Duplicate entries | Occasional | Same paper with different keys |
| arXiv version instead of published | Common | Links to arXiv when journal version exists |

### IEEE Xplore Errors

| Error | Example |
|-------|---------|
| Venue name scrambled | `booktitle={Intelligent Vehicles Symposium (IV), 2011 IEEE}` |
| Author names without spaces between initials | `author={Glavtchev, V. and Muyan-Ozcelik, P.}` — missing accents, wrong spacing |
| Month as string not macro | `month={june}` instead of `month=jun` |
| Page numbers with spaces | `pages={195 -200}` instead of `pages={195--200}` |
| Title not matching paper | Lowercase conversion applied |

### ACM Digital Library Errors

| Error | Example |
|-------|---------|
| Title capitalization wrong | `title = {Register packing for cyclic reduction: a case study}` — should be title case |
| DOI stored as full URL | `doi = {http://doi.acm.org/10.1145/1234567}` — should be numbers only |
| Redundant URL field | Both `doi` and `url` pointing to same DOI |
| Unnecessary metadata fields | `acmid`, `isbn`, `numpages` — may cause warnings with some styles |

---

## 12. Complete BibTeX Entry Checklist

Run this checklist on EVERY entry in the .bib file:

```
Reference Entry Checklist (per entry):
- [ ] Entry type correct (@article / @inproceedings / @misc / etc.)
- [ ] All required fields present for this entry type
- [ ] Author names: spaces between initials (J. D. not J.D.)
- [ ] Author names: special characters escaped ({\"o}, {\'{e}}, etc.)
- [ ] Author names: hyphenated names braced (Wu-{chun} Feng)
- [ ] Title: proper nouns, acronyms, method names protected with {}
- [ ] Title: NOT double-braced ({{Title}})
- [ ] Title: matches the published paper's capitalization
- [ ] Journal name: follows publisher convention (abbreviated for IEEE, full for Elsevier/ACM)
- [ ] Journal name: consistent style across ALL entries (not mixed)
- [ ] Conference name (booktitle): correctly formatted, not raw digital library export
- [ ] Pages: uses en-dash (--), not hyphen (-) or em-dash (---)
- [ ] Volume and number: present for journal articles
- [ ] Month: uses BibTeX macro (jan, feb, ...) without quotes or braces
- [ ] DOI: stored as numbers only (10.xxxx/xxxx), not full URL
- [ ] DOI: no redundant url field pointing to same DOI
- [ ] Year: correct and matches the published version (not preprint year if published)
- [ ] No trailing comma after last field (some parsers fail)
- [ ] Closing brace present and not nesting into next entry
```

```
Reference List Checklist (whole bibliography):
- [ ] Every \cite{key} has a matching .bib entry
- [ ] No unused .bib entries (optional cleanup)
- [ ] No duplicate citation keys
- [ ] Journal name style consistent (all abbreviated OR all full, never mixed)
- [ ] DOI inclusion consistent (all or none, not random)
- [ ] Author name format consistent across entries
- [ ] In-text citations use ~ before \cite{} (non-breaking space)
- [ ] Grouped citations sorted numerically: [6, 8, 10] not [8, 6, 10]
- [ ] Author names in text match .bib first author
- [ ] Reference list compiled with zero LaTeX/BibTeX warnings
```
