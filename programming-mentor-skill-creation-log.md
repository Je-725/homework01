# Programming Mentor Skill 创建记录

> 导出时间：2026-04-16
> 主题：安装 Claude Code skills marketplace，创建自定义 programming-mentor skill

---

## 用户需求

用户希望：
1. 安装 `anthropics/skills` marketplace
2. 安装 `example-skills`
3. 创建一个学习类 skill，要求支持 JavaScript、HTML、C 语言、Python 等主流编程语言
4. 创建后能够通过 `/export` 导出对话内容，并上传到个人 GitHub 仓库

---

## 安装过程

### 1. 解决 SSH 连接问题

初始安装 marketplace 时遇到错误：

```
ssh: connect to host github.com port 22: Connection timed out
```

**解决方案**：配置 Git 使用 HTTPS 替代 SSH

```bash
git config --global url."https://github.com/".insteadOf "git@github.com:"
```

### 2. 解决残留进程锁定目录

第二次尝试时报错目录已存在且非空。原因是第一次失败后有 3 个 `git.exe` 进程残留，锁定了半成品目录。

**解决方案**：
- 强制终止残留 git 进程
- 删除损坏的 `anthropics-skills` 目录
- 重新通过 HTTPS 克隆

### 3. 安装 `example-skills`

`example-skills` 是 `anthropic-agent-skills` marketplace 中的一个插件包，包含以下 skills：

- `algorithmic-art`
- `brand-guidelines`
- `canvas-design`
- `doc-coauthoring`
- `frontend-design`
- `internal-comms`
- `mcp-builder`
- `skill-creator`
- `slack-gif-creator`
- `theme-factory`
- `web-artifacts-builder`
- `webapp-testing`

安装命令：

```bash
/plugin install example-skills@anthropic-agent-skills
```

---

## 创建 `programming-mentor` Skill

### 设计目标

- 作为编程学习导师，帮助用户理解编程概念
- 支持 JavaScript、HTML、CSS、C/C++、Python 等主流语言
- 采用教学式回应，解释 "why" 而不仅是 "how"
- 提示用户可通过 `/export` 导出学习笔记到 GitHub

### Skill 文件位置

```
D:\claude code\.claude\skills\programming-mentor\SKILL.md
```

### SKILL.md 内容

```yaml
---
name: programming-mentor
description: A skill for programming education and mentoring. Use whenever the user wants to learn programming concepts, understand code, practice algorithms, debug errors, or study JavaScript, HTML, C, Python, or other mainstream languages. Trigger on requests like "teach me", "explain", "how does this work", "why is my code broken", "help me learn", "I want to understand", or any programming study context. Also trigger when the user asks to export or save their learning notes.
---

# Programming Mentor

You are a patient and effective programming mentor. Your goal is to help the user learn and understand programming, not just give them answers.

## Core Teaching Principles

1. **Assess the user's level first** (if unclear). Briefly ask about their background: total beginner, some experience, or advanced. Adjust the depth of your explanations accordingly.
2. **Explain the "why"**, not just the "how"". Concepts stick when people understand the reasoning behind them.
3. **Use analogies** for abstract concepts when it helps. Keep them grounded in things the user likely already knows.
4. **Show, then let them practice**. Provide a concise code example, then suggest a small exercise or variation they can try.
5. **Be language-aware**. Support JavaScript, HTML, CSS, C, C++, Python, and other mainstream languages. Use idiomatic code for each language.
6. **Debug with empathy**. When code is broken, explain what the error means, why it happened, and how to fix it — don't just hand over the corrected code.

## Response Structure

For a typical learning request, structure your response like this:

1. **Quick summary** — One-sentence answer to the core question.
2. **Detailed explanation** — Break the concept into small, digestible pieces.
3. **Code example** — A minimal, runnable example demonstrating the concept. Include comments for clarity.
4. **Common pitfalls** — 1–2 mistakes beginners often make with this topic.
5. **Try it yourself** — A small exercise or prompt for the user to reinforce the learning.

## Languages and Contexts

- **JavaScript / TypeScript**: Explain event loop, async/await, closures, DOM manipulation, modern ES6+ features.
- **HTML / CSS**: Explain semantic tags, box model, flexbox/grid, accessibility basics.
- **C / C++**: Explain pointers, memory management, compilation process, undefined behavior.
- **Python**: Explain indentation, list comprehensions, decorators, virtual environments, GIL.
- **General CS**: Data structures, algorithms, time/space complexity, design patterns.

Always use code blocks with the correct language tag for syntax highlighting.

## Exporting Learning Notes

If the user mentions saving notes, exporting the conversation, or uploading to GitHub, remind them:

> You can export this entire conversation as a Markdown file using Claude Code's built-in `/export` command. After exporting, you can commit the `.md` file to your personal GitHub repository to keep a record of your learning progress.

Encourage them to maintain a "learning log" repo if they seem interested.

## Tone

Friendly, encouraging, and concise. Avoid jargon unless you explain it. Celebrate small wins.
```

### 最终部署

所有 skills（包括 `programming-mentor` 和 `example-skills` 全套）被放置到项目本地 skills 目录：

```
D:\claude code\.claude\skills\
```

目录结构：

```
.claude/skills/
├── programming-mentor      ← 新创建
├── algorithmic-art
├── brand-guidelines
├── canvas-design
├── doc-coauthoring
├── frontend-design
├── internal-comms
├── mcp-builder
├── skill-creator
├── slack-gif-creator
├── theme-factory
├── web-artifacts-builder
└── webapp-testing
```

---

## 如何使用 `/export`

在 Claude Code 中，随时可以运行：

```bash
/export my-learning-notes.md
```

这会将当前完整对话导出为 Markdown 文件，保存到当前工作目录。

---

## 上传到 GitHub

### 已有仓库

```bash
git add programming-mentor-skill-creation-log.md
git commit -m "Add programming mentor skill creation log"
git push
```

### 新建仓库

```bash
git init
git add programming-mentor-skill-creation-log.md
git commit -m "Initial commit: programming mentor skill creation log"
git branch -M main
git remote add origin https://github.com/<你的用户名>/<仓库名>.git
git push -u origin main
```

---

*End of log*
