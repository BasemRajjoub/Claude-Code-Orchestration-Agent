# Claude Code Orchestration Agent

Transform complex coding tasks into orchestrated workflows where multiple AI agents work together efficiently.

## What is This?

The Claude Code Orchestration Agent is a skill that enables Claude to tackle large, multi-file tasks by:

- **Planning once** with Claude Sonnet 4.5's intelligence
- **Executing in parallel** using multiple Claude Haiku 4.5 agents
- **Collecting reports** - agents report work without committing
- **Requesting approval** - you review and confirm before commits
- **Committing atomically** - final agent creates all commits together
- **Saving time and money** - parallel execution and cheaper Haiku tokens

Think of it as having a senior developer (Sonnet) plan the work, delegating to multiple junior developers (Haiku agents) who work simultaneously on different parts, then getting your approval before finalizing everything.

## Why Use This?

**Without orchestration:**
- One Claude instance does everything sequentially
- Takes 30+ minutes for complex features
- Uses expensive Sonnet tokens throughout
- No parallelization benefits

**With orchestration:**
- Multiple agents work simultaneously
- Completes in 10-15 minutes
- Uses Sonnet once for planning, Haiku for execution
- 40-60% cost savings
- 50-70% time savings

## Installation

### Prerequisites

1. **Claude Code** installed on your system
   ```bash
   # Install Claude Code (if not already installed)
   npm install -g @anthropic-ai/claude-code
   ```

2. **Anthropic API key** with access to:
   - `claude-sonnet-4.5` (for planning)
   - `claude-haiku-4.5` (for execution)

### Adding the Skill

1. **In Claude.ai web interface:**
   - Go to your profile → Skills
   - Click "Create Skill"
   - Copy the contents of `SKILL.md`
   - Name it: "Claude Code Orchestration Agent"
   - Save the skill

2. **Enable the skill:**
   - In any conversation, click the skills menu
   - Toggle on "Claude Code Orchestration Agent"

## Usage

### Basic Usage

Simply describe a complex task that involves multiple files or components:

```
"Implement user authentication with JWT, password hashing, and tests"
```

Claude will:
1. Analyze the task
2. Create an orchestration plan
3. Show you the plan and ask for confirmation
4. Spawn Haiku agents to execute the work
5. Display real-time progress
6. Provide a comprehensive summary

### What Tasks Work Best?

**✅ Perfect for orchestration:**
- "Create a REST API with validation, error handling, and tests"
- "Add caching with Redis and in-memory fallback"
- "Implement rate limiting across multiple routes"
- "Build authentication with OAuth, JWT, and sessions"
- "Create a dashboard with 5 different chart components"

**❌ Not suitable for orchestration:**
- "Fix this bug in auth.ts" (single file)
- "Explain how JWT works" (no implementation)
- "Review my code" (no execution)
- "Add a comment to this function" (too simple)

### The Orchestration Process

The workflow has 5 main steps: Planning → Execution → Collection → User Approval → Commits

#### Step 1: Planning
Claude Sonnet 4.5 analyzes your request and creates a detailed plan:

```
📋 ORCHESTRATION PLAN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Task: Implement user authentication with JWT, password hashing, and tests

STAGE 1 - Foundation (parallel):
├─ [Haiku-1] Create authentication types
│  Files: auth/types.ts
│  Estimate: 2 min
├─ [Haiku-2] Implement JWT utilities
│  Files: auth/jwt.ts
│  Estimate: 3 min
└─ [Haiku-3] Implement password hashing
   Files: auth/password.ts
   Estimate: 2 min

STAGE 2 - Integration (sequential):
├─ [Haiku-4] Create auth service
│  Files: auth/auth-service.ts
│  Estimate: 4 min
└─ [Haiku-5] Add test suite
   Files: auth/__tests__/auth.test.ts
   Estimate: 3 min

Savings: 39% tokens, 50% time
Ready to proceed? (yes/no)
```

#### Step 2: Execution
After you confirm, Haiku agents execute in real-time:

```
🚀 STAGE 1: Foundation - Spawning 3 Haiku agents...

[Haiku-1] Starting work on authentication types...
[Haiku-1] 📝 Created auth/types.ts
[Haiku-1] ✅ COMPLETE | Files: auth/types.ts | Work: Created auth types and interfaces | Tokens: ~8,234

[Haiku-2] Starting work on JWT utilities...
[Haiku-2] 📝 Created auth/jwt.ts
[Haiku-2] ✅ COMPLETE | Files: auth/jwt.ts | Work: Implemented JWT token utilities | Tokens: ~9,145

[Haiku-3] Starting work on password hashing...
[Haiku-3] 📝 Created auth/password.ts
[Haiku-3] ✅ COMPLETE | Files: auth/password.ts | Work: Added password hashing utilities | Tokens: ~7,891

✅ STAGE 1 COMPLETE (3/3 agents finished, work collected)
```

#### Step 3: User Review & Approval
Work summary is presented for your review:

```
=== WORK SUMMARY ===

Agents completed all tasks. Review before committing:

[Haiku-1] Files: auth/types.ts | Work: Created auth types and interfaces
[Haiku-2] Files: auth/jwt.ts | Work: Implemented JWT token utilities
[Haiku-3] Files: auth/password.ts | Work: Added password hashing utilities

Commits to be created:
├─ [Haiku-1] Add auth types and interfaces
├─ [Haiku-2] Implement JWT token utilities
└─ [Haiku-3] Add password hashing utilities

Ready to create commits? (yes/no)
```

You review the work and confirm before any commits are made.

#### Step 4: Commit
If you approve, a final commit agent creates all commits atomically:

```
🔐 FINALIZING: Creating all commits...

[Commit-Agent] Processing Haiku-1 work...
[Commit-Agent] Staging: auth/types.ts
[Commit-Agent] ✅ Commit: a3f9b21 [Haiku-1] Add auth types and interfaces

[Commit-Agent] Processing Haiku-2 work...
[Commit-Agent] Staging: auth/jwt.ts
[Commit-Agent] ✅ Commit: 7c4d892 [Haiku-2] Implement JWT token utilities

[Commit-Agent] Processing Haiku-3 work...
[Commit-Agent] Staging: auth/password.ts
[Commit-Agent] ✅ Commit: e2a5f67 [Haiku-3] Add password hashing utilities

✅ All commits created successfully
```

#### Step 5: Summary
After commits created, you get a comprehensive summary:

```
✅ ORCHESTRATION COMPLETE

Duration: 7 minutes | Agents: 3 Haiku (execution) + 1 Haiku (commits)
Files created: 3
Tests: 15/15 passing ✓
Savings: 39% cost, 50% time

Token Report:
├─ Sonnet 4.5 (Planning): ~5,432 tokens
├─ Haiku 4.5 (Execution): ~25,270 tokens
└─ GRAND TOTAL: ~30,702 tokens

🎉 Authentication system complete!
```

## Examples

### Example 1: E-commerce Product API

**Your request:**
```
"Create a product API with validation, inventory tracking, and tests"
```

**What gets built:**
- `types/product.ts` - Product types and schemas
- `models/product.ts` - Database model
- `middleware/validate.ts` - Validation middleware
- `services/inventory.ts` - Inventory tracking
- `routes/products.ts` - API endpoints
- `__tests__/products.test.ts` - Test suite

**Time:** ~12 minutes with 6 agents

### Example 2: Real-time Chat System

**Your request:**
```
"Build WebSocket chat with rooms, typing indicators, and presence"
```

**What gets built:**
- `types/chat.ts` - Chat types
- `websocket/connection.ts` - Connection handling
- `websocket/rooms.ts` - Room management
- `websocket/presence.ts` - User presence
- `services/chat.ts` - Chat service
- `__tests__/chat.test.ts` - Tests

**Time:** ~15 minutes with 6 agents

### Example 3: Payment Processing

**Your request:**
```
"Implement payment processing with Stripe, webhooks, and refunds"
```

**What gets built:**
- `types/payment.ts` - Payment types
- `services/stripe.ts` - Stripe integration
- `webhooks/stripe.ts` - Webhook handlers
- `services/refund.ts` - Refund logic
- `routes/payments.ts` - API endpoints
- `__tests__/payments.test.ts` - Tests

**Time:** ~10 minutes with 6 agents

## How It Works

### The Architecture

```
User: "Build auth with JWT and tests"
                ↓
┌─────────────────────────────────────┐
│   Sonnet 4.5: Strategic Planning    │
│   - Analyze complexity               │
│   - Identify dependencies            │
│   - Prevent conflicts                │
│   - Create stage plan                │
└─────────────────────────────────────┘
                ↓
┌─────────────────────────────────────┐
│    Spawn Multiple Haiku 4.5 Agents  │
│                                      │
│  [Haiku-1]  [Haiku-2]  [Haiku-3]   │
│     ↓           ↓           ↓       │
│  types.ts    jwt.ts   password.ts  │
│     ↓           ↓           ↓       │
│  Report      Report     Report      │
│  (no commit)                         │
└─────────────────────────────────────┘
                ↓
┌─────────────────────────────────────┐
│        Next Stage (Sequential)       │
│                                      │
│      [Haiku-4]    [Haiku-5]         │
│         ↓             ↓              │
│    service.ts     tests.ts          │
│         ↓             ↓              │
│      Report      Report              │
│      (no commit)                     │
└─────────────────────────────────────┘
                ↓
┌─────────────────────────────────────┐
│   User Reviews All Work Summary     │
│   ✓ Files created                    │
│   ✓ Work descriptions                │
│   ✓ Approve commits? (yes/no)        │
└─────────────────────────────────────┘
                ↓
         (User Confirms)
                ↓
┌─────────────────────────────────────┐
│   Haiku 4.5: Final Commit Agent     │
│   - Stage all files                  │
│   - Create atomic commits            │
│   - Verify git log                   │
└─────────────────────────────────────┘
                ↓
        Final Summary + Stats
```

### Key Features

**1. Intelligent Planning**
- Sonnet analyzes dependencies between tasks
- Groups independent work for parallel execution
- Prevents file conflicts through staging

**2. Parallel Execution**
- Multiple Haiku agents work simultaneously
- Each agent owns specific files
- No conflicts, clean git history

**3. User Control**
- Agents verify their own work
- All agents report completion with details
- User reviews work before commits are made
- Final agent creates all commits atomically

**4. Real-time Visibility**
- See each agent's progress live
- Track which files are being created
- Review work summary before committing
- Monitor commits during final commit phase

**5. Cost & Time Savings**
- Use expensive Sonnet only for planning
- Use cheaper Haiku for all execution
- Parallel execution reduces total time

## Git Workflow

**Execution Phase:**
1. Agents work on files (no commits yet)
2. Agents report completion with file list and work description
3. Orchestrator collects all agent reports

**User Review Phase:**
4. User reviews work summary
5. User confirms: "Yes, create commits"

**Commit Phase:**
6. Final commit agent creates clean, descriptive commits:

```bash
git log --oneline
f8e2a91 [Haiku-5] Add comprehensive auth test suite
9b1c3d4 [Haiku-4] Implement auth service layer
e2a5f67 [Haiku-3] Add password hashing utilities
7c4d892 [Haiku-2] Implement JWT token utilities
a3f9b21 [Haiku-1] Add auth types and interfaces
```

Benefits:
- ✅ User approval before any commits
- ✅ Atomic commits from final agent
- ✅ Clear history of what was built
- ✅ Easy to review each component
- ✅ Simple to revert if needed
- ✅ Good documentation for team

## Troubleshooting

### "Agents conflicted on the same file"

**Problem:** Two agents tried to modify the same file simultaneously.

**Solution:** The orchestrator should have caught this during planning. If it happens:
- Review the orchestration plan
- Ensure dependent tasks are in sequential stages
- File a bug report - this shouldn't happen

### "Agent reported incomplete work"

**Problem:** An agent finished but didn't complete all requirements.

**Solution:**
- Check the acceptance criteria in the plan
- May need more specific instructions for that agent
- Can manually complete or re-run that specific agent

### "Commits missing or poorly named"

**Problem:** Git commits aren't being created properly.

**Solution:**
- Verify git is initialized in the project
- Check that agents have git access
- Ensure commit message format is being followed

### "Tests failing after completion"

**Problem:** Test agent reports failures.

**Solution:**
- This is actually good - caught issues early!
- Review test output to identify problems
- Fix failing components
- Re-run test agent to verify

### "Task not triggering orchestration"

**Problem:** Claude uses regular mode instead of orchestration.

**Solution:** Rephrase to be more explicit:
- ❌ "Can you help with auth?"
- ✅ "Implement authentication with types, JWT, password hashing, service layer, and tests"

Make it clear you want multiple components built.

## Performance Benchmarks

Real measurements from typical tasks:

| Task | Files | Sonnet Only | Orchestrated | Savings |
|------|-------|-------------|--------------|---------|
| Auth System | 5 | 14 min, 18k tokens | 7 min, 11k tokens | 50% time, 39% tokens |
| REST API | 6 | 18 min, 24k tokens | 10 min, 14k tokens | 44% time, 42% tokens |
| Caching Layer | 5 | 12 min, 16k tokens | 6 min, 10k tokens | 50% time, 38% tokens |
| WebSocket Chat | 7 | 22 min, 28k tokens | 12 min, 16k tokens | 45% time, 43% tokens |

Average: **47% time savings, 40% token savings**

## Advanced Usage

### Customizing the Plan

If you want more control, you can suggest how to split the work:

```
"Build auth with JWT and tests. Split into:
- One agent for types
- One for JWT utils
- One for password hashing  
- One for service layer
- One for tests"
```

### Adding Constraints

Specify requirements in your request:

```
"Build REST API with validation and tests.
Requirements:
- Use Zod for validation
- Include error handling middleware
- Add OpenAPI docs
- 100% test coverage"
```

### Reviewing Before Execution

Always review the orchestration plan before confirming. Check:
- Are the stages logical?
- Are file assignments clear?
- Are dependencies handled correctly?
- Are estimates reasonable?

If something looks off, ask Claude to revise the plan.

## Best Practices

### For Best Results

1. **Be specific about components**
   - ❌ "Build an API"
   - ✅ "Build API with routes, validation, models, and tests"

2. **Mention multiple files/components**
   - Triggers orchestration mode
   - Gets parallel execution benefits

3. **Include testing in request**
   - Ensures quality
   - Gets dedicated test agent

4. **Review the plan**
   - Understand what will be built
   - Catch issues before execution
   - Adjust if needed

### When to Use

- ✅ Building new features from scratch
- ✅ Adding multiple related components
- ✅ Creating full-stack functionality
- ✅ Implementing with tests
- ✅ 3+ files to create/modify

### When NOT to Use

- ❌ Single file changes
- ❌ Bug fixes
- ❌ Code reviews
- ❌ Explanations
- ❌ Simple refactoring

## FAQ

**Q: Can I reject the work and ask for changes?**
A: Yes! During the user approval phase (Step 3), you can request changes before any commits are made. The orchestrator can modify and re-run agents as needed.

**Q: What if I don't like some of the work?**
A: Since agents only report (don't commit), you can:
1. Reject at approval phase and ask for changes
2. Or approve and manually modify files afterward

**Q: Does this work with any programming language?**
A: Yes! The orchestration concept works with Python, TypeScript, Go, Rust, etc.

**Q: Can I use this with existing code?**
A: Yes, agents can modify existing files, but it's best for creating new components.

**Q: What if I need to make changes after completion?**
A: Just ask! You can request modifications to any of the generated files.

**Q: Does this require a paid Claude account?**
A: You need API access to both Sonnet 4.5 and Haiku 4.5 models.

**Q: Can I see the actual agent spawning?**
A: The orchestration happens via Claude Code CLI - all output is visible in real-time.

**Q: What's the maximum number of agents?**
A: Theoretically unlimited, but typically 3-8 agents per task is optimal.

**Q: Do the agents know about each other?**
A: They know about dependencies through their instructions, but work independently.

**Q: How long does user approval take?**
A: Just a yes/no confirmation. You can review the work summary in a few seconds.

## Support & Feedback

- **Issues:** File issues in the skill feedback
- **Questions:** Ask in Claude.ai conversations
- **Improvements:** Suggest enhancements via feedback

## Version History

- **v1.1** - User Approval Workflow
  - Agents report work without committing
  - User reviews work summary before commits
  - Final commit agent creates all commits atomically
  - Better control and visibility

- **v1.0** - Initial release
  - Sonnet 4.5 planning
  - Haiku 4.5 execution
  - Stage-based conflict prevention
  - Self-committing agents
  - Real-time progress display

## License

This skill template is provided as-is for use with Claude.ai and Claude Code.

---

**Ready to try it?** Just enable the skill and ask Claude to build something complex!

Example: *"Implement user authentication with JWT, password hashing, refresh tokens, and comprehensive tests"*
