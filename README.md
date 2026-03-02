# Paper Pre-Submission Quality Check

[English](#english) | [中文](#中文)

---

## English

A systematic, 10-phase quality inspection workflow for academic LaTeX papers before journal/conference submission. Works with **Cursor, Claude Code, OpenAI Codex, GitHub Copilot, Windsurf**, and any AI coding assistant that reads markdown instructions.

### Supported Platforms

| Platform | Installation | How It Works |
|----------|-------------|--------------|
| **Cursor** | Clone to `~/.cursor/skills/` | Auto-detected as Agent Skill |
| **Claude Code** | Reference in `CLAUDE.md` | Loaded as project instructions |
| **OpenAI Codex** | Reference in `AGENTS.md` | Loaded as agent instructions |
| **GitHub Copilot** | Reference in `.github/copilot-instructions.md` | Loaded as custom instructions |
| **Windsurf** | Clone to `~/.codeium/windsurf/skills/` | Auto-detected as Skill |
| **Any LLM** | Copy SKILL.md content to system prompt | Direct instruction injection |

### What It Does

This skill guides the AI agent through a 10-phase quality check of your LaTeX paper:

| Phase | Check | Examples |
|-------|-------|---------|
| 0 | **Pre-Check** | Detect citation style (numbered/author-year), template type, journal name |
| 1 | **Pronoun & Subjectivity** | "we/our" overuse with quantitative thresholds, "our proposed" redundancy |
| 2 | **AI-Style Detection** | 80+ AI-generated phrases across 5 tiers, structural AI patterns |
| 3 | **Symbol & Punctuation** | Chinese/English punctuation mixing, dash errors, space issues |
| 4 | **Capitalization** | Section title consistency, acronym definitions |
| 5 | **LaTeX Issues** | Citation format (style-aware), `~` before `\cite`/`\ref`, redundant formatting |
| 6 | **Grammar & Language** | Contractions, comma splices, duplicate content, tense consistency |
| 7 | **Tables, Figures & Numbers** | Number cross-reference verification, decimal precision |
| 8 | **Content Structure** | Abstract quality, contribution check, terminology consistency |
| 9 | **BIB File Integrity** | Brace matching, nested entries, missing fields, cross-references |
| 10 | **Final Checklist** | Per-publisher requirements (Elsevier, IEEE, Springer, ACM) |

### Supported Publishers

- **Elsevier** (`elsarticle`): IPM, COMNET, JSA, KBS, ESWA, etc.
- **IEEE** (`IEEEtran`): Transactions, conferences
- **Springer** (`svjour3`, `llncs`): APIN, TOIT, LNCS, etc.
- **ACM** (`acmart`): Conferences

### Installation

#### Cursor

```bash
# macOS / Linux
cd ~/.cursor/skills
git clone https://github.com/zhousodo/paper-submission-check.git
```

```powershell
# Windows
cd $env:USERPROFILE\.cursor\skills
git clone https://github.com/zhousodo/paper-submission-check.git
```

After cloning, restart Cursor. The skill will be auto-detected.

#### Claude Code

Clone the repo into your project, then reference it in your `CLAUDE.md`:

```bash
cd your-paper-project
git clone https://github.com/zhousodo/paper-submission-check.git .claude/skills/paper-submission-check
```

Add to your project's `CLAUDE.md`:

```markdown
## Paper Quality Check

When asked to review a paper, follow the workflow in
.claude/skills/paper-submission-check/SKILL.md

Reference databases:
- AI phrase detection: .claude/skills/paper-submission-check/ai-phrases.md
- Publisher checklists: .claude/skills/paper-submission-check/checklist.md
```

Or simply reference the files directly in conversation:

```
Read .claude/skills/paper-submission-check/SKILL.md and check my paper.tex
```

#### OpenAI Codex

Clone the repo into your project, then reference it in your `AGENTS.md`:

```bash
cd your-paper-project
git clone https://github.com/zhousodo/paper-submission-check.git .codex/paper-submission-check
```

Add to your project's `AGENTS.md`:

```markdown
## Paper Quality Check

When reviewing LaTeX papers before submission, follow the 10-phase workflow in
.codex/paper-submission-check/SKILL.md

Additional references:
- .codex/paper-submission-check/ai-phrases.md
- .codex/paper-submission-check/checklist.md
```

#### GitHub Copilot

Add to `.github/copilot-instructions.md`:

```markdown
## Paper Review Instructions

When asked to check or review a LaTeX paper, follow the workflow described in:
paper-submission-check/SKILL.md

Use paper-submission-check/ai-phrases.md for AI-style detection
and paper-submission-check/checklist.md for publisher-specific requirements.
```

#### Windsurf

```bash
# macOS / Linux
cd ~/.codeium/windsurf/skills
git clone https://github.com/zhousodo/paper-submission-check.git
```

```powershell
# Windows
cd $env:USERPROFILE\.codeium\windsurf\skills
git clone https://github.com/zhousodo/paper-submission-check.git
```

#### Any Other LLM (ChatGPT, Gemini, etc.)

Copy the content of `SKILL.md` into your system prompt or conversation, then ask:

```
Based on the above instructions, check my paper: [paste LaTeX content]
```

### Usage Examples

#### Cursor

```
@SKILL.md Please check my paper @paper.tex and @refs.bib before submission.
```

The agent auto-detects the skill when you mention "paper check", "proofreading", "submission preparation", or "quality review".

#### Claude Code

```bash
claude "Read paper-submission-check/SKILL.md, then check my paper.tex and my.bib for submission to Elsevier"
```

Or in interactive mode:

```
> Check paper.tex following the workflow in paper-submission-check/SKILL.md
```

#### OpenAI Codex

```bash
codex "Review my paper.tex using the paper-submission-check workflow before I submit to IEEE"
```

#### General Prompts (any platform)

```
Check my paper @paper.tex and @refs.bib before submitting to Elsevier.
```

```
Run a pre-submission quality check on @manuscript.tex.
```

```
Help me review @paper.tex for AI-generated style issues.
```

### File Structure

```
paper-submission-check/
├── SKILL.md          # Main skill instructions (10-phase workflow)
├── ai-phrases.md     # Complete AI-generated phrase database (5 tiers, 80+ phrases)
├── checklist.md      # Per-publisher requirements and regex search commands
├── README.md         # This file
└── LICENSE           # MIT License
```

### Output

The skill produces a structured report:

```markdown
# Paper Pre-Submission Check Report

## Paper Info
- Template: elsarticle
- Citation Style: numbered
- Target Journal: Information Processing & Management

## Summary
- Total issues found: 23
- Critical (must fix): 3
- Warning (should fix): 12
- Suggestion (optional): 8

## Pronoun Statistics
- "we" count: 18 (threshold: ≤15) [OVER]
- "our" count: 5 (threshold: ≤8) [OK]

## Critical Issues
1. [Line 42] BIB entry missing closing brace
2. [Line 156] AI-style phrase: "has garnered significant attention"
...
```

### Contributing

Issues and pull requests are welcome. If you find new AI-generated patterns or publisher-specific requirements, please contribute them.

### License

MIT License - see [LICENSE](LICENSE)

---

## 中文

一个系统化的 10 阶段学术 LaTeX 论文投稿前质量检查工作流。支持 **Cursor、Claude Code、OpenAI Codex、GitHub Copilot、Windsurf** 及所有能读取 Markdown 指令的 AI 编程助手。

### 支持的平台

| 平台 | 安装方式 | 工作原理 |
|------|---------|---------|
| **Cursor** | 克隆到 `~/.cursor/skills/` | 自动识别为 Agent Skill |
| **Claude Code** | 在 `CLAUDE.md` 中引用 | 作为项目指令加载 |
| **OpenAI Codex** | 在 `AGENTS.md` 中引用 | 作为代理指令加载 |
| **GitHub Copilot** | 在 `.github/copilot-instructions.md` 中引用 | 作为自定义指令加载 |
| **Windsurf** | 克隆到 `~/.codeium/windsurf/skills/` | 自动识别为 Skill |
| **其他 LLM** | 将 SKILL.md 内容复制到系统提示词 | 直接注入指令 |

### 功能说明

此 skill 引导 AI 代理对 LaTeX 论文进行 10 阶段质量检查：

| 阶段 | 检查项 | 说明 |
|------|--------|------|
| 0 | **预检** | 检测引用风格（编号/作者-年份）、模板类型、期刊名称 |
| 1 | **代词和主观性** | "we/our" 过度使用（含量化阈值）、"our proposed" 冗余检测 |
| 2 | **AI 风格检测** | 5 个层级的 80+ AI 生成短语、结构性 AI 模式识别 |
| 3 | **符号和标点** | 中英文标点混用、破折号错误、空格问题 |
| 4 | **大小写一致性** | 章节标题风格统一、缩写定义检查 |
| 5 | **LaTeX 问题** | 引用格式（区分编号/作者-年份）、`~` 不间断空格、重复格式化 |
| 6 | **语法和语言** | 缩写词、逗号拼接、重复内容、时态一致性 |
| 7 | **表格、图表和数字** | 正文与表格数字交叉验证、小数精度一致 |
| 8 | **内容结构** | 摘要质量、贡献检查、术语一致性 |
| 9 | **BIB 文件完整性** | 花括号配对、条目嵌套、缺失字段、交叉引用 |
| 10 | **最终清单** | 各出版商特定要求（Elsevier、IEEE、Springer、ACM） |

### 支持的出版商

- **Elsevier** (`elsarticle`)：IPM、COMNET、JSA、KBS、ESWA 等
- **IEEE** (`IEEEtran`)：期刊、会议
- **Springer** (`svjour3`, `llncs`)：APIN、TOIT、LNCS 等
- **ACM** (`acmart`)：会议

### 安装方法

#### Cursor

```powershell
# Windows
cd $env:USERPROFILE\.cursor\skills
git clone https://github.com/zhousodo/paper-submission-check.git
```

```bash
# macOS / Linux
cd ~/.cursor/skills
git clone https://github.com/zhousodo/paper-submission-check.git
```

克隆后重启 Cursor，skill 会自动识别。

#### Claude Code

将仓库克隆到项目中，然后在 `CLAUDE.md` 中引用：

```bash
cd 你的论文项目目录
git clone https://github.com/zhousodo/paper-submission-check.git .claude/skills/paper-submission-check
```

在 `CLAUDE.md` 中添加：

```markdown
## 论文质量检查

当被要求审查论文时，遵循 .claude/skills/paper-submission-check/SKILL.md 中的工作流。
参考数据库：
- AI 短语检测：.claude/skills/paper-submission-check/ai-phrases.md
- 出版商清单：.claude/skills/paper-submission-check/checklist.md
```

#### OpenAI Codex

克隆到项目中，在 `AGENTS.md` 中引用：

```bash
cd 你的论文项目目录
git clone https://github.com/zhousodo/paper-submission-check.git .codex/paper-submission-check
```

在 `AGENTS.md` 中添加：

```markdown
## 论文质量检查

审查 LaTeX 论文时，遵循 .codex/paper-submission-check/SKILL.md 中的 10 阶段工作流。
```

#### 其他 LLM（ChatGPT、Gemini 等）

将 `SKILL.md` 的内容复制到系统提示词或对话中，然后提问：

```
根据上述指令，检查我的论文：[粘贴 LaTeX 内容]
```

### 使用方法

#### Cursor

```
@SKILL.md 请帮我检查论文 @paper.tex 和 @refs.bib，准备投稿。
```

提到"论文检查"、"投稿准备"、"质量审查"等关键词时会自动触发。

#### Claude Code

```bash
claude "读取 paper-submission-check/SKILL.md，然后检查 paper.tex 和 my.bib，准备投稿到 Elsevier"
```

#### OpenAI Codex

```bash
codex "使用 paper-submission-check 工作流审查 paper.tex，准备投稿到 IEEE"
```

### 贡献

欢迎提交 Issue 和 Pull Request。如果发现新的 AI 生成模式或出版商要求，请贡献到此项目。

### 许可证

MIT 许可证 - 详见 [LICENSE](LICENSE)
