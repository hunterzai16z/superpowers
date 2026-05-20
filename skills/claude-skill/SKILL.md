---
name: superpowers
description: >
  Use this skill whenever the user wants to install or use the Superpowers development methodology
  for Claude Code, Codex, Gemini CLI, Cursor, OpenCode, or any coding agent — or when the user asks
  about TDD (test-driven development), subagent-driven development, brainstorming before coding,
  writing implementation plans, systematic debugging, code review workflows, git worktrees for
  parallel development, or any of the named Superpowers skills (brainstorming, writing-plans,
  test-driven-development, systematic-debugging, subagent-driven-development, executing-plans,
  dispatching-parallel-agents, requesting-code-review, finishing-a-development-branch,
  using-git-worktrees, verification-before-completion, writing-skills). Also trigger on phrases
  like "give my agent superpowers", "obra/superpowers", "RED-GREEN-REFACTOR", "YAGNI", "DRY",
  or "spec before code".
---

# Superpowers Skill

Superpowers is a **complete software development methodology** delivered as a set of composable
Claude Code skills. It installs a set of mandatory workflows that activate automatically —
brainstorming before any feature, TDD during implementation, two-stage code review between tasks,
and subagent-driven parallel development. The agent *just has Superpowers*; no manual invocation
needed.

---

## Install

### Claude Code (official marketplace — easiest)

```bash
/plugin install superpowers@claude-plugins-official
```

### Claude Code (Superpowers marketplace)

```bash
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

### Other agents

| Agent | Install command |
|-------|----------------|
| **Codex CLI** | Search for "superpowers" via `/plugins` → Install Plugin |
| **Codex App** | Plugins sidebar → Coding section → `+` next to Superpowers |
| **Gemini CLI** | `gemini extensions install https://github.com/obra/superpowers` |
| **OpenCode** | Tell OpenCode: `Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md` |
| **Cursor** | `/add-plugin superpowers` in Agent chat |
| **GitHub Copilot CLI** | `copilot plugin marketplace add obra/superpowers-marketplace && copilot plugin install superpowers@superpowers-marketplace` |
| **Factory Droid** | `droid plugin marketplace add https://github.com/obra/superpowers && droid plugin install superpowers@superpowers` |

---

## The core workflow

Superpowers replaces ad-hoc coding with a structured loop. The agent checks for relevant
skills before any task — **mandatory workflows, not suggestions**.

```
1. brainstorming          → refines idea, gets design approved before any code
2. using-git-worktrees    → creates isolated branch + workspace after design approval
3. writing-plans          → breaks work into 2-5 min tasks with exact files + verification steps
4. subagent-driven-development  → dispatches subagents per task with 2-stage review
   OR executing-plans           → batch execution with human checkpoints
5. test-driven-development      → RED → GREEN → REFACTOR on every task
6. requesting-code-review       → reviews against plan, blocks on critical issues
7. finishing-a-development-branch → merges/PRs/discards cleanly
```

---

## All 14 skills

### Testing
**`test-driven-development`** — The Iron Law: `NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST`.
RED (write failing test) → GREEN (minimal code to pass) → REFACTOR (clean up). Write code before
the test? Delete it. Start over. No exceptions for "simple" changes.

**`verification-before-completion`** — Before marking anything done: run the tests, verify the
behavior, check edge cases. Ensure it's actually fixed, not just differently broken.

### Debugging
**`systematic-debugging`** — 4-phase root cause process: reproduce reliably → isolate variables →
identify root cause → verify fix. Includes root-cause-tracing, defense-in-depth, and
condition-based-waiting techniques.

### Collaboration
**`brainstorming`** — HARD GATE: do NOT write any code until design is approved. Asks clarifying
questions one at a time, proposes 2-3 approaches with trade-offs, presents design in sections,
saves design doc to `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`.

**`writing-plans`** — Breaks approved design into bite-sized tasks (2-5 minutes each). Every task
has exact file paths, complete code snippets, and step-by-step verification. Plan saved as
`docs/superpowers/plans/YYYY-MM-DD-<topic>-plan.md`.

**`executing-plans`** — Works through plan in batches with human checkpoints between batches.
Alternative to subagent-driven-development for smaller tasks.

**`dispatching-parallel-agents`** — Concurrent subagent workflows for independent tasks.
Coordinates via shared state files; each subagent gets its own isolated context.

**`requesting-code-review`** — Pre-review checklist before moving to next task. Reviews against
plan, reports issues by severity (critical / major / minor). Critical issues block progress.

**`receiving-code-review`** — Protocol for responding to review feedback. Never argue; understand
first, then discuss.

**`using-git-worktrees`** — Creates isolated workspace on a new branch after design approval.
Runs project setup commands, verifies clean test baseline before implementation starts.

**`finishing-a-development-branch`** — Verifies all tests pass, presents options
(merge / open PR / keep branch / discard), cleans up worktree.

**`subagent-driven-development`** — Fast iteration: dispatches a fresh subagent per task with
two-stage review (spec compliance first, then code quality). Inspects output before continuing.

### Meta
**`writing-skills`** — How to create new Superpowers skills following best practices. Includes
testing methodology for skills.

**`using-superpowers`** — The entry skill: loads on session start, explains how to find and invoke
all other skills. Sets priority: user instructions > Superpowers skills > default behavior.

---

## Key principles

**YAGNI** — You Aren't Gonna Need It. Don't build features that aren't in the current spec.

**DRY** — Don't Repeat Yourself. Extract duplication after the third occurrence.

**TDD is non-negotiable** — Thinking "skip TDD just this once"? That's rationalization. Stop.

**Design before code** — The brainstorming skill has a HARD GATE. Every project goes through it,
including "simple" ones. Unexamined assumptions in "simple" projects cause the most wasted work.

**Subagents are tools, not shortcuts** — Each subagent gets a fresh context + full task spec.
Two-stage review (spec compliance, then code quality) before accepting output.

**User instructions always win** — If `CLAUDE.md` says "don't use TDD" and a skill says "always
use TDD," follow the user's instructions. Superpowers enhances default behavior; it doesn't
override explicit user choices.

---

## Instruction priority

```
1. User's explicit instructions (CLAUDE.md / GEMINI.md / AGENTS.md / direct requests)
2. Superpowers skills
3. Default system prompt behavior
```

---

## What the agent does automatically (after install)

- **Before any feature/component/behavior:** invokes `brainstorming`, refuses to code until design approved
- **After design approval:** invokes `using-git-worktrees`, creates branch + workspace
- **With approved design:** invokes `writing-plans`, produces task-level plan
- **During implementation:** invokes `test-driven-development`, enforces RED-GREEN-REFACTOR
- **Between tasks:** invokes `requesting-code-review`, blocks on critical issues
- **When tasks complete:** invokes `finishing-a-development-branch`

The agent announces skill invocations: `"Using [skill] to [purpose]"`.

---

## Skill invocation rule

> Invoke relevant skills BEFORE any response or action. If there's even a 1% chance a skill
> applies, invoke it to check. If it turns out to be wrong for the situation, you don't need
> to follow it — but you must check first.

This is the core guarantee: Superpowers skills activate proactively, not reactively.

---

## When NOT to use this skill

- The user is asking a factual question with no implementation involved
- The user explicitly said "skip the design phase" or "just write the code"
- You are a **subagent** dispatched to execute a specific task (skip `using-superpowers` entirely;
  the SUBAGENT-STOP marker in the skill handles this automatically)
- The user wants a throwaway prototype or generated scaffolding (TDD exception applies — ask first)
