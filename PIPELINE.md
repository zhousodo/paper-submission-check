# Paper Skills Pipeline — End-to-End Orchestration Guide

[English](#english) | [中文](#中文)

---

## English

This document defines the **recommended execution order**, **stage-gate criteria**, and **decision logic** for processing a draft paper through all four skills. The goal: a raw draft goes in, a submission-ready paper comes out — with minimal errors.

## Pipeline Overview

```
                        ┌─────────────────────────────────────────────────────────┐
                        │                PAPER SKILLS PIPELINE                    │
                        │                                                         │
  Raw Draft             │  Stage 1         Stage 2         Stage 3      Stage 4   │   Submission-
  (CN/EN)   ──────────▶ │  English    ───▶  Multi-Round ──▶ Figure  ──▶ Submission │ ──▶  Ready
                        │  Polishing        Review          Styling     Check      │     Paper
                        │                     │                                    │
                        │                     ▼                                    │
                        │               Author Revision                            │
                        │               (manual step)                              │
                        └─────────────────────────────────────────────────────────┘
```

## The Four Stages (Detailed)

### Stage 1: English Polishing (`paper-english-polishing`)

**When**: The paper is a Chinese draft or an English draft needing language refinement.

**Input**: `paper.tex` (raw draft, Chinese or English)

**What it does**:
- Translates Chinese → English (if applicable) with bilingual scaffolding
- Eliminates Chinglish patterns
- Enforces academic register and sentence structure
- Maintains terminology consistency via a running ledger
- Removes AI-generated style markers
- Adds `\zhpara{}` bilingual comparison mode (Chinese source only)

**Key phases (0–11)**:
1. Pre-analysis: scope lock, venue detection, terminology ledger
2. Structural replacement: titles, theorems, TikZ, table headers, algorithms
3. Section-by-section polishing: Title/Abstract → Introduction → Related Work → Method → Experiments → Discussion → Conclusion → Appendix
4. Cross-section consistency verification
5. Final AI-style sweep
6. Submission toggle (`\showchinesefalse`)

**Output**: Polished `.tex` file + Polishing Report (statistics, terminology ledger, pronoun counts)

**Stage gate — proceed to Stage 2 when**:
- [ ] All sections translated and polished (no Chinese remaining in body text for EN-only mode)
- [ ] Terminology ledger finalized — every concept uses one chosen term
- [ ] Pronoun counts within budget: "we" ≤5, "our" ≤3
- [ ] Zero Tier-1 AI phrases detected
- [ ] LaTeX compiles without errors

---

### Stage 2: Multi-Round Review (`paper-multi-round-review`)

**When**: The paper has clean English text and is ready for technical scrutiny.

**Input**: Polished `paper.tex` + `refs.bib` (from Stage 1 output)

**What it does**:
- Assigns 4 specialized reviewer roles (Domain Expert, Methods Expert, Methodology Expert, Presentation Expert)
- Each reviewer produces an independent review with severity-labeled weaknesses (Critical/Major/Minor)
- Area Chair synthesizes a meta-review with consensus and divergence
- Generates a prioritized rebuttal plan
- Guides revision execution with diff tracking
- Optional re-review after revisions

**Key phases (0–6)**:
1. Paper intake: venue detection, domain identification, model assignment
2. Independent reviews: 4 reviewers in parallel
3. Meta-review synthesis: consensus strengths/weaknesses, critical path to acceptance
4. Author response guidance: rebuttal strategy per weakness
5. Revision execution: implement changes with revision log
6. Re-review (optional): verify fixes, update scores
7. Final polish: handoff to next stage

**Output**: 4 reviewer reports + Meta-review + Rebuttal plan + Revision log

**Decision point after Stage 2**:
```
Meta-Review Preliminary Decision:
├── Strong Accept / Accept / Weak Accept
│   └── Proceed to Stage 3 (address Minor issues during Stage 3-4)
├── Borderline Accept / Borderline Reject
│   └── Address ALL Critical + Major issues first
│   └── Re-run Stage 2 Phase 5 (Re-Review) to verify fixes
│   └── If re-review passes → proceed to Stage 3
│   └── If re-review fails → revise and re-run Stage 2
├── Revise & Resubmit / Reject
│   └── Major revision needed — revise substantially
│   └── Re-run Stage 1 (if language quality affected) then Stage 2
```

**Stage gate — proceed to Stage 3 when**:
- [ ] All Critical weaknesses addressed (zero remaining)
- [ ] All Major weaknesses addressed or justified in rebuttal
- [ ] Re-review scores ≥ 5 (Borderline Accept) from all reviewers
- [ ] Revision log complete with section/line references
- [ ] Numbers in text still match tables after revision (cross-check!)

---

### Stage 3: Figure Styling (`paper-figure-styling`)

**When**: Paper content is stable (no more major text changes expected).

**Input**: Revised `paper.tex` (from Stage 2 output)

**What it does**:
- Establishes a unified color system (Okabe-Ito palette by default)
- Audits typography across all figures (font family, sizes, math mode)
- Standardizes axis, grid, and aspect ratios per chart type
- Applies triple encoding (color + line style + marker)
- Reviews each figure type against its specific checklist
- Designs legend system (position, shared legends, direct labels)
- Validates statistical visualization (error bars, significance markers)
- Optimizes float placement and page density
- Standardizes table visual design (booktabs, no vertical lines)
- Formats captions and panel labels
- Audits cross-references and hyperlink colors
- Ensures cross-figure consistency (same entity = same color everywhere)
- Validates export quality (vector format, font embedding)
- Tests grayscale and colorblind accessibility
- Checks publisher compliance

**Key phases (1–15)**:
1. Color system → Typography → Axis/Grid → Encoding → Per-type review → Legend → Statistics → Layout → Tables → Captions → Cross-refs → Consistency → Export → Accessibility → Publisher compliance

**Output**: Updated `.tex` with figure/table fixes + Figure styling checklist report

**Stage gate — proceed to Stage 4 when**:
- [ ] All figures use the same palette (semantic lock verified)
- [ ] All figures pass per-type checklist
- [ ] Cross-figure consistency audit passed (color, font, axis, legend)
- [ ] All tables use booktabs, no vertical lines, best/second-best marked correctly
- [ ] Float placement produces no full-page whitespace
- [ ] Grayscale test passed — all series distinguishable
- [ ] LaTeX compiles without overfull hbox warnings from figures/tables

---

### Stage 4: Submission Check (`paper-submission-check`)

**When**: The paper is nearly final — this is the last quality gate.

**Input**: Near-final `paper.tex` + `refs.bib`

**What it does**:
- Detects citation style and template
- Checks pronoun counts against thresholds
- Scans for AI-generated style patterns (3 severity levels, 21+ focal words)
- Finds symbol and punctuation errors (mixed Chinese/English, dash errors)
- Verifies capitalization consistency
- Audits LaTeX-specific issues (non-breaking spaces, float placement)
- Checks grammar and language quality (contractions, comma splices, tense)
- Verifies tables/figures (cross-references, number consistency, bold/underline markings)
- Audits content structure (abstract PRKS, introduction CARS, keywords, sections)
- Deep-checks BIB file (entry types, title protection, journal abbreviations, DOI)
- Verifies reference content authenticity
- Produces a final pre-submission checklist

**Key phases (0–10)**:
1. Pre-check → Pronouns → AI-style → Symbols → Capitalization → LaTeX → Grammar → Tables/Figures → Structure → BIB → Final checklist

**Output**: Structured issue report (Critical / Warning / Suggestion) + Final submission checklist

**Decision point after Stage 4**:
```
Issue Report Summary:
├── 0 Critical + 0 Warning
│   └── Ready to submit!
├── 0 Critical + N Warnings
│   └── Fix all warnings → re-run Stage 4 (quick pass)
├── N Critical issues
│   └── Fix all critical issues
│   └── If fixes are language-related → re-run Stage 1 then Stage 4
│   └── If fixes are content-related → re-run Stage 2 then Stage 4
│   └── If fixes are figure-related → re-run Stage 3 then Stage 4
│   └── Otherwise → fix and re-run Stage 4
```

**Submission gate — paper is ready when**:
- [ ] Zero Critical issues
- [ ] Zero Warning issues (or explicitly accepted by author)
- [ ] Abstract numbers match result tables
- [ ] Each contribution maps to a specific section
- [ ] All `\label{}` have matching `\ref{}` and vice versa
- [ ] BIB compiles without errors; all `\cite{}` resolved
- [ ] Table bold/underline markings verified correct
- [ ] `\journal{}` contains correct target journal name
- [ ] Page limit satisfied
- [ ] LaTeX compiles with zero errors and minimal warnings

---

## Quick Reference: When to Loop Back

| Situation | Action |
|-----------|--------|
| Stage 2 review finds language issues | Loop to Stage 1, then return to Stage 2 |
| Stage 2 review finds figure issues | Note for Stage 3; no loop needed |
| Stage 2 review requires new experiments | Author revises, then re-run Stage 2 |
| Stage 3 finds content errors in captions | Fix in-place; no loop needed |
| Stage 4 finds AI-style phrases | Fix in-place; re-run Stage 4 |
| Stage 4 finds BIB errors | Fix BIB; re-run Stage 4 Phase 9 only |
| Stage 4 finds number mismatches | Fix source; if extensive, re-run Stage 2 |
| Major revision after Stage 4 | Loop to Stage 1 → Stage 2 → Stage 3 → Stage 4 |

---

## Cross-Skill Data Flow

```
                Input           Skill                   Output                  Consumed By
                ─────           ─────                   ──────                  ───────────
                paper.tex  ──▶  paper-english-polishing  ──▶  polished.tex       Stage 2, 3, 4
                                                         ──▶  terminology ledger  Stage 2 (reviewer context)
                                                         ──▶  pronoun counts      Stage 4 (verification)

                polished.tex──▶ paper-multi-round-review ──▶  reviewer reports    Author (revision)
                refs.bib                                 ──▶  meta-review         Author (prioritization)
                                                         ──▶  revision log        Stage 4 (consistency check)

                revised.tex ──▶ paper-figure-styling     ──▶  styled.tex          Stage 4
                                                         ──▶  figure checklist    Stage 4 (verification)

                styled.tex  ──▶ paper-submission-check   ──▶  issue report        Author (final fixes)
                refs.bib                                 ──▶  submission checklist Author (sign-off)
```

---

## Shared Rules Across All Stages

These rules are enforced consistently across all four skills:

| Rule | Stage 1 | Stage 2 | Stage 3 | Stage 4 |
|------|---------|---------|---------|---------|
| Pronoun budget: "we" ≤5, "our" ≤3 | Enforced during polishing | Checked in presentation review | — | Final verification |
| Zero Tier-1 AI phrases | Avoided during rewriting | Flagged if found | — | Hard ban (zero tolerance) |
| Terminology consistency | Ledger created and maintained | Verified in reviews | Verified in figure legends/captions | Verified in final check |
| Acronym first-use protocol | Full name at first use | Verified by Reviewer D | — | Systematic inventory check |
| Number accuracy | Preserved during translation | Cross-checked in review | Preserved in figures | Final cross-reference verification |
| Equation/variable consistency | Preserved during translation | Checked by Reviewer B | — | Cross-equation variable audit |
| Algorithm pseudocode style | Algorithm text translated | — | — | Keyword/variable consistency check |
| Chinese comments allowed | `%` comments preserved | — | — | PDF verified: zero CN in body |
| LaTeX safety | Commands preserved exactly | — | Float/figure LaTeX validated | Compilation verified |
| booktabs tables (no vertical lines) | — | Flagged if violated | Enforced and styled | Verified |
| Table header length | — | — | ≤3 words; abbreviated with notes | Caption conciseness verified |

---

## One-Prompt Full Pipeline (Copy-Paste)

For users who want to run all stages in sequence:

### English

```text
Step 1: Use $paper-english-polishing to polish @paper.tex section by section.
        Output the polishing report and terminology ledger when done.
Step 2: Use $paper-multi-round-review to review the polished draft.
        Output all 4 reviewer reports and the meta-review.
Step 3: List only Critical and Major issues from the meta-review.
        Revise @paper.tex to address each issue. Output a revision log.
Step 4: Use $paper-figure-styling to audit and fix all figures and tables in @paper.tex.
        Output the figure styling checklist.
Step 5: Use $paper-submission-check to check @paper.tex and @refs.bib.
        Output the full issue report.
Step 6: Fix any remaining Critical or Warning issues.
        Output the final must-fix checklist before submission.
```

### 中文

```text
第 1 步：用 $paper-english-polishing 逐段润色 @paper.tex。
        完成后输出润色报告和术语表。
第 2 步：用 $paper-multi-round-review 评审润色后的稿件。
        输出全部 4 位评审报告和 Meta-Review。
第 3 步：仅列出 Meta-Review 中的 Critical 和 Major 问题。
        逐项修改 @paper.tex，输出修订日志。
第 4 步：用 $paper-figure-styling 审查并修复 @paper.tex 中的所有图表。
        输出图表样式检查清单。
第 5 步：用 $paper-submission-check 检查 @paper.tex 和 @refs.bib。
        输出完整问题报告。
第 6 步：修复所有剩余的 Critical 和 Warning 问题。
        输出投稿前终检清单。
```

---

## Estimated Time per Stage

| Stage | Skill | Typical Duration (AI agent) | Notes |
|-------|-------|-----------------------------|-------|
| 1 | paper-english-polishing | 15–40 min (full paper) | Longer for Chinese → English translation |
| 2 | paper-multi-round-review | 10–25 min | Phase 1 parallelizable; revision adds time |
| 3 | paper-figure-styling | 10–20 min | Depends on figure count |
| 4 | paper-submission-check | 5–15 min | Fast scanning; BIB deep-check adds time |
| — | **Total (first pass)** | **40–100 min** | Loops add 20–50% |

---

## FAQ

### Q: Can I skip a stage?

**Stage 1** (Polishing): Skip if the paper is already in polished English (native speaker quality). But still recommended — even native speakers benefit from academic register enforcement and AI-style removal.

**Stage 2** (Review): Skip only for camera-ready revisions where content is frozen. Never skip for first submissions.

**Stage 3** (Figure Styling): Skip if the paper has no figures/tables (rare) or if figures are already styled to venue specs.

**Stage 4** (Submission Check): **Never skip.** This is the final safety net.

### Q: Can I run stages in parallel?

Stage 1 must complete before Stage 2 (review needs clean text). Stages 3 and 4 are technically independent but Stage 4 should be last. In practice, **run sequentially** for best results.

### Q: What if I only have one AI model (no multi-model)?

Stage 2 (`paper-multi-round-review`) is designed for multi-model diversity. If only one model is available, see the single-model mitigation protocol in `skills/paper-multi-round-review/multi-model-strategy.md`.

### Q: My paper is already in English — do I need Stage 1?

Yes, but set it to "English revision / camera-ready" mode (light polishing). Stage 1 still catches:
- AI-style phrases introduced during drafting
- Chinglish patterns (if any co-author is a non-native speaker)
- Pronoun overuse
- Terminology inconsistency

---

## 中文

本文档定义了四个 Skill 的**推荐执行顺序**、**阶段门控标准**和**决策逻辑**。目标：一篇初稿输入，一篇投稿级论文输出——最大限度减少错误。

## 流水线概览

```
                        ┌──────────────────────────────────────────────────────────┐
                        │                  论文 SKILL 流水线                        │
                        │                                                          │
  初稿                  │  阶段 1          阶段 2          阶段 3       阶段 4       │   投稿级
  (中/英)  ──────────▶  │  英文润色    ───▶ 多轮评审   ───▶ 图表美化 ──▶ 投稿终检    │ ──▶ 论文
                        │                    │                                     │
                        │                    ▼                                     │
                        │               作者修改                                    │
                        │              (人工步骤)                                   │
                        └──────────────────────────────────────────────────────────┘
```

## 四个阶段详解

### 阶段 1：英文润色 (`paper-english-polishing`)

**适用时机**：论文为中文初稿或需要语言打磨的英文初稿。

**做什么**：
- 中译英翻译 + 双语对照脚手架
- 消除中式英语模式
- 学术语域规范化
- 术语一致性维护
- AI 风格清除

**阶段门控——满足以下条件后进入阶段 2**：
- [ ] 所有章节翻译/润色完成
- [ ] 术语表已定稿
- [ ] 代词计数达标："we" ≤5, "our" ≤3
- [ ] 零 Tier-1 AI 短语
- [ ] LaTeX 编译无报错

---

### 阶段 2：多轮评审 (`paper-multi-round-review`)

**适用时机**：论文英文文本已干净，可进行技术审查。

**做什么**：
- 4 位专业审稿人独立评审（领域专家、方法专家、实验方法学专家、写作专家）
- AC 综合评审 + 共识/分歧分析
- 反驳策略生成
- 修改执行与跟踪
- 可选的再评审

**阶段 2 后的决策**：
```
Meta-Review 初步决定：
├── Strong Accept / Accept / Weak Accept
│   └── 进入阶段 3（Minor 问题在后续阶段中修复）
├── Borderline Accept / Borderline Reject
│   └── 先修复所有 Critical + Major 问题
│   └── 重新评审验证修复
│   └── 通过后 → 进入阶段 3
├── Revise & Resubmit / Reject
│   └── 大修后重新从阶段 1 或阶段 2 开始
```

**阶段门控——满足以下条件后进入阶段 3**：
- [ ] 所有 Critical 问题已解决
- [ ] 所有 Major 问题已解决或在反驳中论证
- [ ] 修订日志完整
- [ ] 修改后的数字仍与表格一致

---

### 阶段 3：图表美化 (`paper-figure-styling`)

**适用时机**：论文内容已稳定（不再有大的文字改动）。

**做什么**：
- 统一配色方案 + 语义绑定
- 字体、坐标轴、网格标准化
- 三重编码（颜色 + 线型 + 标记）
- 按图表类型逐一检查
- 图例系统设计
- 浮动体布局优化
- 表格视觉设计
- 可访问性测试
- 出版社规范合规检查

**阶段门控——满足以下条件后进入阶段 4**：
- [ ] 所有图表使用统一配色
- [ ] 跨图一致性审计通过
- [ ] 灰度测试通过
- [ ] 表格使用 booktabs，无竖线
- [ ] 浮动体布局无大面积空白

---

### 阶段 4：投稿终检 (`paper-submission-check`)

**适用时机**：论文已接近最终版——这是最后一道安全门。

**做什么**：
- 代词计数验证
- AI 风格深度扫描（3 级严重度，21+ 标志词）
- 标点符号和空格检查
- LaTeX 问题审计
- 语法和语言质量检查
- 表格/图表交叉引用验证
- 内容结构审计（PRKS、CARS）
- BIB 文件深度检查
- 参考文献真实性验证
- 投稿前终检清单

**投稿门控——满足以下条件即可投稿**：
- [ ] Critical 问题为零
- [ ] Warning 问题为零（或作者明确接受）
- [ ] 摘要数字 = 结果表格数字
- [ ] 所有 `\label{}` 有对应 `\ref{}`
- [ ] BIB 编译无错误
- [ ] 页数符合要求
- [ ] LaTeX 编译零错误

---

## 回退规则速查

| 情况 | 操作 |
|------|------|
| 阶段 2 评审发现语言问题 | 回退阶段 1，再回阶段 2 |
| 阶段 2 评审发现图表问题 | 记下，阶段 3 处理 |
| 阶段 2 评审需要新实验 | 作者修改后重跑阶段 2 |
| 阶段 4 发现 AI 风格短语 | 就地修复，重跑阶段 4 |
| 阶段 4 发现 BIB 错误 | 修复 BIB，重跑阶段 4 第 9 阶段 |
| 阶段 4 发现数字不匹配 | 修复源头；严重则回退阶段 2 |
| 阶段 4 后需大修 | 回退阶段 1 → 2 → 3 → 4 |

---

## 跨 Skill 一致性规则

| 规则 | 阶段 1 | 阶段 2 | 阶段 3 | 阶段 4 |
|------|--------|--------|--------|--------|
| 代词预算 "we" ≤5, "our" ≤3 | 润色时执行 | 写作评审时检查 | — | 最终验证 |
| 零 Tier-1 AI 短语 | 改写时避免 | 发现则标记 | — | 硬性禁止 |
| 术语一致性 | 建立并维护术语表 | 评审中验证 | 图表图例/标题中验证 | 终检中验证 |
| 缩写首次全称协议 | 首次使用给全称 | Reviewer D 验证 | — | 系统化清查 |
| 数字准确性 | 翻译时保留 | 评审中交叉核查 | 图表中保留 | 最终交叉引用验证 |
| 公式/变量一致性 | 翻译时保留 | Reviewer B 检查 | — | 跨公式变量审计 |
| 算法伪代码规范 | 算法文本翻译 | — | — | 关键字/变量一致性检查 |
| 中文注释允许保留 | `%` 注释不翻译 | — | — | PDF 验证：正文零中文 |
| 表头长度 | — | — | ≤3 词；缩写加脚注 | 标题简洁性验证 |

---

## 常见问题

### 问：可以跳过某个阶段吗？

- **阶段 1**：如果论文已是高质量英文可跳过，但仍推荐运行以捕获 AI 风格和术语不一致。
- **阶段 2**：仅在内容已冻结的 camera-ready 修改中可跳过。首次投稿不可跳过。
- **阶段 3**：如果论文无图表（罕见）或图表已完全合规可跳过。
- **阶段 4**：**永远不要跳过。** 这是最后的安全网。

### 问：可以并行运行吗？

阶段 1 必须在阶段 2 之前完成（评审需要干净文本）。阶段 3 和 4 理论上独立，但阶段 4 应最后运行。实际操作建议**按顺序执行**。
