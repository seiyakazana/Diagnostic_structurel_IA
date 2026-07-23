# BMad Steps to Success

A complete walkthrough for building a product from idea to production using **Claude Code** and the **BMad Method** (Breakthrough Method for Agile AI-Driven Development).

---

## Before You Begin

### Prerequisites

Make sure the following are installed and available on your machine:

- **Node.js 20+** — required by the BMad installer.
- **Git** — strongly recommended for version control of your generated artifacts.
- **Claude Code** — your AI IDE. BMad also supports Cursor and other tools, but this guide assumes Claude Code.

### The Golden Rule: Context Management

Every BMad command should start with a **fresh context**. In Claude Code CLI, that means killing the current instance and relaunching before each new step. BMad agents accumulate context from your project documents progressively — each phase produces artifacts that feed the next — so a clean slate prevents stale or conflicting instructions from leaking across phases.

> **Why it matters:** BMad agents make better decisions when they load exactly the context they need for their role, rather than carrying residual context from a prior agent session.

### Commit Early, Commit Often

After every step that produces an artifact (product brief, PRD, architecture document, etc.), commit it to version control. This gives you rollback points and makes it easy to diff what changed between iterations.

---

## Step 1 — Install BMad in Your Project Repository

Navigate to your project folder (or create one) and run the interactive installer:

```bash
cd your-project-folder
npx bmad-method install
```

The installer will prompt you to choose your installation location, AI tool (select **Claude Code**), and which modules to install. Most projects only need the **BMad Method** module.

Once installed, verify everything is working:

```bash
# Inside Claude Code
bmad-help
```

`bmad-help` is your intelligent guide throughout the entire process. It confirms your installation, shows available workflows, and recommends the next step based on your project's current state. Use it whenever you're unsure about what to do next.

---

## Step 2 — Explore and Capture Your Product Idea (Analysis Phase)

Before building anything, you need a clear picture of **what** you're building and **why**. BMad's Analysis Phase provides several tools for this.

### Option A: Brainstorm with the BMad Agent

Launch Claude Code and start a guided brainstorming session:

```bash
# Inside Claude Code
/bmad-brainstorming
```

This produces a `brainstorming-report.md` that captures your ideas in a structured format. It's a great starting point if you're working solo.

### Option B: Record a Live Brainstorming Session

Pair up with a colleague in a video call. Brief them on your product idea, take their feedback, and let them challenge your assumptions. The goal is to produce a **transcript** with clear alignment on your vision and deliberate positions on feature scope. Save this transcript — it becomes input for the product brief.

### Option C: Validate with Research (Optional but Recommended)

If your idea touches a domain you're not deeply familiar with, or if you want to stress-test assumptions before committing, BMad provides dedicated research workflows:

```bash
# Inside Claude Code — pick whichever applies
/bmad-domain-research
/bmad-market-research
/bmad-technical-research
```

These produce research findings documents that feed into later planning steps.

### Build Your Design Inputs

Now that the core idea is captured, shape the user-facing side of your product:

1. **Design System** — Work with Claude to define your visual language: colors, typography, spacing, and component patterns following atomic design principles (atoms, molecules, organisms). This ensures visual consistency across the product.

2. **Functional Model** — Map out the screens, user flows, and interactions your product needs. This is the artifact you can share with peers or stakeholders to get feedback and buy-in before any code is written.

Both of these become critical context for the agents in later phases.

---

## Step 3 — Create the Product Brief

The product brief is your foundation document. It synthesizes your brainstorming output, design system, and functional model into a single artifact that answers: **what are we building and why?**

```bash
# Inside Claude Code — fresh context
/bmad-product-brief
```

> **Alternative approach:** If you prefer the Amazon "working backwards" method, BMad also offers a PR/FAQ workflow (`/bmad-prfaq`) that stress-tests your concept through a mock press release and FAQ format.

**Output:** `product-brief.md` (or `prfaq-{project}.md`)

---

## Step 4 — Create the Product Requirements Document (PRD)

The PRD translates your product brief into structured requirements — functional and non-functional — that both AI agents and human collaborators can work from. Think of it as the roadmap in a machine-readable format.

```bash
# Inside Claude Code — fresh context
/bmad-agent-pm
/bmad-prd
```

Take your time answering the PM agent's questions. The quality of your PRD directly determines the quality of your epics, stories, and ultimately your code. Be specific about scope boundaries, user personas, and acceptance criteria.

**Output:** `PRD.md`

### UX Design (Optional but Recommended)

If your product has meaningful user-facing interactions, create a UX specification before moving to architecture:

```bash
# Inside Claude Code — fresh context
/bmad-agent-ux-designer
/bmad-ux
```

**Output:** `ux-spec.md`

---

## Step 5 — Create the Architecture Decision Document

With the _what_ and _why_ documented, it's time to define the _how_. The architecture document captures every technical decision: language, frameworks, ORM, hosting, deployment pipeline, testing strategy, API design, and so on.

```bash
# Inside Claude Code — fresh context
/bmad-agent-architect
/bmad-architecture
```

This artifact is aimed at engineering profiles. It gives any developer (or AI agent) the full picture of how the product is built, and it includes Architecture Decision Records (ADRs) that explain the rationale behind each choice.

**Output:** `architecture.md` with embedded ADRs

### Generate Project Context (Recommended)

After architecture is defined, generate a `project-context.md` file. This acts as a constitution for your project — it guides implementation decisions across all subsequent workflows.

```bash
# Inside Claude Code
/bmad-generate-project-context
```

**Output:** `project-context.md`

---

## Step 6 — Validate Your Specifications (First Major Review)

Before investing in epic and story creation, take a step back and validate everything you've produced so far.

### Adversarial Review

BMad agents typically offer an adversarial review at the end of each specification step. This review systematically challenges your decisions to surface gaps, contradictions, or unrealistic assumptions. **Don't skip it** — it's one of the most valuable parts of the process.

### Cross-Document Validation

You can also validate the PRD against its upstream inputs (product brief, design system, functional model) with a dedicated command:

```bash
# Inside Claude Code — fresh context
/bmad-agent-pm
/bmad-prd validate
```

This ensures nothing was lost or distorted as context passed from one phase to the next.

---

## Step 7 — Create Epics and Stories

With validated specifications in hand, break down the work into epics (large feature areas) and stories (implementable units of work).

```bash
# Inside Claude Code — fresh context
/bmad-agent-pm
/bmad-create-epics-and-stories
```

This step shapes the development effort and sequences the tasks. Each epic gets its own file containing the associated stories with scope definitions and acceptance criteria.

**Output:** Epic files with embedded stories

---

## Step 8 — Check Implementation Readiness (Solutioning Gate)

This is a formal gate check before entering the implementation phase. The architect agent reviews the full chain of artifacts — PRD, architecture, epics, and stories — to catch remaining ambiguities, contradictions, or technical blind spots.

```bash
# Inside Claude Code — fresh context
/bmad-agent-architect
/bmad-check-implementation-readiness
```

The check produces a clear verdict: **PASS**, **CONCERNS**, or **FAIL**. Address any concerns before proceeding. This is your last opportunity to course-correct cheaply.

---

## Step 9 — Sprint Planning

Everything is ready and validated. Now organize the work into a development plan.

```bash
# Inside Claude Code — fresh context
/bmad-sprint-planning
```

This initializes sprint tracking and sequences stories for development. Run this once per project to set up the development cycle.

**Output:** `sprint-status.yaml`

---

## Step 10 — Story Development Cycle

This is where code gets written. For each story, the cycle is: **prepare → implement → review**. Stories from independent epics can be developed in parallel if they were correctly scoped during planning.

### 10a. Prepare the Story

This workflow prepares a story for implementation by pulling together all the context a developer (or dev agent) will need.

```bash
# Inside Claude Code — fresh context
/bmad-create-story
```

**Output:** `story-[slug].md`

### 10b. Implement the Story

The developer agent picks up the prepared story and writes the code, including tests.

```bash
# Inside Claude Code — fresh context
/bmad-agent-dev
/bmad-dev-story
```

**Output:** Working code + tests

### 10c. Review the Code

The developer agent performs a code review against the architecture document, project context, and story acceptance criteria.

```bash
# Inside Claude Code — fresh context
/bmad-agent-dev
/bmad-code-review
```

**Output:** Approved, or changes requested with specifics

### Handling Mid-Sprint Changes

If a significant change comes up during development that affects scope or approach, don't improvise — use the dedicated course-correction workflow:

```bash
# Inside Claude Code — fresh context
/bmad-correct-course
```

### Tracking Progress

You can check sprint status at any time:

```bash
# Inside Claude Code
/bmad-sprint-status
```

---

## Step 11 — Retrospective and Finalization

### Run a Retrospective

After completing an epic (or all epics), run a retrospective to capture what worked, what didn't, and what to improve.

```bash
# Inside Claude Code — fresh context
/bmad-retrospective "all epics"
```

This is how you catch lingering issues and consolidate quality. If the retrospective surfaces problems, loop back to Step 10 to address them.

### Generate Project Context for Future Work

Create or update the project context document so that any future development — by you, your team, or AI agents — starts with an accurate understanding of the codebase.

```bash
# Inside Claude Code — fresh context
/bmad-generate-project-context
```

This file is essential for maintainability. It captures your technology stack, conventions, and implementation patterns in a format that both AI agents and engineers can consume immediately.

### Document Your Project

Finally, generate project documentation:

```bash
# Inside Claude Code — fresh context
/bmad-document-project
```

---

## Quick Reference: The Full Pipeline

| Phase              | Step                 | Command                                | Produces                    |
| ------------------ | -------------------- | -------------------------------------- | --------------------------- |
| **Analysis**       | 1. Install           | `npx bmad-method install`              | BMad project scaffolding    |
|                    | 2. Brainstorm        | `/bmad-brainstorming`                  | `brainstorming-report.md`   |
|                    | 3. Product Brief     | `/bmad-product-brief`                  | `product-brief.md`          |
| **Planning**       | 4. PRD               | `/bmad-prd`                            | `PRD.md`                    |
|                    | 4b. UX Design        | `/bmad-ux`                             | `ux-spec.md`                |
| **Solutioning**    | 5. Architecture      | `/bmad-architecture`                   | `architecture.md`           |
|                    | 6. Validate          | `/bmad-prd validate`                   | Validation report           |
|                    | 7. Epics & Stories   | `/bmad-create-epics-and-stories`       | Epic files                  |
|                    | 8. Readiness Check   | `/bmad-check-implementation-readiness` | PASS / CONCERNS / FAIL      |
| **Implementation** | 9. Sprint Planning   | `/bmad-sprint-planning`                | `sprint-status.yaml`        |
|                    | 10a. Create Story    | `/bmad-create-story`                   | `story-[slug].md`           |
|                    | 10b. Develop         | `/bmad-dev-story`                      | Code + tests                |
|                    | 10c. Review          | `/bmad-code-review`                    | Approval or change requests |
| **Finalization**   | 11a. Retrospective   | `/bmad-retrospective`                  | Lessons learned             |
|                    | 11b. Project Context | `/bmad-generate-project-context`       | `project-context.md`        |
|                    | 11c. Documentation   | `/bmad-document-project`               | Project docs                |

---

## Tips for Success

- **One agent, one context.** Never carry context from a PM session into an Architect session. Kill and relaunch between agent switches.
- **Answer agent questions thoroughly.** The quality of BMad's output is directly proportional to the quality of your input during elicitation.
- **Don't skip adversarial reviews.** They're the cheapest way to catch mistakes.
- **Use `bmad-help` liberally.** It adapts to your project's current state and knows what comes next.
- **Commit artifacts to git.** Every `.md` file BMad produces is a versioned decision record.
- **Parallelize wisely.** Independent epics can be developed simultaneously, but shared dependencies require sequencing.
 