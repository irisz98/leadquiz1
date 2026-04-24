# New Grad Aura Check — Lead Quiz Design Spec
**Date:** 2026-04-14  
**Product:** MYLS Interview — New Grad Aura Check  
**Deliverable:** Single-file HTML click-through prototype (9 screens)

---

## 1. Overview

A registration-free, shareable quiz that drives viral acquisition for MYLS Interview 3.0. Users complete 3 situational MCQs + 1 voice prompt and receive a "New Grad Potential Report" with a persona label, radar chart, and voice analysis. A blurred/locked section reveals 3 personalized interview questions upon sharing.

**Target audience:** North American university students (Sophomores–Recent Grads) seeking internships or new grad roles.  
**Distribution:** GroupMe, Instagram, LinkedIn, Discord — shared as a single link.

---

## 2. Prototype Spec

- **Format:** Single HTML file, self-contained (no build step, no server)
- **Navigation:** JS state machine — screen transitions via fade/slide animation
- **Viewport:** Mobile-first, 375px reference width
- **Fonts:** DM Sans (Google Fonts CDN)

---

## 3. Visual Direction: Aura Scan

**Aesthetic:** Dark, structured, AI-premium. Concentric rings + subtle grid texture on hero convey the "aura scan" concept. No glassmorphism, no pink/orchid accent — blue-only brand palette throughout.

**Color tokens used:**
| Token | Value | Usage |
|---|---|---|
| bg-base | #09091A | All screen backgrounds |
| bg-surface | #0F0F26 | Cards, prompt boxes, radar chart bg |
| primary | #3D51FF | Progress bar, selected states, borders |
| primary-light | #6D7CFF | Labels, eyebrows, icons |
| gradient-a | #BA8FFF → #717FFF | Gradient CTA only (hero + share button) |
| text-primary | #FFFFFF | Headlines |
| text-secondary | rgba(255,255,255,0.55) | Body copy |
| text-muted | rgba(255,255,255,0.30) | Labels, hints, footers |
| selected-bg | rgba(61,81,255,0.12) | Tag / MCQ selected fill |
| selected-border | rgba(61,81,255,0.45) | Tag / MCQ selected border |
| error | #FF5C5C | Record button only |

**Typography:**
- All screens: DM Sans
- Headlines: 700–800 weight
- Body/labels: 400–600 weight
- Scale follows .impeccable.md type tokens

---

## 4. Screen-by-Screen Specification

### Screen 1 — Hero
- **Background:** #09091A + 36px grid (5% opacity) + 2 concentric rings (30% / 12% opacity) + soft radial glow
- **Content:** Eyebrow "New Grad Aura Check" (uppercase, #6D7CFF), H1 "Decode Your New Grad First Impression", subhead, 3 meta pills (60 seconds · 3 questions · 1 voice prompt)
- **CTA:** Gradient button "Decode My Aura →" (full width, 14px border-radius)
- **Footer note:** "No signup · Audio is never stored" (10px, 22% opacity)
- **Transition:** Button tap → fade to Screen 2

### Screen 2 — Identity Tags
- **Header:** Thin progress bar (10% fill, #3D51FF), step label "Step 1 of 5", headline "Tell us about yourself"
- **Section 1:** "I am a..." — 3 pill options: Sophomore/Junior · Senior · Recent Grad
- **Section 2:** "Target Industry" — 4 pill options: Tech/Engineering · Finance/Consulting · Marketing/Sales · Other
- **Tag selection style:** Selected = rgba(61,81,255,0.12) fill + rgba(61,81,255,0.45) border + #8FA9FF text. Both groups use same blue.
- **CTA:** Primary blue "Continue →" button (active only when both groups have a selection)
- **Transition:** Continue → fade to Screen 3

### Screens 3–5 — MCQ Template (reused × 3)
- **Header:** Progress bar (33% / 66% / 100% fill) + fraction label "1/3", "2/3", "3/3"
- **Question:** Small uppercase label "Situational Question", question text in 17px/700, "You..." in 15px/500 muted
- **Options:** 3 rows. Each: 28px letter badge (A/B/C, 8px radius, 1px border), option text 13px. Full-width tap target.
- **Selection interaction:**
  1. Tapped option → blue border + fill + letter badge fills solid blue
  2. Other options → 35% opacity (dimmed)
  3. "✓ Recorded — moving to next question..." note appears
  4. 600ms delay → fade to next screen
- **Content Q1:** Group project / ghosted teammate (3 options per PRD)
- **Content Q2:** Career Fair 45-min line (3 options per PRD)
- **Content Q3:** Resume walk-through opener (3 options per PRD)

### Screen 6 — Voice Prompt
- **Header:** Progress bar (85% fill) + "Step 4 of 5", headline "Let's hear how you sound."
- **Prompt card:** Surface bg (#0F0F26), blue border, label "Your Prompt", italic text "Describe yourself in three words — and tell us why."
- **Waveform area:** Surface bg, 17 bars of varying heights (3px wide, #3D51FF, 70% opacity), timer "0:00 / 0:30"
- **Record button:** 64px circle, #FF5C5C, ring shadow rgba(255,92,92,0.12), white square icon (stop shape)
- **Skip link:** "Skip voice analysis →" text link at bottom (22% opacity)
- **Prototype interaction:** Tap Record → waveform animates (CSS keyframes), countdown ticks → tap Stop → fade to Screen 7

### Screen 7 — Analysis Loader
- **Background:** bg-base + same 36px grid
- **Visual:** 3 concentric rings + radar sweep arm (CSS transform-origin rotation, 2s linear infinite) + 5 axis lines
- **Copy:** Uppercase label "Analyzing Profile", rotating message (3 states, 1.2s interval):
  1. "Benchmarking against 45,000+ New Grad JDs..."
  2. "Generating your personalized 2026 recruiting questions..."
  3. "Analyzing speech patterns and filler words..."
- **Progress:** 3-dot indicator, one active at a time
- **Auto-advance:** 3.5s total → fade to Screen 8

### Screen 8 — Results: Persona + Radar
- **Header:** "Your Report" label + "New Grad Aura Report" title
- **Persona card:** Dark gradient bg (#1a1a3e → #0F0F26), blue border, persona emoji + "Your Persona" label + persona name (20px/800) + one-liner verdict. Subtle radial glow top-right corner.
- **Radar chart:** SVG pentagon (5-axis: Communication, Execution, Grit, Learning Agility, Collaboration). 3 grid rings. Data polygon filled rgba(61,81,255,0.20) with #3D51FF stroke + dots.
- **Voice analysis:** 3 row cards on #0F0F26. Each: emoji icon in blue-tinted 32px box, label + sub-hint left, value right. (Pace WPM / Keywords / Confidence %)
- **Scroll:** Screen scrolls. Bottom continues to Screen 9 (or swipe/button to continue)

### Screen 9 — Results: Locked → Unlocked
- **Header:** Back arrow + "Your Interview Questions"
- **Intro copy:** "Based on your aura profile, MYLS generated 3 high-probability interview questions tailored to you."
- **Locked state:** 3 question cards (blurred with CSS `filter: blur(4px)`), gradient overlay (rgba fade to #09091A 85%), lock icon + "Share with a friend to unlock your questions"
- **Primary CTA:** Gradient button "Share with a Friend to Unlock" → triggers `navigator.share()` (native share sheet)
- **Unlocked state (post-share):** Blur removed instantly, full question text revealed, record button replaced by "Register for MYLS 3.0 to watch your personalized video feedback" card
- **Secondary CTA:** Text link "Practice this out loud on MYLS →" → opens MYLS signup URL
- **Footer:** "Powered by MYLS Interview · Privacy Policy" + privacy consent copy

---

## 5. Interaction & Animation Specs

| Interaction | Behaviour |
|---|---|
| Screen transitions | Fade (opacity 0→1), 250ms ease-out |
| MCQ selection | Instant highlight, 600ms delay before advance |
| Tag selection | Instant color change, no delay |
| Continue button lock | Disabled/semi-opaque until both tag groups selected |
| Record button | Waveform bars animate via CSS keyframes on active |
| Radar sweep | CSS rotate animation, 2s linear infinite |
| Loader copy rotation | JS setInterval, 1200ms per message |
| Auto-advance loader | 3500ms total, then fade to results |
| Blur removal (unlock) | CSS transition on filter: blur, 400ms |
| Share CTA | `navigator.share()` → on resolve: unlock animation |

---

## 6. Persona Mapping (Prototype uses one fixed persona)

Prototype shows **"The High-Potential Grinder"** as the demo output. Full dynamic mapping per Appendix 9 of PRD is a backend concern, not prototyped.

- Emoji: ⚡
- Name: The High-Potential Grinder
- Verdict: "You come across as driven and results-focused. Pro tip: Slow down — you risk sounding scripted under pressure."
- Radar scores: Communication 80%, Execution 90%, Grit 75%, Learning Agility 65%, Collaboration 70%
- Voice: 146 WPM · Keywords: Project, Lead, Team · Confidence: 75%

---

## 7. What's Not Prototyped

- Real microphone recording / audio analysis API
- Dynamic persona calculation from answers
- UTM tracking / referral loop
- Dynamic OG image generation
- MYLS signup redirect (placeholder link)
- Text fallback for mic-denied state (skip link covers this)

---

## 8. File Output

- **Single file:** `new-grad-aura-check.html`
- **Location:** `E:\mine\工作\My Learning Space\New Grad Aura Check\`
- **Dependencies:** Google Fonts CDN (DM Sans) — requires internet connection to render correctly
