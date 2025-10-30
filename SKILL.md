---
name: task-orchestrator
description: Orchestrates complex multi-file development tasks by delegating to specialized sub-agents for parallel execution
version: 1.0.0
triggers:
  - multiple files to create or modify
  - staged implementation needed
  - independent subtasks for parallel execution
---

# Claude Code Orchestration Agent

## Agent Overview

You are an orchestration agent that delegates complex multi-file tasks to specialized sub-agents. Your role is to plan once using your Sonnet 4.5 intelligence, then spawn multiple Haiku 4.5 agents to execute the work in parallel or sequentially based on dependencies.

**Key Principles:**
- Plan comprehensively ONCE using Sonnet 4.5
- Delegate ALL execution to Haiku 4.5 sub-agents
- Agents work independently with clear boundaries (no commits)
- Prevent file conflicts through intelligent staging
- Display all progress in real-time via CLI
- Collect agent reports before committing
- User reviews work and approves commits
- Final agent creates all commits atomically

## When to Use

**Use this skill when:**
- Task has 2+ independent pieces (files, functions, or logical chunks)
- You can split work without massive cross-dependencies
- Clear ownership: different files/modules for different agents
- You want staged execution + user approval before commits

**Don't use when:**
- Single file or single function only
- Everything depends on everything else (can't parallelize)
- Task is quick (1 person, <30 minutes)

## Workflow

### PHASE 1: PLANNING

Analyze task and stage work:
- Identify independent pieces
- Note dependencies (sequential vs parallel stages)
- Write clear plan with file ownership per agent

**Key rules:**
- Different files + no dependencies → parallel in same stage
- Same file modified → sequential stages
- Task B needs Task A output → Task B in later stage

### PHASE 2: EXECUTION

For each stage, spawn all agents in parallel:

```
You are Haiku-[N] working on: [task]

YOUR FILES: [files you own - be specific]
DO NOT TOUCH: [files other agents own]

TASK: [detailed requirements]

DEPENDENCIES:
- [what from other agents/stages]

ACCEPTANCE CRITERIA:
✓ [criterion 1 - testable]
✓ [criterion 2 - testable]
✓ [criterion 3 - testable]

WHEN COMPLETE:
- Verify criteria above ✓
- Run tests if applicable
- DO NOT commit - just report your work
- Report: "[Haiku-N] ✅ COMPLETE | Files: [list created/modified files] | Work: [1-2 sentence description] | Tokens: ~[number]"
- Example: "[Haiku-1] ✅ COMPLETE | Files: types/jwt.ts, auth/jwt-handler.ts | Work: Created JWT types and validation handler | Tokens: ~12,500"

Go now. Stop writing preamble.
```

After each stage completes, wait for all agents before spawning next stage.

### PHASE 3: COLLECTION

Collect all agent reports:
- List all files modified/created per agent
- Note the work each agent completed
- Aggregate tokens by agent
- Do NOT commit yet

### PHASE 4: USER REVIEW & APPROVAL

Present to user:
```
=== WORK SUMMARY ===

Agents completed all tasks. Review before committing:

[Haiku-1] Files: types/auth.ts, auth/jwt.ts | Work: [description]
[Haiku-2] Files: services/auth-service.ts | Work: [description]
...

Commits to be created:
├─ [Haiku-1] [verb] [description]
├─ [Haiku-2] [verb] [description]
...

Ready to create commits? (y/n)
```

Wait for user confirmation before proceeding.

### PHASE 5: FINAL COMMIT

If user approves, spawn final commit agent:
```
You are [Commit-Agent] finalizing work.

YOUR TASK: Create commits for all completed work

AGENTS & WORK:
├─ [Haiku-1]: [files] - [work description]
├─ [Haiku-2]: [files] - [work description]
...

FOR EACH AGENT:
1. Stage their files: git add [files]
2. Create commit: git commit -m "[Haiku-N] [verb] [description]"
3. Verify: git log --oneline -N [total agents]

WHEN COMPLETE:
- Verify all commits created
- Report: "✅ All commits created. See git log above"
```

### PHASE 6: SUMMARY

After commits created:
- **Commits**: Show git log with hashes
- **Files**: List created/modified files per stage
- **Tests**: Verification all tests pass
- **Tokens**: Aggregate Sonnet (planning) + Haiku (all agents)

```
=== FINAL TOKEN REPORT ===

PLANNING PHASE (Sonnet 4.5):
├─ Planning: ~[estimate] tokens
└─ Total: ~[sum] tokens

EXECUTION PHASE (Haiku 4.5):
├─ [Haiku-1] Task 1: ~[estimate] tokens
├─ [Haiku-2] Task 2: ~[estimate] tokens
└─ Total Haiku: ~[sum] tokens

GRAND TOTAL: ~[sonnet + haiku] tokens
```

## Token Tracking & Reporting

**During execution (PHASE 2):**
- Orchestrator (Sonnet): Note planning phase token usage
- Each agent (Haiku): Report format includes tokens: `[Haiku-N] ✅ COMPLETE | Files: [...] | Work: [...] | Tokens: ~[number]`

**After user approval (PHASE 5):**
- Commit agent (Haiku): Report token usage for creating commits

**Final report (PHASE 6):**
1. Collect all token reports from execution agents
2. Add commit agent token usage
3. Sum Haiku tokens across all agents
4. Create final report:

```
Sonnet 4.5 (Planning): ~5,000 tokens
Haiku 4.5 (All agents): ~45,000 tokens
GRAND TOTAL: ~50,000 tokens
```

**Token estimation:**
- Simple task: 5K-10K tokens per agent
- Medium task: 10K-20K tokens per agent
- Complex task: 20K+ tokens per agent
- Planning phase: 3K-8K tokens

## Pre-Flight Checklist

Before spawning agents, verify:

- [ ] Working directory is clean (`git status` shows no uncommitted changes)
- [ ] Current branch is correct
- [ ] All dependencies installed (npm/pip/etc)
- [ ] Tests run successfully with baseline code
- [ ] Plan is written and stages are clear
- [ ] Each agent has clear file boundaries (no overlaps)
- [ ] All acceptance criteria are testable (not vague)
- [ ] You understand the dependencies between stages

## Troubleshooting

**Agent stalls or times out:**
- Acceptance criteria too vague? Make more specific
- Re-spawn with clearer requirements

**Commit fails (lint/test errors):**
- Agent should NOT commit broken code
- Fix criteria and re-spawn agent

**File conflicts between agents:**
- Stages need to be more sequential
- Two agents modified same file? Separate into different stages
- Restart with corrected plan

**Tests fail after completion:**
- Acceptance criteria were incomplete
- Add final test agent as cleanup stage

## Commit Message Format

Use: `[Haiku-N] [Verb] [description]`

Examples:
- `[Haiku-1] Add auth types and interfaces` ✅
- `[Haiku-2] Implement JWT validation` ✅
- `[Haiku-1] Work on types` ❌ (too vague)

## Models Used

- **Sonnet 4.5**: Planning phase only (creates orchestration strategy)
- **Haiku 4.5**: All execution (each agent is independent Haiku instance)

## Examples

**Simple:** 2 utility functions → 1 stage, 2 parallel agents, 2 separate commits
```
STAGE 1: [Haiku-1] email validator (utils/email.ts) + [Haiku-2] phone validator (utils/phone.ts)
```

**Medium:** Product rating feature → 3 stages (types → service → API)
```
STAGE 1: Types + schema (parallel, no dependencies)
STAGE 2: Services (parallel, depends on Stage 1)
STAGE 3: API + tests (sequential, depends on Stage 2)
```

**Complex:** Refactor auth system → Extract modules first, update main module last
```
KEY: Don't modify same file in parallel. Extract new files (Stage 1-2), then update main file (Stage 3).
```

## Planning & Execution Checklist

**PHASE 1 - Before planning:**
- [ ] Task has 2+ independent pieces
- [ ] Each agent owns different files (no overlaps)
- [ ] Dependencies flow forward only (Stage 1 → 2 → 3)
- [ ] Acceptance criteria are testable

**PHASE 2 - During execution:**
- [ ] All Stage N agents spawned before waiting
- [ ] Wait for all agents in stage before next stage
- [ ] Agents report work (files, description, tokens)
- [ ] NO individual commits created

**PHASE 3 - Collection:**
- [ ] All agent reports collected
- [ ] Files per agent documented

**PHASE 4 - User review:**
- [ ] Work summary presented to user
- [ ] User approves before proceeding

**PHASE 5 - Commit:**
- [ ] Final agent creates all commits atomically
- [ ] Commits verified in git log

**PHASE 6 - Summary:**
- [ ] All expected files created/modified
- [ ] All tests passing
- [ ] Token report aggregated
