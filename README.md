🧠 Structured Prompting with Claude — The XML Label Method
> Stop writing one big paragraph. Label your sections instead.  
> Claude reads labeled prompts the way a professional reads a brief — clearly, completely, and without guessing.
![Source: Anthropic Docs](https://img.shields.io/badge/Source-Anthropic%20Official%20Docs-blue?style=for-the-badge)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)
[![Beginner Friendly](https://img.shields.io/badge/Level-Beginner%20Friendly-brightgreen?style=for-the-badge)]()
![Built With Claude](https://img.shields.io/badge/Built%20with-Claude%20AI-blueviolet?style=for-the-badge)
![LinkedIn](https://img.shields.io/badge/Connect-LinkedIn-0A66C2?style=for-the-badge&logo=linkedin)
---
📌 What This Is
A practical reference for structuring Claude prompts using XML labels — based directly on Anthropic's official prompting best practices.
Instead of typing one block of text and hoping Claude figures out what matters, you split your prompt into labeled sections. Each label has one job. Claude handles the rest.
This repo accompanies a LinkedIn post showing the before/after in a real supply chain example. The examples here go further — across production, sales, and project management.
---
🔍 The Problem in One Paragraph
When you write an unstructured prompt, Claude silently answers three questions before it can even start: Who should I be? What exactly is the task? What should the answer look like? If it guesses wrong on any of them, you get a generic response and go back and forth fixing it. Labels eliminate the guesswork entirely.
---
🏷️ The Five Labels — Quick Reference
Label	What It Tells Claude	Without It
`<role>`	Who to be and what expertise to bring	Surface-level, generic answers
`<context>`	The situation — what's happening and why it matters	Claude fills gaps with assumptions
`<instructions>`	Exactly what to do, in what order	Claude decides the scope — not you
`<constraints>`	Rules, limits, and what to avoid	Answers that look right but aren't usable
`<output_format>`	What the response should look like	Different structure every single time
> **The label names aren't fixed.** `<background>` works as well as `<context>`. `<rules>` works as well as `<constraints>`. Pick names that fit your domain and stay consistent.
---
🌍 Real-World Examples
Three domains. Same five-label structure throughout. Copy any of these into Claude and replace the context with your own situation.
---
1. 🏭 Production — Machine Failure Diagnosis
```xml
<role>
  You are a manufacturing quality engineer with 10 years of production floor experience.
</role>

<context>
  Three machines on our assembly line have each failed twice this week.
  We suspect operator error or equipment wear but have no confirmed root cause.
</context>

<instructions>
  1. List the three most likely root causes of repeated machine failure.
  2. Recommend a short-term workaround for each.
  3. Suggest one long-term prevention measure.
</instructions>

<constraints>
  - Solutions must be actionable by a team of five operators.
  - Avoid recommendations that require outside contractors or capital expenditure.
</constraints>

<output_format>
  Root Causes (3 bullets)
  Short-Term Fixes (one per cause, numbered)
  Long-Term Prevention (1 paragraph)
</output_format>
```
---
2. 💼 Sales — Difficult Renewal Call Prep
```xml
<role>
  You are a senior B2B sales coach with experience in SaaS account management.
</role>

<context>
  A key client's annual contract renews in two weeks.
  Their product usage has dropped 40% this quarter and they have not responded
  to two check-in emails.
</context>

<instructions>
  1. Identify the most likely reasons for reduced engagement.
  2. Suggest three talking points to open the renewal conversation.
  3. Recommend how to handle a price objection if it comes up.
</instructions>

<constraints>
  - Tone must be consultative, not pushy.
  - Do not suggest discounting before understanding the client's actual concern.
</constraints>

<output_format>
  Risk Assessment (2 sentences)
  Opening Talking Points (3 bullets)
  Objection Handling Script (short paragraph)
</output_format>
```
---
3. 📋 Project Management — Stakeholder Status Update
```xml
<role>
  You are a senior project manager communicating with executive stakeholders.
</role>

<context>
  A product launch project is three weeks behind schedule due to a delayed
  vendor integration. The core team is aware but leadership has not been formally updated.
</context>

<instructions>
  1. Draft a concise status update email for senior leadership.
  2. Include the reason for the delay without assigning blame.
  3. Propose a revised timeline and the key decision needed from leadership.
</instructions>

<constraints>
  - Email must be under 150 words.
  - Tone should be confident and solution-focused, not defensive.
</constraints>

<output_format>
  Subject Line
  Email Body (under 150 words)
  Decision Required (1 sentence, clearly flagged)
</output_format>
```
---
💡 One Tip That Saves You Time
Retyping your full label structure into every new Claude conversation wastes tokens. Save it once in a Claude Project under "Project Instructions" — every chat inside that project inherits it automatically.
→ claude.ai · Projects · New Project · Project Instructions
---
🗂️ What's in This Repo
File	What It Is
`README.md`	This page — the reference guide with all examples
`index.html`	Interactive demo with color-coded labels and copy buttons
Open `index.html` in any browser. No setup or installation needed.
---
📄 Source & License
Prompting techniques sourced from Anthropic's official documentation · MIT License · June 2026
