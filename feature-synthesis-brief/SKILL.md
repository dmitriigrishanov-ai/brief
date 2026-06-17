---
name: feature-synthesis-brief
description: >
  Synthesizes raw design research inputs — feature requirements, user interview notes, session recordings, metrics data, sticky note exercise photos, ideation session outputs, or any combination — into a structured, interactive HTML feature brief dashboard. Use this skill whenever a designer, PM, or researcher says things like "summarize this feature", "turn these notes into a brief", "here are my user interview notes", "I have sticky note outputs", "build me a feature summary", "here's what we learned from research", or uploads screenshots of whiteboards/sticky notes alongside feature context. Always use this skill when the user wants to go from messy raw inputs to a structured feature brief, even if they don't use those exact words.
---

# Feature Synthesis Brief Skill

Turn raw, messy design inputs into a polished, interactive HTML feature brief dashboard — ready to share with designers, PMs, engineers, and leadership.

---

## What You Produce

A single self-contained HTML file that renders as an interactive brief dashboard with:

- Collapsible sections for each part of the brief
- Charts/visualizations for any metric data provided
- Color-coded signal tags (user research, stakeholder input, data/analytics, assumption)
- Analyst flags — gaps, risks, open questions Claude identifies
- Audience toggle or clear labeling so different readers find what they need

---

## Input Types to Handle

Accept any combination of:
- **Text**: feature requirements, PRD fragments, Slack threads, meeting notes, interview transcripts
- **Images**: photos of sticky note exercises, whiteboard captures, affinity maps, screenshots
- **Metrics**: numbers, tables, CSV-style data, or verbal descriptions of metrics
- **Recordings/transcripts**: user interview quotes, usability session notes

If the user gives you partial information, work with what you have and explicitly flag what's missing.

---

## Output Structure

Produce the brief with these sections (all collapsible, all present even if sparse):

### 1. Feature Title & One-Liner
- Synthesize a clear feature name if not given
- One sentence: what it does and for whom

### 2. Problem Statement
- What user or business problem does this solve?
- Be specific. Avoid vague language like "improve experience"
- Flag if the problem statement is unclear or assumed

### 3. Signal Sources
- Where did this idea come from? Tag each signal:
  - 🧑 User research (interviews, usability, surveys)
  - 📊 Data / analytics
  - 💼 Stakeholder / business input
  - 💡 Internal assumption or hypothesis
- If an insight has no clear source, flag it as ⚠️ Unvalidated assumption

### 4. User Benefit
- What does the user gain?
- Tie directly to signals from research if available
- Quote user language where possible

### 5. Business Benefit
- What does the company gain? (retention, revenue, NPS, competitive positioning, etc.)
- If no business case is stated, flag it as a gap

### 6. Metrics & Success Criteria
- Render any provided metrics as a chart (bar, line, or simple stat cards depending on data type)
- Proposed success metrics if not provided — but clearly label them as suggested, not confirmed
- Flag if there are no measurable success criteria

### 7. What We're NOT Solving (Scope Boundaries)
- Explicitly state what's out of scope
- If not provided, flag this as a risk (scope creep)

### 8. Open Questions & Gaps (Claude's Analysis)
- This is where Claude adds value beyond summarizing
- Flag: missing data, conflicting signals, unclear ownership, untested assumptions, risks
- Use a clear visual treatment (warning color, icon) to distinguish these from the summarized content

### 9. Recommended Next Steps
- 2–4 concrete actions: e.g., "Validate problem statement with 5 more user interviews", "Define success metric with data team", "Align with engineering on technical constraints"

---

## HTML Dashboard Spec

Build a single self-contained HTML file. No external dependencies except CDN-hosted Chart.js for charts.

### Visual Design — Dark Mode
- Dark background: `#0E0E10`, surface cards: `#18181C`, borders: `#2E2E38`
- Instructure orange `#E66000` as primary accent — use for active states, stat numbers, step circles
- All text uses dark-mode-safe shades: primary `#E8E8F0`, muted `#7A7A90`, dim `#50505E`
- Flag blocks use transparent dark overlays: amber `rgba(240,180,40,0.12)`, red `rgba(239,68,68,0.12)`, green `rgba(16,185,129,0.12)`
- Tags use dark translucent backgrounds with colored borders and text

### Layout — Fixed Sidebar + Topbar
- Fixed topbar (56px) with feature title, badge, date, and audience toggle pills
- Fixed left sidebar (220px wide) with section nav links, scrollspy highlighting active section
- Main content area with `margin-left: 220px`, max-width ~780px

### Sidebar Contents
1. **Nav links** — one per section, with emoji icon. Add a red/amber badge on the Gaps link showing the count of flags.
2. **Ready to Build? score** — percentage readiness (0–100%) based on how many key inputs are present: problem defined, user research present, success metrics defined, scope finalised, business case stated, engineering alignment noted. Show as a large number, a color-coded verdict ("Ready", "Almost", "Not yet"), an animated fill bar, and a row-by-row checklist with ✓ / ~ / ✗.
3. **Feature Heat gauge** — a hot-cold score (0–100) rendered as a semicircle doughnut chart using Chart.js. Score is computed from 4 dimensions, each rated 1–10: User Pain, Business Value, Evidence Strength, Feasibility. Show each dimension as a mini bar below the gauge with its score. Color the gauge fill: blue (<40 cold), green (40–59), yellow (60–74), orange (75–89), red (90+ 🔥).

### Interactivity
### Page Architecture
Each brief section is a separate page. The sidebar nav switches between pages (JS routing, no reload). No audience toggle — one unified view.

Pages: Dashboard (stat cards + all charts + critical flags summary), one page per brief section, Add Input (type selector + paste area + context + section checkboxes + session log), Sources & Links (add/remove URLs with title/type badge).

- Sidebar nav switches pages with active orange highlight
- Scrollspy: sidebar nav item highlights as user scrolls to each section
- Audience toggle in topbar: Designer / PM / Engineer / Leadership — hide cards not relevant to selected audience
- Charts render inline using Chart.js from CDN

### Chart Guidance
- If metric data is numeric and time-based → line chart
- If metric data is categorical comparisons → bar chart
- If only single numbers/stats → render as large stat cards
- If no metrics data → show an empty state card: "No metrics provided — add data to see charts here"

---

## Tone & Analytical Voice

- Summarize faithfully — don't invent facts
- Be analytical: connect dots between signals, spot contradictions, notice what's missing
- Flag gaps and risks clearly but constructively — not as criticism, as open questions
- Use plain language. Avoid jargon like "leverage synergies" or "unlock value"
- Write as a thoughtful senior designer/researcher reading the inputs critically

---

## Step-by-Step Process

1. **Read all inputs carefully** — text, images, metrics, quotes
2. **Extract signal by type** — user research, data, stakeholder, assumption
3. **Synthesize each section** — use only what's given, flag what's missing
4. **Run your own analysis** — identify gaps, risks, contradictions, open questions
5. **Build the HTML dashboard** — self-contained, Chart.js for metrics, collapsible sections
6. **Save and present the file** — output to `/mnt/user-data/outputs/feature-brief.html`

---

## Example Flag Messages

Use these patterns when flagging gaps:

- ⚠️ **Unvalidated assumption** — This insight has no cited user research or data source. Recommend validating before committing to scope.
- ⚠️ **Missing business case** — No business benefit was provided in the inputs. Clarify the business rationale before stakeholder review.
- 🚨 **Scope risk** — No out-of-scope boundaries defined. This feature may expand during development without clear guardrails.
- ⚠️ **No success metrics** — There are no measurable success criteria in the inputs. Suggest working with the data team to define at least one primary metric.
- 🚨 **Conflicting signals** — User interviews suggest X, but stakeholder requirements suggest Y. Recommend alignment session before moving to design.
