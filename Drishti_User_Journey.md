# AI Product Discovery — User Journey — Drishti

**Author:** Ushasree Jakilinki
**Date:** 2026-08
**Status:** Discovery — user journey (grounds the PRD).

> **Drishti** (Sanskrit: vision, insight) — *See beyond your resume.*

---

## 1. Personas & Jobs To Be Done

| **Primary Users** | **Secondary Users** |
| --- | --- |
| The Ambitious Professional (mid/senior, 5–15 yrs) | Recruiters & hiring platforms |
| The Career Transitioner (3–10 yrs, actively pivoting) | Learning platforms |
| The Continuous Learner (upskilling toward a new role) | Career coaches / mentors |

### Job To Be Done

> When I'm searching for my next role *while actively learning and changing direction*, help me find jobs that match **who I'm becoming** — not just my resume — and show me the gaps and the path to get there, so I stop wasting time on roles that ignore my trajectory.

- **What triggers the need?** A job search during a transition; a new skill or project; a decision to pivot.
- **What does success look like?** A ranked list of roles I'd *realistically* consider, each explained, with the gaps named — in one pass.
- **What alternatives are used today?** LinkedIn, job boards, Google, spreadsheets, and a lot of manual guessing (see §2).

---

## 2. Current User Journey (Before Drishti)

### Journey board — phases, actions, and pain

|  | ① SEARCH | ② RECOMMENDED | ③ APPLY BLIND | ④ SILENCE | ⑤ RE-STRATEGIZE |
| --- | --- | --- | --- | --- | --- |
| **Job-seeker does** | Opens LinkedIn / boards · Searches by title & keywords | Gets jobs matched to the resume · Sees the roles they're *leaving* | Guesses which roles fit who they're *becoming* · Applies broadly | Ghosted · No feedback · Few or no callbacks | Guesses what to learn / highlight · Starts over |
| **Pain** | Keyword search matches the *past*, not the trajectory | The transition is invisible; recommendations point backward | Manual and slow; can't tell which roles they're realistically qualified for | No explanation, no gaps, no path | No roadmap — only guessing; the loop restarts |

> **The loop is the problem.** Every mismatch sends the seeker back to guessing — what to learn, what to highlight, which roles to try — and the whole process restarts. Weeks pass while the transition stays invisible to every platform.

---

## 3. Pain Points & User Needs

| **Backward-Looking Matching** | **Invisible Trajectory** | **No Guidance** | **Wasted Effort** |
| --- | --- | --- | --- |
| Matches on resume keywords (the past) | Current learning & projects ignored | No explanation for a match | Time lost on irrelevant recommendations |
| Transferable skills not understood | Aspirations / target roles ignored | No gaps identified | Applications into a black hole |
| One title → the same roles forever | Evidence of "becoming" lives on GitHub, Medium, courses, portfolio — unread | No path to the goal | Repeated re-strategizing |

> ### ⇩ And the result of all of it:
>
> ## **The job-seeker becomes the trajectory integrator.**

**Key insight:** to find roles that fit who they're becoming, seekers must manually reconcile their resume, their current learning, their aspirations, and scattered job listings — because **no platform reads their trajectory. No single participant owns the "becoming" workflow.**

---

## 4. Future AI User Journey (With Drishti)

### Journey board — who does the work now

|  | ① SHARE | ② TRAJECTORY PROFILE | ③ MATCH & SCORE | ④ INSIGHT | ⑤ DECIDE |
| --- | --- | --- | --- | --- | --- |
| **Job-seeker does** | Shares resume + signals (courses, projects, GitHub, target roles, goals) | *nothing to do* | *nothing to do* | *nothing to do* | Reviews clear matches + gaps · Applies with confidence — or knows what to build |
| **Drishti does** | Ingests resume & signals | Builds the **Career Trajectory Profile** (Past / Present / Future) | Computes Past / Present / Future scores → **Drishti Score**, ranks | Explains every match: **WHY** / **CONCERNS (gaps)** / **ASSESSMENT & advice** | *(future)* Recommends the **learning path** to close the gap |

> ✓ **No loop.** The seeker acts twice — *share*, then *decide*; Drishti does everything in between, in one pass, with explanations and gaps.

### Worked example — a Product Manager becoming an AI Product Manager

|  | Signal | What Drishti sees | Weight |
| --- | --- | --- | --- |
| **Past** | Resume: Product / Program Manager, 25 yrs | Proven delivery & product leadership | 0.4 |
| **Present** | Building AI agents, studying agentic AI, MS Applied Data Science | An active pivot into AI — **invisible to keyword search** | 0.3 |
| **Future** | Target role: AI Product Manager | Clear, stated intent | 0.3 |

| Result | Drishti Score | Insight |
| --- | --- | --- |
| **AI Product Manager** | **0.86** | **WHY:** strong trajectory across all three signals · **GAP:** limited *shipped* AI-product depth · **ADVICE:** lead with your AI projects and evaluation work |

**Why this example matters:** a keyword engine shows this person *Product Manager* roles forever, because the evidence of the transition lives outside the resume. Drishti reads the *becoming* — and surfaces the role a resume-based system never would.

---

## 5. What Traces Back to the Journey

Every Drishti capability answers a pain from §3:

| Pain (from §3) | Capability |
| --- | --- |
| Matches on resume keywords (the past) | **Trajectory Matching** — Past + Present + Future, not keywords |
| Current learning & aspirations ignored | **Signal Intelligence** — projects, courses, GitHub, target roles as first-class inputs |
| No explanation, no gaps | **Explainable Insight** — WHY / CONCERNS / ASSESSMENT per match |
| No path to the goal | **Career Intelligence** *(future)* — gap analysis + learning path |

---

*User journey grounds the product. Every capability traces back to a pain in the current journey — Drishti's discipline, mirrored from the Sanjeevani discovery.*
