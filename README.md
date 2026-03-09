# Paper Skills Repository

[English](#english) | [中文](#中文)

---

## English

Beginner-friendly paper workflow skills for AI coding assistants.

This repo provides four skills:

| Skill | Best Use Time | What It Does |
|---|---|---|
| `paper-english-polishing` | Draft translation and language refinement | Translates Chinese-to-English and polishes academic writing paragraph by paragraph with structured diffs and LaTeX-safe output. |
| `paper-multi-round-review` | During research iteration | Simulates multi-round peer review (4 reviewers + meta-review + rebuttal/revision guidance). |
| `paper-figure-styling` | Figure/table production and visual cleanup | Standardizes figure color, typography, legends, float layout, and publisher-specific visual compliance for LaTeX papers. |
| `paper-submission-check` | Final stage before submission | Runs pre-submission quality checks (AI-style cleanup, language, references, LaTeX quality, structure). |

## 5-Minute Quick Start (Recommended)

1. Clone this repo locally.
2. Copy one or more skill folders into your assistant's skill directory.
3. Restart your IDE/agent session.
4. Run one explicit invocation prompt (copy-paste from examples below).
5. If output follows the skill workflow, it is enabled.

### macOS / Linux

```bash
cd ~
git clone https://github.com/<your-username>/paper-submission-check.git paper-skills

# Install one or more
cp -r ~/paper-skills/skills/paper-english-polishing ~/.cursor/skills/
cp -r ~/paper-skills/skills/paper-submission-check ~/.cursor/skills/
cp -r ~/paper-skills/skills/paper-multi-round-review ~/.cursor/skills/
cp -r ~/paper-skills/skills/paper-figure-styling ~/.cursor/skills/
```

### Windows PowerShell

```powershell
cd $env:USERPROFILE
git clone https://github.com/<your-username>/paper-submission-check.git paper-skills

# Install one or more
Copy-Item -Recurse -Force "$env:USERPROFILE\paper-skills\skills\paper-english-polishing" "$env:USERPROFILE\.cursor\skills\"
Copy-Item -Recurse -Force "$env:USERPROFILE\paper-skills\skills\paper-submission-check" "$env:USERPROFILE\.cursor\skills\"
Copy-Item -Recurse -Force "$env:USERPROFILE\paper-skills\skills\paper-multi-round-review" "$env:USERPROFILE\.cursor\skills\"
Copy-Item -Recurse -Force "$env:USERPROFILE\paper-skills\skills\paper-figure-styling" "$env:USERPROFILE\.cursor\skills\"
```

## Platform Paths

Each platform loads skills from a folder where each skill is one directory containing `SKILL.md`.

- Cursor: `~/.cursor/skills/`
- OpenAI Codex: `~/.agents/skills/`
- Claude Code: `~/.claude/skills/`

For Codex or Claude, replace the destination path accordingly.

## How To Enable

After copying skill folders:

1. Restart IDE/assistant session.
2. Open a project containing your paper files (typically `paper.tex`, optional `refs.bib`).
3. Use explicit invocation once to avoid trigger ambiguity.

## How To Invoke (Copy-Paste Examples)

### Explicit invocation (best for beginners)

```text
Use $paper-multi-round-review to run Round-1 review on @paper.tex and provide a meta-review.
```

```text
Use $paper-submission-check to check @paper.tex and @refs.bib before submission.
```

```text
Use $paper-english-polishing to polish @paper.tex section by section and return sentence-level diffs.
```

```text
Use $paper-figure-styling to standardize figure/table colors, fonts, legends, and float layout in @paper.tex.
```

### Natural-language invocation

```text
Please simulate 4 reviewers for @paper.tex and provide an AC-style meta-review.
```

```text
Please run a final pre-submission quality check on @paper.tex and @refs.bib.
```

```text
Please translate and polish the English of @paper.tex from Chinese draft, with LaTeX-safe output.
```

```text
Please optimize all figures and tables in @paper.tex for readability and venue style compliance.
```

### End-to-end workflow example

```text
Step 1: Use $paper-english-polishing on @paper.tex.
Step 2: Use $paper-multi-round-review on the polished draft.
Step 3: List critical and major fixes only, then revise.
Step 4: Use $paper-figure-styling to finalize figures/tables and layout.
Step 5: Use $paper-submission-check on @paper.tex and @refs.bib.
Step 6: Return a final must-fix checklist before submission.
```

## Which Skill Should I Use?

Use this rule:

- Need Chinese-to-English translation / language polishing -> `paper-english-polishing`
- Need technical depth / novelty / experiment critique -> `paper-multi-round-review`
- Need figure/table visual quality and layout control -> `paper-figure-styling`
- Need language/style/format/reference cleanup -> `paper-submission-check`
- Want strongest pipeline -> use all four, in this order:
  `paper-english-polishing` -> `paper-multi-round-review` -> revise -> `paper-figure-styling` -> `paper-submission-check`

## What Success Looks Like

- `paper-english-polishing`: You should see paragraph-level rewrites with sentence-level diffs and clear reasons.
- `paper-multi-round-review`: You should see reviewer-by-reviewer strengths, weaknesses, scores, and a meta-review with priority fixes.
- `paper-figure-styling`: You should see concrete style and layout fixes for figures/tables, with LaTeX-ready changes.
- `paper-submission-check`: You should see phase-based issues (language, AI style, citations, BibTeX, formatting) with fix recommendations.

## Common Problems and Fixes

### Skill not triggering

- Use explicit `$skill-name` invocation.
- Confirm folder exists and contains `SKILL.md`.
- Restart the assistant session.

### Assistant ignores paper files

- Reference files explicitly (for example `@paper.tex`, `@refs.bib`).
- Ensure files are in current workspace.

### Wrong skill triggered

- Always call by name: `$paper-english-polishing`, `$paper-multi-round-review`, `$paper-figure-styling`, or `$paper-submission-check`.

### Output is too generic

- Ask for strict format in your prompt:
  `Use section-level citations, severity labels (Critical/Major/Minor), and concrete fix actions.`

## Repository Structure

```text
paper-submission-check/
├── skills/
│   ├── paper-english-polishing/
│   │   ├── SKILL.md
│   │   ├── chinglish-patterns.md
│   │   ├── section-conventions.md
│   │   ├── polishing-examples.md
│   │   └── agents/
│   │       └── openai.yaml
│   ├── paper-submission-check/
│   │   ├── SKILL.md
│   │   ├── ai-phrases.md
│   │   ├── ai-style-removal.md
│   │   ├── checklist.md
│   │   ├── paper-structure-guide.md
│   │   ├── reference-format-guide.md
│   │   └── LICENSE
│   ├── paper-figure-styling/
│   │   ├── SKILL.md
│   │   ├── color-reference.md
│   │   ├── figure-types.md
│   │   ├── layout-placement.md
│   │   └── publisher-specs.md
│   └── paper-multi-round-review/
│       ├── SKILL.md
│       ├── reviewer-profiles.md
│       ├── review-dimensions.md
│       ├── ml-security-pitfalls.md
│       ├── multi-model-strategy.md
│       └── review-examples.md
├── README.md
└── LICENSE
```

---

## 中文

面向新手的论文工作流 Skill 仓库，适用于主流 AI 编程助手。

本仓库提供四个 Skill：

| Skill | 适用阶段 | 作用 |
|---|---|---|
| `paper-english-polishing` | 初稿翻译与语言打磨 | 对中英混合或中文初稿做学术英语翻译与润色，按段输出改写与差异说明。 |
| `paper-multi-round-review` | 研究迭代阶段 | 多轮同行评审模拟（4 位评审 + Meta-Review + rebuttal/revision 建议）。 |
| `paper-figure-styling` | 图表与排版优化阶段 | 统一图表配色、字体、图例、浮动体布局，并按投稿方规范做视觉合规检查。 |
| `paper-submission-check` | 投稿前最后阶段 | 投稿前终检（AI 痕迹、语言、参考文献、LaTeX 质量、结构完整性）。 |

## 5 分钟快速上手（推荐）

1. 克隆仓库。
2. 将一个或多个 Skill 目录复制到你的平台技能目录。
3. 重启 IDE 或 agent 会话。
4. 执行一次显式调用（下面有可复制示例）。
5. 如果输出进入对应流程，说明启用成功。

### macOS / Linux

```bash
cd ~
git clone https://github.com/<your-username>/paper-submission-check.git paper-skills

# 安装一个或多个
cp -r ~/paper-skills/skills/paper-english-polishing ~/.cursor/skills/
cp -r ~/paper-skills/skills/paper-submission-check ~/.cursor/skills/
cp -r ~/paper-skills/skills/paper-multi-round-review ~/.cursor/skills/
cp -r ~/paper-skills/skills/paper-figure-styling ~/.cursor/skills/
```

### Windows PowerShell

```powershell
cd $env:USERPROFILE
git clone https://github.com/<your-username>/paper-submission-check.git paper-skills

# 安装一个或多个
Copy-Item -Recurse -Force "$env:USERPROFILE\paper-skills\skills\paper-english-polishing" "$env:USERPROFILE\.cursor\skills\"
Copy-Item -Recurse -Force "$env:USERPROFILE\paper-skills\skills\paper-submission-check" "$env:USERPROFILE\.cursor\skills\"
Copy-Item -Recurse -Force "$env:USERPROFILE\paper-skills\skills\paper-multi-round-review" "$env:USERPROFILE\.cursor\skills\"
Copy-Item -Recurse -Force "$env:USERPROFILE\paper-skills\skills\paper-figure-styling" "$env:USERPROFILE\.cursor\skills\"
```

## 平台目录

每个 Skill 都是一个包含 `SKILL.md` 的文件夹。

- Cursor: `~/.cursor/skills/`
- OpenAI Codex: `~/.agents/skills/`
- Claude Code: `~/.claude/skills/`

如果你用 Codex 或 Claude，只需把目标路径换成对应目录。

## 如何启用

复制完成后：

1. 重启 IDE / assistant 会话。
2. 打开包含论文文件的工作区（通常是 `paper.tex`，可选 `refs.bib`）。
3. 第一次建议用显式 `$skill-name` 调用，避免触发歧义。

## 如何调用（可直接复制）

### 显式调用（新手推荐）

```text
请使用 $paper-multi-round-review 对 @paper.tex 做第一轮评审，并输出 Meta-Review。
```

```text
请使用 $paper-submission-check 检查 @paper.tex 和 @refs.bib，按投稿前清单输出问题。
```

```text
请使用 $paper-english-polishing 对 @paper.tex 逐段润色，并输出句级 diff。
```

```text
请使用 $paper-figure-styling 统一 @paper.tex 中所有图表的配色、字体、图例和排版。
```

### 自然语言调用

```text
请模拟 4 位审稿人评审 @paper.tex，并给出 AC 风格综合意见。
```

```text
请对 @paper.tex 和 @refs.bib 执行投稿前最终质检。
```

```text
请把 @paper.tex 的中文初稿翻译并润色成学术英语，输出可直接粘贴到 LaTeX 的版本。
```

```text
请对 @paper.tex 的所有图表执行投稿前视觉优化，并输出可直接应用的 LaTeX 修改建议。
```

### 端到端组合示例

```text
第 1 步：用 $paper-english-polishing 润色 @paper.tex。
第 2 步：用 $paper-multi-round-review 评审润色后的稿件。
第 3 步：只列出 Critical 和 Major 问题并修稿。
第 4 步：用 $paper-figure-styling 优化图表与排版。
第 5 步：用 $paper-submission-check 复检 @paper.tex 和 @refs.bib。
第 6 步：输出最终投稿前 must-fix 清单。
```

## 如何选择 Skill

简单规则：

- 需要中译英或英文润色 -> `paper-english-polishing`
- 关注技术深度、创新性、实验可信度 -> `paper-multi-round-review`
- 关注图表可读性、配色一致性、排版与投稿规范 -> `paper-figure-styling`
- 关注语言、格式、引用、AI 痕迹清理 -> `paper-submission-check`
- 最稳妥流程 -> 先 `paper-english-polishing`，再 `paper-multi-round-review`，修稿后 `paper-figure-styling`，最后 `paper-submission-check`

## 启用成功的判断标准

- `paper-english-polishing`：输出应包含分段改写、句级差异和修改理由。
- `paper-multi-round-review`：输出应包含按 reviewer 分组的优缺点、评分和 Meta-Review 优先级。
- `paper-figure-styling`：输出应包含图表视觉与排版的具体修复建议和可落地修改。
- `paper-submission-check`：输出应包含分阶段问题清单（语言、AI 风格、引用、BibTeX、格式）及修复建议。

## 常见问题与排查

### 触发不到 Skill

- 先用显式 `$skill-name` 调用。
- 检查目标目录是否存在且含 `SKILL.md`。
- 重启 assistant 会话。

### 没有读取论文文件

- 在 prompt 显式引用文件（例如 `@paper.tex`、`@refs.bib`）。
- 确保文件在当前工作区。

### 触发了错误 Skill

- 直接指定：`$paper-english-polishing`、`$paper-multi-round-review`、`$paper-figure-styling` 或 `$paper-submission-check`。

### 输出太空泛

- 在 prompt 增加约束：
  `请按章节引用位置，使用 Critical/Major/Minor 分级，并给出具体修改动作。`

## 仓库结构

```text
paper-submission-check/
├── skills/
│   ├── paper-english-polishing/
│   ├── paper-figure-styling/
│   ├── paper-multi-round-review/
│   └── paper-submission-check/
├── README.md
└── LICENSE
```
