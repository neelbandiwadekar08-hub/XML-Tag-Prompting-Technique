# XML-Tag-Prompting-Technique
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Structured Prompting with Claude — The XML Label Method</title>
  <style>
    :root {
      --bg: #0f1117;
      --surface: #181c27;
      --surface2: #1e2436;
      --border: #2a3050;
      --accent: #7c6aff;
      --accent2: #4fd1c5;
      --accent3: #f6c90e;
      --text: #e2e8f0;
      --text-muted: #8892aa;
      --role-color: #f6c90e;
      --context-color: #4fd1c5;
      --instructions-color: #7c6aff;
      --constraints-color: #f97316;
      --output-color: #4ade80;
      --tag-bg-role: rgba(246,201,14,0.10);
      --tag-bg-context: rgba(79,209,197,0.10);
      --tag-bg-instructions: rgba(124,106,255,0.10);
      --tag-bg-constraints: rgba(249,115,22,0.10);
      --tag-bg-output: rgba(74,222,128,0.10);
      --radius: 12px;
      --mono: 'JetBrains Mono', 'Fira Mono', 'Cascadia Code', monospace;
      --sans: 'Inter', system-ui, sans-serif;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: var(--sans);
      line-height: 1.7;
      min-height: 100vh;
    }

    /* ── Nav ── */
    nav {
      background: var(--surface);
      border-bottom: 1px solid var(--border);
      padding: 0 2rem;
      position: sticky;
      top: 0;
      z-index: 100;
      display: flex;
      align-items: center;
      justify-content: space-between;
      height: 56px;
    }
    .nav-brand {
      font-size: 0.85rem;
      font-weight: 700;
      color: var(--accent);
      letter-spacing: 0.03em;
      text-decoration: none;
    }
    .nav-links { display: flex; gap: 1.5rem; }
    .nav-links a {
      font-size: 0.8rem;
      color: var(--text-muted);
      text-decoration: none;
      transition: color 0.2s;
    }
    .nav-links a:hover { color: var(--text); }

    /* ── Hero ── */
    .hero {
      text-align: center;
      padding: 5rem 2rem 3.5rem;
      max-width: 760px;
      margin: 0 auto;
    }
    .hero-eyebrow {
      font-size: 0.75rem;
      font-weight: 700;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 1rem;
    }
    h1 {
      font-size: clamp(2rem, 5vw, 3.2rem);
      font-weight: 800;
      line-height: 1.2;
      color: var(--text);
      margin-bottom: 1.25rem;
    }
    h1 span { color: var(--accent); }
    .hero-sub {
      font-size: 1.1rem;
      color: var(--text-muted);
      max-width: 520px;
      margin: 0 auto 2rem;
    }
    .badges { display: flex; gap: 0.5rem; justify-content: center; flex-wrap: wrap; }
    .badge {
      background: var(--surface2);
      border: 1px solid var(--border);
      border-radius: 999px;
      font-size: 0.72rem;
      font-weight: 600;
      padding: 0.3em 0.9em;
      color: var(--text-muted);
    }
    .badge.accent { border-color: var(--accent); color: var(--accent); }

    /* ── Section layout ── */
    .section {
      max-width: 900px;
      margin: 0 auto;
      padding: 3.5rem 2rem;
    }
    .section + .section { border-top: 1px solid var(--border); }
    .section-label {
      font-size: 0.72rem;
      font-weight: 700;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 0.6rem;
    }
    h2 {
      font-size: 1.75rem;
      font-weight: 800;
      margin-bottom: 0.8rem;
    }
    h3 {
      font-size: 1.2rem;
      font-weight: 700;
      margin-bottom: 0.5rem;
    }
    p { color: var(--text-muted); margin-bottom: 1rem; }
    p strong { color: var(--text); }

    /* ── Before/After comparison ── */
    .compare-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 1.25rem;
      margin-top: 2rem;
    }
    @media (max-width: 640px) { .compare-grid { grid-template-columns: 1fr; } }
    .compare-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      overflow: hidden;
    }
    .compare-card.bad { border-color: #ef4444; }
    .compare-card.good { border-color: var(--accent2); }
    .compare-header {
      padding: 0.6rem 1rem;
      font-size: 0.75rem;
      font-weight: 700;
      letter-spacing: 0.06em;
      text-transform: uppercase;
    }
    .compare-card.bad .compare-header { background: rgba(239,68,68,0.12); color: #ef4444; }
    .compare-card.good .compare-header { background: rgba(79,209,197,0.12); color: var(--accent2); }
    .compare-body {
      padding: 1rem;
      font-family: var(--mono);
      font-size: 0.8rem;
      line-height: 1.65;
      color: var(--text-muted);
      white-space: pre-wrap;
    }
    .compare-card.good .compare-body { color: var(--text); }

    /* ── Label reference table ── */
    .label-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 1rem;
      margin-top: 2rem;
    }
    .label-card {
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 1.1rem 1.2rem;
      position: relative;
      overflow: hidden;
    }
    .label-card::before {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 3px;
    }
    .label-card.role { background: var(--tag-bg-role); border-color: var(--role-color); }
    .label-card.role::before { background: var(--role-color); }
    .label-card.context { background: var(--tag-bg-context); border-color: var(--context-color); }
    .label-card.context::before { background: var(--context-color); }
    .label-card.instructions { background: var(--tag-bg-instructions); border-color: var(--instructions-color); }
    .label-card.instructions::before { background: var(--instructions-color); }
    .label-card.constraints { background: var(--tag-bg-constraints); border-color: var(--constraints-color); }
    .label-card.constraints::before { background: var(--constraints-color); }
    .label-card.output { background: var(--tag-bg-output); border-color: var(--output-color); }
    .label-card.output::before { background: var(--output-color); }
    .label-tag {
      font-family: var(--mono);
      font-size: 0.78rem;
      font-weight: 700;
      margin-bottom: 0.4rem;
    }
    .label-card.role .label-tag { color: var(--role-color); }
    .label-card.context .label-tag { color: var(--context-color); }
    .label-card.instructions .label-tag { color: var(--instructions-color); }
    .label-card.constraints .label-tag { color: var(--constraints-color); }
    .label-card.output .label-tag { color: var(--output-color); }
    .label-title { font-size: 0.9rem; font-weight: 700; color: var(--text); margin-bottom: 0.3rem; }
    .label-desc { font-size: 0.8rem; color: var(--text-muted); line-height: 1.5; }
    .label-without { font-size: 0.75rem; color: var(--text-muted); margin-top: 0.5rem; font-style: italic; }

    /* ── Examples tabs ── */
    .tabs { display: flex; gap: 0.5rem; margin-bottom: 1.5rem; flex-wrap: wrap; }
    .tab-btn {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 999px;
      color: var(--text-muted);
      cursor: pointer;
      font-size: 0.82rem;
      font-weight: 600;
      padding: 0.4em 1.1em;
      transition: all 0.2s;
    }
    .tab-btn:hover { border-color: var(--accent); color: var(--accent); }
    .tab-btn.active { background: var(--accent); border-color: var(--accent); color: #fff; }
    .tab-content { display: none; }
    .tab-content.active { display: block; }

    /* ── Code / Prompt blocks ── */
    .prompt-block {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      overflow: hidden;
      margin-bottom: 1rem;
    }
    .prompt-toolbar {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0.55rem 1rem;
      background: var(--surface2);
      border-bottom: 1px solid var(--border);
    }
    .prompt-toolbar-title {
      font-size: 0.72rem;
      font-weight: 700;
      letter-spacing: 0.06em;
      text-transform: uppercase;
      color: var(--text-muted);
    }
    .copy-btn {
      background: none;
      border: 1px solid var(--border);
      border-radius: 6px;
      color: var(--text-muted);
      cursor: pointer;
      font-size: 0.72rem;
      font-weight: 600;
      padding: 0.25em 0.7em;
      transition: all 0.2s;
    }
    .copy-btn:hover { border-color: var(--accent2); color: var(--accent2); }
    .copy-btn.copied { border-color: var(--output-color); color: var(--output-color); }
    .prompt-code {
      padding: 1.2rem;
      font-family: var(--mono);
      font-size: 0.8rem;
      line-height: 1.75;
      overflow-x: auto;
      white-space: pre;
    }

    /* XML syntax colors */
    .xml-tag { color: var(--text-muted); }
    .xml-role { color: var(--role-color); }
    .xml-context { color: var(--context-color); }
    .xml-instructions { color: var(--instructions-color); }
    .xml-constraints { color: var(--constraints-color); }
    .xml-output { color: var(--output-color); }
    .xml-content { color: var(--text); }

    /* ── Why it works callout ── */
    .why-card {
      background: rgba(124,106,255,0.07);
      border: 1px solid rgba(124,106,255,0.25);
      border-radius: var(--radius);
      padding: 1rem 1.2rem;
      margin-top: 1rem;
    }
    .why-card .why-label {
      font-size: 0.7rem;
      font-weight: 700;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 0.4rem;
    }
    .why-card p { color: var(--text); font-size: 0.88rem; margin: 0; }

    /* ── Anthropic docs grid ── */
    .docs-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
      gap: 1rem;
      margin-top: 2rem;
    }
    .doc-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 1.1rem 1.2rem;
    }
    .doc-num {
      font-size: 0.7rem;
      font-weight: 700;
      letter-spacing: 0.1em;
      color: var(--accent);
      margin-bottom: 0.3rem;
    }
    .doc-title { font-size: 0.95rem; font-weight: 700; color: var(--text); margin-bottom: 0.3rem; }
    .doc-desc { font-size: 0.8rem; color: var(--text-muted); line-height: 1.55; }

    /* ── Token warning ── */
    .warning-box {
      background: rgba(249,115,22,0.08);
      border: 1px solid rgba(249,115,22,0.3);
      border-radius: var(--radius);
      padding: 1.2rem 1.4rem;
      margin-top: 1.5rem;
    }
    .warning-label {
      font-size: 0.7rem;
      font-weight: 700;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      color: var(--constraints-color);
      margin-bottom: 0.5rem;
    }
    .warning-box p { color: var(--text); font-size: 0.88rem; margin: 0; }

    /* ── Steps ── */
    .steps { list-style: none; margin-top: 1rem; display: flex; flex-direction: column; gap: 0.6rem; }
    .steps li {
      display: flex;
      align-items: flex-start;
      gap: 0.9rem;
      font-size: 0.88rem;
      color: var(--text);
    }
    .step-num {
      flex-shrink: 0;
      width: 26px;
      height: 26px;
      background: var(--accent);
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.72rem;
      font-weight: 800;
      color: #fff;
    }

    /* ── Template section ── */
    .template-block {
      background: var(--surface);
      border: 1px solid var(--accent);
      border-radius: var(--radius);
      overflow: hidden;
      margin-top: 1.5rem;
    }
    .template-header {
      background: rgba(124,106,255,0.15);
      padding: 0.6rem 1rem;
      font-size: 0.75rem;
      font-weight: 700;
      letter-spacing: 0.06em;
      text-transform: uppercase;
      color: var(--accent);
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    /* ── Footer ── */
    footer {
      border-top: 1px solid var(--border);
      padding: 2rem;
      text-align: center;
      color: var(--text-muted);
      font-size: 0.8rem;
    }
    footer a { color: var(--accent2); text-decoration: none; }
  </style>
</head>
<body>

<nav>
  <a href="#" class="nav-brand">🧠 Structured Prompting</a>
  <div class="nav-links">
    <a href="#problem">Problem</a>
    <a href="#labels">Labels</a>
    <a href="#examples">Examples</a>
    <a href="#docs">Docs</a>
    <a href="#template">Template</a>
  </div>
</nav>

<!-- ── HERO ── -->
<div class="hero">
  <div class="hero-eyebrow">Prompt Engineering · Claude · XML Labels</div>
  <h1>Stop writing one big paragraph.<br><span>Label your sections instead.</span></h1>
  <p class="hero-sub">
    A practical guide to getting dramatically better results from Claude —
    with real examples from production, supply chain, and sales.
  </p>
  <div class="badges">
    <span class="badge accent">Based on Anthropic's Official Docs</span>
    <span class="badge">Beginner Friendly</span>
    <span class="badge">3 Industry Examples</span>
    <span class="badge">Copy-Paste Ready</span>
  </div>
</div>

<!-- ── THE PROBLEM ── -->
<div class="section" id="problem">
  <div class="section-label">The Problem</div>
  <h2>When you write one block of text, Claude has to guess</h2>
  <p>
    Claude is described in Anthropic's own documentation as a <strong>"brilliant but new employee
    who lacks context on your norms and workflows."</strong> When you dump everything into one paragraph,
    Claude silently answers three questions before it can help you — and if it guesses wrong on any of them, you get a generic answer.
  </p>
  <p>The three questions Claude asks when it reads an unstructured prompt:</p>

  <div class="compare-grid">
    <div class="compare-card bad">
      <div class="compare-header">❌ Unstructured — Claude is guessing</div>
      <div class="compare-body">I work in a factory and we keep having
quality issues on the assembly line,
three machines have failed twice this
week and the shift supervisor doesn't
know why. Can you help me figure out
what's going wrong and what we should
do about it?

→ Who should I be?
→ What exactly is the task?
→ What should the answer look like?</div>
    </div>
    <div class="compare-card good">
      <div class="compare-header">✅ Labeled — Claude knows exactly what to do</div>
      <div class="compare-body">&lt;role&gt;
  Manufacturing quality engineer,
  10 years of experience.
&lt;/role&gt;
&lt;context&gt;
  3 machines failed twice this week.
  No confirmed root cause yet.
&lt;/context&gt;
&lt;instructions&gt;
  1. Diagnose root causes.
  2. Recommend fixes.
&lt;/instructions&gt;
&lt;output_format&gt;
  Root Causes (3 bullets)
  Action Plan (numbered steps)
&lt;/output_format&gt;</div>
    </div>
  </div>
</div>

<!-- ── THE FIVE LABELS ── -->
<div class="section" id="labels">
  <div class="section-label">The Fix</div>
  <h2>Five labels. Each one has a specific job.</h2>
  <p>
    There are no fixed label names — use whatever makes sense for your work and stay consistent.
    The <strong>structure</strong> matters more than the exact words.
  </p>

  <div class="label-grid">
    <div class="label-card role">
      <div class="label-tag">&lt;role&gt;</div>
      <div class="label-title">Who Claude Is</div>
      <div class="label-desc">Tells Claude what expertise and perspective to bring to every answer.</div>
      <div class="label-without">Without it: generic, surface-level responses.</div>
    </div>
    <div class="label-card context">
      <div class="label-tag">&lt;context&gt;</div>
      <div class="label-title">The Situation</div>
      <div class="label-desc">Gives Claude the background it needs to understand your specific problem.</div>
      <div class="label-without">Without it: Claude makes assumptions that may be wrong.</div>
    </div>
    <div class="label-card instructions">
      <div class="label-tag">&lt;instructions&gt;</div>
      <div class="label-title">What To Do</div>
      <div class="label-desc">Numbered steps telling Claude exactly what to do, in what order.</div>
      <div class="label-without">Without it: Claude decides what's important — not you.</div>
    </div>
    <div class="label-card constraints">
      <div class="label-tag">&lt;constraints&gt;</div>
      <div class="label-title">The Rules</div>
      <div class="label-desc">Sets limits so Claude doesn't give impractical or off-scope answers.</div>
      <div class="label-without">Without it: Claude may recommend things you can't actually do.</div>
    </div>
    <div class="label-card output">
      <div class="label-tag">&lt;output_format&gt;</div>
      <div class="label-title">What It Looks Like</div>
      <div class="label-desc">Describes the exact structure and format you want in the response.</div>
      <div class="label-without">Without it: inconsistent structure every single time.</div>
    </div>
  </div>
</div>

<!-- ── EXAMPLES ── -->
<div class="section" id="examples">
  <div class="section-label">Real-World Examples</div>
  <h2>Three industries. All five labels. Copy-paste ready.</h2>
  <p>Each example below is intentionally concise — every word earns its place. Click a tab to switch industries.</p>

  <div class="tabs">
    <button class="tab-btn active" onclick="switchTab('production', this)">🏭 Production</button>
    <button class="tab-btn" onclick="switchTab('supply', this)">📦 Supply Chain</button>
    <button class="tab-btn" onclick="switchTab('sales', this)">💼 Sales</button>
  </div>

  <!-- PRODUCTION -->
  <div class="tab-content active" id="tab-production">
    <p><strong>Situation:</strong> Repeated machine failures on an assembly line. You need root causes and a fix, fast.</p>
    <div class="prompt-block">
      <div class="prompt-toolbar">
        <span class="prompt-toolbar-title">Production Example — Full Prompt</span>
        <button class="copy-btn" onclick="copyPrompt(this, 'production')">Copy</button>
      </div>
      <div class="prompt-code" id="production"><span class="xml-role">&lt;role&gt;</span>
<span class="xml-content">  You are a manufacturing quality engineer with 10 years of production floor experience.</span>
<span class="xml-role">&lt;/role&gt;</span>

<span class="xml-context">&lt;context&gt;</span>
<span class="xml-content">  Three machines on our assembly line have each failed twice this week.</span>
<span class="xml-content">  We suspect operator error or equipment wear, but have no confirmed root cause.</span>
<span class="xml-context">&lt;/context&gt;</span>

<span class="xml-instructions">&lt;instructions&gt;</span>
<span class="xml-content">  1. List the three most likely root causes of repeated machine failure.</span>
<span class="xml-content">  2. Recommend a short-term workaround for each cause.</span>
<span class="xml-content">  3. Suggest one long-term prevention measure.</span>
<span class="xml-instructions">&lt;/instructions&gt;</span>

<span class="xml-constraints">&lt;constraints&gt;</span>
<span class="xml-content">  - Solutions must be actionable by a team of five operators.</span>
<span class="xml-content">  - Avoid recommendations requiring capital expenditure or outside contractors.</span>
<span class="xml-constraints">&lt;/constraints&gt;</span>

<span class="xml-output">&lt;output_format&gt;</span>
<span class="xml-content">  Root Causes (3 bullets)</span>
<span class="xml-content">  Short-Term Fixes (numbered list, one per cause)</span>
<span class="xml-content">  Long-Term Prevention (1 paragraph)</span>
<span class="xml-output">&lt;/output_format&gt;</span></div>
    </div>
    <div class="why-card">
      <div class="why-label">💡 Why This Works</div>
      <p>The <code style="color:var(--role-color)">&lt;role&gt;</code> anchors every answer in production-floor thinking — not generic management advice. The <code style="color:var(--constraints-color)">&lt;constraints&gt;</code> prevent Claude from recommending something impractical like "hire a specialist." The <code style="color:var(--output-color)">&lt;output_format&gt;</code> means you get the same structure every time, whether you run this prompt once or fifty times.</p>
    </div>
  </div>

  <!-- SUPPLY CHAIN -->
  <div class="tab-content" id="tab-supply">
    <p><strong>Situation:</strong> Best-selling products keep going out of stock, causing lost sales. This is the exact example from the LinkedIn post.</p>
    <div class="prompt-block">
      <div class="prompt-toolbar">
        <span class="prompt-toolbar-title">Supply Chain Example — Full Prompt</span>
        <button class="copy-btn" onclick="copyPrompt(this, 'supply')">Copy</button>
      </div>
      <div class="prompt-code" id="supply"><span class="xml-role">&lt;role&gt;</span>
<span class="xml-content">  You are a senior supply chain analyst with 10 years of experience</span>
<span class="xml-content">  in retail inventory management.</span>
<span class="xml-role">&lt;/role&gt;</span>

<span class="xml-context">&lt;context&gt;</span>
<span class="xml-content">  Three best-selling products have gone out of stock twice this month,</span>
<span class="xml-content">  causing lost sales and customer complaints.</span>
<span class="xml-context">&lt;/context&gt;</span>

<span class="xml-instructions">&lt;instructions&gt;</span>
<span class="xml-content">  1. Identify why stockouts keep happening.</span>
<span class="xml-content">  2. Suggest a reordering process based on demand patterns.</span>
<span class="xml-content">  3. Recommend which products to monitor most closely.</span>
<span class="xml-instructions">&lt;/instructions&gt;</span>

<span class="xml-constraints">&lt;constraints&gt;</span>
<span class="xml-content">  - Base recommendations on sales history and current stock levels.</span>
<span class="xml-content">  - Keep solutions practical for a small operations team.</span>
<span class="xml-constraints">&lt;/constraints&gt;</span>

<span class="xml-output">&lt;output_format&gt;</span>
<span class="xml-content">  Root Causes (3 bullets)</span>
<span class="xml-content">  Reordering Process (step-by-step)</span>
<span class="xml-content">  Watch List (2 sentences)</span>
<span class="xml-output">&lt;/output_format&gt;</span></div>
    </div>
    <div class="why-card">
      <div class="why-label">💡 Why This Works</div>
      <p>Notice how the <code style="color:var(--output-color)">&lt;output_format&gt;</code> produces a <em>predictable structure</em> every time. "Root Causes → Reordering Process → Watch List" — always in that order, always that shape. That predictability is what makes AI outputs usable in real workflows, not just one-off answers.</p>
    </div>
  </div>

  <!-- SALES -->
  <div class="tab-content" id="tab-sales">
    <p><strong>Situation:</strong> A key client's contract renews in two weeks but engagement has dropped 40%. You need a strategy for the call.</p>
    <div class="prompt-block">
      <div class="prompt-toolbar">
        <span class="prompt-toolbar-title">Sales Example — Full Prompt</span>
        <button class="copy-btn" onclick="copyPrompt(this, 'sales')">Copy</button>
      </div>
      <div class="prompt-code" id="sales"><span class="xml-role">&lt;role&gt;</span>
<span class="xml-content">  You are a senior B2B sales coach with experience in SaaS account management.</span>
<span class="xml-role">&lt;/role&gt;</span>

<span class="xml-context">&lt;context&gt;</span>
<span class="xml-content">  A key client's annual contract renews in two weeks.</span>
<span class="xml-content">  Their usage has dropped 40% over the last quarter and they haven't</span>
<span class="xml-content">  responded to two check-in emails.</span>
<span class="xml-context">&lt;/context&gt;</span>

<span class="xml-instructions">&lt;instructions&gt;</span>
<span class="xml-content">  1. Identify the most likely reasons for reduced engagement.</span>
<span class="xml-content">  2. Suggest three talking points to open the renewal conversation.</span>
<span class="xml-content">  3. Recommend how to handle a price objection if it comes up.</span>
<span class="xml-instructions">&lt;/instructions&gt;</span>

<span class="xml-constraints">&lt;constraints&gt;</span>
<span class="xml-content">  - Tone should be consultative, not pushy.</span>
<span class="xml-content">  - Do not suggest discounting without understanding the client's concerns first.</span>
<span class="xml-constraints">&lt;/constraints&gt;</span>

<span class="xml-output">&lt;output_format&gt;</span>
<span class="xml-content">  Risk Assessment (2 sentences)</span>
<span class="xml-content">  Opening Talking Points (3 bullets)</span>
<span class="xml-content">  Objection Handling Script (short paragraph)</span>
<span class="xml-output">&lt;/output_format&gt;</span></div>
    </div>
    <div class="why-card">
      <div class="why-label">💡 Why This Works</div>
      <p>The <code style="color:var(--constraints-color)">&lt;constraints&gt;</code> label is doing important protective work here. Without it, Claude might jump straight to "offer a discount" — which could undermine the whole negotiation. The constraint forces a consultative approach. The <code style="color:var(--output-color)">&lt;output_format&gt;</code> produces something you can literally print out and bring to the call.</p>
    </div>
  </div>
</div>

<!-- ── ANTHROPIC DOCS ── -->
<div class="section" id="docs">
  <div class="section-label">Source Material</div>
  <h2>What Anthropic's official docs actually say</h2>
  <p>
    Every technique on this page comes directly from
    <a href="https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices" target="_blank" style="color:var(--accent2)">Anthropic's official prompting best practices</a>.
    Here are the six core principles, in plain English:
  </p>

  <div class="docs-grid">
    <div class="doc-card">
      <div class="doc-num">01</div>
      <div class="doc-title">Be clear and direct</div>
      <div class="doc-desc">Claude takes you literally. "Can you suggest some changes?" may give suggestions instead of changes. Say exactly what you want.</div>
    </div>
    <div class="doc-card">
      <div class="doc-num">02</div>
      <div class="doc-title">Give Claude a role</div>
      <div class="doc-desc">Anthropic calls role prompting "one of the most effective ways to steer Claude's behavior." Even one sentence makes a big difference.</div>
    </div>
    <div class="doc-card">
      <div class="doc-num">03</div>
      <div class="doc-title">Use XML tags</div>
      <div class="doc-desc">Tags help Claude parse complex prompts without guessing where one section ends and another begins. Reduces misinterpretation.</div>
    </div>
    <div class="doc-card">
      <div class="doc-num">04</div>
      <div class="doc-title">Use examples</div>
      <div class="doc-desc">Including 2–5 examples (called "few-shot prompting") dramatically improves accuracy. Wrap them in &lt;example&gt; tags.</div>
    </div>
    <div class="doc-card">
      <div class="doc-num">05</div>
      <div class="doc-title">Match your format</div>
      <div class="doc-desc">Claude mirrors the structure it receives. Write in prose if you want prose. Use numbered steps if you want numbered steps.</div>
    </div>
    <div class="doc-card">
      <div class="doc-num">06</div>
      <div class="doc-title">Add context &amp; motivation</div>
      <div class="doc-desc">Telling Claude <em>why</em> something matters helps it generalize correctly — not just follow instructions mechanically.</div>
    </div>
  </div>
</div>

<!-- ── TOKEN WARNING ── -->
<div class="section" id="token">
  <div class="section-label">Important Note</div>
  <h2>The token cost problem — and the fix</h2>
  <p>
    Every word you type uses up part of Claude's <strong>context window</strong> — the amount of text Claude can hold in working memory at once.
    If you retype your full labeled structure into every new chat, you're spending tokens on setup instead of the actual task.
  </p>

  <div class="warning-box">
    <div class="warning-label">⚠️ The Problem</div>
    <p>Typing a full 5-label prompt into every conversation wastes tokens on boilerplate you've already written. Claude also has no memory between separate conversations — it starts fresh every time.</p>
  </div>

  <div style="margin-top:1.5rem">
    <h3>The better approach: Claude Projects</h3>
    <p style="margin-top:0.5rem">Projects let you save a system prompt once. Every conversation inside that project inherits it automatically.</p>
    <ul class="steps">
      <li><span class="step-num">1</span>Go to <a href="https://claude.ai" target="_blank" style="color:var(--accent2)">claude.ai</a> and click <strong>Projects</strong> in the left sidebar</li>
      <li><span class="step-num">2</span>Create a new project and give it a name (e.g. "Supply Chain Analyst")</li>
      <li><span class="step-num">3</span>Paste your labeled structure into <strong>Project Instructions</strong></li>
      <li><span class="step-num">4</span>Every chat inside that project uses your structure by default — no retyping needed</li>
    </ul>
  </div>
</div>

<!-- ── TEMPLATE ── -->
<div class="section" id="template">
  <div class="section-label">Start Here</div>
  <h2>The universal template — adapt it to anything</h2>
  <p>Replace the bracketed parts with your own situation. Every field is optional — leave out any label that doesn't apply to your task.</p>

  <div class="template-block">
    <div class="template-header">
      Universal Template
      <button class="copy-btn" onclick="copyTemplate(this)">Copy</button>
    </div>
    <div class="prompt-code" id="template-code"><span class="xml-role">&lt;role&gt;</span>
<span class="xml-content">  You are a [job title] with [X] years of experience in [domain].</span>
<span class="xml-role">&lt;/role&gt;</span>

<span class="xml-context">&lt;context&gt;</span>
<span class="xml-content">  [Describe the current situation in 2–3 sentences.]</span>
<span class="xml-context">&lt;/context&gt;</span>

<span class="xml-instructions">&lt;instructions&gt;</span>
<span class="xml-content">  1. [First thing you want Claude to do]</span>
<span class="xml-content">  2. [Second thing]</span>
<span class="xml-content">  3. [Third thing]</span>
<span class="xml-instructions">&lt;/instructions&gt;</span>

<span class="xml-constraints">&lt;constraints&gt;</span>
<span class="xml-content">  - [Rule or limit #1]</span>
<span class="xml-content">  - [Rule or limit #2]</span>
<span class="xml-constraints">&lt;/constraints&gt;</span>

<span class="xml-output">&lt;output_format&gt;</span>
<span class="xml-content">  [Describe exactly what the answer should look like]</span>
<span class="xml-output">&lt;/output_format&gt;</span></div>
  </div>
</div>

<!-- ── FOOTER ── -->
<footer>
  <p>Techniques sourced from <a href="https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices" target="_blank">Anthropic's official prompting documentation</a> · MIT License · June 2026</p>
</footer>

<script>
  function switchTab(name, btn) {
    document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
    document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
    document.getElementById('tab-' + name).classList.add('active');
    btn.classList.add('active');
  }

  function getPlainText(elementId) {
    const el = document.getElementById(elementId);
    return el.innerText || el.textContent;
  }

  function copyPrompt(btn, id) {
    const text = getPlainText(id);
    navigator.clipboard.writeText(text).then(() => {
      btn.textContent = 'Copied!';
      btn.classList.add('copied');
      setTimeout(() => {
        btn.textContent = 'Copy';
        btn.classList.remove('copied');
      }, 2000);
    });
  }

  function copyTemplate(btn) {
    const text = getPlainText('template-code');
    navigator.clipboard.writeText(text).then(() => {
      btn.textContent = 'Copied!';
      btn.classList.add('copied');
      setTimeout(() => {
        btn.textContent = 'Copy';
        btn.classList.remove('copied');
      }, 2000);
    });
  }
</script>
</body>
</html>
