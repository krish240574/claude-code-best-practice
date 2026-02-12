# Plan: Presentation Refactor — "From Vibe Coding to Agentic Engineering"

## Context

The current presentation is an 82-slide onboarding guide organized as a flat reference manual. The user wants to transform it into a **story-driven journey** that shows developers how each Claude Code best practice incrementally moves them from unstructured "vibe coding" (0%) to fully agentic engineering (100%). The presentation should use a **TodoApp monorepo** (FastAPI + Next.js) as a running example throughout.

## Core Concept: The Journey Progress Bar

Every slide shows a fixed bar at the top: `Vibe Coding ←[bar]→ Agentic Engineering` with a percentage (0%→100%). Weighted slides display a `+N%` badge. The bar color transitions from red (0%) to green (100%).

---

## Phase 1: Add Journey Bar Infrastructure (CSS + JS only)

**File:** `presentation/index.html`

### CSS additions (inside `<style>`)
- `.journey-bar` — fixed bar below the existing progress bar with left label "Vibe Coding", center percentage, right label "Agentic Engineering"
- `.journey-track` / `.journey-fill` — the gradient progress track (red→green via HSL interpolation)
- `.weight-badge` — green pill badge showing `+N%` next to slide titles
- Increase `.slide` top padding from 60px → 100px to accommodate the new bar

### JS additions (inside `<script>`)
- Read `data-weight` attributes from slides at page load to build a `SLIDE_WEIGHTS` map
- Pre-compute `cumulativeWeights[]` array (running sum for each slide)
- `updateJourneyBar(slideNum)` — updates fill width, color (HSL hue 0→120), percentage text, and weight badge
- Call `updateJourneyBar()` inside existing `showSlide()` function
- Hide journey bar on slide 1 (title slide)

### HTML addition
- Insert `<div class="journey-bar">` element after the existing progress bar div

**No existing slides are modified in this phase.**

---

## Phase 2: New Opening Slides + Section Dividers

### New slides to create:

| # | Slide | Content |
|---|-------|---------|
| 1 | **Title** | "Claude Code: From Vibe Coding to Agentic Engineering" |
| 2 | **The Example Project** | TodoApp monorepo structure: `/backend` (FastAPI CRUD), `/frontend` (Next.js + sidebar nav) — shown as a code-block directory tree |
| 3 | **What is Vibe Coding?** | Two-column: "What Happens" (bad — random routes, no patterns) vs "What We Want" (good — follows architecture). Establishes the 0% baseline |
| 4 | **Journey Map** | New TOC showing the 6 parts of the journey with clickable navigation |

### New section dividers (replace old ones):
Each shows current journey % and the Part name:
- **Part 2: Better Prompting** (journey: 0%)
- **Part 3: Project Memory** (journey: 20%)
- **Part 4: Structured Workflows** (journey: 40%)
- **Part 5: Domain Knowledge** (journey: 55%)
- **Part 6: Agentic Engineering** (journey: 70%)
- **Appendix: Reference** (no percentage)

### New agent example slides:
- **Frontend Engineer Agent** (+5%) — before/after showing how without the agent Claude creates random pages, with the agent it uses existing sidebar nav, Tailwind tokens, and API client
- **Backend Engineer Agent** (+5%) — before/after showing how the agent follows existing FastAPI route patterns, SQLAlchemy models, and adds tests
- **The 100% Slide** — celebration slide showing the fully agentic TodoApp

---

## Phase 3: Reorder Slides + Assign Weights

### Weight Distribution (must sum to 100%)

| Journey % | Slide Topic | Weight |
|-----------|-------------|--------|
| 0% → 5% | Good vs Bad Prompts | +5% |
| 5% → 10% | Providing Context (@files) | +5% |
| 10% → 15% | Context Window & /compact | +5% |
| 15% → 20% | /plan mode | +5% |
| 20% → 25% | CLAUDE.md & /init | +5% |
| 25% → 30% | What to Include in CLAUDE.md | +5% |
| 30% → 40% | Rules (.claude/rules/) | +10% |
| 40% → 45% | Task Lists | +5% |
| 45% → 50% | /model Selection | +5% |
| 50% → 55% | What Are Skills | +5% |
| 55% → 60% | Creating Skills (TodoApp) | +5% |
| 60% → 65% | Skill Frontmatter & Invocation | +5% |
| 65% → 70% | What Are Agents | +5% |
| 70% → 75% | Frontend Engineer Agent | +5% |
| 75% → 80% | Backend Engineer Agent | +5% |
| 80% → 90% | Commands & Orchestration | +10% |
| 90% → 95% | Hooks + MCP Servers | +5% |
| 95% → 100% | Full Architecture (Command → Agent → Skills) | +5% |
| **Total** | | **100%** |

> **Decisions:** CLAUDE.md/init = +5% (not +10%). Commit/PR workflows = no weight (moved to appendix). Extra 5% redistributed to "Full Architecture" slide as the final capstone.

### New section structure:

**Part 0: Introduction (slides 1-4, no weight)**
- Title, Example Project, What is Vibe Coding, Journey Map

**Part 1: Prerequisites (slides 5-9, no weight)**
- Installing, Login-Subscription, Login-API Key, First Session (this IS vibe coding), The Interface

**Part 2: Better Prompting (slides 10-17, 0%→20%)**
- Section divider, Your First Prompt, Good vs Bad Prompts (+5%), Providing Context (+5%), Context Window & /compact (+5%), /plan mode (+5%), Plan Mode in Practice, Prompting Best Practices

**Part 3: Project Memory (slides 18-24, 20%→45%)**
- Section divider, CLAUDE.md & /init (+10%), What to Include (+5%), Keep Under 150 Lines, Rules (+10%), /memory & Hierarchy, Memory Best Practices

**Part 4: Structured Workflows (slides 25-28, 40%→50%)**
- Section divider, Task Lists (+5%), /model Selection (+5%), Workflow Best Practices

**Part 5: Domain Knowledge (slides 29-33, 50%→65%)**
- Section divider, What Are Skills (+5%), Creating Skills (+5%), Frontmatter & Invocation (+5%), Skills Summary

**Part 6: Agentic Engineering (slides 34-48, 65%→100%)**
- Section divider, What Are Agents (+5%), Frontend Engineer Agent (+5%), Backend Engineer Agent (+5%), Agent Frontmatter, Agent Tools & Skills, Subagents, Commands & Orchestration (+10%), Hooks + MCP (+5%), Full Architecture: Command → Agent → Skills (+5%), The 100% Slide, Agent Memory, Agents Summary

**Appendix: Reference (slides 49+, no weight)**
All non-weighted command references, workflows, and settings slides:
- How Claude Uses Tools, File Operations, Bash & Search
- /help, Effort Level, /fast, /context & /cost, /clear & /rewind, /resume, /doctor, /config, /permissions, /sandbox, Commands Cheat Sheet
- Commit Workflow, PR Workflow, Workflow Best Practices
- Default Main Agent, settings.json, Spinner Verbs, Status Line, Output Styles, Keybindings, Terminal & Vim, Customization Summary
- Debugging Tips, Golden Rules, Resources, Next Steps, Thank You

### Slide merges (reduce total count):
- CLAUDE.md concept + /init command → single slide
- /memory + memory hierarchy → single slide
- Context Window + /compact → single slide
- /plan + Why Plan First → single slide
- Hooks + Plugins + MCP → combined slide

### Renumbering approach:
After all moves/merges, run a Python script to:
1. Renumber all `data-slide` attributes sequentially (1, 2, 3, ...)
2. Update `totalSlides` in JS
3. Update slide counter text
4. Update `goToSlide()` calls in TOC

---

## Phase 4: Thread TodoApp Examples (Detailed Code Snippets)

Update existing slides with **detailed, realistic code snippets** from the TodoApp:

- **Good vs Bad Prompts**: Show actual FastAPI route code — bad prompt produces random `/notes` endpoint, good prompt targets existing `/api/todos` with validation
- **Providing Context**: Full `@backend/routes/todos.py` reference showing actual route definitions, `@frontend/components/TodoList.tsx` with component code
- **CLAUDE.md**: Full TodoApp CLAUDE.md example (~20 lines) showing project structure, tech stack, conventions, API patterns
- **Rules**: Complete `backend-testing.md` rule scoped to `backend/tests/**` with pytest conventions, fixture patterns, assertion style
- **Skills**: Full `frontend-conventions/SKILL.md` with Tailwind tokens, component patterns, sidebar navigation integration rules
- **Frontend Agent**: Complete `.claude/agents/frontend-engineer.md` frontmatter + instructions showing how it uses existing `/frontend/components/`, adds routes to sidebar layout, follows design tokens
- **Backend Agent**: Complete `.claude/agents/backend-engineer.md` with FastAPI route patterns, SQLAlchemy model conventions, test requirements
- **Commands & Orchestration**: Full TodoApp feature workflow command showing how it coordinates frontend + backend agents

---

## Phase 5: Create `/update-presentation` Command Pipeline

Following the weather orchestration pattern (Command → Agent → Skills):

### Files to create:

| File | Purpose |
|------|---------|
| `.claude/commands/update-presentation.md` | Entry point command — asks user what to change, invokes agent via Task tool |
| `.claude/agents/presentation-updater.md` | Agent with preloaded skills — handles content/structure changes |
| `.claude/skills/presentation-structure/SKILL.md` | Knowledge about slide format, weight system, JS navigation |
| `.claude/skills/presentation-styling/SKILL.md` | Knowledge about CSS classes, component patterns, syntax highlighting |

### Command flow:
```
User runs /update-presentation
  → Command asks what changes are needed (AskUserQuestion)
  → Command invokes presentation-updater agent (Task tool)
    → Agent reads presentation-structure skill (understands format)
    → Agent reads presentation-styling skill (matches conventions)
    → Agent makes changes, renumbers slides, verifies weights = 100%
```

---

## Execution Order

1. **Phase 1** (Infrastructure) — safest, no content changes, testable immediately
2. **Phase 5** (Command pipeline) — new files only, no HTML changes
3. **Phase 2** (New slides) — insert new content, existing slides untouched
4. **Phase 3** (Reorder) — highest risk, full restructure
5. **Phase 4** (TodoApp examples) — update content within finalized structure

Work on a git branch: `presentation-refactor`

## Verification

After completion, validate in browser console:
```javascript
const slides = document.querySelectorAll('[data-slide]');
console.log('Total slides:', slides.length);
let weightSum = 0;
slides.forEach(s => { weightSum += parseInt(s.dataset.weight || 0); });
console.log('Weight sum:', weightSum, weightSum === 100 ? 'OK' : 'ERROR');
let ok = true;
slides.forEach((s, i) => {
    if (parseInt(s.dataset.slide) !== i + 1) { ok = false; console.error('Gap at', i+1); }
});
console.log('Sequential numbering:', ok ? 'OK' : 'ERROR');
```
