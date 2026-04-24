1. Background & Objectives
1.1 Background
MYLS Interview 3.0 will introduce a dedicated Career/Job module featuring:

Dynamic Evaluation Criteria: Weighted scoring rubrics that adapt based on the specific Job Description (JD).

Personalized Questions: AI-generated interview questions tailored to the user's resume and target JD.

Immersive Video Simulation: Multimodal AI interviewer conducting realistic, face-to-face video mock interviews.

1.2 Objectives
Design a registration-free, lightweight, shareable quiz that drives:

Viral Acquisition: Organic sharing within North American student communities (GroupMe, Instagram, LinkedIn, Discord).

Feature Teaser: Showcase MYLS's multimodal voice analysis capability and nudge users toward the full video simulation.

Lowered Barrier: Reframe "interview prep" (heavy) as a "First Impression Decoder" (light, social, curiosity-driven).

1.3 Target Audience (North America Focus)
Identity: Sophomores/Juniors/Seniors seeking Summer Internships or Co-op placements, and New Grads (Class of 2024/2025/2026).

Context: Scrolling LinkedIn, Reddit r/csMajors, Discord servers, or Instagram during recruiting season.

Pain Point: "How do recruiters actually perceive me?" / "What's my biggest interview blind spot?"

2. Core Gameplay Overview
Users complete 3 situational multiple-choice questions + 1 voice recording prompt. The system combines choice logic with multimodal speech analysis to generate a "New Grad Potential Report" .

Report Contents (see Section 4.2.6 for full detail):

- Persona Name + Character Trait Tags
- Rarity Statement (conditional)
- One-Liner
- 5-Dimension Radar Chart
- Persona Analysis (5-line commentary)
- Blurred Lock Section

Viral Mechanism:
A blurred/locked section at the bottom of the report contains *"3 Personalized Interview Questions + 1 AI Video Review."* Users must share the quiz with a peer to unlock the full content.

Conversion Hook:
Post-unlock, users are prompted to sign up for MYLS to record video responses and receive dynamic scoring.

3. User Flow
graph TD
    A[Landing Page] --> B[Select Identity Tags]
    B --> C[Answer Q1-Q3 Situational MCQs]
    C --> D[Q4 Voice Prompt]
    D --> E{User Grants Mic Permission?}
    E -- Yes --> F[Record & Analyze Audio]
    E -- No --> G[Fallback: Text Input]
    F --> H[Generate Report]
    G --> H
    H --> I[Display Results with Blurred Lock]
    I --> J{User Clicks Primary CTA}
    J -- Share Completed --> K[Unlock Full Content]
    J -- Cancelled --> I
    K --> L[View Full Questions + AI Video Preview]
    L --> M[Click Secondary CTA to Sign Up]
4. Page-by-Page Specifications
4.1 Page Structure (Single Scrolling H5)
#    Module    Description
1    Hero Section    Headline + Subhead + Background visual hinting at video interview
2    Identity Tags    Dropdown/Buttons: Year in School + Target Industry
3    Q1-Q3 Multiple Choice    3 card-style situational questions
4    Q4 Voice Interaction    Mic permission → Recording → Multimodal feedback preview
5    Analysis Loader    Radar scan animation + JD matching copy
6    Results Report    Persona name + Trait Tags + Rarity Statement + One-Liner + Radar Chart + Persona Analysis + Blurred Lock
7    CTA Buttons    Primary: Share to Unlock / Secondary: Sign Up / Corner radius: 12px
8    About Us Card    MYLS brand description card below CTA buttons
9    Footer    Privacy Policy, Powered by MYLS
4.2 Detailed Module Design
4.2.1 Hero Section
Headline: How do recruiters see you?

Subhead: Your persona, your blind spots, your voice.

Visual: Blurred screenshot of MYLS video interface (a student speaking to camera).

4.2.2 Identity Tag Selector
Field 1: I am a... [Sophomore/Junior seeking Internship] [Senior seeking New Grad] [Recent Grad (2024/2025)]

Field 2: Target Industry [Tech/Engineering] [Finance/Consulting] [Marketing/Sales] [Other]

Purpose: Signals personalization for dynamic question weighting.

4.2.3 Q1-Q3 Situational Questions (Optimized — North American Campus Context)

Each option maps to five persona dimensions: Act (Action), Comm (Communication), Inter (Interpersonal), Conf (Confidence), Strat (Strategy). Scores are averaged across Q1–Q3 then adjusted by voice analysis to produce a final 5D vector, which is matched to the 20-persona library via Euclidean distance (see Scoring Algorithm section).

Interaction: Selecting an option records the answer and auto-advances to the next question.

---

**Q1. Group project due tomorrow. One teammate ghosted and did zero work. You:**

| Option | Text | Act | Comm | Inter | Conf | Strat |
|--------|------|:---:|:----:|:-----:|:----:|:-----:|
| A | Do it myself, submit, and don't mention it unless asked. | 15 | 35 | 55 | 25 | 20 |
| B | Call them out in the group chat — public accountability. | 90 | 55 | 25 | 80 | 40 |
| C | Message them privately and offer to help finish tonight. | 50 | 60 | 85 | 50 | 60 |

---

**Q2. Career Fair line for your dream company is 45 mins long. You:**

| Option | Text | Act | Comm | Inter | Conf | Strat |
|--------|------|:---:|:----:|:-----:|:----:|:-----:|
| A | Warm up at a smaller booth, then circle back later. | 65 | 50 | 55 | 60 | 70 |
| B | Stay in line and rehearse exactly what I'll say. | 45 | 35 | 40 | 40 | 85 |
| C | Grab a card, skip the line, send a cold email tonight. | 90 | 80 | 65 | 85 | 80 |

---

**Q3. Recruiter kicks off with "Walk me through your resume." You:**

| Option | Text | Act | Comm | Inter | Conf | Strat |
|--------|------|:---:|:----:|:-----:|:----:|:-----:|
| A | Start with my most recent internship — it's my strongest. | 40 | 50 | 55 | 50 | 45 |
| B | Pull 2-3 experiences that map directly to this role. | 55 | 85 | 35 | 60 | 90 |
| C | Open with why I'm in this field, then back it up. | 50 | 70 | 80 | 65 | 70 |

4.2.4 Q4 Multimodal Voice Prompt (Critical Feature)
Screen Copy:

"Let's hear how you sound."
Answer this in one or two sentences: Describe yourself in three words, and tell us why.

Interaction Flow:

Step    Action    UI Feedback
1    Request Microphone Permission    Modal: "MYLS AI needs mic access to analyze pace and confidence (Audio is NOT stored)."
2    User taps "Start Recording"    Animated waveform + 30s countdown. Optional Prompt Card (collapsible).
3    Recording    Live waveform. "Stop" button available.
4    Upload & Analyze    Spinner: "AI is listening..." (< 3s).
5    Feedback Preview    3 Tags appear: 🗣️ Pace (wpm) / 🎯 Keywords (Top 3) / 😌 Confidence Score (%)
Exception Handling:

Mic Denied: Fallback to a text input field. Show warning: "Text mode works, but you'll miss the full multimodal analysis."

No Audio Detected: Prompt "Hmm, we didn't catch that. Try again?" with Skip option.

Network Timeout: "Connection hiccup. Please retry."

Technical Requirements:

Frontend: Record WAV/MP3, Mono, 16kHz.

Backend API: Return WPM, Top 3 Keywords, Confidence Score (0-100), Transcription Text.

Privacy: Zero retention policy. Audio stream is processed ephemerally.

4.2.5 Analysis Loading Animation
Visual: Dynamic radar chart scanning (matching MYLS 3.0 Dynamic Evaluation Criteria branding).

Rotating Copy:

*"Benchmarking against 45,000+ New Grad JDs..."*

"Generating your personalized 2026 recruiting season questions..."

"Analyzing speech patterns and filler words..."

4.2.6 Results Report Page
Top Bar: Sticky header with brand name ("MYLS Interview") on the left and the current date on the right. Date must be displayed in English format (e.g. "April 22"). Do not use locale-based Chinese date formatting.


A. Persona Section

Labels (Dynamically assigned based on choices + voice confidence). Full list in Section 9.1.

Character Trait Tags: Displayed inline directly below the persona name — no card wrapper. Tags appear as pill-shaped labels (e.g. ⚡ Ambitious, 📋 Over-prepared, 🔥 High-motor, 🎯 Focused).

One-Liner: A short, punchy description of the persona. Source: Section 9.1 One-Liner table. Displayed in large quoted style below the Rarity Statement.

B. Rarity Statement (Conditional — shown only for Rare / Very Rare / Ultra Rare personas)

Displayed as a badge beneath the Trait Tags, above the One-Liner.

Copy template: *"STATISTICAL OUTLIER — Only X% of candidates match this profile."*

Where X% is pulled from the persona's rarity value in Section 9.1. Not shown for Common (≥ 12%) or Uncommon (10–11%) personas.

Rarity tiers:
- Common (≥ 12%): not shown
- Uncommon (10–11%): not shown
- Rare (6–9%): shown
- Very Rare (4–5%): shown
- Ultra Rare (≤ 3%): shown with additional emphasis

C. 5-Dimension Radar Chart

An animated radar chart visualizing the user's final scored vector across all five dimensions: Act, Comm, Inter, Conf, Strat.

- Axes labeled with full dimension names
- Data source: averaged MCQ scores + voice adjustments (see Section 5)
- Animates in on results reveal
- Style: minimal, monochrome, consistent with overall light theme

D. Persona Analysis

5-line persona-specific commentary in direct, slightly uncomfortable narrator tone. Displayed in the "PERSONA ANALYSIS" card below the radar chart.

Full per-persona copy (EN + 中文) in Section 9.7.

Examples:
- The High-Potential Grinder: *"You're driven and always prepared. You've put in more work than most. But it feels heavy. Like you're trying to prove everything at once. Ambition is clear. Focus is not."*

E. AI Recruiter's Lens

> **Status:** Currently hidden on the results page (`display:none`). Card is populated in the data layer but not visible to users. Reserved for future activation.

A single highlighted callout card showing the AI recruiter's contextual perspective on the user (distinct from the Persona Analysis, which is persona-fixed; the Lens can be dynamically inflected by voice score).

Example: "High motor, but slow down. Breathe."

Label: 🕶️ AI Recruiter's Lens

Note: Detailed voice metrics (WPM, keywords, confidence score) and the "How you come across" section are not shown on the results page. Voice feedback depth is reserved for the Unlock page.

F. Blurred Lock Section (Viral Hook)

Visual: Blurred card with faint text showing the first question and a teaser for the "Feedback Preview."

Overlay Text:

"Your New Grad Aura is ready. Share with a friend to instantly unlock:

✅ 3 Personalized Interview Questions (based on your voice + choices)

✅ 1 AI Video Feedback Preview (see exactly what MYLS coaches on)

✅ The 'First 30 Days' Playbook (quick tips for your first internship month)

Tap below to share and claim your free toolkit."

4.2.7 CTA Buttons
Button Type    Copy    Action
Primary (Large)    Invite a Friend & Unlock My Free Toolkit    Trigger native share sheet (iMessage, LinkedIn, Instagram, Snapchat)
Secondary (Ghost)    Would recruiters hire you?    Navigate to Unlock Page
Text Link    *Ready to ace the real thing? 100+ roles. 30,000+ prepared.* →    Redirect to MYLS Interview Sign Up

Design spec: All CTA buttons use corner radius of 12px (not pill/full-round).

Post-Share Callback:
Upon successful share interaction, the user is taken to the Unlock Page.

4.2.8 About Us Card (Below CTA Buttons)
A brand description card placed below the CTA buttons to build trust and product awareness.

Copy:
"We're builders. We got obsessed with how AI is changing hiring so we built MYLS.
30,000+ students have used it to find their footing, fix their blind spots, and walk into interviews with more confidence."

Style: Subtle card with muted background, smaller body text. Not a conversion element — purely credibility/brand.

---

4.2.9 Workplace Habits — Collapsible Deep-Dive Card

**Purpose:** Transform the quiz from an entertainment label into a genuine career personality insight. Answers the user's hidden question: *"Now that I know who I am — how will I show up at work? Will my manager and coworkers actually like me?"* Modeled after MBTI depth but grounded in the New Grad job-seeking context.

**Placement:** On the Unlock page ("Your Personalized Prep Kit"), in the "More Analysis" section below "Here's How You Actually Sounded." Does not appear on the Results page.

**Default State:** Collapsed. The card shows a teaser row with the section title and a chevron/arrow indicating it is expandable. The collapsed state takes up minimal vertical space (~56px) so it does not add perceived length to the page.

**Expanded State:** On tap, the card animates open (smooth height transition, ~200ms ease-out). The full Workplace Habits content is revealed inside.

**Dual-Perspective Layout:**
The expanded card displays two perspectives using **horizontal swipe tabs** (preferred on mobile) or a two-item accordion if tab gesture conflicts with scroll:

| Tab | Label | Icon |
|-----|-------|------|
| Tab 1 | Your Manager's View | 👔 |
| Tab 2 | Your Colleagues' View | 💬 |

- Tabs are pill-style toggles at the top of the expanded card.
- Default active tab: Tab 1 (Manager's View).
- Switching tabs replaces body copy in-place (no full card re-render); transition: fade 120ms.

**Content Source:** Per-persona copy from section 9.5. Content is dynamically loaded based on the matched persona (same matching logic as 4.2.6-A).

**Visual Style:**
- Card background: `#F5F5F5`
- Border-radius: 18px
- Section header: uppercase label "Workplace Habits" in muted text, same style as other section labels
- Tab pills: active tab uses `var(--sel-bg)` / `#19181A` with white text; inactive tab uses surface + border
- Body copy: 13–14px, `var(--text-2)`, line-height 1.75
- Max visible body height before internal scroll: none (card grows to full content height)

**Collapsed Teaser Row Design:**
```
[ 🧩 Workplace Habits    How do you show up at work? ›  ]
```
- Left: icon + "Workplace Habits" label (font-weight 700, 13px)
- Right: short hook copy + expand chevron
- Background: `#F5F5F5`, border-radius 18px, padding 16px 20px

**Accessibility:** Collapsed/expanded state toggled via `aria-expanded`. Card section uses `role="region"` with `aria-label="Workplace Habits"`.

---

4.3 Your Personalized Prep Kit (Unlock Page)

**Entry Point:** User lands here after completing share action.

**Page Name:** Your Personalized Prep Kit

**Product Goals:**
- Deliver immediate, persona-specific value after share
- Position MYLS 3.0 as the product that turns insight into practice
- Drive conversion into full mock interview

**Page Header:**
```
🎉 Unlocked
Your Personalized
Prep Kit
```

---

**Page Structure (post-unlock scroll):**

**1. I Will Hire You If… Card** *(NEW — persona-specific checklist)*

- Eyebrow: `[Persona Name]` (e.g., "The Dependable Intern")
- Title: **I Will Hire You If…**
- Content: 3 persona-specific checklist items (checkbox style ☐), drawn from section 9.6
- Each item is a concrete, actionable behavior the interviewer would reward — and a micro-step MYLS can train

*Purpose: Make the result feel actionable and personal. Bridge from "who you are" to "what to do."*

---

**2. Role Match Card** *(Dynamic role recommendations)*

- Eyebrow: **HOW READY ARE YOU FOR THESE ROLES?**
- Subtitle: "These roles match how you think and how you show up."
- Content: 3 roles dynamically selected based on the user's Target Industry + matched Persona (see Section 9.8), displayed as emoji + pill-style capsules
- Footer line: "All available on MYLS Interview 3.0."
- CTA link: "See your readiness for 100+ roles →"

*Purpose: Make the result feel career-relevant and personally targeted. Bridge persona insight to real job titles.*

---

**3. A glimpse of your MYLS 3.0 analysis** *(AI Feedback Preview)*

Section title: A glimpse of your MYLS 3.0 analysis

Preview framing: In a real session, every word gets analyzed

**3.1 Transcript**
Label: What you said:
Content: transcript with highlights on filler phrases

**3.2 Feedback Insights**
Section title: What to fix

Format: 1 item shown openly; remaining items blurred with lock overlay.

Shown item example:
> *You overused filler phrases* — Saying "basically what I mean is…" repeatedly makes you sound less confident than you actually are. 👉 Try: pause instead of filling space

Blurred items: `🔒 Unlock full voice breakdown in MYLS`

*Purpose: Deliver enough value to feel credible; blur the rest to drive MYLS sign-up.*

---

**4. More Analysis** *(Expandable — Workplace Habits)*

Expandable accordion section containing the Workplace Habits card (see section 4.2.9).
- Collapsed teaser: `🧩 Workplace Habits | How do you show up at work? ›`
- Expanded: dual-tab view (👔 Manager's View / 💬 Colleagues' View)

*Purpose: Reward deeper exploration. Add replayability and shareability.*

---

**5. Conversion Section**

Title: Ready to actually practice?

Body: This was just a preview. Real interviews are way harder — and way more revealing.

CTA: Try a real interview →
Ghost CTA: Test Again
Subtext: Practice across 100+ roles · get feedback on how you actually perform

---

**Design Principles (Mandatory):**

1. Not a tool — a coach
   - ❌ "Recommended improvement"
   - ✅ "you repeated yourself"

2. Insight → Action loop: every section tells you who you are AND what to do next.

3. Keep emotional tone: slightly direct, slightly uncomfortable, always helpful.

**Success Metrics:**
- Share → Unlock completion rate
- Scroll depth (did user reach AI feedback?)
- CTA click rate
- Conversion to mock interview

5. Scoring Algorithm (Five Dimensions)

### 5.1 Dimension Definitions

| Dimension | Code | Low (0–30) | Mid (31–70) | High (71–100) |
|-----------|------|-----------|------------|--------------|
| Action Style | Act | Silent Executor | Balanced | Initiator |
| Communication | Comm | Structured / Framework | Balanced | Storyteller |
| Interpersonal | Inter | Independent | Balanced | Collaborative |
| Confidence | Conf | Reserved | Moderate | Assertive |
| Strategy | Strat | Gut-feel (Raw) | Situational | Calculated (Prepped) |

### 5.2 MCQ to Dimension Mapping

See section 4.2.3 for full per-option dimension scores across Q1–Q3.

User's raw vector = average of selected option scores across all three questions, per dimension.

### 5.3 Voice Analysis Adjustments

Applied on top of the MCQ average to produce the final 5D vector before persona matching.

| Metric | Threshold | Dimension Impact | Adjustment Range |
|--------|-----------|-----------------|-----------------|
| Speech Rate (WPM) | > 160 | Comm +10, Conf +5 | ±5–15 |
| | < 110 | Comm −10, Conf −5 | |
| Filler Word Rate (%) | > 8% | Conf −15, Strat −10 | ±10–20 |
| | < 3% | Conf +5, Strat +10 (signals rehearsal) | |
| Keyword Match | Matches high-Strat words ("first/second/finally", STAR terms) | Strat +5–10 | Contextual |
| | Matches low-Strat words ("just", "whatever", "I guess") | Strat −5–10 | Contextual |
| AI Confidence Score | 0–100 | Weighted 7:3 with MCQ Conf → final Conf value | — |
| STAR Structure Detected | Pattern identified in transcript | Strat +15 | Binary trigger |

### 5.4 Persona Vector Table (Five Dimensions)

| # | Persona | Act | Comm | Inter | Conf | Strat |
|---|---------|:---:|:----:|:-----:|:----:|:-----:|
| 1 | The Dependable Intern | 20 | 45 | 55 | 35 | 25 |
| 2 | The High-Potential Grinder | 85 | 55 | 30 | 85 | 80 |
| 3 | The Case Study Natural | 50 | 85 | 35 | 60 | 90 |
| 4 | The Coffee Chat Pro | 70 | 75 | 80 | 85 | 75 |
| 5 | The Silent Sniper | 25 | 35 | 45 | 40 | 30 |
| 6 | The People Pleaser | 40 | 55 | 90 | 35 | 40 |
| 7 | The Over-Prepared Overthinker | 45 | 40 | 50 | 25 | 85 |
| 8 | The Storyteller | 45 | 75 | 80 | 60 | 55 |
| 9 | The Data Whisperer | 50 | 85 | 30 | 55 | 80 |
| 10 | The Empathy Anchor | 35 | 50 | 95 | 40 | 35 |
| 11 | The Hustler | 90 | 70 | 60 | 85 | 65 |
| 12 | The Technical Purist | 30 | 50 | 40 | 40 | 25 |
| 13 | The Charmer | 65 | 70 | 75 | 90 | 70 |
| 14 | The Rule Follower | 45 | 80 | 35 | 50 | 70 |
| 15 | The Wild Card | 50 | 50 | 50 | 50 | 50 |
| 16 | The Imposter Syndrome Warrior | 50 | 55 | 60 | 20 | 45 |
| 17 | The "I Can Fix It" | 92 | 45 | 35 | 85 | 35 |
| 18 | The Curious Cat | 45 | 60 | 70 | 55 | 40 |
| 19 | The Calm in the Storm | 35 | 50 | 50 | 55 | 50 |
| 20 | The Visionary | 60 | 80 | 50 | 80 | 65 |

### 5.5 Matching Logic

1. Compute user's final (Act, Comm, Inter, Conf, Strat) vector = MCQ average + voice adjustments + Industry Nudge + Year Modifier (see section 5.6).
2. Calculate Euclidean distance to each persona's standard vector (section 5.4).
3. Select the closest persona.
4. If tie (distance delta < 5), use keyword frequency from voice transcript as tiebreaker.
5. If still ambiguous, default to closest Conf score match.

---

### 5.6 Cohort-Based Modifier

在原有算法（MCQ + 语音 → 五维向量 → 欧氏距离 → 关键词决胜）的基础上，增加 Cohort Modifier。该调整不大幅修改向量，而是在距离相近时提供有意义的偏向，让结果更贴合现实职场人格分布。

**完整向量公式：**
```
用户向量 = MCQ 均值 + 语音调整 + Industry Nudge + Year Modifier
```
所有维度值在 0–100 范围内 clamp，防止溢出。

---

#### 5.6.1 Industry Affinity Nudge

各行业对某些维度有轻微偏好，作为**向量偏置**直接加到用户的计算向量上（计算欧氏距离之前应用）。偏置上限不超过 ±10，防止过度扭曲用户原本的选择。

| Target Industry | Act | Comm | Inter | Conf | Strat | 解释 |
|----------------|:---:|:----:|:-----:|:----:|:-----:|------|
| Tech / Engineering | ±0 | ±0 | −5 | ±0 | +5 | 更重视独立解决问题与系统化准备，人际温度略低 |
| Finance / Consulting | +3 | +8 | −3 | +5 | +10 | 对口才、结构化思维、自信和准备程度要求最高，人际协作需求相对较低 |
| Marketing / Sales | ±0 | +5 | +8 | +3 | ±0 | 侧重故事叙述、人际连接和自信，策略性中等 |
| Healthcare / Biotech | −3 | ±0 | +5 | −3 | +3 | 强调团队协作与精准执行，自信和侵略性不宜过高，策略性中等 |
| Creative / Design | +3 | +5 | +3 | +5 | −5 | 重视叙事与原创性，准备程度可能不如其他行业，更看直觉和自信 |
| Other | ±0 | ±0 | ±0 | ±0 | ±0 | 不调整 |

---

#### 5.6.2 School Year Modifier

不同年级在求职心态上存在系统性差异，通过微调 Confidence 和 Strategy 维度来体现。

| School Year | Conf Adj. | Strat Adj. | Rationale |
|-------------|:---------:|:----------:|-----------|
| Sophomore / Junior | −3 | −5 | 处于早期探索阶段，面试经验较少，回答可能更依赖直觉而非刻意策略 |
| Senior | +3 | +5 | 正处于秋招/春招高峰期，大量练习使策略性和自信度达到峰值 |
| Recent Grad | +5 | +3 | 已毕业身份带来一定自信，但缺乏校园招聘支持，策略性可能略有回落 |

---

#### 5.6.3 组合应用示例

一个 Senior 申请 Tech 的用户，原始 MCQ 向量 Strat = 60, Conf = 55：

1. Year Modifier (Senior): Strat +5 → 65, Conf +3 → 58
2. Industry Nudge (Tech): Strat +5 → 70, Conf ±0 → 58
3. 最终向量 Strat = 70 → 更可能匹配 预制卷王 / AI 的嘴替，而非 盲盒选手

---

6. Share & Virality Design (North America Context)
6.1 Share Card Configuration
Dynamic Share Copy Options (North America student tone):

*"Just found out my 'New Grad Aura' and unlocked 3 personalized interview Qs + a video feedback sneak peek. Low-key helpful for fall recruiting szn 👀 [Link]"*

"30,000 students use MYLS to practice video interviews. I just got my free prep kit—see what questions you'd get. [Link]"

*"If you're stressed about behavioral interviews, this 60-sec quiz gave me a solid list of what to work on. Plus a free preview of AI video coaching. [Link]"*

Share Image: Dynamically generated card with Persona Label + Radar Chart silhouette.

Link: UTM-tagged short link.

6.2 Referral Loop
User B lands on page via User A's link.

System tracks UTM source. Optional: Show "You were invited by [Name] to compare results."

6. Analytics & Tracking Requirements
Event Name    Trigger    Parameters
page_view    Landing page load    source, utm_params
identity_selected    User picks year/industry    school_year, target_industry
quiz_answer    MCQ selection    question_id, answer_id
mic_permission    Permission prompt result    status (granted/denied)
audio_recording_start    User clicks record    -
audio_analysis_success    Analysis returns 200 OK    duration_sec, word_count
audio_analysis_error    Analysis fails    error_code
report_view    Results page rendered    persona_type, confidence_score
share_click    Primary CTA click    -
share_success    Share sheet interaction    channel (if detectable)
content_unlocked    Blur removed    -
signup_cta_click    Secondary link click    -
7. Technical & Non-Functional Requirements
7.1 Compatibility
H5 fully responsive. Tested on iOS Safari, Android Chrome, and in-app browsers (Instagram, LinkedIn, Discord).

getUserMedia fallback for strict browser policies.

7.2 Performance
First Contentful Paint < 1.5s on 4G.

Audio analysis latency < 3s.

7.3 Privacy & Compliance
Footer must include Privacy Policy link.

Explicit consent copy: "Audio is processed in real-time and immediately discarded. Nothing is stored."

Compliance with FERPA (indirectly, since it's students) and standard US/Canada data privacy laws.

8. Timeline & Prioritization
Phase    Task    Priority    Est. Effort
P0 (MVP)    Frontend flow: MCQs + Results logic + Blur/Lock state    Highest    3d
P0 (MVP)    Microphone integration + Multimodal API connection    Highest    2d
P1    Voice AI feedback preview UI (Unlock page)    High    1d
P2    Dynamic OG image generation for sharing    Medium    1d
P2    UTM tracking & referral logging    Medium    0.5d
P3    AI Video Review static preview design    Low    0.5d

9. Appendix: Persona Vector Configuration & Keyword Library

---

### 9.1 20-Persona Library (Bilingual — with Rarity)

Persona is assigned via Euclidean distance matching against the 5D vectors in section 5.4. Rarity percentages reflect expected distribution across test-takers based on the 5D vector space; thresholds will be calibrated post-launch from live data.

**Rarity tiers:** Ultra Rare ≤ 3% · Very Rare 4–5% · Rare 6–9% · Uncommon 10–11% · Common ≥ 12%

| # | EN Label | 中文标签 | Rarity | Persona Analysis |
|---|----------|---------|--------|-----------------|
| 1 | The Dependable Intern | 小组作业の神 | 15% (Common) | See section 9.7 |
| 2 | The High-Potential Grinder | 预制卷王 | 7% (Rare) | See section 9.7 |
| 3 | The Case Study Natural | AI 的嘴替 | 8% (Rare) | See section 9.7 |
| 4 | The Coffee Chat Pro | 社交悍匪 | 9% (Rare) | See section 9.7 |
| 5 | The Silent Sniper | 潜水区大佬 | 10% (Uncommon) | See section 9.7 |
| 6 | The People Pleaser | 情绪价值打工人 | 12% (Common) | See section 9.7 |
| 7 | The Over-Prepared Overthinker | 内耗 MVP | 8% (Rare) | See section 9.7 |
| 8 | The Storyteller | 古希腊掌管叙事的神 | 10% (Uncommon) | See section 9.7 |
| 9 | The Data Whisperer | Excel 成精 | 6% (Rare) | See section 9.7 |
| 10 | The Empathy Anchor | 赛博热水 | 10% (Uncommon) | See section 9.7 |
| 11 | The Hustler | 斜杠永动机 | 7% (Rare) | See section 9.7 |
| 12 | The Technical Purist | 赛博老实人 | 8% (Uncommon) | See section 9.7 |
| 13 | The Charmer | 高质量人类 | 5% (Very Rare) | See section 9.7 |
| 14 | The Rule Follower | 标准答案复制人 | 12% (Common) | See section 9.7 |
| 15 | The Wild Card | 盲盒选手 | 3% (Ultra Rare) | See section 9.7 |
| 16 | The Imposter Syndrome Warrior | 配得感洼地 | 4% (Very Rare) | See section 9.7 |
| 17 | The "I Can Fix It" | 救火队长 | 5% (Very Rare) | See section 9.7 |
| 18 | The Curious Cat | 十万个为什么成精 | 7% (Rare) | See section 9.7 |
| 19 | The Calm in the Storm | 情绪稳定如死狗 | 6% (Rare) | See section 9.7 |
| 20 | The Visionary | 大气层玩家 | 4% (Very Rare) | See section 9.7 |

> Persona Analysis copy (EN + 中文, 5-line format) is in Section 9.7.

> 5D vectors for matching are in Section 5.4. Persona Analysis is shown on the results page (Section 4.2.6-D).

**One-Liner Copy — Results Page Display (EN + 中文)**

| # | EN Label | One-Liner (EN) | 中文一句话定义 |
|---|----------|----------------|---------------|
| 1 | The Dependable Intern | Silent workhorse who saves the project without asking for credit. | 小组作业的兜底之神，活全干话不多。 |
| 2 | The High-Potential Grinder | Runs on caffeine and ambition. Prepped since orientation. | 人生 1.5 倍速，浑身是劲没处使。 |
| 3 | The Case Study Natural | Brain organizes chaos into 2x2 matrices by default. | 大脑预装 MECE，结构比 ChatGPT 还工整。 |
| 4 | The Coffee Chat Pro | Mastered the 15‑min Zoom coffee. LinkedIn is a flex. | 喝过的 informational coffee 能填满人工湖。 |
| 5 | The Silent Sniper | Lurks in meetings, delivers work that hits like a headshot. | 开会头像都是灰的，交出来的活刀刀暴击。 |
| 6 | The People Pleaser | The group's unofficial therapist. | 团队的赛博布洛芬，自己累点没关系。 |
| 7 | The Over-Prepared Overthinker | Notes app is a graveyard of interview scripts. | 面经刷到 2021 年，脑子里模拟 N 种死法。 |
| 8 | The Storyteller | Turns a grocery run into a leadership case study. | 买个菜都能讲出用户增长方法论。 |
| 9 | The Data Whisperer | Numbers are your love language. | 血液里流淌着 VLOOKUP。 |
| 10 | The Empathy Anchor | The person everyone DMs when spiraling. | 团队的群公告"有事私戳"。 |
| 11 | The Hustler | Side projects, startups, eye bags to prove it. | 不是在搞钱就是在搞钱的路上。 |
| 12 | The Technical Purist | GitHub is green. Behavioral is a foreign language. | 代码比情书漂亮，BQ 像去前任婚礼。 |
| 13 | The Charmer | Main character energy. Smooth, polished, camera‑ready. | 开口自带背景音乐和柔光滤镜。 |
| 14 | The Rule Follower | Syllabus is law. Never deviates. | 每一步都按规矩来，安全得像教科书。 |
| 15 | The Wild Card | Interview performance is a dice roll. | 状态来了超神，状态没了超度面试官。 |
| 16 | The Imposter Syndrome Warrior | Voice trails off like you're apologizing. | 开口像来借钱，句尾自带降调虚音。 |
| 17 | The "I Can Fix It" | Solves problems without waiting for permission. | 默认自己是全场唯一清醒的救世主。 |
| 18 | The Curious Cat | Reverse interview questions are legendary. | 反问环节能从战略问到 HR 午饭。 |
| 19 | The Calm in the Storm | Radiates Zen even when prod is down. | DDL 烧眉毛了还在泡茶。 |
| 20 | The Visionary | Thinking about 2030 strategy during first round. | 格局在天上飘着，想重塑部门。 |

---

### 9.2 Keyword Trigger Library (Voice Tiebreaker)

When two personas are within close Euclidean distance, voice keyword hits serve as a tiebreaker, adding +5–10 to the matched dimension.

| # | Persona | Trigger Keywords (EN) | 触发词 (中文) |
|---|---------|----------------------|-------------|
| 1 | 小组作业の神 | "group project", "carry", "silent", "just did it" | 小组作业、帮他们、默默、我做完了 |
| 2 | 预制卷王 | "fast", "deadline", "goal", "prep" | 速度、截止日、目标、刷题、提前 |
| 3 | AI 的嘴替 | "structure", "framework", "first", "second", "finally" | 首先、其次、框架、逻辑、总结 |
| 4 | 社交悍匪 | "network", "coffee", "connect", "chat" | 人脉、咖啡、聊、认识、加个微信 |
| 5 | 潜水区大佬 | "quiet", "alone", "focus", "deep work" | 安静、一个人、专注、深度 |
| 6 | 情绪价值打工人 | "we", "team", "support", "help them" | 我们、团队、支持、帮他们 |
| 7 | 内耗 MVP | "worry", "overthink", "perfect", "what if" | 担心、完美、反复想、万一 |
| 8 | 古希腊掌管叙事的神 | "story", "imagine", "one time", "so there I was" | 故事、有一次、我觉得、当时 |
| 9 | Excel 成精 | "data", "number", "metric", "percent" | 数据、数字、指标、百分比 |
| 10 | 赛博热水 | "listen", "feel", "help", "here for you" | 倾听、感觉、帮忙、我在 |
| 11 | 斜杠永动机 | "side project", "startup", "hustle", "also" | 副业、创业、同时、还做了 |
| 12 | 赛博老实人 | "code", "build", "technical", "function" | 代码、写、技术、功能 |
| 13 | 高质量人类 | "confident", "excited", "love", "passion" | 自信、热爱、兴奋、激情 |
| 14 | 标准答案复制人 | "rule", "syllabus", "correct", "supposed to" | 规定、应该、标准、按道理 |
| 15 | 盲盒选手 | — (fallback, no dominant keyword) | — |
| 16 | 配得感洼地 | "sorry", "maybe", "just", "I think" | 抱歉、可能、只是、我觉得 |
| 17 | 救火队长 | "fix", "solve", "I did", "handled" | 解决、搞定、我来、处理了 |
| 18 | 十万个为什么成精 | "why", "curious", "question", "how come" | 为什么、好奇、请问、怎么 |
| 19 | 情绪稳定如死狗 | "fine", "whatever", "chill", "it's okay" | 还好、随便、不急、没事 |
| 20 | 大气层玩家 | "vision", "future", "change", "impact" | 未来、改变、格局、影响 |

---

### 9.3 Voice Analysis → Dimension Adjustment Rules

See Section 5.3 for the full voice adjustment table. This section is intentionally kept as a reference pointer to avoid duplication.

---

### 9.5 Workplace Habits — Per-Persona Content (All 20)

Used by section 4.2.9. Each persona has two blocks: Manager's View and Colleagues' View. Content is dynamically rendered based on the matched persona ID.

---

**1. The Dependable Intern** *(小组作业の神)*

👔 **Your Manager's View**
Your manager sees you as the safest bet on the team. You never miss a deadline, you never push back, and you never make drama. This sounds like a dream — and in many ways, it is. You're the person they quietly assign the messy work to because they know you'll handle it without complaint.

But here's the blind spot: your silence can be mistaken for satisfaction. Managers may assume you're happy with your workload when you're actually drowning. They may overlook you for promotions because you don't advocate for yourself. The work you do is visible; the person doing it is not. You need a manager who checks in proactively, not one who waits for you to raise a flag — because you probably won't.

💬 **Your Colleagues' View**
Your coworkers genuinely like you. You're low-drama, high-output, and never steal credit. On group projects, you're the one people hope shows up in their breakout room. You make other people's lives easier, often at your own expense.

But your colleagues may not realize how much you're carrying. Because you don't talk about your contributions, they might assume the work distributed itself evenly. Some may even take advantage — intentionally or not — knowing you'll pick up whatever they drop. The people who notice your quiet competence will become your biggest advocates. The ones who don't? They'll just keep handing you work.

---

**2. The High-Potential Grinder** *(预制卷王)*

👔 **Your Manager's View**
Your manager is simultaneously impressed and slightly exhausted just watching you. You're the first to volunteer, the last to log off, and the one who treats stretch goals as the bare minimum. For a manager who craves output, you're a gift. For one who worries about burnout, you're a headache.

The risk with your intensity is that managers may keep feeding you more instead of telling you to slow down. They might assume you're fine because you never show cracks — until you do. You need a manager who can match your pace but also protect you from yourself. Someone who says "good work" as often as they say "what's next." Without that, you'll grind until there's nothing left.

💬 **Your Colleagues' View**
Your coworkers have complicated feelings about you. On one hand, you raise the bar for everyone. Your energy is contagious, and you make the team feel ambitious. On the other hand, you can unintentionally make people feel slow or inadequate. Not everyone wants to run at your speed, and they shouldn't have to.

Some colleagues will be inspired by you. Others will resent you. The difference often comes down to whether you share your strategies or keep them to yourself. When you help others level up, you become the team's engine. When you just lap them silently, you become the teammate they avoid at happy hour.

---

**3. The Case Study Natural** *(AI 的嘴替)*

👔 **Your Manager's View**
Your manager trusts your brain more than most people's spreadsheets. You bring structure to chaos, and your presentations practically write themselves. When a VP asks a tough question, your manager glances at you first. You're the strategic backbone of the team.

But there's a "however." Your structured delivery can feel cold. Managers may appreciate your clarity but miss your personality. They might think you're all framework and no feeling — which becomes a problem when you're being considered for roles that require influencing people, not just analyzing problems. You need a manager who values precision but also coaches you to add warmth to your delivery. One human sentence at the end of a perfect analysis can change everything.

💬 **Your Colleagues' View**
Your coworkers respect your mind. When the team is lost in ambiguity, you're the one who draws the 2x2 on the whiteboard and suddenly everything makes sense. People seek you out for input on their toughest problems.

But they might not seek you out for lunch. Your colleagues may find you intellectually impressive but emotionally distant. They wonder what you actually care about beyond getting the answer right. The moment you let them see your thought process instead of just the polished output — including the mess and the doubts — you become not just the smart one, but the trusted one.

---

**4. The Coffee Chat Pro** *(社交悍匪)*

👔 **Your Manager's View**
Your manager sees a future leader in you. You're the one who can walk into any room and make something happen. You build relationships effortlessly, and your network is your superpower. For roles that require influence and client-facing charm, you're the obvious choice.

The worry is depth. Your manager may wonder if the substance matches the style. You can get the meeting — but can you deliver the follow-up? Can you turn the coffee into a closed deal? You need a manager who celebrates your charisma while holding you accountable for execution. Charm opens doors; delivery keeps them open.

💬 **Your Colleagues' View**
You make work feel less like work. Your coworkers genuinely enjoy being around you. You're the one who remembers everyone's weekend plans, who brings energy to Zoom calls, who makes networking feel like hanging out. You're often the social glue of the team.

But some colleagues may quietly wonder if you pull your weight when the socializing stops. They've seen you shine in the meeting — but did you do the deck? They love grabbing coffee with you — but did you send the recap email? The coworkers who trust you most are the ones who've seen you deliver, not just dazzle.

---

**5. The Silent Sniper** *(潜水区大佬)*

👔 **Your Manager's View**
Your manager knows you deliver excellent work. They just have no idea how you do it, when you do it, or what you're thinking while you do it. Your output speaks for itself, but your process is a black box. For a hands-off manager, this is fine. For one who needs visibility, you're a source of low-grade anxiety.

The danger is that your silence gets misinterpreted. Managers may read your quietness as disengagement, your independence as aloofness, your lack of updates as lack of progress. You need a manager who trusts the work and doesn't micromanage, but also one who occasionally pulls you aside to say "tell me what's in your head." You have to meet them halfway by opening the door just a crack.

💬 **Your Colleagues' View**
Your coworkers are intrigued by you. You're the one who shows up to the meeting, says almost nothing, and then drops a piece of work so good it makes everyone else's look like a rough draft. You have a quiet mystique that people respect.

But they also find you hard to collaborate with. They don't know when to check in with you, how to give you feedback, or whether you even want to be on the team. When you share your thinking — not just the finished product — you go from mysterious to magical. The colleagues who crack your shell become your fiercest allies.

---

**6. The People Pleaser** *(情绪价值打工人)*

👔 **Your Manager's View**
Your manager appreciates the harmony you bring. Team morale is higher because you're in it. You're the one who smooths over conflicts, checks on people who seem quiet, and makes sure everyone feels included. In a team that struggles with culture, you're a gift.

But your manager may not see your individual contributions clearly. When everything is "we did this" and "the team worked on that," your personal impact becomes invisible. You need a manager who can distinguish between your collaborative spirit and your actual output — and who will push you to claim your wins out loud. If they don't, your performance review will read like a group project credit roll.

💬 **Your Colleagues' View**
Your coworkers genuinely like you. You're the one they vent to, the one who remembers birthdays, the one who checks in when something feels off. You're the emotional radiator of the team, and people feel safe around you.

But some coworkers may take advantage of your desire to help. They'll let you pick up their slack, knowing you won't complain. They'll offload emotional weight onto you because you're "such a good listener." The healthiest colleagues will appreciate your warmth while also checking in on you. Hold onto those ones. The others? They'll drain you dry if you let them.

---

**7. The Over-Prepared Overthinker** *(内耗 MVP)*

👔 **Your Manager's View**
Your manager receives flawless work from you. The deck is perfect. The backup analysis is ready. The edge cases have been considered. You deliver at a quality level that most people need twice the time to achieve.

What your manager doesn't see is the cost. The 2am anxiety emails. The seventeen drafts. The rehearsal of a presentation that ended up being fine anyway. You need a manager who can tell you "this is good enough — send it" and mean it. Someone who rewards the result but also helps you calibrate what "done" looks like. Without that, you'll keep polishing until you burn out.

💬 **Your Colleagues' View**
Your coworkers admire your thoroughness. When you're on the project, they know there won't be surprises. You've already thought of everything that could go wrong. You're the contingency plan machine.

But they also wish you'd relax. Your anxiety can be contagious — when you're stressed about something that seems small, others start wondering if they should be stressed too. The colleagues who tell you "it's going to be okay" and mean it are your anchors. Trust them when they say you've done enough.

---

**8. The Storyteller** *(古希腊掌管叙事的神)*

👔 **Your Manager's View**
Your manager enjoys your presence. In client meetings, you make the team look good. You can take a dry quarterly update and turn it into something people actually listen to. You're the one they send when the message needs to land with impact.

The watch-out is brevity. When time is tight, your manager may start checking their watch. They value your narrative gift, but they also need you to know when to land the plane. You need a manager who gives you the stage but also coaches you on the art of the quick hit. A well-told story is memorable; a well-timed one is powerful.

💬 **Your Colleagues' View**
Your coworkers enjoy listening to you. You make meetings less boring. Your stories stick — people remember what you said days later. You bring a human element to work that makes the team feel less transactional.

But they also know that a quick question to you might turn into a ten-minute anecdote. They love your energy, but sometimes they just need the headline. The colleagues who learn to say "give me the short version" with a smile will be your best collaborators. And when they need inspiration, they'll come straight to you.

---

**9. The Data Whisperer** *(Excel 成精)*

👔 **Your Manager's View**
Your manager trusts your numbers implicitly. When you say a metric is up 12%, they don't ask to see the spreadsheet — they put it in the board deck. Your rigor is your credibility, and it gives your manager confidence in high-stakes situations.

The gap is synthesis. Your manager gets the data; they might not get the so what. You need to add one sentence after every data point: "And here's what this means for us." Without that, you're a brilliant analyst. With it, you're a strategic advisor.

💬 **Your Colleagues' View**
Your coworkers respect your precision. When they need evidence for an argument, they come to you. You're the person who makes sure the team doesn't make decisions based on vibes alone.

But your data-heavy communication can lose people. Not everyone thinks in numbers. Some colleagues will zone out when the pivot table appears. When you translate data into decisions — "The numbers say X, so I recommend Y" — you become not just the analyst, but the person everyone wants on their project.

---

**10. The Empathy Anchor** *(赛博热水)*

👔 **Your Manager's View**
Your manager sees you as the emotional backbone of the team. When morale dips, you're the one who brings it back. When someone is struggling, you notice before anyone else. You make the team feel like a team.

The risk is that your manager may pigeonhole you as "the culture person" and overlook your other capabilities. Your empathy is a strength, but make sure they also see your hard skills. You need a manager who values both — who sees your warmth as a leadership trait, not a soft skill footnote.

💬 **Your Colleagues' View**
Your coworkers feel safe with you. You're the person they DM when they're having a rough day. You listen without judgment, and you make people feel understood. In a high-pressure environment, you're everyone's pressure release valve.

But being everyone's therapist comes at a cost. Some colleagues will keep taking without giving back. They'll offload their stress onto you and walk away lighter, while you carry the weight. The coworkers who check in on you — who ask how you're doing and actually wait for the answer — are the ones worth keeping close.

---

**11. The Hustler** *(斜杠永动机)*

👔 **Your Manager's View**
Your manager is impressed by your portfolio and slightly intimidated by your energy. You've done more in three years than most people do in ten. You bring ideas, initiative, and a fire that's hard to find.

But your manager may quietly wonder if you'll stay. Are you here for the long haul, or is this just another side project? You need to signal commitment — not by dimming your ambition, but by showing how this role connects to your bigger picture. A manager who can channel your energy is gold. One who fears it will hold you back.

💬 **Your Colleagues' View**
Your coworkers are fascinated by you. You're the one who launches startups on weekends and freelances for fun. You bring a different energy to the team — more entrepreneurial, less conventional.

But your colleagues may struggle to keep up with your variety. One week you're deep in a project, the next you seem distracted by something new. The coworkers who love working with you are the ones who share your curiosity. The ones who don't may find you scattered. Help them connect the dots between your many pursuits.

---

**12. The Technical Purist** *(赛博老实人)*

👔 **Your Manager's View**
Your manager sees a brilliant engineer in you. Your technical output is solid, your code is clean, and you solve problems that others can't. In a technical role, you're exactly what they need.

But your manager worries about your visibility. When it comes to presenting your work, explaining your decisions, or navigating non-technical conversations, you retreat. You need a manager who shields you from unnecessary noise but also pushes you gently into the rooms where your career gets shaped. One presentation won't kill you — and it might just change your trajectory.

💬 **Your Colleagues' View**
Your coworkers respect your craft. They know you're the one who can fix the thing when it breaks. When a technical question comes up, heads turn toward you.

But they find you hard to read. You don't share your process, your reasoning, or sometimes even your status. Collaborating with you can feel like throwing requests into a black box and waiting for output. When you take five minutes to walk someone through your thinking, you go from "the mysterious expert" to "the teammate everyone wants on their squad."

---

**13. The Charmer** *(高质量人类)*

👔 **Your Manager's View**
Your manager enjoys having you around. You're polished, articulate, and you make the team look good in front of leadership. You have a natural presence that can't be taught.

But your smoothness can create a quiet skepticism. Your manager may wonder: is this all polish, or is there depth behind it? You need to show your work — the messy drafts, the honest struggles, the moments of doubt. A manager who sees both your shine and your substance will champion you. One who only sees the shine may hesitate.

💬 **Your Colleagues' View**
Your coworkers are drawn to you. You have that "main character" energy that makes people want to be on your team. You make presentations look easy and networking look fun.

But some colleagues will keep a bit of distance. They've been burned by charm before. They wonder if you're as genuine in private as you are in public. The coworkers who get to know the real you — the one who also has bad days and insecurities — become your most loyal allies. Let a few people see behind the curtain.

---

**14. The Rule Follower** *(标准答案复制人)*

👔 **Your Manager's View**
Your manager loves that you're reliable. Deadlines are met, processes are followed, and nothing slips through the cracks. You're the person who reads the instructions — and actually follows them. In a team full of chaos, you're an anchor.

But your manager may wonder if you can operate without the playbook. When there's no clear process, do you freeze? When a situation calls for bending a rule, do you panic? You need a manager who appreciates your orderliness but gradually gives you permission to improvise. The most valuable people can follow the recipe and know when to throw it out.

💬 **Your Colleagues' View**
Your coworkers count on your consistency. They know that if you said you'd do it, it will be done. On time. Correctly. You're the person who keeps the team's operations from falling apart.

But they might not think of you when they need creative ideas. You're seen as the executor, not the innovator. The colleagues who ask for your opinion — not just your execution — are the ones who see your full potential. When you share an unconventional thought, even one, it shifts how people perceive you.

---

**15. The Wild Card** *(盲盒选手)*

👔 **Your Manager's View**
Your manager has a love-hate relationship with your unpredictability. When you're on, you're brilliant — insightful, creative, a step ahead of the room. When you're off, you're distracted, inconsistent, and hard to read. They never know which version is showing up.

The most important thing you can do for your career is narrow the variance. You don't need to be perfect every day; you need to be more consistent. A manager who can give you honest, real-time feedback about which days are which will be your greatest asset. Self-awareness about your own patterns turns "unpredictable" into "dynamic."

💬 **Your Colleagues' View**
Your coworkers find you memorable. You're the wildcard on the team — sometimes the unexpected genius, sometimes the person who derails the meeting. Working with you is never boring.

But "never boring" isn't always a compliment. Some colleagues will love your unpredictability; others will find it exhausting. The difference often comes down to whether you communicate. When you give people a heads-up about where your head is at, they can adjust. When you show up as a complete surprise every time, they stop trying to plan around you.

---

**16. The Imposter Syndrome Warrior** *(配得感洼地)*

👔 **Your Manager's View**
Your manager values you more than you know. They see your work, they see your potential, and they've probably said it to your face. The problem isn't their perception — it's yours. You keep waiting for someone to discover you're a fraud, but the only person who believes that story is you.

You need a manager who doesn't just give you praise, but gives you specific evidence. "Your analysis on that project changed our approach" lands differently than "good job." When you can't deny the facts, you start to believe them. A manager who documents your wins for you — literally puts them in writing — is the one who will unlock your confidence.

💬 **Your Colleagues' View**
Your coworkers respect you more than you realize. They see someone competent, reliable, and thoughtful. They don't hear the self-doubt that echoes in your head. In fact, some of them probably look up to you.

But your constant self-deprecation can be confusing. When you dismiss your own contributions, your colleagues start to wonder if they should doubt you too. The coworkers who build you up — who say "you actually crushed that" and mean it — are your mirrors. Believe them. They're not just being nice.

---

**17. The "I Can Fix It"** *(救火队长)*

👔 **Your Manager's View**
Your manager loves your initiative. When something breaks, you're already fixing it before anyone else notices. You don't wait for permission, and in a crisis, that's invaluable. You're the person they call when something absolutely has to get done.

But you bypass people. Your manager may appreciate the result while resenting the process. You make decisions that affect others without consulting them. You need a manager who can say "great initiative, next time loop in Sarah first." Someone who redirects your energy without extinguishing it.

💬 **Your Colleagues' View**
Your coworkers are grateful for you in a crisis. When the project is on fire, you're the one running toward it while others freeze. You've saved projects, and people remember that.

But you can also make people feel like obstacles. When you charge ahead without including them, the message is: "I don't trust you to handle this." Even if that's not what you mean, it's what they hear. The colleagues who get the best version of you are the ones you let in before the fire starts — not just when it's already burning.

---

**18. The Curious Cat** *(十万个为什么成精)*

👔 **Your Manager's View**
Your manager appreciates your intellectual hunger. You ask questions that make the room think harder. You're not satisfied with surface answers, and you push for deeper understanding. In brainstorming sessions, you're a spark.

But your manager also needs you to know when to stop asking and start doing. Endless curiosity without execution can stall a project. You need a manager who can say "great question — let's park that and move forward" without killing your spirit. Curiosity is your superpower; knowing when to act is your growth edge.

💬 **Your Colleagues' View**
Your coworkers enjoy your curiosity — in doses. You ask the questions no one else thinks of, and you make conversations richer. Your "what ifs" often lead to better ideas.

But you can also exhaust people. Not every moment is a deep dive. Sometimes your colleagues just need a quick answer, and your cascade of follow-up questions feels like a detour. When you balance curiosity with momentum — asking the best question rather than every question — you become the person everyone wants in the room.

---

**19. The Calm in the Storm** *(情绪稳定如死狗)*

👔 **Your Manager's View**
Your manager sees you as an anchor. When deadlines are crashing, when a client is escalating, when the team is panicking — you're steady. Your heart rate doesn't spike. You do not spiral. That kind of stability is rare and deeply valuable.

But your calm can be misread as indifference. Your manager may wonder if you actually care. When something big is at stake, they need to see not just your steadiness, but your investment. You need a manager who understands that calm and passion can coexist — and who reminds you to occasionally show the fire beneath the ice.

💬 **Your Colleagues' View**
Your coworkers feel safe around you. In chaotic moments, they glance at you and feel a little calmer themselves. You're the eye of the hurricane, and your presence stabilizes the team.

But your unflappable nature can also create distance. Colleagues may wonder if you're really invested or just going through the motions. When you share what you actually care about — when you let excitement or frustration show — people connect with you on a deeper level. The calm is a gift. The passion is the missing piece.

---

**20. The Visionary** *(大气层玩家)*

👔 **Your Manager's View**
Your manager appreciates your big-picture thinking. You see the future in a way that others don't. You're thinking about 2030 while everyone else is trying to get through Q3. That long-range vision is what companies need to stay ahead.

But your manager also needs you to land the plane. The weekly reports, the small deliverables, the mundane updates — these matter too. You need a manager who values your vision but also holds you accountable to the present. Someone who says "love the future, now show me the spreadsheet."

💬 **Your Colleagues' View**
Your coworkers are inspired by your thinking. Your ideas make them feel like they're building something meaningful. You give the team a sense of direction and purpose.

But they can also feel left behind. While you're operating at 30,000 feet, they're trying to figure out today's to-do list. When you connect the big vision to the next small step — "here's what this means for this week" — your colleagues go from inspired to empowered. The combination of vision and execution makes people want to follow you anywhere.

---

### 9.4 Typical Answer Path → Persona Quick-Reference Table

| Q1 | Q2 | Q3 | Voice Signal | Expected Persona |
|----|----|----|-------------|-----------------|
| A | A | A | Slow pace, high filler | 小组作业の神 / 情绪稳定如死狗 |
| B | B | B | Fast pace, high confidence | 预制卷王 / 救火队长 |
| C | C | C | Mid pace, "we" keyword | 情绪价值打工人 / 赛博热水 |
| B | C | C | Fast pace, "network" keyword | 社交悍匪 |
| A | B | B | Slow pace, "structure" keyword | 内耗 MVP |
| C | A | B | Mid pace, "data" keyword | Excel 成精 |
| B | B | A | Fast pace, "fix" keyword | 救火队长 |
| A | C | C | Mid pace, "story" keyword | 古希腊掌管叙事的神 |
| C | C | A | Slow pace, "sorry" keyword | 配得感洼地 |

---

### 9.6 "I Will Hire You If…" — Per-Persona Checklist (All 20)

Used by the Unlock page (section 4.3). Each persona has 4 checklist items; the UI displays the first 3. Each item is a concrete, recruiter-observable behavior — and a micro-step MYLS can coach.

**1. The Dependable Intern** *(小组作业の神)*
☐ You tell me what you specifically did, not "we" but "I."
☐ You add a confident "because" after every claim.
☐ You ask at least one bold question that shows you've been thinking.
☐ You show me you want the job, not just that you'll do it quietly.

**2. The High‑Potential Grinder** *(预制卷王)*
☐ You slow down to let me finish my question before you answer.
☐ You replace filler words like "basically" or "you know" with a pause.
☐ You structure your answer with a clear beginning, middle, and end.
☐ You mention how you collaborated, not just how you crushed it alone.

**3. The Case Study Natural** *(AI 的嘴替)*
☐ You open with a human sentence before the framework.
☐ You show me you care about the people in your case study, not just the logic.
☐ You make eye contact and vary your tone so you don't sound scripted.
☐ You tell me why this problem matters to you personally.

**4. The Coffee Chat Pro** *(社交悍匪)*
☐ You follow your charm with a specific, measurable result.
☐ You keep your answers under 90 seconds and every story has a landing.
☐ You show me your prep work, not just your networking.
☐ You demonstrate that you can deliver alone, not just with a room behind you.

**5. The Silent Sniper** *(潜水区大佬)*
☐ You walk me through your thought process out loud, step by step.
☐ You give me context before the conclusion.
☐ You share one story where you influenced a decision, not just executed it.
☐ You speak up without waiting for the perfect moment.

**6. The People Pleaser** *(情绪价值打工人)*
☐ You use "I" at least five times in this interview.
☐ You tell me about a time you disagreed with the team, and how it turned out.
☐ You set a boundary: "Here's what I need to do my best work."
☐ You show me your edge, not just your warmth.

**7. The Over‑Prepared Overthinker** *(内耗 MVP)*
☐ You trust your first draft, send it, and move on.
☐ You answer naturally instead of reciting a memorized script.
☐ You let a little spontaneity into the conversation.
☐ You admit when you don't know something without spiraling.

**8. The Storyteller** *(古希腊掌管叙事的神)*
☐ You give me the headline first, then the story.
☐ You cut the length of your best story in half, and it's still powerful.
☐ You check in: "Am I giving you what you need, or should I go deeper?"
☐ You balance narrative with hard numbers.

**9. The Data Whisperer** *(Excel 成精)*
☐ You tell me the "so what" after every number.
☐ You translate one complex metric into everyday language.
☐ You show me you can make decisions without 100% of the data.
☐ You connect the data to a human outcome.

**10. The Empathy Anchor** *(赛博热水)*
☐ You prove you have the hard skills to back up the soft ones.
☐ You take credit for a project you led without deflecting.
☐ You tell me about a time you prioritized your own goal over the group's comfort.
☐ You show me ambition, not just support.

**11. The Hustler** *(斜杠永动机)*
☐ You explain why this role, specifically, isn't just another side quest.
☐ You connect your many projects into one coherent career narrative.
☐ You mention a long‑term goal that ties to staying here.
☐ You show depth in one area, not just breadth.

**12. The Technical Purist** *(赛博老实人)*
☐ You explain a technical concept to me as if I'm non‑technical, clearly and warmly.
☐ You share a story about working with a difficult teammate.
☐ You show curiosity about the business side, not just the code.
☐ You practice behavioral answers out loud until they feel natural.

**13. The Charmer** *(高质量人类)*
☐ You let me see one genuine imperfection — a struggle, a doubt, a mistake.
☐ You follow your polished intro with a concrete, unpolished example.
☐ You go deeper on one topic rather than skating across five.
☐ You prove you can be serious when the moment calls for it.

**14. The Rule Follower** *(标准答案复制人)*
☐ You tell me about a time you bent a rule and got a better result.
☐ You share a creative idea, not just an executed one.
☐ You show me you can operate without the playbook.
☐ You ask "what if we tried this differently?" at least once.

**15. The Wild Card** *(盲盒选手)*
☐ You show up consistently, bringing your best self, not just a lucky day.
☐ You build a prep routine so your performance isn't a dice roll.
☐ You reflect on your patterns: what makes a good day good?
☐ You ground your unpredictable brilliance with one repeatable strength.

**16. The Imposter Syndrome Warrior** *(配得感洼地)*
☐ You land every sentence with conviction, no trailing off.
☐ You list three concrete wins you're proud of, and say them out loud.
☐ You remove "just," "maybe," and "sorry" from your vocabulary for this interview.
☐ You internalize that you're not lucky, you're good.

**17. The "I Can Fix It"** *(救火队长)*
☐ You tell me about a time you collaborated before the fire started.
☐ You say "we fixed it" instead of "I fixed it."
☐ You check in with at least one stakeholder before taking action.
☐ You show me you can prevent fires, not just fight them.

**18. The Curious Cat** *(十万个为什么成精)*
☐ You finish one complete thought before jumping to the next question.
☐ You turn one "why" into a "here's what I'd do."
☐ You demonstrate execution: "I'm curious about X, so I built Y."
☐ You balance deep questioning with decisive action.

**19. The Calm in the Storm** *(情绪稳定如死狗)*
☐ You show me a moment of genuine excitement — let the fire out.
☐ You express one strong opinion in the interview.
☐ You prove your calm isn't indifference by naming what you really care about.
☐ You raise your energy level to match the room when it matters.

**20. The Visionary** *(大气层玩家)*
☐ You connect your big vision to today's to‑do list.
☐ You give me one small, concrete example of a time you executed, not just imagined.
☐ You show patience for the boring stuff, because it funds the dream.
☐ You tell me what you'd do in your first 90 days, not just in 2030.

> UI note: Display first 3 items only (items 4 is stored in data layer, reserved for future activation).

---

### 9.7 Persona Analysis — Full Copy (EN + 中文, All 20)

Shown on the Results page in the "Persona Analysis" card. Format: 5 short lines (Playfair Display italic, 16px, `white-space: pre-line`). Tone: direct narrator voice — observational, slightly uncomfortable, always accurate.

---

**1. The Dependable Intern** *(小组作业の神)*

🇺🇸
"You did all the work, but someone else took all the credit.
But others and the interviewers aren't your teammates!
They have no idea how much you carried behind the scenes.
Yet when you speak, it's always 'we,'
making you sound like an outsider reporting on the team rather than someone who actually drove the work."

🇨🇳 活你都干了，但话全让别人说了。面试官又不是你组员，不知道你背后扛了多少。你一张嘴就是"我们"，听起来像个旁观者在替团队汇报。

---

**2. The High‑Potential Grinder** *(预制卷王)*

🇺🇸
"You prepared so intensely that every sentence sounds like you've rehearsed it ten times.
Like you're trying to prove everything at once.
Ambition is clear. Focus is not."

🇨🇳 你准备得太狠了，狠到每句话都像背了十遍。语速飙起来，重点全糊在一起。面试官知道你拼，但ta更想听清楚你到底在说什么。

---

**3. The Case Study Natural** *(AI 的嘴替)*

🇺🇸
"Your answers are airtight, with layers of structure stacked neatly on top of each other.
But they're so polished that they feel almost impersonal.
The interviewer sees that you're smart,
but isn't sure whether working with you would feel exhausting."

🇨🇳 你的回答滴水不漏，框架一层套一层。但太工整了，工整到没人味儿。面试官觉得你聪明，但不确定跟你共事会不会心累。

---

**4. The Coffee Chat Pro** *(社交悍匪)*

🇺🇸
"You connect easily with people.
You can make someone feel like an old friend in just three minutes.
But when the conversation gets too lively, it's easy to lose track of the point.
The interviewer likes your personality,
but afterward, they can't clearly remember what you can actually do."

🇨🇳 你特别会聊，三分钟就能让人觉得是老朋友。但聊嗨了容易忘了正事。面试官喜欢你的性格，回头想想，你具体能干什么活，ta其实没记住。

---

**5. The Silent Sniper** *(潜水区大佬)*

🇺🇸
"You're the kind of person who quietly gets big things done.
But you keep yourself too hidden.
If you don't say it, how will anyone know those impressive results were your work?"

🇨🇳 你是那种闷声干大事的人，活交出来谁都服。但面试的时候你把自己藏得太深了，你不说，人家怎么知道那些厉害的东西是你搞出来的。

---

**6. The People Pleaser** *(情绪价值打工人)*

🇺🇸
"You support the team and keep things smooth.
People rely on your presence.
But you shrink your own role.
You talk about others more than yourself.
You create value. You don't claim it."

🇨🇳 你习惯性地当团队的粘合剂，谁都顾到了就是没顾自己。面试官想了解你，你却在夸整个组。你的好，你不讲，没人替你讲。

---

**7. The Over‑Prepared Overthinker** *(内耗 MVP)*

🇺🇸
"You've prepared for everything.
You think through every possible answer.
But that pressure shows.
Your answers feel tight and controlled.
You sound careful. Not confident."

🇨🇳 你脑子里已经预演了八百种死法，每个问题都想好了标准答案。果然，面试时90%都是准备过的问题，结果你不自觉就开始背稿了……危险Flag已被悄悄打上了。

---

**8. The Storyteller** *(古希腊掌管叙事的神)*

🇺🇸
"You know how to tell a story.
You make things engaging and memorable.
But sometimes you go too far.
The story takes over the substance.
It sounds good. It proves less."

🇨🇳 讲故事是好手，开头引人入胜。但你收不住，讲着讲着就跑远了。面试官前面听得很爽，后面开始看表，因为ta还没听到重点，时间不够问完所有问题了。

---

**9. The Data Whisperer** *(Excel 成精)*

🇺🇸
"You genuinely love numbers — every conclusion has to be backed by data.
But the interviewer isn't looking for a dashboard; they want your judgment.
If you just present the results without explaining what they actually mean,
they'll keep nodding along while quietly accumulating more and more questions."

🇨🇳 对数字是真爱，每个结论都要有数据撑腰。但面试官要的不是看板，是你的判断。你光给结果，不说这个结果到底意味着什么，ta就只能一直点头，心里问号越攒越多。

---

**10. The Empathy Anchor** *(赛博热水)*

🇺🇸
"You understand people deeply.
You know how to read the room.
But you soften your answers too much.
You avoid being direct.
You feel supportive. Not decisive."

🇨🇳 你特别能理解别人，共情力是天赋。但你太怕说错话，把锋芒都收起来了。面试官觉得你很好相处，但也隐隐担心你为了和谐牺牲效率。

---

**11. The Hustler** *(斜杠永动机)*

🇺🇸
"You've worked on a ton of things. Your résumé is packed to the brim.
But after hearing you, the interviewer feels a bit overwhelmed.
It sounds like you've touched everything, but haven't gone deep on anything.
You've done so much that it's hard to find the throughline:
what are you actually best at?"

🇨🇳 折腾了一大堆东西，简历丰富到挤不下。但面试官听完有点晕，感觉你什么都碰过，但什么都还没沉下去。你做了太多，反而让人找不到主线：你到底最擅长什么？

---

**12. The Technical Purist** *(赛博老实人)*

🇺🇸
"Your code is beautifully written, your work is perfectly done,
but the moment you're asked, 'How do you handle team conflict?' you freeze.
Tech is your comfort zone,
but interviews aren't just about technical skills.
You also need to translate those experiences into human terms."

🇨🇳 代码写得那叫一个漂亮，但一被问到"你怎么处理团队冲突"，你就卡壳了。技术是你舒适区，可面试不只考技术，你还得把那些经历翻译成人话。

---

**13. The Charmer** *(高质量人类)*

🇺🇸
"You present well and speak smoothly.
You know how to leave a good impression.
But it can feel polished on the surface.
Depth is harder to see.
You sound strong. Proof feels light."

🇨🇳 从进门到开口都自带光环，面试官第一印象拉满。但聊深了发现，你说的怎么好像都是漂亮话，具体干了啥有点模糊。面试官脑子里飘过一个念头：这位是不是更适合当我老板？

---

**14. The Rule Follower** *(标答复制人)*

🇺🇸
"You follow structure and expectations.
You stay safe and consistent.
But nothing stands out.
Your answers feel predictable.
You avoid mistakes. You also avoid impact."

🇨🇳 你答得太标准了，标准到像抄了面经模版。面试官面了几个人转头就能把你忘了，因为你没露出一丁点自己的棱角。

---

**15. The Wild Card** *(盲盒选手)*

🇺🇸
"When you're on, you're great.
You can deliver strong answers.
But it's not consistent.
Performance changes from moment to moment.
They don't know which version they'll get."

🇨🇳 面试发挥纯靠手感。手感好了对答如流，手感差了连问题都听不清。面试官不是不信你能力，ta是不知道今天这个状态到底代表你几成水平。

---

**16. The Imposter Syndrome Warrior** *(配得感洼地)*

🇺🇸
"You know more than you show.
You've done more than you say.
But your voice pulls back.
Your confidence drops mid-answer.
You sound unsure. Even when you're right."

🇨🇳 明明比大多数人强，但开口总像在道歉。每句话说到后半段就开始虚，像在问对方"我说的对吗"。面试官本来觉得你不错，结果你一直强行给ta降预期。

---

**17. The "I Can Fix It"** *(救火队长)*

🇺🇸
"When you run into a problem, you charge straight in and get it done — that's impressive!
But once it's done, you forget to explain how you were thinking,
skipping the most critical step.
The interviewer only sees the result,
not the judgment and decisions that went on in your head along the way."

🇨🇳 碰到问题冲上去就是干，这点很猛。但你干完忘了告诉别人你是怎么想的，中间省略了最关键的一步。面试官只看到了结果，没看到你的脑子在中间做了多少判断。

---

**18. The Curious Cat** *(十万个为什么成精)*

🇺🇸
"You ask great questions.
You think beyond what's given.
But you shift the focus.
Too much curiosity, not enough clarity.
You explore well. You don't always answer directly."

🇨🇳 好奇心很宝贵，问出来的问题都挺有深度。但你得分清什么时候该问，什么时候该答。你把面试变成了双向探索，却忘了先把底牌亮清楚。提问之前，先让人看见你的实力。

---

**19. The Calm in the Storm** *(情绪稳定如死狗)*

🇺🇸
"You stay calm under pressure.
Nothing seems to shake you.
But that calm can feel flat.
Your energy doesn't come through.
You feel stable. Not impactful."

🇨🇳 稳，是真的稳。火烧到眉毛了你看上去像在度假。可是稳过头就让面试官有点慌了，他不确定你是真心想要这个岗位，还是拿他练手顺便看看。

---

**20. The Visionary** *(大气层玩家)*

🇺🇸
"You talk about five-year plans and industry shifts — big-picture thinking all the way.
But the interviewer first wants to know what you can do starting this week.
Don't stay up in the clouds,
show them where your feet hit the ground."

🇨🇳 聊的都是五年计划、行业变革，格局很大。但面试官想先知道这周你上手能干嘛。你别一直飘在云里，先告诉ta你落地的那一刻脚在哪。

---

### 9.8 Role Recommendation Matrix (Industry × Persona)

Used by Section 4.3 Role Match Card. 3 roles are selected per user based on their Target Industry + matched Persona.

**Role Pool by Industry:**

| Industry | Available Roles |
|----------|----------------|
| Tech / Engineering | Software Engineer, Product Manager, Data Analyst, UX Researcher, Solutions Engineer |
| Finance / Consulting | Investment Banking Analyst, Management Consultant, Financial Analyst, Strategy Associate, Risk Analyst |
| Marketing / Sales | Marketing Manager, Brand Strategist, Sales Development Representative, Growth Analyst, Content Strategist |
| Healthcare / Biotech | Clinical Research Associate, Healthcare Consultant, Biotech Analyst, Medical Affairs Associate, Health Policy Analyst |
| Other / Creative | Project Manager, Operations Analyst, Business Development Representative, UX Designer, Communications Manager |

**Selection Logic:** Each persona has a dominant dimension profile (Act / Comm / Inter / Conf / Strat). Roles are mapped to the 3 best-fit options within the user's selected industry based on that profile.

| Persona | Dominant Dimensions | Tech | Finance / Consulting | Marketing / Sales | Healthcare / Biotech | Other / Creative |
|---------|-------------------|------|---------------------|-------------------|---------------------|-----------------|
| The Dependable Intern | Low Act, Mid Inter | Data Analyst, UX Researcher, Software Engineer | Financial Analyst, Risk Analyst, Strategy Associate | Growth Analyst, Content Strategist, Marketing Manager | Clinical Research Associate, Biotech Analyst, Medical Affairs Associate | Operations Analyst, Project Manager, UX Designer |
| The High-Potential Grinder | High Act, High Conf, High Strat | Product Manager, Software Engineer, Data Analyst | Investment Banking Analyst, Management Consultant, Strategy Associate | Marketing Manager, Sales Development Representative, Growth Analyst | Healthcare Consultant, Health Policy Analyst, Biotech Analyst | Business Development Representative, Project Manager, Operations Analyst |
| The Case Study Natural | High Comm, High Strat | Product Manager, Data Analyst, Solutions Engineer | Management Consultant, Strategy Associate, Investment Banking Analyst | Brand Strategist, Marketing Manager, Growth Analyst | Healthcare Consultant, Health Policy Analyst, Medical Affairs Associate | Project Manager, Communications Manager, Business Development Representative |
| The Coffee Chat Pro | High Inter, High Conf, High Comm | Product Manager, Solutions Engineer, UX Researcher | Management Consultant, Investment Banking Analyst, Strategy Associate | Sales Development Representative, Marketing Manager, Brand Strategist | Medical Affairs Associate, Healthcare Consultant, Health Policy Analyst | Business Development Representative, Communications Manager, Project Manager |
| The Silent Sniper | Low Comm, Low Act, Mid Strat | Software Engineer, Data Analyst, UX Researcher | Financial Analyst, Risk Analyst, Strategy Associate | Growth Analyst, Content Strategist, Brand Strategist | Clinical Research Associate, Biotech Analyst, Medical Affairs Associate | Operations Analyst, UX Designer, Project Manager |
| The People Pleaser | High Inter, Low Conf | UX Researcher, Product Manager, Solutions Engineer | Risk Analyst, Financial Analyst, Strategy Associate | Marketing Manager, Content Strategist, Brand Strategist | Clinical Research Associate, Medical Affairs Associate, Healthcare Consultant | Communications Manager, Project Manager, Operations Analyst |
| The Over-Prepared Overthinker | High Strat, Low Conf | Data Analyst, Software Engineer, Product Manager | Financial Analyst, Risk Analyst, Management Consultant | Growth Analyst, Brand Strategist, Content Strategist | Biotech Analyst, Clinical Research Associate, Health Policy Analyst | Operations Analyst, Project Manager, UX Designer |
| The Storyteller | High Comm, High Inter | Product Manager, UX Researcher, Solutions Engineer | Management Consultant, Strategy Associate, Investment Banking Analyst | Brand Strategist, Content Strategist, Marketing Manager | Medical Affairs Associate, Healthcare Consultant, Health Policy Analyst | Communications Manager, Business Development Representative, Project Manager |
| The Data Whisperer | High Comm, High Strat | Data Analyst, Software Engineer, Product Manager | Financial Analyst, Risk Analyst, Strategy Associate | Growth Analyst, Marketing Manager, Brand Strategist | Biotech Analyst, Clinical Research Associate, Health Policy Analyst | Operations Analyst, Project Manager, UX Designer |
| The Empathy Anchor | High Inter, Low Act | UX Researcher, Product Manager, Solutions Engineer | Risk Analyst, Financial Analyst, Strategy Associate | Content Strategist, Marketing Manager, Brand Strategist | Clinical Research Associate, Medical Affairs Associate, Healthcare Consultant | Communications Manager, UX Designer, Project Manager |
| The Hustler | High Act, High Conf | Product Manager, Solutions Engineer, Software Engineer | Investment Banking Analyst, Strategy Associate, Management Consultant | Sales Development Representative, Marketing Manager, Growth Analyst | Healthcare Consultant, Health Policy Analyst, Medical Affairs Associate | Business Development Representative, Project Manager, Operations Analyst |
| The Technical Purist | Low Comm, High Act | Software Engineer, Data Analyst, UX Researcher | Financial Analyst, Risk Analyst, Strategy Associate | Growth Analyst, Content Strategist, Brand Strategist | Biotech Analyst, Clinical Research Associate, Medical Affairs Associate | UX Designer, Operations Analyst, Project Manager |
| The Charmer | High Conf, High Comm, High Inter | Product Manager, Solutions Engineer, UX Researcher | Management Consultant, Investment Banking Analyst, Strategy Associate | Sales Development Representative, Brand Strategist, Marketing Manager | Medical Affairs Associate, Healthcare Consultant, Health Policy Analyst | Business Development Representative, Communications Manager, Project Manager |
| The Rule Follower | High Strat, Mid Comm | Data Analyst, Software Engineer, Product Manager | Financial Analyst, Risk Analyst, Management Consultant | Growth Analyst, Content Strategist, Marketing Manager | Clinical Research Associate, Biotech Analyst, Health Policy Analyst | Operations Analyst, Project Manager, UX Designer |
| The Wild Card | Mid across all dimensions | Product Manager, Solutions Engineer, UX Researcher | Strategy Associate, Management Consultant, Financial Analyst | Marketing Manager, Brand Strategist, Sales Development Representative | Healthcare Consultant, Medical Affairs Associate, Biotech Analyst | Project Manager, Business Development Representative, Communications Manager |
| The Imposter Syndrome Warrior | Low Conf, Mid Comm | Data Analyst, UX Researcher, Software Engineer | Financial Analyst, Risk Analyst, Strategy Associate | Content Strategist, Growth Analyst, Brand Strategist | Clinical Research Associate, Biotech Analyst, Medical Affairs Associate | Operations Analyst, UX Designer, Project Manager |
| The "I Can Fix It" | High Act, High Conf, Low Comm | Software Engineer, Product Manager, Solutions Engineer | Investment Banking Analyst, Strategy Associate, Management Consultant | Sales Development Representative, Marketing Manager, Growth Analyst | Healthcare Consultant, Health Policy Analyst, Medical Affairs Associate | Business Development Representative, Project Manager, Operations Analyst |
| The Curious Cat | Mid Act, High Comm, Mid Inter | UX Researcher, Product Manager, Data Analyst | Strategy Associate, Management Consultant, Risk Analyst | Brand Strategist, Content Strategist, Growth Analyst | Health Policy Analyst, Healthcare Consultant, Medical Affairs Associate | Communications Manager, UX Designer, Project Manager |
| The Calm in the Storm | Low Act, Mid Conf | Data Analyst, UX Researcher, Software Engineer | Risk Analyst, Financial Analyst, Strategy Associate | Content Strategist, Growth Analyst, Brand Strategist | Clinical Research Associate, Biotech Analyst, Medical Affairs Associate | Operations Analyst, Project Manager, UX Designer |
| The Visionary | High Comm, High Conf, Mid Strat | Product Manager, Solutions Engineer, UX Researcher | Strategy Associate, Management Consultant, Investment Banking Analyst | Brand Strategist, Marketing Manager, Sales Development Representative | Health Policy Analyst, Healthcare Consultant, Medical Affairs Associate | Business Development Representative, Communications Manager, Project Manager |
