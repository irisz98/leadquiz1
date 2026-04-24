# New Grad Aura Check — Lead Quiz Prototype Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-file HTML click-through prototype of the New Grad Aura Check quiz with 9 screens, Aura Scan visual direction, and JS state machine navigation.

**Architecture:** One self-contained `new-grad-aura-check.html` file. A JS `showScreen(id)` function manages visibility. Each screen is a `<section data-screen="N">` element hidden by default; only the active screen is visible. Screens 3–5 share one MCQ template rendered by a JS data array. No build step, no dependencies beyond Google Fonts CDN.

**Tech Stack:** Vanilla HTML5 / CSS3 / ES6 JS · Google Fonts (DM Sans) · SVG for radar chart · `navigator.share()` for share CTA

---

## File Structure

| File | Role |
|---|---|
| `new-grad-aura-check.html` | Entire prototype — shell, CSS, all 9 screens, JS |

---

## Task 1: HTML Shell + CSS Design Tokens + State Machine

**Files:**
- Create: `E:\mine\工作\My Learning Space\New Grad Aura Check\new-grad-aura-check.html`

- [ ] **Step 1: Create the file with base shell, CSS tokens, and screen manager**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
  <title>New Grad Aura Check · MYLS Interview</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:opsz,wght@9..40,400;9..40,500;9..40,600;9..40,700;9..40,800&display=swap" rel="stylesheet">
  <style>
    /* ── RESET ── */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    html, body { height: 100%; }

    /* ── DESIGN TOKENS ── */
    :root {
      --bg-base:        #09091A;
      --bg-surface:     #0F0F26;
      --bg-elevated:    #161632;
      --primary:        #3D51FF;
      --primary-light:  #6D7CFF;
      --primary-alpha12: rgba(61,81,255,0.12);
      --primary-alpha45: rgba(61,81,255,0.45);
      --grad-a-start:   #BA8FFF;
      --grad-a-end:     #717FFF;
      --error:          #FF5C5C;
      --text-primary:   #FFFFFF;
      --text-secondary: rgba(255,255,255,0.55);
      --text-muted:     rgba(255,255,255,0.30);
      --text-faint:     rgba(255,255,255,0.22);
      --border-subtle:  rgba(255,255,255,0.07);
      --selected-text:  #8FA9FF;
      --radius-sm:   8px;
      --radius-md:  12px;
      --radius-lg:  14px;
      --radius-xl:  16px;
      --radius-pill:50px;
    }

    /* ── BASE ── */
    body {
      background: var(--bg-base);
      color: var(--text-primary);
      font-family: 'DM Sans', sans-serif;
      display: flex;
      justify-content: center;
      align-items: flex-start;
      min-height: 100vh;
    }

    /* ── APP CONTAINER ── */
    #app {
      width: 100%;
      max-width: 430px;
      min-height: 100vh;
      position: relative;
      overflow: hidden;
      background: var(--bg-base);
    }

    /* ── SCREEN SYSTEM ── */
    [data-screen] {
      display: none;
      flex-direction: column;
      min-height: 100vh;
      animation: screenIn 0.25s ease-out;
    }
    [data-screen].active { display: flex; }

    @keyframes screenIn {
      from { opacity: 0; transform: translateY(8px); }
      to   { opacity: 1; transform: translateY(0); }
    }

    /* ── STATUS BAR ── */
    .status-bar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 12px 20px 8px;
      font-size: 11px;
      font-weight: 700;
      color: rgba(255,255,255,0.4);
      flex-shrink: 0;
    }
    .status-bar .time { color: rgba(255,255,255,0.7); }

    /* ── GRID BACKGROUND ── */
    .bg-grid {
      position: absolute; inset: 0; pointer-events: none;
      background-image:
        linear-gradient(rgba(61,81,255,0.05) 1px, transparent 1px),
        linear-gradient(90deg, rgba(61,81,255,0.05) 1px, transparent 1px);
      background-size: 36px 36px;
    }

    /* ── GRADIENT BUTTON ── */
    .btn-gradient {
      background: linear-gradient(135deg, var(--grad-a-start) 0%, var(--grad-a-end) 100%);
      color: #fff; border: none;
      padding: 15px 0; border-radius: var(--radius-lg);
      font-size: 15px; font-weight: 700;
      font-family: 'DM Sans', sans-serif;
      cursor: pointer; width: 100%;
      letter-spacing: 0.2px;
      box-shadow: 0 6px 24px rgba(113,127,255,0.28);
      transition: opacity 0.15s;
    }
    .btn-gradient:active { opacity: 0.85; }

    /* ── PRIMARY BUTTON ── */
    .btn-primary {
      background: var(--primary);
      color: #fff; border: none;
      padding: 15px 0; border-radius: var(--radius-lg);
      font-size: 15px; font-weight: 700;
      font-family: 'DM Sans', sans-serif;
      cursor: pointer; width: 100%;
      box-shadow: 0 6px 20px rgba(61,81,255,0.3);
      transition: opacity 0.15s;
    }
    .btn-primary:disabled { opacity: 0.35; pointer-events: none; box-shadow: none; }
    .btn-primary:active { opacity: 0.85; }

    /* ── PROGRESS BAR ── */
    .progress-wrap {
      height: 2px; background: rgba(255,255,255,0.07);
      border-radius: 2px; overflow: hidden; margin-bottom: 24px;
    }
    .progress-fill {
      height: 100%; border-radius: 2px;
      background: var(--primary);
      transition: width 0.4s ease;
    }

    /* ── SECTION EYEBROW ── */
    .eyebrow {
      font-size: 10px; letter-spacing: 2px; text-transform: uppercase;
      color: var(--primary-light); font-weight: 600;
    }
  </style>
</head>
<body>
<div id="app">
  <!-- screens injected in subsequent tasks -->
</div>

<script>
  // ── STATE ──
  const state = {
    identity: { year: null, industry: null },
    answers:  [null, null, null],   // Q1, Q2, Q3
    currentMcq: 0,
  };

  // ── SCREEN MANAGER ──
  function showScreen(id) {
    document.querySelectorAll('[data-screen]').forEach(s => s.classList.remove('active'));
    const target = document.querySelector(`[data-screen="${id}"]`);
    if (target) target.classList.add('active');
    window.scrollTo(0, 0);
  }

  // Boot
  document.addEventListener('DOMContentLoaded', () => showScreen('hero'));
</script>
</body>
</html>
```

- [ ] **Step 2: Open `new-grad-aura-check.html` in a browser**

Expected: blank dark page (`#09091A` background), no errors in console.

---

## Task 2: Screen 1 — Hero

**Files:**
- Modify: `new-grad-aura-check.html` — add Screen 1 markup inside `#app` before `</div>`, add Screen 1 CSS inside `<style>`

- [ ] **Step 1: Add Hero CSS inside `<style>` (before closing `</style>`)**

```css
/* ── SCREEN 1: HERO ── */
[data-screen="hero"] {
  position: relative;
  overflow: hidden;
  align-items: center;
  justify-content: center;
  padding: 48px 28px 44px;
  text-align: center;
}

.hero-ring {
  position: absolute; border-radius: 50%;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  pointer-events: none;
}
.hero-ring-1 { width: 160px; height: 160px; border: 1px solid rgba(109,124,255,0.3); }
.hero-ring-2 { width: 300px; height: 300px; border: 1px solid rgba(61,81,255,0.12); }
.hero-glow {
  position: absolute; width: 280px; height: 280px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(61,81,255,0.12) 0%, transparent 70%);
  top: 50%; left: 50%; transform: translate(-50%, -60%);
  pointer-events: none;
}
.hero-center-dot {
  position: absolute; width: 6px; height: 6px; border-radius: 50%;
  background: var(--primary-light);
  top: 50%; left: 50%; transform: translate(-50%, -50%);
  box-shadow: 0 0 12px rgba(109,124,255,0.8);
}
.hero-content { position: relative; z-index: 2; width: 100%; }
.hero-eyebrow { color: var(--primary-light); margin-bottom: 18px; }
.hero-headline {
  font-size: 28px; font-weight: 800; line-height: 1.18;
  color: var(--text-primary); letter-spacing: -0.5px; margin-bottom: 14px;
}
.hero-sub {
  font-size: 13px; color: var(--text-secondary);
  line-height: 1.65; margin-bottom: 28px; max-width: 240px; margin-left: auto; margin-right: auto;
}
.hero-meta { display: flex; gap: 6px; margin-bottom: 32px; justify-content: center; flex-wrap: wrap; }
.hero-meta-pill {
  font-size: 11px; font-weight: 500; color: var(--text-muted);
  background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.08);
  padding: 4px 10px; border-radius: 20px;
}
.hero-privacy { margin-top: 12px; font-size: 10px; color: var(--text-faint); }
```

- [ ] **Step 2: Add Hero HTML inside `#app` (before `<script>`)**

```html
<!-- SCREEN 1: HERO -->
<section data-screen="hero">
  <div class="bg-grid"></div>
  <div class="hero-ring hero-ring-1"></div>
  <div class="hero-ring hero-ring-2"></div>
  <div class="hero-glow"></div>
  <div class="hero-center-dot"></div>
  <div class="status-bar" style="position:absolute;top:0;left:0;right:0;">
    <span class="time">9:41</span><span>●●●</span>
  </div>
  <div class="hero-content">
    <div class="eyebrow hero-eyebrow">New Grad Aura Check</div>
    <h1 class="hero-headline">Decode Your New Grad<br>First Impression</h1>
    <p class="hero-sub">What do recruiters actually see when you walk in the room?</p>
    <div class="hero-meta">
      <span class="hero-meta-pill">60 seconds</span>
      <span class="hero-meta-pill">3 questions</span>
      <span class="hero-meta-pill">1 voice prompt</span>
    </div>
    <button class="btn-gradient" onclick="showScreen('identity')">Decode My Aura →</button>
    <p class="hero-privacy">No signup required · Audio is never stored</p>
  </div>
</section>
```

- [ ] **Step 3: Verify in browser**

Expected: Hero screen visible. Concentric rings visible. "Decode My Aura →" gradient button present. Clicking it does nothing yet (identity screen not built).

---

## Task 3: Screen 2 — Identity Tags

**Files:**
- Modify: `new-grad-aura-check.html`

- [ ] **Step 1: Add Identity CSS inside `<style>`**

```css
/* ── SCREEN 2: IDENTITY ── */
[data-screen="identity"] { padding: 0; }
.id-header { padding: 20px 24px 0; }
.id-step { font-size: 10px; letter-spacing: 2px; text-transform: uppercase; color: var(--text-muted); font-weight: 600; margin-bottom: 10px; }
.id-headline { font-size: 22px; font-weight: 800; color: var(--text-primary); line-height: 1.25; margin-bottom: 6px; }
.id-sub { font-size: 12px; color: rgba(255,255,255,0.4); margin-bottom: 32px; line-height: 1.5; }
.id-section-label {
  font-size: 11px; font-weight: 700; letter-spacing: 1.2px;
  text-transform: uppercase; color: var(--text-muted); margin-bottom: 12px; padding: 0 24px;
}
.tag-group { padding: 0 24px; margin-bottom: 28px; display: flex; flex-wrap: wrap; gap: 8px; }
.tag {
  padding: 9px 16px; border-radius: 10px; font-size: 12px; font-weight: 600;
  border: 1px solid rgba(255,255,255,0.10); color: rgba(255,255,255,0.45);
  background: rgba(255,255,255,0.03); cursor: pointer;
  transition: background 0.15s, border-color 0.15s, color 0.15s;
  -webkit-tap-highlight-color: transparent;
}
.tag.sel {
  background: var(--primary-alpha12);
  border-color: var(--primary-alpha45);
  color: var(--selected-text);
}
.id-footer { padding: 0 24px 32px; margin-top: auto; }
.id-hint { text-align: center; margin-top: 12px; font-size: 10px; color: var(--text-faint); }
```

- [ ] **Step 2: Add Identity HTML inside `#app` (after Hero section, before `<script>`)**

```html
<!-- SCREEN 2: IDENTITY TAGS -->
<section data-screen="identity">
  <div class="status-bar"><span class="time">9:41</span><span>●●●</span></div>
  <div class="id-header">
    <div class="progress-wrap"><div class="progress-fill" style="width:10%"></div></div>
    <div class="id-step">Step 1 of 5</div>
    <h2 class="id-headline">Tell us about<br>yourself</h2>
    <p class="id-sub">We'll personalize your questions based on your profile.</p>
  </div>
  <div class="id-section-label">I am a...</div>
  <div class="tag-group" id="tg-year">
    <button class="tag" onclick="selectTag('year','sophomore', this)">Sophomore / Junior</button>
    <button class="tag" onclick="selectTag('year','senior', this)">Senior</button>
    <button class="tag" onclick="selectTag('year','recent', this)">Recent Grad</button>
  </div>
  <div class="id-section-label">Target Industry</div>
  <div class="tag-group" id="tg-industry">
    <button class="tag" onclick="selectTag('industry','tech', this)">Tech / Engineering</button>
    <button class="tag" onclick="selectTag('industry','finance', this)">Finance / Consulting</button>
    <button class="tag" onclick="selectTag('industry','marketing', this)">Marketing / Sales</button>
    <button class="tag" onclick="selectTag('industry','other', this)">Other</button>
  </div>
  <div class="id-footer">
    <button class="btn-primary" id="btn-continue" disabled onclick="showScreen('mcq')">Continue →</button>
    <p class="id-hint">Questions adapt based on your selections</p>
  </div>
</section>
```

- [ ] **Step 3: Add `selectTag` function inside `<script>` (before closing `</script>`)**

```js
function selectTag(group, value, el) {
  // deselect siblings
  el.closest('.tag-group').querySelectorAll('.tag').forEach(t => t.classList.remove('sel'));
  el.classList.add('sel');
  state.identity[group] = value;
  // unlock Continue when both groups selected
  const btn = document.getElementById('btn-continue');
  if (state.identity.year && state.identity.industry) btn.disabled = false;
}
```

- [ ] **Step 4: Verify in browser**

Expected: Tap Hero CTA → Identity screen slides in. Tag groups selectable (blue highlight). Continue button disabled until both groups have a selection. Tapping Continue does nothing yet (MCQ screen not built).

---

## Task 4: Screens 3–5 — MCQ Template

**Files:**
- Modify: `new-grad-aura-check.html`

- [ ] **Step 1: Add MCQ CSS inside `<style>`**

```css
/* ── SCREENS 3-5: MCQ ── */
[data-screen="mcq"] { padding: 20px 0 0; }
.mcq-header { padding: 0 24px; margin-bottom: 24px; }
.mcq-progress-row { display: flex; align-items: center; gap: 10px; margin-bottom: 20px; }
.mcq-progress-row .progress-wrap { flex: 1; margin-bottom: 0; }
.mcq-frac { font-size: 11px; font-weight: 600; color: var(--text-muted); white-space: nowrap; flex-shrink: 0; }
.mcq-label { font-size: 10px; letter-spacing: 2px; text-transform: uppercase; color: var(--text-muted); font-weight: 600; margin-bottom: 10px; }
.mcq-question { font-size: 17px; font-weight: 700; color: var(--text-primary); line-height: 1.4; }
.mcq-question em { font-style: normal; color: var(--text-secondary); font-weight: 500; font-size: 15px; }
.mcq-options { padding: 0 24px; display: flex; flex-direction: column; gap: 10px; flex: 1; }
.mcq-opt {
  display: flex; align-items: flex-start; gap: 14px;
  padding: 14px 16px;
  border: 1px solid var(--border-subtle);
  border-radius: var(--radius-md);
  background: rgba(255,255,255,0.02);
  cursor: pointer;
  transition: background 0.15s, border-color 0.15s, opacity 0.2s;
  -webkit-tap-highlight-color: transparent;
  text-align: left; width: 100%;
}
.mcq-opt.selected { border-color: var(--primary); background: var(--primary-alpha12); }
.mcq-opt.dim      { opacity: 0.35; }
.opt-letter {
  width: 28px; height: 28px; border-radius: var(--radius-sm); flex-shrink: 0; margin-top: 1px;
  border: 1px solid rgba(255,255,255,0.15);
  display: flex; align-items: center; justify-content: center;
  font-size: 12px; font-weight: 800; color: rgba(255,255,255,0.4);
  transition: background 0.15s, border-color 0.15s, color 0.15s;
}
.mcq-opt.selected .opt-letter { background: var(--primary); border-color: var(--primary); color: #fff; }
.opt-text { font-size: 13px; color: rgba(255,255,255,0.65); line-height: 1.5; padding-top: 4px; }
.mcq-opt.selected .opt-text { color: var(--text-primary); }
.mcq-recorded {
  margin: 16px 24px 4px;
  font-size: 11px; font-weight: 600; color: rgba(61,81,255,0.85);
  min-height: 18px;
}
```

- [ ] **Step 2: Add MCQ data + `renderMcq` function inside `<script>`**

```js
const MCQ_DATA = [
  {
    progress: '33%',
    frac: '1 / 3',
    q: 'Group project due tomorrow. One teammate ghosted and did zero work.',
    opts: [
      'Grind it out solo, submit, and then talk to the professor later.',
      'Tag them in the group chat with a specific list of what\'s missing.',
      'Email the TA/Professor privately to flag the issue before the deadline.',
    ],
  },
  {
    progress: '66%',
    frac: '2 / 3',
    q: 'Career Fair line for your dream company is 45 mins long.',
    opts: [
      'Hit a smaller booth nearby to warm up, then circle back when the line dies down.',
      'Stay in line and eavesdrop on the conversations ahead of you.',
      'Grab a business card, skip the line, and send a killer cold email tonight.',
    ],
  },
  {
    progress: '100%',
    frac: '3 / 3',
    q: 'Recruiter kicks off with "Walk me through your resume." Your brain defaults to:',
    opts: [
      'Chronological order: "So freshman year I took CS101..."',
      'Picking 3 bullet points that match the job description exactly.',
      'Opening with a quick anecdote that defines my work style.',
    ],
  },
];

function renderMcq(index) {
  state.currentMcq = index;
  const d = MCQ_DATA[index];
  const screen = document.querySelector('[data-screen="mcq"]');

  screen.querySelector('.progress-fill').style.width = d.progress;
  screen.querySelector('.mcq-frac').textContent = d.frac;
  screen.querySelector('.mcq-question').innerHTML =
    d.q + ' <em>You...</em>';
  screen.querySelector('.mcq-recorded').textContent = '';

  const container = screen.querySelector('.mcq-options');
  container.innerHTML = '';
  d.opts.forEach((text, i) => {
    const btn = document.createElement('button');
    btn.className = 'mcq-opt';
    btn.innerHTML = `<span class="opt-letter">${String.fromCharCode(65+i)}</span>
                     <span class="opt-text">${text}</span>`;
    btn.addEventListener('click', () => selectMcqAnswer(index, i));
    container.appendChild(btn);
  });
}

function selectMcqAnswer(qIndex, optIndex) {
  state.answers[qIndex] = optIndex;
  const opts = document.querySelectorAll('[data-screen="mcq"] .mcq-opt');
  opts.forEach((el, i) => {
    el.classList.toggle('selected', i === optIndex);
    el.classList.toggle('dim', i !== optIndex);
  });
  document.querySelector('[data-screen="mcq"] .mcq-recorded').textContent =
    '✓  Recorded — moving to next question...';

  setTimeout(() => {
    if (qIndex < 2) {
      renderMcq(qIndex + 1);
    } else {
      showScreen('voice');
    }
  }, 600);
}
```

- [ ] **Step 3: Update `showScreen` to call `renderMcq(0)` when entering MCQ screen**

Replace the existing `showScreen` function:
```js
function showScreen(id) {
  document.querySelectorAll('[data-screen]').forEach(s => s.classList.remove('active'));
  const target = document.querySelector(`[data-screen="${id}"]`);
  if (target) target.classList.add('active');
  window.scrollTo(0, 0);
  if (id === 'mcq') renderMcq(0);
}
```

- [ ] **Step 4: Add MCQ HTML inside `#app` (after Identity section)**

```html
<!-- SCREENS 3-5: MCQ (single template, content swapped by JS) -->
<section data-screen="mcq">
  <div class="status-bar"><span class="time">9:41</span><span>●●●</span></div>
  <div class="mcq-header">
    <div class="mcq-progress-row">
      <div class="progress-wrap"><div class="progress-fill" style="width:33%"></div></div>
      <span class="mcq-frac">1 / 3</span>
    </div>
    <div class="mcq-label">Situational Question</div>
    <p class="mcq-question"></p>
  </div>
  <div class="mcq-options"></div>
  <p class="mcq-recorded"></p>
</section>
```

- [ ] **Step 5: Verify in browser**

Expected: Identity Continue → MCQ screen shows Q1. Selecting an option highlights it (blue), dims others, shows "✓ Recorded" note, then auto-advances to Q2 after 600ms. Q2 → Q3 same behaviour. After Q3 selection, nothing yet (voice screen not built).

---

## Task 5: Screen 6 — Voice Prompt

**Files:**
- Modify: `new-grad-aura-check.html`

- [ ] **Step 1: Add Voice CSS inside `<style>`**

```css
/* ── SCREEN 6: VOICE ── */
[data-screen="voice"] { padding: 20px 0 0; }
.voice-header { padding: 0 24px; }
.voice-step  { font-size: 10px; letter-spacing: 2px; text-transform: uppercase; color: var(--text-muted); font-weight: 600; margin-bottom: 28px; }
.voice-headline { font-size: 20px; font-weight: 800; color: var(--text-primary); line-height: 1.25; margin-bottom: 8px; }
.voice-sub  { font-size: 13px; color: var(--text-secondary); line-height: 1.6; margin-bottom: 20px; }
.prompt-card {
  margin: 0 24px 20px;
  background: var(--bg-surface); border: 1px solid rgba(61,81,255,0.2);
  border-radius: var(--radius-xl); padding: 16px 18px;
}
.prompt-card-label { font-size: 10px; letter-spacing: 1.5px; text-transform: uppercase; color: var(--primary-light); font-weight: 700; margin-bottom: 8px; }
.prompt-card-text { font-size: 13px; color: rgba(255,255,255,0.75); line-height: 1.6; font-style: italic; }
.prompt-card-text strong { font-style: normal; color: var(--text-primary); font-weight: 700; }
.waveform-area {
  margin: 0 24px 20px;
  background: rgba(255,255,255,0.02); border: 1px solid var(--border-subtle);
  border-radius: var(--radius-xl); padding: 16px;
  display: flex; flex-direction: column; align-items: center; gap: 12px;
}
.waveform { display: flex; align-items: center; justify-content: center; gap: 3px; height: 40px; width: 100%; }
.wave-bar { width: 3px; border-radius: 3px; background: var(--primary); opacity: 0.7; transition: height 0.1s; }
.wave-bar.animating { animation: wavePulse 0.6s ease-in-out infinite alternate; }
@keyframes wavePulse {
  from { opacity: 0.4; transform: scaleY(0.4); }
  to   { opacity: 0.9; transform: scaleY(1.0); }
}
.voice-timer { font-size: 13px; font-weight: 700; color: var(--text-muted); letter-spacing: 1px; }
.voice-timer span { color: var(--text-primary); }
.record-area { display: flex; flex-direction: column; align-items: center; gap: 10px; padding: 0 24px 20px; margin-top: auto; }
.record-btn {
  width: 64px; height: 64px; border-radius: 50%;
  background: var(--error); border: none; cursor: pointer;
  display: flex; align-items: center; justify-content: center;
  box-shadow: 0 0 0 8px rgba(255,92,92,0.12), 0 4px 20px rgba(255,92,92,0.3);
  transition: transform 0.15s;
}
.record-btn:active { transform: scale(0.93); }
.record-btn .rec-icon { width: 22px; height: 22px; border-radius: 6px; background: #fff; transition: border-radius 0.2s; }
.record-btn.recording .rec-icon { border-radius: 50%; width: 18px; height: 18px; }
.record-hint { font-size: 11px; color: var(--text-muted); }
.skip-link { text-align: center; padding-bottom: 24px; font-size: 11px; color: var(--text-faint); text-decoration: underline; cursor: pointer; }
```

- [ ] **Step 2: Add Voice HTML inside `#app` (after MCQ section)**

```html
<!-- SCREEN 6: VOICE PROMPT -->
<section data-screen="voice">
  <div class="status-bar"><span class="time">9:41</span><span>●●●</span></div>
  <div class="voice-header">
    <div class="mcq-progress-row">
      <div class="progress-wrap"><div class="progress-fill" style="width:85%"></div></div>
      <span class="mcq-frac" style="font-size:11px;font-weight:600;color:var(--text-muted)">Voice</span>
    </div>
    <div class="voice-step">Step 4 of 5</div>
    <h2 class="voice-headline">Let's hear<br>how you sound.</h2>
    <p class="voice-sub">Answer in one or two sentences.</p>
  </div>
  <div class="prompt-card">
    <div class="prompt-card-label">Your Prompt</div>
    <p class="prompt-card-text"><strong>Describe yourself in three words</strong> — and tell us why.</p>
  </div>
  <div class="waveform-area">
    <div class="waveform" id="waveform">
      <!-- bars injected by JS -->
    </div>
    <div class="voice-timer">0:00 / <span>0:30</span></div>
  </div>
  <div class="record-area">
    <button class="record-btn" id="record-btn" onclick="toggleRecord()">
      <div class="rec-icon"></div>
    </button>
    <p class="record-hint" id="record-hint">Tap to start recording</p>
  </div>
  <p class="skip-link" onclick="showScreen('loader')">Skip voice analysis →</p>
</section>
```

- [ ] **Step 3: Add voice waveform + recording logic inside `<script>`**

```js
// ── VOICE ──
let recordTimer = null;
let recordSeconds = 0;

function initWaveform() {
  const heights = [8,20,32,16,36,24,12,30,38,20,14,28,10,22,34,18,8];
  const wf = document.getElementById('waveform');
  wf.innerHTML = '';
  heights.forEach(h => {
    const bar = document.createElement('div');
    bar.className = 'wave-bar';
    bar.style.height = h + 'px';
    wf.appendChild(bar);
  });
}

function toggleRecord() {
  const btn = document.getElementById('record-btn');
  const hint = document.getElementById('record-hint');
  const bars = document.querySelectorAll('#waveform .wave-bar');
  const timerEl = document.querySelector('[data-screen="voice"] .voice-timer');

  if (!btn.classList.contains('recording')) {
    // START
    btn.classList.add('recording');
    hint.textContent = 'Recording... tap to stop';
    bars.forEach((b, i) => {
      b.style.animationDelay = (i * 0.04) + 's';
      b.classList.add('animating');
    });
    recordSeconds = 0;
    recordTimer = setInterval(() => {
      recordSeconds++;
      const s = String(recordSeconds % 60).padStart(2, '0');
      const m = String(Math.floor(recordSeconds / 60)).padStart(2, '0');
      timerEl.innerHTML = `${m}:${s} / <span>0:30</span>`;
      if (recordSeconds >= 30) stopRecord();
    }, 1000);
  } else {
    stopRecord();
  }
}

function stopRecord() {
  const btn = document.getElementById('record-btn');
  const hint = document.getElementById('record-hint');
  const bars = document.querySelectorAll('#waveform .wave-bar');
  clearInterval(recordTimer);
  btn.classList.remove('recording');
  hint.textContent = 'Tap to start recording';
  bars.forEach(b => b.classList.remove('animating'));
  showScreen('loader');
}

// initialise waveform bars when voice screen first shown
const _origShowScreen = showScreen;
// (handled in showScreen extension below)
```

- [ ] **Step 4: Update `showScreen` to init waveform when entering voice screen**

Replace the `showScreen` function with:
```js
function showScreen(id) {
  document.querySelectorAll('[data-screen]').forEach(s => s.classList.remove('active'));
  const target = document.querySelector(`[data-screen="${id}"]`);
  if (target) target.classList.add('active');
  window.scrollTo(0, 0);
  if (id === 'mcq')   renderMcq(0);
  if (id === 'voice') initWaveform();
}
```

- [ ] **Step 5: Remove the `_origShowScreen` line added in Step 3** (it was a placeholder comment only — delete that line)

- [ ] **Step 6: Verify in browser**

Expected: After Q3 answer → Voice screen. Waveform bars visible. Tapping record button: bars animate, timer counts up. Tapping stop: animation stops, advances to loader (not built yet). Skip link also advances to loader.

---

## Task 6: Screen 7 — Analysis Loader

**Files:**
- Modify: `new-grad-aura-check.html`

- [ ] **Step 1: Add Loader CSS inside `<style>`**

```css
/* ── SCREEN 7: LOADER ── */
[data-screen="loader"] {
  position: relative; overflow: hidden;
  align-items: center; justify-content: center;
  padding: 32px 28px; text-align: center;
}
.loader-radar { position: relative; width: 160px; height: 160px; margin-bottom: 32px; }
.radar-ring {
  position: absolute; border-radius: 50%;
  top: 50%; left: 50%; transform: translate(-50%, -50%);
}
.radar-ring-1 { width: 60px;  height: 60px;  border: 1px solid rgba(61,81,255,0.25); }
.radar-ring-2 { width: 100px; height: 100px; border: 1px solid rgba(61,81,255,0.18); }
.radar-ring-3 { width: 140px; height: 140px; border: 1px solid rgba(61,81,255,0.12); }
.radar-axis {
  position: absolute; height: 1px; background: rgba(61,81,255,0.12);
  top: 50%; left: 50%; transform-origin: 0% 50%;
}
.radar-sweep {
  position: absolute; width: 50%; height: 1px;
  background: linear-gradient(90deg, transparent, var(--primary));
  top: 50%; left: 50%; transform-origin: 0% 50%;
  animation: sweep 2s linear infinite;
}
.radar-sweep::after {
  content: ''; position: absolute; right: 0; top: -2px;
  width: 5px; height: 5px; border-radius: 50%;
  background: var(--primary); box-shadow: 0 0 8px rgba(61,81,255,0.8);
}
@keyframes sweep { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
.loader-label { margin-bottom: 14px; }
.loader-copy { font-size: 15px; font-weight: 700; color: var(--text-primary); line-height: 1.5; max-width: 260px; min-height: 48px; }
.loader-dots { display: flex; gap: 6px; justify-content: center; margin-top: 16px; }
.loader-dot { width: 6px; height: 6px; border-radius: 50%; background: rgba(61,81,255,0.25); transition: background 0.3s; }
.loader-dot.active { background: var(--primary); }
```

- [ ] **Step 2: Add Loader HTML inside `#app` (after Voice section)**

```html
<!-- SCREEN 7: ANALYSIS LOADER -->
<section data-screen="loader">
  <div class="bg-grid"></div>
  <div class="status-bar" style="position:absolute;top:0;left:0;right:0;"><span class="time">9:41</span><span>●●●</span></div>
  <div class="loader-radar">
    <div class="radar-ring radar-ring-1"></div>
    <div class="radar-ring radar-ring-2"></div>
    <div class="radar-ring radar-ring-3"></div>
    <!-- 5 axes at 72° each -->
    <div class="radar-axis" style="width:80px; transform:rotate(0deg)"></div>
    <div class="radar-axis" style="width:80px; transform:rotate(72deg)"></div>
    <div class="radar-axis" style="width:80px; transform:rotate(144deg)"></div>
    <div class="radar-axis" style="width:80px; transform:rotate(216deg)"></div>
    <div class="radar-axis" style="width:80px; transform:rotate(288deg)"></div>
    <div class="radar-sweep"></div>
  </div>
  <div>
    <div class="eyebrow loader-label">Analyzing Profile</div>
    <p class="loader-copy" id="loader-copy">Benchmarking against 45,000+ New Grad JDs...</p>
    <div class="loader-dots">
      <div class="loader-dot active" id="ldot-0"></div>
      <div class="loader-dot" id="ldot-1"></div>
      <div class="loader-dot" id="ldot-2"></div>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Add loader rotation logic inside `<script>`**

```js
// ── LOADER ──
const LOADER_COPY = [
  'Benchmarking against 45,000+ New Grad JDs...',
  'Generating your personalized 2026 recruiting questions...',
  'Analyzing speech patterns and filler words...',
];

let loaderInterval = null;

function startLoader() {
  let step = 0;
  document.getElementById('loader-copy').textContent = LOADER_COPY[0];
  [0,1,2].forEach(i => document.getElementById('ldot-'+i).classList.toggle('active', i === 0));

  loaderInterval = setInterval(() => {
    step++;
    if (step < LOADER_COPY.length) {
      document.getElementById('loader-copy').textContent = LOADER_COPY[step];
      [0,1,2].forEach(i => document.getElementById('ldot-'+i).classList.toggle('active', i === step));
    }
  }, 1200);

  setTimeout(() => {
    clearInterval(loaderInterval);
    showScreen('results');
  }, 3600);
}
```

- [ ] **Step 4: Update `showScreen` to call `startLoader()` when entering loader screen**

```js
function showScreen(id) {
  document.querySelectorAll('[data-screen]').forEach(s => s.classList.remove('active'));
  const target = document.querySelector(`[data-screen="${id}"]`);
  if (target) target.classList.add('active');
  window.scrollTo(0, 0);
  if (id === 'mcq')    renderMcq(0);
  if (id === 'voice')  initWaveform();
  if (id === 'loader') startLoader();
}
```

- [ ] **Step 5: Verify in browser**

Expected: After recording stop (or skip) → Loader screen. Radar sweep arm animates continuously. Copy cycles through 3 messages, dot indicator updates. After 3.6s, auto-advances to results (not built yet).

---

## Task 7: Screen 8 — Results: Persona + Radar + Voice Analysis

**Files:**
- Modify: `new-grad-aura-check.html`

- [ ] **Step 1: Add Results CSS inside `<style>`**

```css
/* ── SCREEN 8: RESULTS ── */
[data-screen="results"] { padding: 0; overflow-y: auto; }
.results-header { padding: 20px 24px 16px; border-bottom: 1px solid rgba(255,255,255,0.05); }
.results-step  { font-size: 10px; letter-spacing: 2px; text-transform: uppercase; color: var(--text-muted); font-weight: 600; margin-bottom: 4px; }
.results-title { font-size: 18px; font-weight: 800; color: var(--text-primary); }

.persona-card {
  margin: 16px 24px;
  background: linear-gradient(135deg, #1a1a3e 0%, var(--bg-surface) 100%);
  border: 1px solid rgba(61,81,255,0.25); border-radius: var(--radius-xl);
  padding: 20px; position: relative; overflow: hidden;
}
.persona-card::before {
  content: ''; position: absolute; top: -30px; right: -30px;
  width: 100px; height: 100px; border-radius: 50%;
  background: radial-gradient(circle, rgba(61,81,255,0.15) 0%, transparent 70%);
}
.persona-emoji { font-size: 28px; margin-bottom: 10px; display: block; }
.persona-type-label { margin-bottom: 6px; }
.persona-name { font-size: 20px; font-weight: 800; color: var(--text-primary); margin-bottom: 10px; }
.persona-verdict {
  font-size: 12px; color: var(--text-secondary); line-height: 1.6;
  border-top: 1px solid rgba(255,255,255,0.07); padding-top: 12px;
}

.results-section { margin: 0 24px 16px; }
.results-section-label {
  font-size: 10px; letter-spacing: 2px; text-transform: uppercase;
  color: var(--text-muted); font-weight: 700; margin-bottom: 12px;
}
.radar-chart-wrap {
  background: var(--bg-surface); border: 1px solid var(--border-subtle);
  border-radius: var(--radius-xl); padding: 16px;
  display: flex; justify-content: center;
}

.voice-tags { display: flex; flex-direction: column; gap: 8px; }
.voice-tag {
  display: flex; align-items: center; justify-content: space-between;
  background: var(--bg-surface); border: 1px solid var(--border-subtle);
  border-radius: var(--radius-md); padding: 12px 16px;
}
.vtag-left  { display: flex; align-items: center; gap: 10px; }
.vtag-icon  {
  width: 32px; height: 32px; border-radius: var(--radius-sm);
  background: var(--primary-alpha12);
  display: flex; align-items: center; justify-content: center; font-size: 15px; flex-shrink: 0;
}
.vtag-name  { font-size: 12px; font-weight: 700; color: var(--text-muted); }
.vtag-sub   { font-size: 10px; color: rgba(255,255,255,0.25); margin-top: 2px; }
.vtag-value { font-size: 13px; font-weight: 700; color: var(--text-primary); text-align: right; max-width: 100px; font-size: 12px; }

.results-next-btn { padding: 0 24px 32px; }
```

- [ ] **Step 2: Add Results HTML inside `#app` (after Loader section)**

```html
<!-- SCREEN 8: RESULTS — PERSONA + RADAR + VOICE -->
<section data-screen="results">
  <div class="status-bar"><span class="time">9:41</span><span>●●●</span></div>
  <div class="results-header">
    <div class="results-step">Your Report</div>
    <div class="results-title">New Grad Aura Report</div>
  </div>

  <!-- Persona Card -->
  <div class="persona-card">
    <span class="persona-emoji">⚡</span>
    <div class="eyebrow persona-type-label">Your Persona</div>
    <div class="persona-name">The High-Potential Grinder</div>
    <p class="persona-verdict">You come across as driven and results-focused. Pro tip: Slow down — you risk sounding scripted under pressure.</p>
  </div>

  <!-- Radar Chart -->
  <div class="results-section">
    <div class="results-section-label">Soft Skills Radar</div>
    <div class="radar-chart-wrap">
      <svg width="220" height="210" viewBox="-15 -15 250 240" style="overflow:visible">
        <!-- Pentagon grid rings (3 rings at 100%, 66%, 33% scale) -->
        <!-- Ring 3 (outer) -->
        <polygon points="110,10 203,68 168,175 52,175 17,68"
          fill="none" stroke="rgba(255,255,255,0.06)" stroke-width="1"/>
        <!-- Ring 2 -->
        <polygon points="110,43 170,82 145,158 75,158 50,82"
          fill="none" stroke="rgba(255,255,255,0.06)" stroke-width="1"/>
        <!-- Ring 1 (inner) -->
        <polygon points="110,76 137,97 122,141 98,141 83,97"
          fill="none" stroke="rgba(255,255,255,0.06)" stroke-width="1"/>
        <!-- Axes from center (110,100) to each outer vertex -->
        <line x1="110" y1="100" x2="110" y2="10"  stroke="rgba(255,255,255,0.08)" stroke-width="1"/>
        <line x1="110" y1="100" x2="203" y2="68"  stroke="rgba(255,255,255,0.08)" stroke-width="1"/>
        <line x1="110" y1="100" x2="168" y2="175" stroke="rgba(255,255,255,0.08)" stroke-width="1"/>
        <line x1="110" y1="100" x2="52"  y2="175" stroke="rgba(255,255,255,0.08)" stroke-width="1"/>
        <line x1="110" y1="100" x2="17"  y2="68"  stroke="rgba(255,255,255,0.08)" stroke-width="1"/>
        <!--
          Scores: Communication 80%, Execution 90%, Grit 75%, Learning Agility 65%, Collaboration 70%
          Each vertex = center + score * (outer - center)
          Center = (110, 100)
          Top    (110,10):  80% → (110, 100 - 72) = (110, 28)
          TR     (203,68):  90% → (110+(84.7*.9), 100-(28.8*.9)) = (186, 74)  — approx
          BR     (168,175): 75% → (110+(51.75), 100+(56.25)) = (162, 156)
          BL     (52,175):  65% → (110-(37.7), 100+(48.75)) = (72, 149)
          TL     (17,68):   70% → (110-(64.4), 100-(22.4)) = (45, 78)
        -->
        <polygon
          points="110,28 186,74 162,156 72,149 45,78"
          fill="rgba(61,81,255,0.18)" stroke="#3D51FF" stroke-width="1.5"/>
        <circle cx="110" cy="28"  r="3.5" fill="#3D51FF"/>
        <circle cx="186" cy="74"  r="3.5" fill="#3D51FF"/>
        <circle cx="162" cy="156" r="3.5" fill="#3D51FF"/>
        <circle cx="72"  cy="149" r="3.5" fill="#3D51FF"/>
        <circle cx="45"  cy="78"  r="3.5" fill="#3D51FF"/>
        <!-- Axis labels -->
        <text x="110" y="4"   text-anchor="middle" fill="rgba(255,255,255,0.45)" font-size="9" font-family="DM Sans,sans-serif">Communication</text>
        <text x="215" y="66"  text-anchor="start"  fill="rgba(255,255,255,0.45)" font-size="9" font-family="DM Sans,sans-serif">Execution</text>
        <text x="178" y="190" text-anchor="middle" fill="rgba(255,255,255,0.45)" font-size="9" font-family="DM Sans,sans-serif">Grit</text>
        <text x="42"  y="190" text-anchor="middle" fill="rgba(255,255,255,0.45)" font-size="9" font-family="DM Sans,sans-serif">Learning</text>
        <text x="4"   y="66"  text-anchor="end"    fill="rgba(255,255,255,0.45)" font-size="9" font-family="DM Sans,sans-serif">Collab</text>
      </svg>
    </div>
  </div>

  <!-- Voice Analysis -->
  <div class="results-section">
    <div class="results-section-label">Voice Analysis</div>
    <div class="voice-tags">
      <div class="voice-tag">
        <div class="vtag-left">
          <div class="vtag-icon">🗣️</div>
          <div>
            <div class="vtag-name">Speaking Pace</div>
            <div class="vtag-sub">Slightly fast — try pausing after key points</div>
          </div>
        </div>
        <div class="vtag-value">146 WPM</div>
      </div>
      <div class="voice-tag">
        <div class="vtag-left">
          <div class="vtag-icon">🎯</div>
          <div>
            <div class="vtag-name">Top Keywords</div>
            <div class="vtag-sub">Detected in your response</div>
          </div>
        </div>
        <div class="vtag-value">Project · Lead · Team</div>
      </div>
      <div class="voice-tag">
        <div class="vtag-left">
          <div class="vtag-icon">😌</div>
          <div>
            <div class="vtag-name">Confidence Score</div>
            <div class="vtag-sub">Filler words lowered score slightly</div>
          </div>
        </div>
        <div class="vtag-value">75%</div>
      </div>
    </div>
  </div>

  <div class="results-next-btn">
    <button class="btn-primary" onclick="showScreen('unlock')">See Your Interview Questions →</button>
  </div>
</section>
```

- [ ] **Step 3: Verify in browser**

Expected: After loader → Results screen. Persona card visible with emoji, name, verdict. Pentagon radar chart renders with blue fill and dots. Three voice tag rows show values. "See Your Interview Questions →" button present.

---

## Task 8: Screen 9 — Locked → Unlocked State

**Files:**
- Modify: `new-grad-aura-check.html`

- [ ] **Step 1: Add Unlock CSS inside `<style>`**

```css
/* ── SCREEN 9: UNLOCK ── */
[data-screen="unlock"] { padding: 0; }
.unlock-header { padding: 20px 24px 16px; display: flex; align-items: center; gap: 12px; }
.back-btn {
  width: 32px; height: 32px; border-radius: 50%;
  background: rgba(255,255,255,0.05); border: none; cursor: pointer;
  display: flex; align-items: center; justify-content: center;
  color: var(--text-muted); font-size: 14px; flex-shrink: 0;
  -webkit-tap-highlight-color: transparent;
}
.unlock-title { font-size: 16px; font-weight: 800; color: var(--text-primary); }
.unlock-intro { padding: 0 24px 16px; font-size: 13px; color: var(--text-secondary); line-height: 1.6; }
.unlock-intro strong { color: var(--text-primary); }

.locked-cards-wrap { padding: 0 24px; position: relative; margin-bottom: 20px; }
.locked-q {
  background: var(--bg-surface); border: 1px solid var(--border-subtle);
  border-radius: var(--radius-md); padding: 14px 16px; margin-bottom: 8px;
  transition: filter 0.4s ease;
}
.locked-q.blurred { filter: blur(4px); user-select: none; }
.locked-q-num { font-size: 10px; color: var(--primary-light); font-weight: 700; margin-bottom: 4px; }
.locked-q-text { font-size: 13px; color: rgba(255,255,255,0.7); line-height: 1.5; }

.lock-overlay {
  position: absolute; inset: 0;
  background: linear-gradient(to bottom, rgba(9,9,26,0.1) 0%, rgba(9,9,26,0.82) 100%);
  border-radius: var(--radius-md);
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  gap: 8px; transition: opacity 0.4s ease;
}
.lock-overlay.hidden { opacity: 0; pointer-events: none; }
.lock-icon-lg { font-size: 24px; }
.lock-msg { font-size: 12px; font-weight: 600; color: rgba(255,255,255,0.6); text-align: center; max-width: 180px; line-height: 1.4; }

.share-section { padding: 0 24px 16px; }
.secondary-cta {
  display: block; text-align: center; margin-top: 12px;
  font-size: 12px; color: var(--text-muted); text-decoration: underline; cursor: pointer;
  background: none; border: none; width: 100%; font-family: 'DM Sans', sans-serif;
}

.ai-preview-card {
  margin: 0 24px 16px;
  background: var(--bg-surface); border: 1px solid rgba(61,81,255,0.2);
  border-radius: var(--radius-xl); padding: 16px 18px;
  display: none;
}
.ai-preview-card.visible { display: block; }
.ai-preview-label { font-size: 10px; letter-spacing: 1.5px; text-transform: uppercase; color: var(--primary-light); font-weight: 700; margin-bottom: 8px; }
.ai-preview-text { font-size: 12px; color: var(--text-secondary); line-height: 1.6; }

.quiz-footer { padding: 12px 24px 28px; text-align: center; font-size: 10px; color: rgba(255,255,255,0.18); line-height: 1.6; }
```

- [ ] **Step 2: Add Unlock HTML inside `#app` (after Results section)**

```html
<!-- SCREEN 9: LOCKED → UNLOCKED -->
<section data-screen="unlock">
  <div class="status-bar"><span class="time">9:41</span><span>●●●</span></div>
  <div class="unlock-header">
    <button class="back-btn" onclick="showScreen('results')">←</button>
    <div class="unlock-title">Your Interview Questions</div>
  </div>
  <p class="unlock-intro">
    Based on your aura profile, MYLS generated <strong>3 high-probability interview questions</strong> tailored to you.
  </p>

  <div class="locked-cards-wrap">
    <div class="locked-q blurred" id="lq-1">
      <div class="locked-q-num">Q1</div>
      <p class="locked-q-text">Tell me about a time you faced a conflict on a team and how you resolved it.</p>
    </div>
    <div class="locked-q blurred" id="lq-2">
      <div class="locked-q-num">Q2</div>
      <p class="locked-q-text">Why do you think you're a strong fit for this role given your background?</p>
    </div>
    <div class="locked-q blurred" id="lq-3">
      <div class="locked-q-num">Q3</div>
      <p class="locked-q-text">If we offered you the internship today, what would be your first priority in week one?</p>
    </div>
    <div class="lock-overlay" id="lock-overlay">
      <div class="lock-icon-lg">🔒</div>
      <p class="lock-msg">Share with a friend to unlock your questions</p>
    </div>
  </div>

  <div class="share-section">
    <button class="btn-gradient" id="share-btn" onclick="handleShare()">Share with a Friend to Unlock</button>
    <button class="secondary-cta" onclick="window.open('#','_blank')">Want to practice this on video? Try MYLS →</button>
  </div>

  <!-- Shown post-unlock -->
  <div class="ai-preview-card" id="ai-preview">
    <div class="ai-preview-label">AI Video Review</div>
    <p class="ai-preview-text">Register for MYLS 3.0 to record your video responses and receive dynamic scoring on Communication, Execution, and Grit.</p>
  </div>

  <div class="quiz-footer">
    Powered by MYLS Interview · <span style="text-decoration:underline;cursor:pointer">Privacy Policy</span><br>
    Audio processed in real-time and immediately discarded. Nothing is stored.
  </div>
</section>
```

- [ ] **Step 3: Add share + unlock logic inside `<script>`**

```js
// ── SHARE + UNLOCK ──
function handleShare() {
  const shareData = {
    title: 'New Grad Aura Check',
    text: 'Apparently I\'m "The High-Potential Grinder"... time to work on my edge. What\'s your New Grad Aura? 👀',
    url: window.location.href,
  };
  if (navigator.share) {
    navigator.share(shareData)
      .then(() => unlockContent())
      .catch(() => unlockContent()); // unlock even if user dismisses
  } else {
    // Fallback: copy link then unlock
    navigator.clipboard?.writeText(window.location.href).catch(() => {});
    unlockContent();
  }
}

function unlockContent() {
  ['lq-1','lq-2','lq-3'].forEach(id => {
    document.getElementById(id).classList.remove('blurred');
  });
  const overlay = document.getElementById('lock-overlay');
  overlay.classList.add('hidden');
  const preview = document.getElementById('ai-preview');
  preview.classList.add('visible');
  const shareBtn = document.getElementById('share-btn');
  shareBtn.textContent = '✓ Unlocked — Questions Revealed';
  shareBtn.disabled = true;
  shareBtn.style.opacity = '0.5';
}
```

- [ ] **Step 4: Verify in browser — full flow**

Walk through the complete prototype:
1. Hero → tap "Decode My Aura →" → Identity Tags
2. Select year + industry → Continue → Q1
3. Select any MCQ answer → auto-advances Q1→Q2→Q3 → Voice
4. Tap record → bars animate → tap stop → Loader
5. Loader rotates copy for 3.6s → Results
6. Scroll results: persona card, radar, voice tags → tap "See Your Interview Questions →" → Unlock
7. Tap "Share with a Friend to Unlock" → share sheet appears (or clipboard fallback) → questions unblur, AI preview appears

Expected: Full flow works end-to-end with no JS errors in console.

---

## Task 9: Polish — Transitions, Mobile Refinements, Final Check

**Files:**
- Modify: `new-grad-aura-check.html`

- [ ] **Step 1: Add smooth waveform animation stagger + mobile tap refinements inside `<style>`**

```css
/* ── POLISH ── */

/* Stagger waveform bar animations */
#waveform .wave-bar:nth-child(odd)  { animation-direction: alternate; }
#waveform .wave-bar:nth-child(even) { animation-direction: alternate-reverse; }

/* Prevent text selection on tap targets */
.tag, .mcq-opt, .record-btn, .btn-gradient, .btn-primary { -webkit-user-select: none; user-select: none; }

/* Safe area insets for notched phones */
#app { padding-bottom: env(safe-area-inset-bottom); }

/* Ensure results and unlock screens scroll correctly */
[data-screen="results"], [data-screen="unlock"] {
  min-height: 100vh; height: auto;
}

/* Loader copy fade-transition */
#loader-copy { transition: opacity 0.3s ease; }

/* Voice timer monospace digits */
.voice-timer { font-variant-numeric: tabular-nums; }
```

- [ ] **Step 2: Add loader copy fade in JS — update the `startLoader` setInterval**

Replace the `setInterval` callback inside `startLoader`:
```js
loaderInterval = setInterval(() => {
  step++;
  if (step < LOADER_COPY.length) {
    const el = document.getElementById('loader-copy');
    el.style.opacity = '0';
    setTimeout(() => {
      el.textContent = LOADER_COPY[step];
      el.style.opacity = '1';
    }, 150);
    [0,1,2].forEach(i => document.getElementById('ldot-'+i).classList.toggle('active', i === step));
  }
}, 1200);
```

- [ ] **Step 3: Final browser verification**

Open the file in:
- Desktop browser (Chrome/Firefox) — confirm layout centers at max-width 430px
- Browser DevTools → iPhone 14 Pro viewport (390×844) — confirm no horizontal scroll, no clipped text
- Test the full flow once more end-to-end

Expected: All 9 screens render correctly. Animations play. No layout overflow. Waveform bars animate with stagger. Loader copy fades between messages. Share/unlock interaction works.

---

## Self-Review Notes

**Spec coverage check:**
- ✅ Screen 1 Hero — Task 2
- ✅ Screen 2 Identity Tags — Task 3
- ✅ Screens 3–5 MCQ — Task 4
- ✅ Screen 6 Voice — Task 5
- ✅ Screen 7 Loader — Task 6
- ✅ Screen 8 Results (persona, radar, voice tags) — Task 7
- ✅ Screen 9 Locked → Unlocked — Task 8
- ✅ CSS design tokens — Task 1
- ✅ State machine / screen transitions — Tasks 1 & 4
- ✅ Auto-advance (MCQ 600ms, Loader 3.6s) — Tasks 4 & 6
- ✅ navigator.share() + clipboard fallback — Task 8
- ✅ Gradient button reserved for hero CTA + share CTA only — Tasks 2 & 8
- ✅ Progress bar per screen — Tasks 3, 4, 5
- ✅ Skip voice link — Task 5
- ✅ Back button on unlock screen — Task 8
- ✅ Footer + privacy copy — Task 8
- ✅ Pentagon radar (5-axis) — Task 7
- ✅ Blue-only palette, no pink/orchid — All tasks
