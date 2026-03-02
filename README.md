# Paper Pre-Submission Quality Check (Cursor Agent Skill)

[English](#english) | [中文](#中文)

---

## English

A comprehensive Cursor Agent Skill for systematic quality inspection of academic LaTeX papers before journal/conference submission.

### What It Does

This skill guides the Cursor AI agent through a 10-phase quality check of your LaTeX paper:

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

#### Option 1: Personal Skill (all projects)

```bash
# Clone to your Cursor skills directory
cd ~/.cursor/skills
git clone https://github.com/YOUR_USERNAME/paper-submission-check.git
```

On Windows:
```powershell
cd $env:USERPROFILE\.cursor\skills
git clone https://github.com/YOUR_USERNAME/paper-submission-check.git
```

#### Option 2: Project Skill (shared with team)

```bash
# Clone into your project's .cursor/skills directory
cd your-paper-project
mkdir -p .cursor/skills
cd .cursor/skills
git clone https://github.com/YOUR_USERNAME/paper-submission-check.git
```

### Usage

In Cursor, simply ask the agent:

```
@SKILL.md Please check my paper before submission.
```

Or the agent will automatically detect when you need paper checking if you mention keywords like "paper check", "proofreading", "submission preparation", or "quality review".

**Example prompts:**

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

一个全面的 Cursor Agent Skill，用于学术 LaTeX 论文投稿前的系统化质量检查。

### 功能说明

此 skill 引导 Cursor AI 代理对 LaTeX 论文进行 10 阶段质量检查：

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

#### 方式一：个人 Skill（所有项目可用）

```powershell
# Windows
cd $env:USERPROFILE\.cursor\skills
git clone https://github.com/YOUR_USERNAME/paper-submission-check.git
```

```bash
# macOS / Linux
cd ~/.cursor/skills
git clone https://github.com/YOUR_USERNAME/paper-submission-check.git
```

#### 方式二：项目 Skill（与团队共享）

```bash
cd 你的论文项目目录
mkdir -p .cursor/skills
cd .cursor/skills
git clone https://github.com/YOUR_USERNAME/paper-submission-check.git
```

### 使用方法

在 Cursor 中直接请求代理：

```
@SKILL.md 请帮我检查论文，准备投稿。
```

或者代理会在你提到"论文检查"、"投稿准备"、"质量审查"等关键词时自动触发。

**示例提示词：**

```
帮我检查 @paper.tex 和 @refs.bib，准备投稿到 Elsevier 期刊。
```

```
对 @manuscript.tex 进行投稿前质量检查。
```

```
帮我审查 @paper.tex 中的 AI 风格问题。
```

### 贡献

欢迎提交 Issue 和 Pull Request。如果发现新的 AI 生成模式或出版商要求，请贡献到此项目。

### 许可证

MIT 许可证 - 详见 [LICENSE](LICENSE)
