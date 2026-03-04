# Production Grade Plugin for Claude Code

**Turn an idea into a production-ready SaaS with a single prompt.** This plugin transforms Claude Code into a complete software development pipeline — from requirements analysis to production deployment. You sit in the CEO/CTO seat, Claude handles the rest.

> **v2.0** — All 13 skills bundled. No additional installs needed.

---

## Table of Contents

- [Overview](#overview)
- [Installation](#installation)
- [Usage](#usage)
- [Pipeline Phases](#pipeline-phases)
- [13 Bundled Skills](#13-bundled-skills)
- [Workspace Architecture](#workspace-architecture)
- [Detailed Workflow](#detailed-workflow)
- [Approval Gates](#approval-gates)
- [Partial Execution](#partial-execution)
- [Examples](#examples)
- [Configuration](#configuration)
- [FAQ](#faq)
- [License](#license)

---

## Overview

Production Grade Plugin is a **meta-skill orchestrator** for Claude Code. When you say "Build me a SaaS for X", the plugin automatically runs the entire software development pipeline:

```
DEFINE → BUILD → HARDEN → SHIP → SUSTAIN
```

### Highlights

| Feature | Description |
|---------|-------------|
| **Fully Autonomous** | Only 3 approval gates (BRD, Architecture, Production Readiness) |
| **13 Expert Skills** | Each skill is a specialized "expert" agent |
| **Zero Config** | Install once, all skills ready to go |
| **Real Code** | No stubs, no TODOs — production-ready code that builds and runs |
| **Self-Debugging** | Auto-detects errors, fixes, and retries up to 3 times |
| **Production Standards** | Clean architecture, OWASP, STRIDE, CI/CD, monitoring |

---

## Installation

### Option 1: Marketplace (Recommended)

```bash
/plugin marketplace add nagisanzenin/claude-code-plugins
/plugin install production-grade@nagisanzenin
```

### Option 2: Load directly from directory

```bash
git clone https://github.com/nagisanzenin/claude-code-production-grade-plugin.git
claude --plugin-dir /path/to/claude-code-production-grade-plugin
```

### Requirements

- **Claude Code** (version with plugin support)
- **Docker & Docker Compose** (for local dev environment)
- **Git** (for source control)

> The pipeline will auto-setup everything else it needs inside Docker.

---

## Usage

### Trigger Phrases

Say any of the following to activate the pipeline:

```
"Build a production-grade SaaS for [idea]"
"Full production pipeline for this project"
"Production ready setup"
"Run the complete pipeline"
"Build me a platform for [description]"
```

### Quick Example

```
You: Build a production-grade SaaS for restaurant management
     with online ordering, table reservations, and staff scheduling.
```

The pipeline will automatically:
1. Interview you (3-5 multiple choice questions)
2. Research domain & competitors
3. Write a Business Requirements Document (BRD)
4. **Gate 1: You approve the BRD**
5. Design architecture, API contracts, data models
6. **Gate 2: You approve the Architecture**
7. Implement backend + frontend code
8. Write & run tests (unit, integration, e2e)
9. Security audit (STRIDE + OWASP)
10. Automated code review
11. Setup Terraform, CI/CD, Docker, K8s
12. SRE production readiness check
13. **Gate 3: You approve Production Readiness**
14. Generate documentation
15. Create custom skills for the project

### Interaction Model

You **don't need to type anything** — just use arrow keys and Enter to select options. Every question is multiple choice with a "Chat about this" option at the bottom for free-form input.

---

## Pipeline Phases

```
DEFINE          BUILD              HARDEN             SHIP            SUSTAIN
  |               |                  |                  |               |
  v               v                  v                  v               v
Product       Software           QA Engineer        DevOps            SRE
Manager       Engineer           Security Eng       (CI/CD,IaC)      (Reliability)
(BRD/PRD)     (Services)         Code Reviewer
              Frontend Eng
Solution      (UI/UX)
Architect     Data Scientist
(Design)      (AI/ML)            Technical Writer
                                 (Docs)
```

### Phase Details

| Phase | Name | Description | User Input? |
|-------|------|-------------|-------------|
| 1 | **DEFINE - Product Manager** | Interview CEO, research domain, write BRD/PRD | Gate 1: Approve BRD |
| 2 | **DEFINE - Solution Architect** | System design, tech stack, API contracts, data models, scaffold | Gate 2: Approve Architecture |
| 3a | **BUILD - Software Engineer** | Backend: services, handlers, repositories, middleware | Autonomous |
| 3b | **BUILD - Frontend Engineer** | UI: design system, components, pages, API clients (if needed) | Autonomous |
| 4 | **BUILD - QA Engineer** | Unit, integration, e2e, performance tests + self-healing | Autonomous |
| 5a | **HARDEN - Security Engineer** | STRIDE threat modeling, OWASP audit, compliance check | Autonomous |
| 5b | **HARDEN - Code Reviewer** | Architecture conformance, code quality, auto-fix | Autonomous |
| 6 | **SHIP - DevOps** | Terraform, CI/CD, Docker/K8s, monitoring, security scanning | Autonomous |
| 7 | **SHIP - SRE** | Production readiness, chaos engineering, incident management | Gate 3: Approve Readiness |
| 7b | **SHIP - Data Scientist** | AI/ML/LLM optimization (auto-activates when AI usage detected) | Conditional |
| 8 | **SUSTAIN - Technical Writer** | API reference, dev guides, Docusaurus site | Autonomous |
| 9 | **SUSTAIN - Skill Maker** | Create custom skills for the project | Autonomous |

> **Parallelization:** Phases 3a+3b run in parallel. Phases 5a+5b run in parallel.

---

## 13 Bundled Skills

### 1. `production-grade` — Master Orchestrator
- Coordinates the entire end-to-end pipeline
- Manages state, context bridging between skills
- Adaptive: auto-adjusts plan based on each phase's results
- 3 approval gates with multiple-choice UX

### 2. `product-manager` — Business Requirements
- Interviews CEO/CTO (3-5 focused questions)
- Researches domain via web search
- Writes BRD with user stories, acceptance criteria, business rules
- Self-verifies implementation matches BRD after code is complete

### 3. `solution-architect` — System Design
- Architecture Decision Records (ADRs)
- C4 diagrams (context, container, sequence)
- Tech stack selection with rationale
- OpenAPI 3.1, gRPC proto, AsyncAPI specs
- ERD, SQL migrations, data flow diagrams
- Project scaffold with health checks, logging, graceful shutdown

### 4. `software-engineer` — Backend Implementation
- Clean architecture: handlers → services → repositories
- Dependency injection, DTO mapping, domain models
- Multi-tenancy, RBAC, audit trail
- Payment integration (Stripe/Paddle abstraction)
- Feature flags, caching, rate limiting
- Local dev environment (Docker Compose + seed data)

### 5. `frontend-engineer` — UI/UX
- Design system with tokens, components, patterns
- Next.js / Vue / Svelte support
- API client generation from OpenAPI specs
- Accessibility (WCAG 2.1 AA)
- Storybook documentation

### 6. `data-scientist` — AI/ML/LLM Optimization
- Prompt engineering & token optimization
- LLM caching strategies
- A/B testing framework
- Cost modeling & analytics pipeline
- Auto-activates when AI/ML usage is detected in code

### 7. `qa-engineer` — Testing
- Unit, integration, contract, e2e, performance tests
- Self-healing test protocol: distinguishes test bugs from implementation bugs
- Coverage reports
- Auto-retry & debug failed tests

### 8. `security-engineer` — Security Audit
- STRIDE threat modeling
- OWASP Top 10 code audit
- PII inventory & encryption review
- Dependency vulnerability analysis
- Penetration test plans
- Compliance (GDPR, SOC2, HIPAA)

### 9. `code-reviewer` — Quality Gate
- Architecture conformance check
- Code quality metrics
- Performance review
- Auto-fix for critical & high severity issues
- Findings report by severity

### 10. `devops` — Infrastructure & Deployment
- Terraform IaC (AWS / GCP / Azure)
- CI/CD pipelines (GitHub Actions)
- Docker multi-stage builds
- Kubernetes manifests
- Monitoring: Prometheus, Grafana dashboards
- Security scanning in pipeline

### 11. `sre` — Site Reliability Engineering
- Production readiness checklist
- SLO/SLI definitions
- Chaos engineering scenarios
- Capacity planning
- Incident management & runbooks
- On-call rotation setup

### 12. `technical-writer` — Documentation
- API reference (auto-generated)
- Developer guides
- Operational docs & runbooks
- Docusaurus site scaffold
- Architecture decision documentation

### 13. `skill-maker` — Meta Skill Creator
- Analyzes project patterns
- Creates 3-5 custom skills specific to the project
- Packages & publishes to marketplace
- Auto-generates SKILL.md, plugin.json

---

## Workspace Architecture

The plugin enforces a clean separation between **project deliverables** (at the project root) and **agent workspace artifacts** (in the suite folder).

### Project Root (Deliverables)

All production code, infrastructure, docs, and tests live at the project root:

```
project/
├── services/                    # Backend service implementations
│   └── <service-name>/
│       ├── src/
│       ├── tests/
│       ├── Dockerfile
│       └── Makefile
├── libs/shared/                 # Shared types, utils, clients
├── frontend/                    # Frontend application (if applicable)
│   ├── app/
│   ├── tests/
│   ├── storybook/
│   └── package.json
├── api/                         # API contracts
│   ├── openapi/*.yaml
│   ├── grpc/*.proto
│   └── asyncapi/*.yaml
├── schemas/                     # Data models
│   ├── erd.md
│   ├── migrations/*.sql
│   └── data-flow.md
├── tests/                       # Cross-service tests
│   ├── integration/
│   ├── e2e/
│   └── performance/
├── infrastructure/              # IaC and deployment
│   ├── terraform/
│   ├── kubernetes/
│   └── monitoring/
├── docs/                        # Project documentation
│   ├── architecture/
│   │   ├── architecture-decision-records/
│   │   ├── system-diagrams/
│   │   └── tech-stack.md
│   ├── runbooks/
│   ├── api/
│   └── guides/
├── scripts/                     # Build, deploy, seed scripts
├── .github/workflows/           # CI/CD pipelines
├── docker-compose.yml
├── Makefile
└── README.md
```

### Agent Workspace (Working Artifacts Only)

The `Claude-Production-Grade-Suite/` folder contains **only** agent workspace artifacts — plans, notes, findings, analysis. **No production code lives here.**

```
Claude-Production-Grade-Suite/
├── .orchestrator/               # Pipeline state & logs
│   ├── pipeline-state.json
│   ├── decisions-log.md
│   ├── execution-plan.md
│   └── agent-activity.log
├── product-manager/             # BRD, research notes
├── solution-architect/          # Design analysis, working notes
├── software-engineer/           # Implementation plans, progress
├── frontend-engineer/           # UI analysis, component inventory
├── qa-engineer/                 # Test plans, coverage reports
├── security-engineer/           # Threat models, audit findings
├── code-reviewer/               # Review findings, metrics
├── devops/                      # Deployment planning notes
├── sre/                         # Readiness assessments
├── data-scientist/              # ML analysis notes
├── technical-writer/            # Writing notes
└── skill-maker/                 # Custom skill drafts
```

---

## Detailed Workflow

### 1. Initialization
```
User: "Build me a SaaS for X"
  ↓
Orchestrator creates workspace + researches domain
  ↓
Displays Execution Plan (active phases, parallelization)
  ↓
Automatically starts Phase 1 (NO "should I proceed?" prompts)
```

### 2. Context Bridging

Each phase reads the output of previous phases — no re-asking of already-answered questions:

| Phase | Reads From | Writes To (Project Root) | Writes To (Workspace) |
|-------|------------|--------------------------|----------------------|
| Product Manager | User interview | — | `product-manager/` |
| Solution Architect | `product-manager/BRD/` | `docs/architecture/`, `api/`, `schemas/` | `solution-architect/` |
| Software Engineer | `docs/architecture/`, `api/`, `schemas/` | `services/`, `libs/`, `docker-compose.yml` | `software-engineer/` |
| Frontend Engineer | `api/`, `product-manager/BRD/` | `frontend/` | `frontend-engineer/` |
| QA Engineer | `services/`, `frontend/`, `docs/architecture/` | `tests/` | `qa-engineer/` |
| Security Engineer | All project root code | Security fixes in-place | `security-engineer/` |
| Code Reviewer | All project root code + tests | Auto-fixes in-place | `code-reviewer/` |
| DevOps | `docs/architecture/`, `services/` | `infrastructure/`, `.github/workflows/` | `devops/` |
| SRE | `infrastructure/`, `docs/architecture/` | `docs/runbooks/` | `sre/` |
| Data Scientist | `services/`, `docs/architecture/` | Optimizations in-place | `data-scientist/` |
| Technical Writer | **ALL** project root | `docs/` | `technical-writer/` |
| Skill Maker | **ALL** project root | Custom skill plugins | `skill-maker/` |

### 3. Adaptive Orchestration

The orchestrator is **intelligent** and auto-adjusts based on context:

| Scenario | Action |
|----------|--------|
| User says "no frontend" | Skip Phase 3b, simplify DevOps |
| Architect chooses monolith | Drop K8s, simplify CI/CD |
| Code uses LLM APIs | Auto-enable Phase 7b (Data Scientist) |
| Security finds critical vuln | Pause → call Software Engineer to fix → resume |
| Tests fail > 20% | Flag for user review |
| User says "skip testing" | Warn, continue if user insists |

### 4. Self-Debugging Protocol

Every agent follows this protocol:
1. Write code → **build and test** (`make build`, `docker build`)
2. On error → read error → analyze root cause → fix → retry
3. After 3 failures → stop and report details to user
4. **Never** leave broken code and move on

---

## Approval Gates

The plugin pauses only **3 times** to ask you:

### Gate 1: BRD Approval (after Phase 1)
```
┌─────────────────────────────────────────────┐
│ Gate 1: BRD                                 │
│                                             │
│ BRD complete: 12 user stories,              │
│ 8 acceptance criteria. Approve?             │
│                                             │
│ > Approve — start architecture (Recommended)│
│   Show me the BRD details                   │
│   I have changes                            │
│   Chat about this                           │
└─────────────────────────────────────────────┘
```

### Gate 2: Architecture Approval (after Phase 2)
```
┌─────────────────────────────────────────────┐
│ Gate 2: Architecture                        │
│                                             │
│ Architecture complete: [tech stack].        │
│ Approve to start building?                  │
│                                             │
│ > Approve — start building (Recommended)    │
│   Show architecture details                 │
│   I have concerns                           │
│   Chat about this                           │
└─────────────────────────────────────────────┘
```

### Gate 3: Production Readiness (after Phase 7)
```
┌─────────────────────────────────────────────┐
│ Gate 3: Ship                                │
│                                             │
│ All phases complete. Ship it?               │
│                                             │
│ > Ship it — production ready (Recommended)  │
│   Show full report                          │
│   Fix issues first                          │
│   Chat about this                           │
└─────────────────────────────────────────────┘
```

> All interactions between gates are **autonomous** — the pipeline makes decisions and reports.

---

## Partial Execution

You don't have to run the full pipeline. Run individual sections:

| Command | Phases Run |
|---------|------------|
| `"Just define"` | Product Manager → Solution Architect |
| `"Just build"` | Software Engineer → QA (requires architecture) |
| `"Just harden"` | Security Engineer → Code Reviewer (requires code) |
| `"Just ship"` | DevOps → SRE (requires code) |
| `"Just document"` | Technical Writer (requires output from prior phases) |
| `"Skip frontend"` | Skip Phase 3b |
| `"Start from architecture"` | Skip Product Manager, start Phase 2 |

---

## Examples

### E-commerce Platform
```
Build a production-grade SaaS for multi-vendor e-commerce
with seller dashboards, buyer marketplace, and payment processing.
```

### Project Management Tool
```
Build me a production-ready project management platform
like Linear, with sprint planning, issue tracking, and team collaboration.
```

### AI Content Platform
```
Full production pipeline for an AI content generation platform
with prompt management, usage metering, and team workspaces.
```
> *Phase 7b (Data Scientist) will auto-activate for AI/LLM optimization*

### API-Only Backend
```
Build a production-grade REST API for a fintech lending platform.
No frontend needed. Focus on security and compliance.
```
> *Phase 3b (Frontend) is automatically skipped*

---

## Configuration

### Plugin Metadata

```json
{
  "name": "production-grade",
  "version": "2.0.0",
  "author": "nagisanzenin",
  "license": "MIT"
}
```

### Custom Tech Stack

You can specify your preferred tech stack when triggering:

```
Build a production-grade SaaS for X using:
- TypeScript + NestJS backend
- Next.js frontend
- PostgreSQL + Redis
- Deploy on AWS
```

The Solution Architect will respect your preferences in the ADRs.

### Cloud Support

| Cloud | Terraform | CI/CD | Containers | Monitoring |
|-------|-----------|-------|------------|------------|
| **AWS** | ECS/EKS, RDS, SQS/SNS | GitHub Actions | Docker, K8s | CloudWatch |
| **GCP** | GKE/Cloud Run, Cloud SQL | GitHub Actions | Docker, K8s | Cloud Monitoring |
| **Azure** | AKS, Azure SQL, Service Bus | GitHub Actions | Docker, K8s | Azure Monitor |
| **Multi-cloud** | Provider-agnostic modules | GitHub Actions | Docker, K8s | Multi-provider |

---

## FAQ

### Q: Does the plugin actually write working code?
**A:** Yes. Every agent follows the protocol: write code → build → debug → fix. No stubs or TODOs in production code. Builds must pass, tests must be green.

### Q: How long does the full pipeline take?
**A:** Depends on project complexity. A simple SaaS (5-10 endpoints) takes roughly 30-60 minutes. Complex platforms may take 2-4 hours.

### Q: Can I stop mid-way and resume later?
**A:** Pipeline state is saved in `.orchestrator/pipeline-state.json`. However, resuming mid-pipeline depends on the Claude Code session context.

### Q: What languages are supported?
**A:** TypeScript/Node.js, Go, Python, Rust, Java/Kotlin. The Solution Architect selects based on requirements, or you can specify your preference.

### Q: Is Docker required?
**A:** Recommended for running the local dev environment and verifying builds. Some phases (DevOps, QA integration tests) require Docker.

### Q: Will the plugin overwrite existing code?
**A:** No. All deliverables are written to clearly-defined directories at the project root. The `Claude-Production-Grade-Suite/` folder contains only workspace artifacts.

### Q: How do I add custom skills for my project?
**A:** Phase 9 (Skill Maker) automatically analyzes the project and creates 3-5 custom skills. Or trigger `skill-maker` separately: `"Make a skill for [description]"`.

---

## Architecture Highlights

### Clean Architecture (Software Engineer)
```
Handler (thin) → Service (business logic) → Repository (data access)
     ↓                    ↓                        ↓
  Validate          Apply rules              Query DB
  Delegate          Emit events              Cache aside
  Respond           Return Result            Tenant scoped
```

### Security Layers (Security Engineer)
- **STRIDE** threat modeling for each component
- **OWASP Top 10** code-level audit
- Auto-fix critical/high vulnerabilities
- PII inventory + encryption strategy
- Dependency vulnerability analysis

### Production Standards (SRE + DevOps)
- Health checks (`/healthz`, `/readyz`)
- Structured JSON logging with trace IDs
- Graceful shutdown handling
- Circuit breaker + retry patterns
- Rate limiting (global + per-tenant)
- Feature flags abstraction
- Multi-tenancy at data layer

---

## Contributing

1. Fork the repo
2. Create a branch: `git checkout -b feature/your-feature`
3. Commit: `git commit -m "Add your feature"`
4. Push: `git push origin feature/your-feature`
5. Open a Pull Request

### Adding a New Skill

Create a folder at `skills/your-skill-name/` with a `SKILL.md` file:

```markdown
---
name: your-skill-name
description: When to trigger this skill...
---

# Your Skill Name

## Overview
...

## When to Use
...

## Process Flow
...
```

---

## License

MIT — See [LICENSE](LICENSE) for details.

---

<p align="center">
  <strong>From idea to production-ready SaaS. One prompt. 13 expert AI agents.</strong>
</p>
