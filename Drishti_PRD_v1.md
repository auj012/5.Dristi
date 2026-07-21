# Drishti — Product Requirements Document (v1)

> **Drishti** (Sanskrit: vision, insight, perspective) — *See beyond your resume.*

## TL;DR

Drishti is an AI-powered career intelligence agent built on one thesis: **career decisions should not be based solely on who a person has been, but also on who they are becoming.** Traditional platforms match jobs to resumes (the past). Drishti matches jobs to a candidate's *trajectory* — combining historical experience, current learning and projects, and stated future goals — and explains every recommendation with strengths and gaps. **v1 is a deliberately thin, local-first, single-user slice** that proves trajectory-aware matching beats keyword search, before any APIs, multi-agent orchestration, or roadmap generation are added.

---

## 1. Problem

Job seekers spend enormous time searching across boards whose recommendation engines rely on keyword matching against historical resume data. These systems fail to understand career progression, transferable skills, active learning, and aspirations — so candidates in transition (the people who need discovery most) receive recommendations for the job they are leaving, not the job they are becoming qualified for.

**Example:** a candidate whose resume says *Product Manager* — but who is building AI applications, studying agentic AI and Responsible AI, and targeting AI Product Management — will be shown Product Manager roles forever. The transition is invisible to the machine, because the evidence of it lives outside the resume.

## 2. Product Thesis & Core Design Principle

> **Drishti = Resume (Past) + Activity (Present) + Aspirations (Future) → Opportunities + Gaps.**

Three signal types, deliberately weighted (assumption: not all career signals have equal value):

| Signal | Evidence of | Examples | v1 Weight |
|---|---|---|---|
| **Historical** | Proven capability | Resume, roles, education, certifications | 0.4 |
| **Current** | Active skill development | Projects, courses, GitHub, portfolio | 0.3 |
| **Intent** | Future direction | Target roles, goals, interests | 0.3 |

Weights are tunable and are themselves an experiment.

**Score output per job (data contract):**

```json
{
  "job_id": "job_001",
  "past_match": 85,
  "present_match": 78,
  "future_match": 92,
  "drishti_score": 84,
  "rank": 1
}
```

## 3. Target Users

**Primary — The Ambitious Professional:** mid/senior (5–15 years), employed or exploring, wants relevant opportunities fast, explanations for every match, and a view of gaps. Success criterion in their words: *"Show me jobs I would realistically consider applying for — and help me understand what I need to do to reach my goals."*

**Secondary — The Career Transitioner:** 3–10 years, actively pivoting to a new role or industry. Traditional platforms don't understand transferable skills; Drishti's three-signal model is built precisely for this person.

## 4. v1 Scope

### Included
- Resume upload (PDF/DOCX) and parsing into a structured profile
- **Signals form**: current courses, projects, portfolio/GitHub links, target roles, career goals
- **Career Trajectory Profile** (JSON) — the core data contract (see §6)
- **Curated local job dataset** (100–300 hand-gathered JDs in 1–2 target domains, e.g., Product → AI Product / Applied AI)
- **Trajectory-aware matching**: separate Past / Present / Future match scores combined into a single **Drishti Score**
- **Insight output** per recommendation, in a fixed template:
  - **WHY RECOMMENDED** — strengths across Past / Present / Future signals
  - **POTENTIAL CONCERNS** — gaps and mismatches, stated plainly
  - **ASSESSMENT** — one-paragraph verdict with actionable advice (e.g., "highlight your AI learning activities in your application")
- **Feedback capture**: "Would you seriously consider applying?" — Yes / No / Maybe

### Explicitly excluded from v1 (parked, not abandoned)
Live job APIs · multi-agent orchestration · automated applications · resume rewriting · career roadmap generation · progress tracking · multi-user/SaaS · enterprise integrations · GitHub/portfolio scraping.

*First API step after v1 (v1.5): RemoteOK + Remotive — the two simplest integrations; remote-only scope keeps normalization easy. Adzuna / Jooble / USAJobs follow later.*

## 5. Functional Requirements (v1)

| # | Requirement | Priority | Acceptance |
|---|---|---|---|
| FR-1 | Accept resumes in PDF or DOCX format | P0 | File uploads and parses successfully |
| FR-2 | Extract a structured candidate profile | P0 | Key fields extracted; target ≥ 80% accuracy pre-validation |
| FR-3 | User reviews and corrects the extracted profile | P0 | All parsing errors correctable before matching |
| FR-4 | Collect Present and Intent signals via a short form | P0 | Signals saved into the profile |
| FR-5 | Generate the Career Trajectory Profile (JSON) | P0 | Valid against the frozen schema |
| FR-6 | Load and query the curated local job dataset | P0 | Jobs queryable by skills/title/location |
| FR-7 | Compute Past / Present / Future scores → Drishti Score | P0 | Sub-scores + composite produced per job |
| FR-8 | Rank and display top 5–10 recommendations | P0 | Ranked list rendered |
| FR-9 | Explain each recommendation across all three signals, plus gaps | P0 | Strengths + concerns + assessment per job |
| FR-10 | Provide actionable advice for identified gaps | P2 | Specific suggestion per gap |
| FR-11 | Record Yes / No / Maybe feedback per recommendation | P1 | Feedback persisted with job id + score |
| FR-12 | Personal data stays local; user can export their data as JSON | P0 / P3 | No external transmission; export available |

## 6. Career Trajectory Profile (Data Contract)

```json
{
  "past_roles": [],
  "core_skills": [],
  "certifications": [],
  "industries": [],
  "years_experience": 0,
  "current_learning": [],
  "projects": [],
  "portfolio_links": [],
  "target_roles": [],
  "career_goals": [],
  "career_stage": "e.g., transition_to_ai_product"
}
```

This schema is the contract between every module (and every future agent). It is versioned and frozen in Phase A.

## 7. Architecture (v1 — modules first, agents later)

```
Resume + Signals Form
        ↓
[1] Trajectory Intelligence  →  Career Trajectory Profile (JSON)
        ↓
[2] Job Inventory            →  Curated local dataset (SQLite + ChromaDB embeddings)
        ↓
[3] Trajectory Matching      →  Past/Present/Future scores → Drishti Score
        ↓
[4] Insight                  →  Why recommended (×3 signals) + gap analysis
        ↓
[5] Feedback                 →  Yes/No/Maybe logged per job
```

**Matching methodology (three passes):** semantic similarity via embeddings (retrieve top-K) → structured matching (skills, experience, education, certifications, location) → intent alignment (goals, learning activities, target roles). Modules are promoted to true agents in Phase 3, when orchestration itself becomes the learning objective — not before.

## 8. Non-Functional Requirements

- **Local-first privacy (P0):** resume and personal data never leave the machine; clear consent; data minimization
- **Performance (P0):** recommendations in under 10 seconds for 90% of requests, on local data
- **Explainability (P0):** every recommendation and score explained; transparency about what data is used
- **Cost (P0):** free/open-source stack throughout
- **Maintainability (P1):** modular code, documented interfaces, unit + integration tests, README and user guide

## 9. Technology Stack

Python 3.11+ · **Streamlit** (v1 — fastest to validate; pivot to React only if production demands) · **Ollama** with **Qwen 2.5** primary (Llama 3 / Mistral alternates) · **BGE-small** or all-MiniLM embeddings · **ChromaDB** (chosen over FAISS for Python ergonomics) · **SQLite** (PostgreSQL only at scale) · FastAPI deferred until a real API consumer exists.

## 10. Evaluation Framework

| Evaluation | Method | Target |
|---|---|---|
| Extraction accuracy | Parsed profile vs. hand-labeled resume fields | **≥ 90% on key fields** |
| Recommendation approval | Human eval per recommendation | **≥ 70% rated relevant (first 10)** |
| Would-apply rate | "Would you apply?" on top 5 | **≥ 50% Yes/Maybe** |
| **Recommendation diversity** | % of recommendations a keyword search would NOT surface | **≥ 30% non-obvious matches** — directly tests the thesis |
| Hallucination rate | No fake jobs, skills, or certifications — validated against sources | **< 1%** |
| Performance | Latency measurement | < 10 seconds (90% of requests) |
| Satisfaction | Post-session 1–10 rating | ≥ 7 average |

Same discipline as my other projects: golden data, honest misses, published results. Feedback is both the quality metric and the future re-weighting signal.

## 11. Risks & Mitigations

| Risk | Mitigation |
|---|---|
| Resume parsing inaccuracies | Rule-based + LLM extraction; mandatory user review/edit before matching |
| Recommendation hallucinations | Explainability + confidence scoring; hallucination test suite; validate jobs exist |
| Limited job data (no APIs in v1) | Curated dataset scoped to 1–2 domains done well |
| Performance on local hardware | Small embedding model (BGE-small / MiniLM), caching, batched LLM calls |
| Feature creep *(observed personally during discovery)* | Ideas go to the backlog; MoSCoW prioritization; build validates few hypotheses at a time |
| Insufficient training data | Retrieval and ranking — no model training in v1 |

## 12. Roadmap

- **Phase A — Foundation (≈ wks 1–2):** branding & one-page vision · this PRD · trajectory profile schema frozen
- **Phase B — Trajectory Profile (≈ wks 3–5):** resume ingestion · signals form · profile JSON for a test user
- **Phase C — Job Intelligence (≈ wks 6–7):** curated 100–300 JD dataset · normalization · SQLite + ChromaDB
- **Phase D — Matching & Insight (≈ wks 8–11):** three-signal scoring · explanations + gaps · Streamlit UI · feedback loop → **working Drishti v1 demo**
- **Future phases:** RemoteOK + Remotive APIs (v1.5) → multi-agent orchestration → **Career Intelligence** (gap analysis, learning recommendations, personalized roadmaps — "what do I need to do to get there?")

## 13. Success Criteria

**v1 ships when:** a resume + signals form produces a validated trajectory profile; 5–10 ranked recommendations appear in under 10 seconds; every recommendation carries the WHY / CONCERNS / ASSESSMENT template; feedback is captured; and the evaluation targets in §10 are measured and reported — hits and misses alike.

**The project succeeds if it demonstrates:** AI product management (discovery → PRD → thin-slice delivery), AI engineering (local LLM, embeddings, vector search), AI evaluation (human eval, diversity metric, hallucination testing), and Responsible AI (local-first privacy, consent, explainability) — end to end, on a problem the builder is living.

## 14. North Star

Traditional systems ask: *"What jobs match my resume?"*
Drishti asks: *"What jobs match who I am becoming?"*
And ultimately: *"What do I need to do to get there?"*

---
*Status: discovery complete · PRD v1 (merged from Perplexity + DeepSeek drafts + original PRD) · Phase A in progress · Ushasree Jakilinki · github.com/auj012*
