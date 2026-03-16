# Paper Skills Repository

[English](#english) | [中文](#中文)

---

## English

Beginner-friendly paper workflow skills for AI coding assistants (Cursor, OpenAI Codex, Claude Code). Drop a rough draft in, get a submission-ready paper out.

### What This Repo Does

This repo provides **four skills** that work together as a pipeline. Each skill is a structured instruction set that tells your AI assistant exactly how to process your paper.

| # | Skill | When to Use | What It Does |
|---|---|---|---|
| 1 | `paper-english-polishing` | Draft translation and language refinement | Translates Chinese-to-English and polishes academic writing paragraph by paragraph with structured diffs, terminology consistency, and LaTeX-safe output. |
| 2 | `paper-multi-round-review` | During research iteration | Simulates multi-round peer review with 4 specialized reviewers + meta-review + rebuttal/revision guidance. |
| 3 | `paper-figure-styling` | Figure/table production and visual cleanup | Standardizes figure color, typography, axis/tick/legend design, float layout, table styling, and publisher-specific visual compliance. |
| 4 | `paper-submission-check` | Final stage before submission | Runs pre-submission quality checks: AI-style cleanup, equations, acronyms, references (Elsevier/IEEE template-aware), tense, punctuation, structure. |

### Recommended Pipeline Order

```
Draft → 1. Polishing → 2. Review → Revise → 3. Figure Styling → 4. Submission Check → Submit
```

For the complete pipeline guide with stage-gate criteria, decision points, and loop-back rules, see [PIPELINE.md](PIPELINE.md).

---

## Installation (5 Minutes)

### Step 1: Clone

```bash
git clone https://github.com/<your-username>/paper-submission-check.git ~/paper-skills
```

### Step 2: Copy Skills to Your Platform

| Platform | Skill Directory | Command |
|----------|----------------|---------|
| **Cursor** | `~/.cursor/skills/` | See below |
| **OpenAI Codex** | `~/.agents/skills/` | Replace path in commands below |
| **Claude Code** | `~/.claude/skills/` | Replace path in commands below |

**macOS / Linux (Cursor):**

```bash
cp -r ~/paper-skills/skills/paper-english-polishing   ~/.cursor/skills/
cp -r ~/paper-skills/skills/paper-multi-round-review   ~/.cursor/skills/
cp -r ~/paper-skills/skills/paper-figure-styling       ~/.cursor/skills/
cp -r ~/paper-skills/skills/paper-submission-check     ~/.cursor/skills/
```

**Windows PowerShell (Cursor):**

```powershell
$src = "$env:USERPROFILE\paper-skills\skills"
$dst = "$env:USERPROFILE\.cursor\skills"
Copy-Item -Recurse -Force "$src\paper-english-polishing"   "$dst\"
Copy-Item -Recurse -Force "$src\paper-multi-round-review"  "$dst\"
Copy-Item -Recurse -Force "$src\paper-figure-styling"      "$dst\"
Copy-Item -Recurse -Force "$src\paper-submission-check"    "$dst\"
```

### Step 3: Restart & Verify

1. Restart your IDE or agent session.
2. Open a project containing your paper files (`paper.tex`, optional `refs.bib`).
3. Try one of the invocation prompts below. If the output follows a structured workflow with phases and checklists, the skill is active.

---

## Three Ways to Use (Detailed)

There are **three ways** to drive the model. Pick the one that fits your situation.

### Method 1: Step-by-Step Execution (Recommended for Beginners)

Run **one skill at a time**, review the output, then decide the next step. This gives you maximum control.

**Stage 1 — English Polishing:**

```text
Use $paper-english-polishing to polish @paper.tex section by section.
Requirements:
1. Execute Phases 0-11 in order.
2. Output a sentence-level diff table (Original / Revised / Reason) for each paragraph.
3. Maintain a terminology ledger and output the final version when done.
4. Check pronoun counts (we ≤ 15, our ≤ 8).
5. Preserve Chinese LaTeX comments (% lines) as-is.
```

**Stage 2 — Multi-Round Review:**

```text
Use $paper-multi-round-review to run Round-1 review on @paper.tex.
Requirements:
1. Assign 4 reviewers (Domain Expert, Methods Expert, Methodology Expert, Presentation Expert).
2. Each reviewer: output Summary, Strengths, Weaknesses (Critical/Major/Minor with section refs), Questions, Scores.
3. Output a Meta-Review with consensus, divergence, and Critical Path to Acceptance.
4. Pay special attention to: equation variable consistency, acronym first-use, tense consistency, algorithm pseudocode.
```

**Stage 2 Follow-up — Revision** (after reading the review):

```text
Based on the Meta-Review above, address all Critical and Major issues:
1. Revise @paper.tex for each issue.
2. Output a revision log (Issue # | Section | Change | Line Range).
3. Cross-check: do abstract numbers still match result tables after revision?
```

**Stage 3 — Figure Styling:**

```text
Use $paper-figure-styling to audit and fix all figures and tables in @paper.tex.
Requirements:
1. Execute Phases 1-15 in order.
2. Unify color palette (Okabe-Ito); same entity = same color in all figures.
3. Check axis labels, tick direction, grid style consistency across all figures.
4. Check figure layout (minipage arrangement, panel label consistency).
5. Tables: booktabs style, column headers ≤ 3 words, caption above table.
6. Output the complete figure/table quality checklist.
```

**Stage 4 — Submission Check:**

```text
Use $paper-submission-check to check @paper.tex and @refs.bib.
Requirements:
1. Execute Phases 0-10 in order.
2. Key checks:
   - Equation variable consistency across all formulas; algorithm pseudocode style.
   - All acronyms defined at first use (abstract and body independently).
   - Tense consistency per section.
   - we/our/us pronoun counts.
   - Zero Chinese punctuation in body text; Chinese LaTeX comments allowed.
   - Reference template matches publisher (Elsevier = full journal names; IEEE = abbreviated).
   - Table bold/underline correctness; table header length.
   - BIB entry types, title protection, DOI format.
3. Output a structured report: Critical / Warning / Suggestion.
4. Output the final pre-submission checklist.
```

---

### Method 2: One-Prompt Full Pipeline (For Experienced Users)

Run **all four stages in one prompt**. The model executes them sequentially.

```text
Run the full paper pipeline on @paper.tex and @refs.bib:

Step 1: Use $paper-english-polishing to polish @paper.tex section by section.
        Output the polishing report and terminology ledger when done.
        Preserve Chinese LaTeX comments.

Step 2: Use $paper-multi-round-review to review the polished draft.
        Output all 4 reviewer reports and the meta-review.

Step 3: List only Critical and Major issues from the meta-review.
        Revise @paper.tex to address each issue. Output a revision log.

Step 4: Use $paper-figure-styling to audit and fix all figures and tables.
        Unify colors, axes, tick marks, legends, table styles.
        Output the figure styling checklist.

Step 5: Use $paper-submission-check to run the final quality check.
        Verify: equations, acronyms, tense, pronouns, punctuation,
        reference template (Elsevier/IEEE), table markings, BIB format.
        Output the full issue report.

Step 6: Fix any remaining Critical or Warning issues.
        Output the final must-fix checklist before submission.
```

---

### Method 3: Single Skill (Targeted Use)

Run **just one skill** when you only need a specific type of check.

**Polishing only:**

```text
Use $paper-english-polishing to polish @paper.tex section by section and return sentence-level diffs.
```

**Review only:**

```text
Use $paper-multi-round-review to run Round-1 review on @paper.tex and provide a meta-review.
```

**Figure styling only:**

```text
Use $paper-figure-styling to standardize figure/table colors, fonts, legends, and float layout in @paper.tex.
```

**Submission check only:**

```text
Use $paper-submission-check to check @paper.tex and @refs.bib before submission.
```

---

## Pro Tips

### Use `$skill-name` for Reliable Triggering

The `$` prefix is a skill trigger keyword. Always include it for the most reliable activation.

```text
Use $paper-submission-check ...    ← Reliable (explicit trigger)
Please check my paper ...          ← May work (natural language match)
```

### Use `@` to Reference Your Files

`@paper.tex` and `@refs.bib` tell the model to read your actual files. Make sure these files exist in the current workspace.

### Constrain the Output When It Is Too Generic

```text
Follow the SKILL.md phases in order. Check off each phase when done.
Use Critical/Major/Minor severity labels with section and line references.
Give concrete fix actions, not vague suggestions.
```

### Process Long Papers in Batches

```text
Use $paper-english-polishing to polish @paper.tex, starting from \section{Introduction}
to \section{Related Work}. Process 3-5 paragraphs per batch. Output the diff table and
terminology updates for this batch.
```

### Loop Back When Issues Are Found

```text
The Stage 2 review found language issues in §3 and §5.
Go back to $paper-english-polishing and re-polish those two sections.
Then re-run $paper-submission-check on the updated @paper.tex.
```

---

## Which Skill Should I Use?

| Your Need | Skill |
|-----------|-------|
| Chinese-to-English translation or language polishing | `paper-english-polishing` |
| Technical depth, novelty, or experiment critique | `paper-multi-round-review` |
| Figure/table visual quality and layout | `paper-figure-styling` |
| Language, format, references, AI-style cleanup | `paper-submission-check` |
| **Strongest pipeline (recommended)** | **All four in order:** polishing → review → revise → figure styling → submission check |

## What Success Looks Like

| Skill | Expected Output |
|-------|----------------|
| `paper-english-polishing` | Paragraph-level rewrites with sentence-level diff tables (Original / Revised / Reason) and a terminology ledger. |
| `paper-multi-round-review` | 4 reviewer reports with severity-labeled weaknesses, scores, a meta-review, and a prioritized fix list. |
| `paper-figure-styling` | Concrete color/axis/legend/table fixes with LaTeX-ready code changes and a per-figure checklist. |
| `paper-submission-check` | A phase-by-phase issue report (Critical/Warning/Suggestion) with line references and a final submission checklist. |

## Common Problems and Fixes

| Problem | Fix |
|---------|-----|
| Skill not triggering | Use explicit `$skill-name` invocation. Confirm the folder exists with `SKILL.md` inside. Restart the session. |
| Assistant ignores paper files | Reference files explicitly with `@paper.tex`, `@refs.bib`. Ensure files are in the current workspace. |
| Wrong skill triggered | Always call by name: `$paper-english-polishing`, `$paper-multi-round-review`, `$paper-figure-styling`, or `$paper-submission-check`. |
| Output is too generic | Add constraints: `Follow SKILL.md phases. Use Critical/Major/Minor labels. Give concrete fix actions with line numbers.` |
| Paper is too long for one pass | Process in batches: `Polish \section{Method} only, 3-5 paragraphs per batch.` |
| Reference format wrong after switching publisher | See the publisher migration checklists in `skills/paper-submission-check/reference-format-guide.md` §0. |

## Repository Structure

```text
paper-submission-check/
├── skills/
│   ├── paper-english-polishing/     # Stage 1: Language polishing
│   │   ├── SKILL.md
│   │   ├── chinglish-patterns.md
│   │   ├── section-conventions.md
│   │   ├── polishing-examples.md
│   │   └── agents/openai.yaml
│   ├── paper-multi-round-review/    # Stage 2: Peer review simulation
│   │   ├── SKILL.md
│   │   ├── reviewer-profiles.md
│   │   ├── review-dimensions.md
│   │   ├── ml-security-pitfalls.md
│   │   ├── multi-model-strategy.md
│   │   ├── review-examples.md
│   │   └── agents/openai.yaml
│   ├── paper-figure-styling/        # Stage 3: Figure & table styling
│   │   ├── SKILL.md
│   │   ├── color-reference.md
│   │   ├── figure-types.md
│   │   ├── layout-placement.md
│   │   ├── publisher-specs.md
│   │   └── agents/openai.yaml
│   └── paper-submission-check/      # Stage 4: Pre-submission quality gate
│       ├── SKILL.md
│       ├── ai-phrases.md
│       ├── ai-style-removal.md
│       ├── checklist.md
│       ├── paper-structure-guide.md
│       ├── reference-format-guide.md
│       ├── LICENSE
│       └── agents/openai.yaml
├── PIPELINE.md                      # Complete orchestration guide
├── README.md
└── LICENSE
```

---

## 中文

面向新手的论文工作流 Skill 仓库，适用于 Cursor、OpenAI Codex、Claude Code 等主流 AI 编程助手。放入初稿，输出投稿级论文。

### 本仓库做什么

提供 **四个 Skill**，组成完整的论文处理流水线。每个 Skill 是一套结构化指令，告诉 AI 助手如何处理你的论文。

| 序号 | Skill | 适用阶段 | 作用 |
|------|---|---|---|
| 1 | `paper-english-polishing` | 初稿翻译与语言打磨 | 中译英翻译 + 学术英语润色，按段输出改写差异表、术语表，支持双语对照模式。 |
| 2 | `paper-multi-round-review` | 研究迭代阶段 | 4 位审稿人独立评审 + AC 综合意见 + 反驳策略 + 修改指导。 |
| 3 | `paper-figure-styling` | 图表与排版优化 | 统一配色、字体、坐标轴/刻度线/图例设计、浮动体布局、表格样式、出版社视觉合规。 |
| 4 | `paper-submission-check` | 投稿前最后阶段 | 终检：AI 痕迹清理、公式/符号验证、缩写全称协议、参考文献模板（Elsevier/IEEE 自适应）、时态、标点、结构。 |

### 推荐流水线顺序

```
初稿 → 1. 润色 → 2. 评审 → 修改 → 3. 图表美化 → 4. 投稿终检 → 提交
```

完整流水线指南（阶段门控、决策逻辑、回退规则），请参见 [PIPELINE.md](PIPELINE.md)。

---

## 安装（5 分钟）

### 第 1 步：克隆

```bash
git clone https://github.com/<your-username>/paper-submission-check.git ~/paper-skills
```

### 第 2 步：复制 Skill 到平台目录

| 平台 | Skill 目录 |
|------|-----------|
| **Cursor** | `~/.cursor/skills/` |
| **OpenAI Codex** | `~/.agents/skills/` |
| **Claude Code** | `~/.claude/skills/` |

**macOS / Linux（以 Cursor 为例）：**

```bash
cp -r ~/paper-skills/skills/paper-english-polishing   ~/.cursor/skills/
cp -r ~/paper-skills/skills/paper-multi-round-review   ~/.cursor/skills/
cp -r ~/paper-skills/skills/paper-figure-styling       ~/.cursor/skills/
cp -r ~/paper-skills/skills/paper-submission-check     ~/.cursor/skills/
```

**Windows PowerShell（以 Cursor 为例）：**

```powershell
$src = "$env:USERPROFILE\paper-skills\skills"
$dst = "$env:USERPROFILE\.cursor\skills"
Copy-Item -Recurse -Force "$src\paper-english-polishing"   "$dst\"
Copy-Item -Recurse -Force "$src\paper-multi-round-review"  "$dst\"
Copy-Item -Recurse -Force "$src\paper-figure-styling"      "$dst\"
Copy-Item -Recurse -Force "$src\paper-submission-check"    "$dst\"
```

### 第 3 步：重启并验证

1. 重启 IDE 或 agent 会话。
2. 打开包含论文文件的工作区（`paper.tex`，可选 `refs.bib`）。
3. 尝试下面的调用示例。如果输出有分阶段的结构化流程和检查清单，说明 Skill 已启用。

---

## 三种使用方式（详细说明）

有 **三种方式** 驱动模型执行，选择适合你的方式。

### 方式 1：逐阶段执行（新手推荐，控制力最强）

每个阶段单独发一条 prompt，看到输出后再决定下一步。

**阶段 1 — 英文润色：**

```text
请使用 $paper-english-polishing 对 @paper.tex 逐段润色。
要求：
1. 按 Phase 0-11 顺序完整执行。
2. 每段输出句级 diff 表（原文 / 修改 / 理由）。
3. 维护术语表，完成后输出最终版。
4. 检查代词计数（we ≤ 15, our ≤ 8）。
5. 中文 LaTeX 注释（% 开头的行）保留不动。
```

**阶段 2 — 多轮评审：**

```text
请使用 $paper-multi-round-review 对 @paper.tex 执行第一轮评审。
要求：
1. 分配 4 位审稿人（领域专家、方法专家、实验方法学专家、写作专家）。
2. 每位审稿人输出：摘要、优点、缺点（标注 Critical/Major/Minor 及章节位置）、提问、评分。
3. 输出 Meta-Review：共识优缺点、分歧点、Critical Path to Acceptance。
4. 重点关注：公式变量一致性、缩写首次全称、时态一致性、算法伪代码规范。
```

**阶段 2 追加 — 修改**（看完评审后发送）：

```text
请根据上面的 Meta-Review，处理所有 Critical 和 Major 问题：
1. 逐项修改 @paper.tex。
2. 输出修订日志（问题编号 | 章节 | 修改内容 | 行号范围）。
3. 交叉核查：摘要中的数字是否仍与结果表格一致？
```

**阶段 3 — 图表美化：**

```text
请使用 $paper-figure-styling 审查并优化 @paper.tex 中的所有图表。
要求：
1. 按 Phase 1-15 执行。
2. 统一配色方案（Okabe-Ito），同一实体在所有图中颜色一致。
3. 检查坐标轴标签、刻度线方向、网格样式是否跨图一致。
4. 检查图片组合方式（minipage 布局、panel 标签一致性）。
5. 表格：booktabs 样式，列头 ≤ 3 个词，标题位于表格上方。
6. 输出完整的图表质量检查清单。
```

**阶段 4 — 投稿终检：**

```text
请使用 $paper-submission-check 检查 @paper.tex 和 @refs.bib。
要求：
1. 按 Phase 0-10 完整执行。
2. 重点检查：
   - 公式变量跨公式一致性，算法伪代码关键字/变量规范
   - 所有缩写首次出现全称（摘要和正文各独立定义一次）
   - 时态一致性（方法用现在/过去时，结果用过去时）
   - we/our/us 代词计数
   - 中文标点零容忍，但中文 LaTeX 注释允许保留
   - 参考文献模板匹配目标出版社（Elsevier = 期刊全称；IEEE = ISO 4 缩写）
   - 表格 bold/underline 正确性，表头长度
   - BIB 条目类型、标题大写保护、DOI 格式
3. 输出结构化报告：Critical / Warning / Suggestion 分类。
4. 输出投稿前终检清单。
```

---

### 方式 2：一次性全流水线（适合熟手，一条 prompt 搞定）

一条 prompt 驱动全部 4 个阶段，模型按顺序执行。

```text
请按以下流水线处理 @paper.tex 和 @refs.bib：

第 1 步：用 $paper-english-polishing 逐段润色 @paper.tex。
        完成后输出润色报告和术语表。中文 LaTeX 注释保留。

第 2 步：用 $paper-multi-round-review 评审润色后的稿件。
        输出全部 4 位评审报告和 Meta-Review。

第 3 步：仅列出 Meta-Review 中的 Critical 和 Major 问题。
        逐项修改 @paper.tex，输出修订日志。

第 4 步：用 $paper-figure-styling 审查并修复所有图表。
        统一配色、坐标轴、刻度线、图例、表格样式。
        输出图表样式检查清单。

第 5 步：用 $paper-submission-check 做投稿前终检。
        重点核查：公式变量一致性、缩写首次全称、时态、代词、
        标点符号、参考文献模板（Elsevier/IEEE）、表格标注、BIB 格式。
        输出完整问题报告。

第 6 步：修复所有剩余的 Critical 和 Warning 问题。
        输出投稿前终检清单。
```

---

### 方式 3：单个 Skill 调用（只需某一项检查时使用）

**只需润色：**

```text
请使用 $paper-english-polishing 对 @paper.tex 逐段润色，输出句级 diff。
```

**只需评审：**

```text
请使用 $paper-multi-round-review 对 @paper.tex 做第一轮评审，输出 Meta-Review。
```

**只需图表优化：**

```text
请使用 $paper-figure-styling 统一 @paper.tex 中所有图表的配色、字体、图例和排版。
```

**只需投稿终检：**

```text
请使用 $paper-submission-check 检查 @paper.tex 和 @refs.bib，按投稿前清单输出问题。
```

---

## 使用技巧

### `$skill-name` 显式触发最可靠

`$` 前缀是 Skill 的触发关键字。始终带上它以获得最可靠的激活。

```text
Use $paper-submission-check ...    ← 可靠（显式触发）
请检查我的论文 ...                   ← 可能触发（自然语言匹配）
```

### 用 `@` 引用你的文件

`@paper.tex` 和 `@refs.bib` 让模型读取你的实际文件。确保文件在当前工作区中。

### 输出太笼统时加约束

```text
请严格按 SKILL.md 的 Phase 顺序执行，每个 Phase 完成后打勾。
使用 Critical/Major/Minor 分级，标注章节和行号。
给出具体修改动作，不要空泛建议。
```

### 长论文分批处理

```text
请使用 $paper-english-polishing 润色 @paper.tex 的 Introduction 部分
（\section{Introduction} 到 \section{Related Work} 之间），
每批 3-5 段，输出 diff 表和术语更新。
```

### 发现问题后回退处理

```text
阶段 2 评审发现 §3 和 §5 有语言问题，
请回到 $paper-english-polishing 重新润色这两个章节，
然后重新运行 $paper-submission-check 检查修改后的 @paper.tex。
```

### 切换出版社时检查参考文献

从 Elsevier 转投 IEEE（或反之）时，参考文献格式必须全部调整。详见 `skills/paper-submission-check/reference-format-guide.md` §0 的出版社迁移清单。

---

## 如何选择 Skill

| 你的需求 | 用哪个 Skill |
|----------|-------------|
| 中译英翻译或英文润色 | `paper-english-polishing` |
| 技术深度、创新性、实验可信度评审 | `paper-multi-round-review` |
| 图表可读性、配色一致性、坐标轴/排版规范 | `paper-figure-styling` |
| 语言、格式、引用、AI 痕迹、公式/缩写检查 | `paper-submission-check` |
| **最稳妥流程（推荐）** | **四个按顺序用：** 润色 → 评审 → 修改 → 图表 → 终检 |

## 启用成功的判断标准

| Skill | 预期输出 |
|-------|---------|
| `paper-english-polishing` | 分段改写 + 句级差异表（原文/修改/理由）+ 术语表。 |
| `paper-multi-round-review` | 4 位审稿人报告（含严重级别标注的缺点）+ 评分 + Meta-Review + 优先修复清单。 |
| `paper-figure-styling` | 图表配色/坐标轴/图例/表格的具体修复建议 + 可直接应用的 LaTeX 代码 + 检查清单。 |
| `paper-submission-check` | 分阶段问题报告（Critical/Warning/Suggestion）+ 行号引用 + 投稿前终检清单。 |

## 常见问题与排查

| 问题 | 解决方法 |
|------|---------|
| 触发不到 Skill | 用显式 `$skill-name` 调用。检查目标目录是否存在且含 `SKILL.md`。重启会话。 |
| 没有读取论文文件 | 用 `@paper.tex`、`@refs.bib` 显式引用。确保文件在当前工作区。 |
| 触发了错误 Skill | 直接指定：`$paper-english-polishing`、`$paper-multi-round-review`、`$paper-figure-styling` 或 `$paper-submission-check`。 |
| 输出太空泛 | 增加约束：`请按 SKILL.md 的 Phase 顺序执行。用 Critical/Major/Minor 分级。给出行号和具体修改。` |
| 论文太长一次处理不完 | 分批：`请润色 \section{Method} 部分，每批 3-5 段。` |
| 换出版社后参考文献格式不对 | 见 `skills/paper-submission-check/reference-format-guide.md` §0 出版社迁移清单。 |

## 仓库结构

```text
paper-submission-check/
├── skills/
│   ├── paper-english-polishing/     # 阶段 1：语言润色
│   ├── paper-multi-round-review/    # 阶段 2：同行评审模拟
│   ├── paper-figure-styling/        # 阶段 3：图表美化
│   └── paper-submission-check/      # 阶段 4：投稿终检
├── PIPELINE.md                      # 完整流水线编排指南
├── README.md
└── LICENSE
```
