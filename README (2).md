# 🧠 Structured Prompting with Claude — The XML Label Method

> **Stop writing one big paragraph. Start labeling your sections.**  
> This repository shows you exactly how — with real examples from production, supply chain, and sales.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Source: Anthropic Docs](https://img.shields.io/badge/Source-Anthropic%20Official%20Docs-blue)](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
[![Beginner Friendly](https://img.shields.io/badge/Level-Beginner%20Friendly-green)]()

---

## 📋 Table of Contents

- [What Is This?](#-what-is-this)
- [The Core Problem](#-the-core-problem)
- [The Fix: XML Labels](#-the-fix-xml-labels)
- [The Five Labels Explained](#-the-five-labels-explained)
- [Real-World Examples](#-real-world-examples)
  - [Production](#1-production)
  - [Supply Chain](#2-supply-chain)
  - [Sales](#3-sales)
- [Live Interactive Demo](#-live-interactive-demo)
- [What Anthropic's Docs Actually Say](#-what-anthropics-docs-actually-say)
- [The Token Cost Warning](#-the-token-cost-warning)
- [How to Use This Repo](#-how-to-use-this-repo)
- [File Structure](#-file-structure)
- [License](#-license)

---

## 📌 What Is This?

This repository is a **practical guide** to getting dramatically better results from Claude (Anthropic's AI) by structuring your prompts with XML labels instead of writing unformatted paragraphs.

It accompanies a LinkedIn post demonstrating the difference between:

- ❌ One unstructured block of text (Claude has to guess what matters)
- ✅ A labeled, sectioned prompt (Claude knows exactly what to do)

You'll find full explanations, three real-world examples you can copy, and an interactive web demo.

**Who is this for?**
- Anyone using Claude for work tasks — analysts, managers, writers, developers
- Teams who want consistent, high-quality AI outputs
- Complete beginners who have never thought about "prompt engineering" before

---

## 🔥 The Core Problem

When you write a prompt like this:

```
I work in a factory and we keep having quality issues on the assembly line, 
three machines have failed twice this week and the shift supervisor doesn't 
know why. Can you help me figure out what's going wrong and what we should do?
```

Claude has to answer three silent questions before it can even start:

1. **Who should I be?** (An engineer? A manager? A general assistant?)
2. **What exactly is the task?** (Diagnose? Plan? Summarize?)
3. **What does a good answer look like?** (A bullet list? A report? A step-by-step?)

When Claude guesses wrong on any of these, you get a generic answer — and you go back and forth fixing it.

**The fix is simple: answer those questions upfront, in labeled sections.**

---

## ✅ The Fix: XML Labels

XML labels are just descriptive tags that wrap different parts of your prompt.  
Think of them like folder names for your instructions.

```xml
<role>
  You are a manufacturing quality engineer with 10 years of experience.
</role>

<context>
  Three machines on our assembly line have failed twice this week.
  The shift supervisor cannot identify a pattern.
</context>

<instructions>
  1. Diagnose the most likely causes of repeated machine failure.
  2. Suggest a short-term fix and a long-term prevention plan.
  3. Recommend what data to collect going forward.
</instructions>

<constraints>
  - Keep recommendations practical for a team of 5.
  - Avoid solutions that require budget approval.
</constraints>

<output_format>
  Root Causes (3 bullets)
  Action Plan (numbered steps)
  Data to Collect (2 sentences)
</output_format>
```

**Why does this work?**

<invoke name="web_fetch">
According to [Anthropic's official prompting documentation](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices), Claude is described as a "brilliant but new employee who lacks context on your norms and workflows." The more precisely you explain what you want — including *who* Claude should be — the better the result.

XML tags help Claude **parse complex prompts unambiguously**, especially when your prompt mixes instructions, context, examples, and variable inputs. Wrapping each type of content in its own tag reduces misinterpretation.

---

## 🏷️ The Five Labels Explained

Each label has a specific job. Here's what each one does and why it matters:

| Label | What It Does | What Happens Without It |
|---|---|---|
| `<role>` | Tells Claude *who* to be | Generic, surface-level answers |
| `<context>` | Gives Claude the situation | Claude makes assumptions that may be wrong |
| `<instructions>` | Tells Claude *what* to do, step by step | Claude decides what's important — not you |
| `<constraints>` | Sets the rules and limits | Claude may give impractical or off-scope answers |
| `<output_format>` | Describes what the answer should look like | Inconsistent structure every time |

> 💡 **There are no fixed labels.** Use names that make sense for your work and stay consistent. You could write `<background>` instead of `<context>`, or `<rules>` instead of `<constraints>`. The structure matters more than the exact names.

---

## 🌍 Real-World Examples

Each example below follows the full five-label structure. They are kept intentionally concise — every word earns its place.

---

### 1. Production

**Situation:** Repeated machine failures on an assembly line.

```xml
<role>
  You are a manufacturing quality engineer with 10 years of production floor experience.
</role>

<context>
  Three machines on our assembly line have each failed twice this week.
  We suspect operator error or equipment wear, but have no confirmed root cause.
</context>

<instructions>
  1. List the three most likely root causes of repeated machine failure.
  2. Recommend a short-term workaround for each cause.
  3. Suggest one long-term prevention measure.
</instructions>

<constraints>
  - Solutions must be actionable by a team of five operators.
  - Avoid recommendations requiring capital expenditure or outside contractors.
</constraints>

<output_format>
  Root Causes (3 bullets)
  Short-Term Fixes (numbered list, one per cause)
  Long-Term Prevention (1 paragraph)
</output_format>
```

**Why this works:** The `<role>` anchors every answer in production-floor thinking, not generic advice. The `<constraints>` prevent Claude from recommending something impractical like "hire a specialist."

---

### 2. Supply Chain

**Situation:** A best-selling product keeps going out of stock.

```xml
<role>
  You are a senior supply chain analyst with 10 years of experience
  in retail inventory management.
</role>

<context>
  Three best-selling products have gone out of stock twice this month,
  causing lost sales and customer complaints.
</context>

<instructions>
  1. Identify why stockouts keep happening.
  2. Suggest a reordering process based on demand patterns.
  3. Recommend which products to monitor most closely.
</instructions>

<constraints>
  - Base recommendations on sales history and current stock levels.
  - Keep solutions practical for a small operations team.
</constraints>

<output_format>
  Root Causes (3 bullets)
  Reordering Process (step-by-step)
  Watch List (2 sentences)
</output_format>
```

**Why this works:** This is the exact prompt shown in the LinkedIn post image. Notice how the `<output_format>` label produces a predictable structure every single time you run it — no reformatting needed.

---

### 3. Sales

**Situation:** A sales rep needs help preparing for a difficult renewal call.

```xml
<role>
  You are a senior B2B sales coach with experience in SaaS account management.
</role>

<context>
  A key client's annual contract renews in two weeks.
  Their usage has dropped 40% over the last quarter and they haven't
  responded to two check-in emails.
</context>

<instructions>
  1. Identify the most likely reasons for reduced engagement.
  2. Suggest three talking points to open the renewal conversation.
  3. Recommend how to handle a price objection if it comes up.
</instructions>

<constraints>
  - Tone should be consultative, not pushy.
  - Do not suggest discounting without understanding the client's concerns first.
</constraints>

<output_format>
  Risk Assessment (2 sentences)
  Opening Talking Points (3 bullets)
  Objection Handling Script (short paragraph)
</output_format>
```

**Why this works:** The `<constraints>` label protects the sales strategy — it stops Claude from jumping to a discount offer before understanding the real problem. The `<output_format>` produces something the rep can print and bring to the call.

---

## 🖥️ Live Interactive Demo

This repository includes an interactive web page (`index.html`) that lets you:

- **See** a before/after comparison of unstructured vs. structured prompts
- **Explore** all three industry examples with syntax highlighting
- **Copy** any prompt to your clipboard with one click
- **Understand** what each label does through color-coded annotations

**To open it:** Download `index.html` and open it in any web browser. No installation needed.

---

## 📚 What Anthropic's Docs Actually Say

All techniques in this repository come directly from [Anthropic's official prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices).

Here are the key principles, in plain English:

**1. Be clear and direct.**  
Claude takes instructions literally. "Can you suggest some changes?" may produce suggestions instead of changes. Say exactly what you want.

**2. Give Claude a role.**  
Anthropic calls role prompting "one of the most effective ways to steer Claude's behavior." Even a single sentence like "You are a senior analyst" dramatically shifts the quality and specificity of answers.

**3. Use XML tags to structure complex prompts.**  
When your prompt mixes instructions, background, and examples, XML tags help Claude parse each section without guessing where one ends and another begins.

**4. Use examples.**  
Including 2–5 examples (called "few-shot prompting") is one of the most reliable ways to control output format, tone, and structure. Wrap them in `<example>` tags.

**5. Match your format to your desired output.**  
If you want flowing prose, write your prompt in prose. If you want a numbered list, use numbered steps in your instructions. Claude mirrors the structure it receives.

**6. Provide context and motivation.**  
Telling Claude *why* something matters helps it generalize correctly, not just follow instructions mechanically.

---

## ⚠️ The Token Cost Warning

Every word you type uses up part of Claude's **context window** — the amount of text Claude can hold in "working memory" at once.

If you retype your full labeled structure into every new conversation, you're spending tokens on setup instead of the actual task.

**The better approach: use Claude Projects.**

Claude's Projects feature (available in Claude.ai) lets you save a system prompt once. Every conversation inside that project inherits it automatically — so you only pay for setup once, and every message after that goes straight to the task.

How to set it up:
1. Go to [claude.ai](https://claude.ai)
2. Click **"Projects"** in the left sidebar
3. Create a new project and paste your labeled structure into **"Project Instructions"**
4. Every chat inside that project uses your structure by default

---

## 🚀 How to Use This Repo

**Option 1: Just read and copy**  
Browse the examples above. Copy any prompt into Claude and replace the context with your own situation.

**Option 2: Open the interactive demo**  
Download `index.html` and open it in your browser for a visual walkthrough.

**Option 3: Adapt the template**  
Use this base template for any task:

```xml
<role>
  You are a [job title] with [X] years of experience in [domain].
</role>

<context>
  [Describe the current situation in 2–3 sentences.]
</context>

<instructions>
  1. [First thing you want Claude to do]
  2. [Second thing]
  3. [Third thing]
</instructions>

<constraints>
  - [Rule or limit #1]
  - [Rule or limit #2]
</constraints>

<output_format>
  [Describe exactly what the answer should look like]
</output_format>
```

---

## 📁 File Structure

```
structured-prompting-guide/
│
├── README.md          ← You are here. Full guide, examples, and explanations.
├── index.html         ← Interactive web demo. Open in any browser, no setup needed.
└── LICENSE            ← MIT License
```

**What goes where:**

- **README.md** is the written guide — the "textbook." It explains the concepts, shows the examples, and links to sources. Anyone reading your repository on GitHub sees this first.
- **index.html** is the visual, interactive layer — the "classroom." It brings the same content to life with color, interactivity, and copy buttons, making it easier to explore without reading a wall of text.

Think of README as the *explanation* and index.html as the *experience*.

---

## 📄 License

MIT License — free to use, adapt, and share with attribution.

---

## 🙏 Credits

- Prompting techniques sourced from [Anthropic's official documentation](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
- Supply chain example inspired by the original LinkedIn post demonstrating the XML label method
- Built to help beginners get consistent, high-quality results from Claude on day one

---

*Last updated: June 2026*
