# Paper Skills Repository

[English](#english) | [中文](#中文)

---

## English

This repository uses a flat multi-skill layout under `skills/`.

## Skills

| Skill | Type | Purpose |
|---|---|---|
| `paper-submission-check` | submission | Pre-submission quality checks for LaTeX papers (language, AI-style cleanup, references, formatting, checklist). |
| `paper-multi-round-review` | review | Multi-round peer-review simulation (reviewers, meta-review, rebuttal and revision loop). |

## Repository Structure

```text
paper-submission-check/
├── skills/
│   ├── paper-submission-check/
│   │   ├── SKILL.md
│   │   ├── ai-phrases.md
│   │   ├── ai-style-removal.md
│   │   ├── checklist.md
│   │   ├── paper-structure-guide.md
│   │   ├── reference-format-guide.md
│   │   └── LICENSE
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

## Installation

Most AI platforms load one skill per folder (`<skill-name>/SKILL.md`).
Copy the skill folder(s) you want from `skills/`.

### Cursor

```bash
cd ~
git clone git@github.com:zhousodo/paper-submission-check.git paper-skills
cp -r ~/paper-skills/skills/paper-submission-check ~/.cursor/skills/
cp -r ~/paper-skills/skills/paper-multi-round-review ~/.cursor/skills/
```

### OpenAI Codex

```bash
cd ~
git clone git@github.com:zhousodo/paper-submission-check.git paper-skills
cp -r ~/paper-skills/skills/paper-submission-check ~/.agents/skills/
cp -r ~/paper-skills/skills/paper-multi-round-review ~/.agents/skills/
```

### Claude Code

```bash
cd ~
git clone git@github.com:zhousodo/paper-submission-check.git paper-skills
cp -r ~/paper-skills/skills/paper-submission-check ~/.claude/skills/
cp -r ~/paper-skills/skills/paper-multi-round-review ~/.claude/skills/
```

## Enable (Quick Check)

After copying skill folders:

1. Restart or reload your IDE/agent session.
2. Open a workspace that contains `paper.tex` and optional `refs.bib`.
3. Run one explicit skill invocation (examples below). If the agent follows skill-specific workflow, the skill is enabled.

## Invocation Examples

### Explicit skill invocation (recommended)

```text
Use $paper-submission-check to check @paper.tex and @refs.bib before submission.
```

```text
Use $paper-multi-round-review to run a full Round-1 review on @paper.tex.
```

### Natural-language trigger examples

```text
Please run a pre-submission quality check on @paper.tex.
```

```text
Simulate 4 reviewers and provide a meta-review for @paper.tex.
```

### Combined workflow example

```text
Step 1: Use $paper-multi-round-review for Round-1 review on @paper.tex.
Step 2: Based on issues, revise the manuscript.
Step 3: Use $paper-submission-check for final submission check on @paper.tex and @refs.bib.
```

---

## 中文

仓库采用扁平多 Skill 结构，所有 Skill 直接放在 `skills/` 下。

## Skill 列表

| Skill | 类型 | 用途 |
|---|---|---|
| `paper-submission-check` | submission | 投稿前终检（语言、AI痕迹清理、参考文献、格式、检查清单）。 |
| `paper-multi-round-review` | review | 多轮同行评审模拟（评审、Meta-Review、rebuttal/revision 循环）。 |

## 仓库结构

```text
paper-submission-check/
├── skills/
│   ├── paper-submission-check/
│   └── paper-multi-round-review/
├── README.md
└── LICENSE
```

## 安装

大多数平台按“一个文件夹 = 一个 Skill（必须有 `SKILL.md`）”加载。
按需把 `skills/` 下的 Skill 目录复制到平台技能目录。

### Cursor

```bash
cd ~
git clone git@github.com:zhousodo/paper-submission-check.git paper-skills
cp -r ~/paper-skills/skills/paper-submission-check ~/.cursor/skills/
cp -r ~/paper-skills/skills/paper-multi-round-review ~/.cursor/skills/
```

### OpenAI Codex

```bash
cd ~
git clone git@github.com:zhousodo/paper-submission-check.git paper-skills
cp -r ~/paper-skills/skills/paper-submission-check ~/.agents/skills/
cp -r ~/paper-skills/skills/paper-multi-round-review ~/.agents/skills/
```

### Claude Code

```bash
cd ~
git clone git@github.com:zhousodo/paper-submission-check.git paper-skills
cp -r ~/paper-skills/skills/paper-submission-check ~/.claude/skills/
cp -r ~/paper-skills/skills/paper-multi-round-review ~/.claude/skills/
```

## 启用示例（快速验证）

复制完 Skill 后，按下面做：

1. 重启 IDE 或新开一个 agent 会话。
2. 打开包含 `paper.tex`（可选 `refs.bib`）的项目。
3. 先执行一次显式调用（见下方示例）；若输出遵循对应 skill 的流程，说明已启用。

## 调用示例

### 显式调用（推荐）

```text
请使用 $paper-submission-check 检查 @paper.tex 和 @refs.bib，按投稿前清单输出问题。
```

```text
请使用 $paper-multi-round-review 对 @paper.tex 执行完整第一轮评审并给出 Meta-Review。
```

### 自然语言触发示例

```text
帮我对 @paper.tex 做投稿前终检。
```

```text
请模拟 4 位审稿人评审 @paper.tex，并给出综合意见。
```

### 组合使用示例

```text
第 1 步：用 $paper-multi-round-review 对 @paper.tex 做第一轮评审。
第 2 步：根据问题修稿。
第 3 步：用 $paper-submission-check 对 @paper.tex 和 @refs.bib 做最终投稿前检查。
```
