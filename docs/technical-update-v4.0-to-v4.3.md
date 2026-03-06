# Production Grade Plugin — Technical Update: v4.0 to v4.3

## Summary

Four releases in three days (March 4-6, 2026) transformed the plugin from a greenfield-only build pipeline into an adaptive orchestrator for all software engineering work, with a complete visual identity system and intelligent routing.

---

## v4.0 — Two-Wave Parallelism & Internal Skill Agents (March 4)

### What changed

The orchestrator was rewritten to execute work in two parallel waves instead of sequentially.

**Before (v3.x):** Tasks ran one after another. Build finished, then QA started, then security started. A 13-task pipeline executed serially.

**After (v4.0):** The pipeline splits into Wave A (build + analysis in parallel) and Wave B (execution against written code in parallel).

### Technical details

- **Wave A** launches up to 7+ concurrent agents: backend engineer, frontend engineer, DevOps, QA (test plan), security (STRIDE model), code reviewer (checklist), SRE (SLO definitions). Build and analysis happen simultaneously — analysis agents work from architecture specs, not code, so they don't need to wait.
- **Wave B** launches 4 agents that operate against written code: DevOps (build containers), QA (implement tests), security (code audit), code reviewer (actual review).
- **Internal skill parallelism** — 8 skills now spawn their own parallel sub-agents. Software engineer spawns 1 agent per service. Frontend engineer spawns 1 agent per page group. QA spawns 4 agents (unit/integration/e2e/performance). Security spawns 4 agents (code audit/auth/data/supply chain).
- **Dynamic task generation** — the orchestrator reads architecture output (number of services, pages, modules) and creates tasks accordingly. No hardcoded task count.
- **Parallelism preference** — user selects at pipeline start: Maximum (recommended), Standard, or Sequential. No config file — runtime prompt.

### Performance impact

- ~3x faster execution for multi-service projects (7+ concurrent agents vs sequential)
- ~45% fewer total input tokens (each parallel agent carries minimal context instead of accumulating prior work)
- Sequential mode preserved for debugging/inspection

---

## v4.1 — Engagement Modes & Scale-Driven Architecture (March 5)

### What changed

Two problems solved: (1) one-size-fits-all interaction depth frustrated both power users and beginners, (2) architecture was picked from templates instead of derived from constraints.

### Engagement modes

Four interaction depth levels, chosen at pipeline start:

| Mode | PM Interview | Architect Discovery | Decision Surfacing |
|------|-------------|--------------------|--------------------|
| **Express** | 2-3 questions | Auto-derive from BRD | 3 gates only, fully autonomous |
| **Standard** | 3-5 questions | 5-7 questions, 2 rounds | Surface 1-2 critical decisions per skill |
| **Thorough** | 5-8 questions + competitive analysis | 12-15 questions, 4 rounds, capacity planning | Surface all major decisions, show phase summaries |
| **Meticulous** | 8-12 questions, multi-round, co-authored criteria | Full walkthrough + individual ADR approval | Every decision point surfaced, approve each output |

Engagement mode propagated to ALL 14 skills via `settings.md`. Every agent reads it at startup and adapts its depth.

### Architecture Fitness Function

The Solution Architect no longer picks from templates. Architecture is DERIVED from project constraints:

- **Inputs:** scale (users, CCU), team size, budget, compliance requirements, data patterns, geographic distribution, growth model, uptime SLA, vendor strategy
- **Output:** architecture pattern, infrastructure sizing, data strategy — all derived from the constraint profile
- A 100-user internal tool gets a monolith with managed services. A 10M-user global platform gets microservices with multi-region deployment.

### Quality-first parallelism changes

- **Software Engineer:** shared foundations (types, errors, middleware, auth, config) built SEQUENTIALLY before parallel service agents. Prevents N different error handling implementations.
- **Frontend Engineer:** UI primitives built SEQUENTIALLY first, then layout + feature components in PARALLEL. Prevents duplicate Button/Input implementations.

---

## v4.2 — Adaptive Routing & 10 Execution Modes (March 6)

### What changed

The plugin stopped being greenfield-only. Before v4.2, every invocation ran the full 14-skill pipeline. Now the orchestrator classifies the user's request and runs only the relevant skills.

### Request classification

The orchestrator analyzes the user's message and maps to one of 10 modes:

| Mode | Trigger Examples | Skills |
|------|-----------------|--------|
| **Full Build** | "build a SaaS", "from scratch" | All 14, full pipeline |
| **Feature** | "add auth", "new endpoint" | PM (scoped) + Architect (scoped) + BE/FE + QA |
| **Harden** | "review", "audit", "secure" | Security + QA + Code Review (parallel) + Remediation |
| **Ship** | "deploy", "CI/CD", "terraform" | DevOps + SRE |
| **Test** | "write tests", "test coverage" | QA only |
| **Review** | "review my code", "code quality" | Code Reviewer only |
| **Architect** | "design", "how should I structure" | Solution Architect only |
| **Document** | "write docs", "API docs" | Technical Writer only |
| **Explore** | "help me think", "I'm not sure" | Polymath only |
| **Optimize** | "performance", "slow", "scale" | Code Reviewer + SRE |
| **Custom** | Doesn't fit patterns | Multi-select skill menu |

### Execution differences by mode

- **Single-skill modes** (Test, Review, Architect, Document, Explore): classify and invoke immediately. No plan presentation.
- **Multi-skill modes** (Feature, Harden, Ship, Optimize, Custom): present execution plan for confirmation. User can adjust, escalate to full pipeline, or proceed.
- **Full Build**: unchanged from v4.0/4.1 — full DEFINE > BUILD > HARDEN > SHIP > SUSTAIN pipeline.
- **Lightweight modes**: skip engagement mode and parallelism prompts for 1-2 skill modes (overhead not worth it).

---

## v4.3 — Visual Identity, Observability & Intelligent Routing (March 6)

### What changed

Three things: (1) a complete visual design system for terminal output, (2) structured observability so users can track what every agent is doing, (3) prompt engineering for reliable skill activation.

### Visual Identity Protocol

New shared protocol (`visual-identity.md`) defining the complete design language. Aesthetic: sleek, elegant, high-tech, informative.

**Design principles:**
- Information is the aesthetic — every printed line carries data, no decoration
- Earned elevation — visual weight matches informational weight
- Concrete over vague — numbers in every status line, never "analysis complete"
- Dynamic contrast — different visual patterns for different information types

**3-tier container hierarchy:**

| Tier | Character | Usage | Frequency |
|------|-----------|-------|-----------|
| Tier 1 | `╔═══╗` double-line box | Pipeline dashboard, gate ceremonies, final summary | 3-5 per run |
| Tier 2 | `┌───┐` single-line box | Wave announcements, agent status boards | 6-10 per run |
| Tier 3 | `━━━` heavy rule | Skill headers, findings summaries, section breaks | Frequent |

**Standardized icon vocabulary:**
`◆` brand mark, `⬥` gate marker, `●` active, `○` pending, `✓` complete, `✗` failed, `⧖` in-progress, `⚠` warning, `→` flow/transition. No emoji — Unicode symbols only for monospace alignment and consistent rendering.

### Pipeline Dashboard

Printed at kickoff and reprinted at every phase transition and gate. Shows all 5 phases with live status:

```
╔══════════════════════════════════════════════════════════════╗
║  ◆ PRODUCTION GRADE v4.3                         ⏱ 4m 23s  ║
║  Project: multi-vendor-marketplace                          ║
╠══════════════════════════════════════════════════════════════╣
║                                                              ║
║   DEFINE    ✓ complete    ⏱ 3m 12s                          ║
║   BUILD     ● active      ⏱ 1m 45s                          ║
║   HARDEN    ○ pending                                        ║
║   SHIP      ○ pending                                        ║
║   SUSTAIN   ○ pending                                        ║
║                                                              ║
╚══════════════════════════════════════════════════════════════╝
```

The dashboard re-rendering at each transition IS the progress animation — Claude Code streams text token-by-token, so users watch the dashboard build itself line by line.

### Gate Ceremonies

Visual framing around the 3 approval gates. Each gate prints a metrics block with concrete data before the approval prompt:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  ⬥ GATE 2 — Architecture Approval                  ⏱ 3m 12s
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Pattern      Modular monolith with event-driven boundaries
  Stack        TypeScript · NestJS · PostgreSQL · Redis
  Services     4 bounded contexts
  API          12 endpoints across 3 specs
  ADRs         4 architecture decision records

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Wave Announcements & Checkmark Cascades

When launching a parallel wave, a Tier 2 box shows all agents and their assignments. When the wave completes, the "checkmark cascade" prints each agent's results — the peak visual moment:

```
┌─ WAVE A COMPLETE ─────────────────────────── ⏱ 4m 23s ─┐
│                                                          │
│  ✓ Software Engineer    4 services, 12 endpoints         │
│  ✓ Frontend Engineer    4 page groups, 23 components     │
│  ✓ DevOps               4 Dockerfiles, 1 compose         │
│  ✓ QA Engineer          test plan: 47 test cases         │
│  ✓ Security Engineer    STRIDE: 6 threats identified     │
│  ✓ Code Reviewer        checklist: 15 checkpoints        │
│  ✓ SRE                  4 SLOs, 12 alert rules           │
│                                                          │
│  7/7 complete                                            │
│  → Starting Wave B (4 agents against written code)       │
└──────────────────────────────────────────────────────────┘
```

Every completion line MUST include concrete numbers. No `✓ QA Engineer — complete`. The numbers prove the system did real work.

### Numbered Phase Progress (Per Skill)

Every skill prints structured progress during its own execution:

```
━━━ Software Engineer ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  [1/5] Context & Architecture
    ✓ Read 4 ADRs, 3 API specs, 1 ERD
    ✓ Identified 4 services, 12 endpoints

  [2/5] Shared Foundations
    ✓ libs/shared/types — domain types, DTOs
    ✓ libs/shared/errors — error hierarchy
    ⧖ libs/shared/middleware — auth, logging, validation
    ○ libs/shared/config — env vars, secrets
```

All 13 sub-skills updated with skill-specific phase counts, step indicators, and completion summaries.

### Additional visual patterns

- **Transition announcements:** `→ Starting BUILD phase (Wave A: 7 agents)` between phases/waves
- **Findings severity grid:** Critical/High with detail, Medium/Low with counts, dedup total
- **Before→after deltas:** `12 findings → 0 Critical remaining`, `0% → 94% coverage`
- **Elapsed timing:** tracked at 3 levels — total pipeline, per-phase, per-wave

### Upgraded Final Summary

```
╔══════════════════════════════════════════════════════════════════╗
║                                                                  ║
║   ◆  PRODUCTION GRADE v4.3 — COMPLETE                ⏱ 12m 47s  ║
║   Project: multi-vendor-marketplace                              ║
║                                                                  ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║   DEFINE    ✓ BRD (12 stories, 34 criteria)                      ║
║             ✓ Architecture (modular monolith, 4 services)        ║
║                                                                  ║
║   BUILD     ✓ Backend (4 services, 12 endpoints, 2847 lines)     ║
║             ✓ Frontend (4 page groups, 23 components)            ║
║             ✓ Containers (4 Dockerfiles, 1 compose)              ║
║                                                                  ║
║   HARDEN    ✓ Security (12 findings → 0 Critical remaining)      ║
║             ✓ QA (147 tests, 100% passing)                       ║
║             ✓ Code Review (8 findings → all resolved)            ║
║                                                                  ║
║   SHIP      ✓ Infrastructure (Terraform, 3 environments)        ║
║             ✓ CI/CD (GitHub Actions, 4 workflows)                ║
║             ✓ SRE (4 SLOs, 12 alerts, 3 runbooks)               ║
║                                                                  ║
║   SUSTAIN   ✓ Documentation (3 docs generated)                   ║
║             ✓ Custom Skills (4 project-specific)                 ║
║                                                                  ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║   Agents: 12 used · Tasks: 22 completed · Errors: 0             ║
║   Files: 89 created · Tests: 147 passing · Vulnerabilities: 0   ║
║                                                                  ║
╚══════════════════════════════════════════════════════════════════╝
```

### Intelligent Routing (Prompt Engineering)

The skill description was rewritten for reliable activation by Claude Code's routing model.

**Problem:** Claude Code could build websites natively, so the routing model skipped the skill even when the user said "build me a website." The skill description was passive — it described capabilities without telling Claude Code it SHOULD use them.

**Solution — three layers:**

1. **Enhancement framing:** Description now says "enhances Claude Code from producing raw code into delivering production-ready systems" — positions the skill as a capability upgrade, not a replacement.

2. **`<IMPORTANT>` activation block:** Added at the top of the skill body with explicit trigger phrases ("build me a...", "create a...", "I want to build...") and low-downside framing ("overhead of invoking unnecessarily is near zero").

3. **Courtesy prompt:** If Claude Code still decides not to invoke the skill, the description instructs it to ask the user: "Would you like this production-ready? I can run a structured pipeline with architecture design, testing, security audit, and CI/CD — not just code files." Non-aggressive, user-respecting. If the user declines, Claude proceeds normally.

**Result — three possible outcomes for any build request:**
- Best: skill is invoked directly (routing matches)
- Good: Claude asks if the user wants production-grade execution (courtesy prompt)
- Worst: neither happens (significantly less likely with all three layers)

---

## Cumulative Change Summary (v4.0 → v4.3)

| Metric | v3.x (before) | v4.3 (after) |
|--------|---------------|--------------|
| Execution | Sequential, 13 tasks in order | Two-wave parallel, 7+ concurrent agents |
| Use cases | Greenfield builds only | 10 modes: build, feature, harden, ship, test, review, architect, document, explore, optimize |
| Interaction depth | One-size-fits-all | 4 engagement modes (Express → Meticulous) |
| Architecture | Template-based | Constraint-derived (fitness function) |
| Visual output | Minimal (`⧖`/`✓` lines) | Full design system: dashboard, gate ceremonies, wave announcements, findings grids, numbered progress |
| Observability | No progress visibility during parallel work | Phase dashboard, wave start/end boards, per-skill `[1/N]` progress, concrete metrics in every status line |
| Routing | User must invoke explicitly | Enhancement framing + courtesy prompt for automatic activation |
| Token efficiency | Sequential accumulation | ~45% fewer tokens via parallel minimal-context agents |
| Speed | ~1x (serial) | ~3x (parallel waves + internal skill parallelism) |

### Files changed across v4.0-v4.3

- 1 new protocol: `visual-identity.md`
- 1 updated protocol: `ux-protocol.md`
- 1 orchestrator: `production-grade/SKILL.md` (major)
- 5 phase dispatchers: `phases/*.md`
- 13 sub-skill files: all updated with engagement modes, visual output, progress patterns
- 3 metadata files: `plugin.json`, `CHANGELOG.md`, `README.md`
