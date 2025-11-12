# Agent Development Guide

This document provides context for working on the agentic-development plugin.

## Repository Structure

```
agentic-development/
├── agentic-development/
│   ├── agents/                    # 14 agent definitions
│   │   ├── requirements-refiner.md
│   │   ├── repo-setup-expert.md
│   │   ├── spec-kit-expert.md
│   │   ├── spec-kit-refiner.md
│   │   ├── development-manager.md
│   │   ├── team-lead.md
│   │   ├── documentation-expert.md
│   │   ├── documentation-spec-alignment.md
│   │   ├── python-expert.md
│   │   ├── fastapi-expert.md
│   │   ├── deployment-expert.md
│   │   ├── security-expert.md
│   │   ├── documentation-writer.md
│   │   └── spec-alignment-reviewer.md
│   └── skills/
│       └── orchestrator/
│           ├── SKILL.md          # Orchestration logic
│           └── memory-template.md
├── .claude-plugin/
│   └── plugin.json           # Plugin manifest
├── README.md
├── AGENTS.md                 # This file
└── CLAUDE.md
```

## Core Principles

### 1. Zero Assumptions
All agents fail on ambiguity. No guessing, no inferring, no defaults unless explicitly specified.

### 2. Professional Tone
- Concise, technical, direct
- No marketing speak ("amazing", "powerful")
- No fluff or verbose explanations
- Token-conscious output

### 3. Gates Have Authority
Gates (0, 3, 7) return PASS/FAIL. Orchestrator cannot override. Agents cannot skip gates.

### 4. Memory Management
All agents read/write to `.agent-memory/<agent-name>.md` for persistent learning.

## Agent File Structure

Each agent is a markdown file with YAML frontmatter:

```markdown
---
name: agent-name
description: One-line description of what this agent does
---

# Agent Name (Stage X)

## Role
[One-line role description]

## Responsibilities
[Bullet list of what this agent does]

## Inputs
[What files/data this agent reads]

## Outputs
[What files/data this agent creates]

## Memory Management
- Read `.agent-memory/<agent-name>.md` at start
- Apply learnings from past iterations
- Append new learnings at end (timestamped, concise)
- Track: [what patterns to track]
- Format: Timestamp, Pattern, Action, Context
- Max 50 entries (archive old ones)

## Constraints
[What this agent cannot/must not do]

## Token Efficiency
[How to minimize token usage]
```

## Adding a New Stage 6 Agent

Stage 6 agents are specialists that implement specific tasks. Follow these steps:

### Step 1: Create Agent Definition File

```bash
touch agentic-development/agents/<agent-name>.md
```

### Step 2: Define Agent Responsibilities

Use this template:

```markdown
---
name: agent-name
description: Brief description of specialty
---

# Agent Name (Stage 6)

## Role
[What this agent specializes in]

## Responsibilities
- Read augmented context file (with latest docs)
- [Specific tasks this agent performs]
- Generate tests/docs as needed

## Code Standards
[Technology-specific standards]
- [Frameworks/libraries to use]
- [Patterns to follow]
- [Anti-patterns to avoid]

## Inputs
- `.agent-context/<task>-<timestamp>.md` (augmented by Documentation Expert)

## Outputs
- [Files this agent creates in /src, /tests, /docs, etc.]
- Report completion to orchestrator

## Memory Management
- Read `.agent-memory/<agent-name>.md` at start
- Apply learnings from past iterations ([what patterns])
- Append new learnings at end (timestamped, concise)
- Track: [specific things to remember]
- Format: Timestamp, Pattern, Action, Context
- Max 50 entries (archive old ones)

## Constraints
- Follow spec exactly (zero assumptions)
- Use patterns from Latest Documentation section
- No placeholder code
- If context unclear: FAIL, request clarification

## Token Efficiency
- Code only, minimal comments
- No explanations
```

### Step 3: Update Orchestrator

Edit `agentic-development/skills/orchestrator/SKILL.md`:

**In "Available Agents" section:**
```markdown
- Stage 6 (Specialists): python-expert, fastapi-expert, deployment-expert, security-expert, documentation-writer, your-new-agent
```

### Step 4: Update Team Lead

The Team Lead automatically identifies available agents from the orchestrator's list. No changes needed unless you want to add assignment logic.

### Step 5: Test the Agent

Create a test project:

```bash
mkdir test-project
cd test-project
# Use orchestrator and verify your agent gets invoked for appropriate tasks
```

### Step 6: Document in README

Add to `README.md` under "Stage 6 Agents" section:

```markdown
- **your-agent-name** - [Brief description]
```

## Adding a New Gate Agent

Gates validate work and return PASS/FAIL. More complex than specialists.

### Step 1: Determine Gate Purpose

Gates should:
- Validate quality/completeness
- Have clear PASS/FAIL criteria
- Provide specific feedback on failures
- Track patterns in memory

### Step 2: Create Gate Definition

Similar to Stage 6, but include:
- **PASS Criteria**: Exhaustive list of conditions
- **FAIL Examples**: Concrete examples with specific feedback
- Clear routing logic (loop to which stage?)

### Step 3: Update Orchestrator Flow

Edit `agentic-development/skills/orchestrator/SKILL.md` to add gate to flow diagram and routing logic.

### Step 4: Test Thoroughly

Gates are critical. Test all failure scenarios.

## Modifying Existing Agents

### Rules for Changes

1. **Never remove constraints** - only add them
2. **Keep tone consistent** - concise, professional, no fluff
3. **Maintain memory management** - all agents must track learnings
4. **Test gate changes carefully** - broken gates break entire flow
5. **Update examples** - if you change standards, update FAIL examples

### Testing Changes

1. Read through agent definition
2. Check for contradictions with other agents
3. Verify token efficiency (no verbose output)
4. Test with orchestrator on sample project
5. Check that memory files are created/updated

## Memory File Format

All agents use this format in `.agent-memory/<agent-name>.md`:

```markdown
# Agent Memory: <agent-name>

## Purpose
[What this agent learns and tracks]

## Learnings

### 2025-01-12T14:30:00Z
- **Pattern**: [What was observed]
- **Action**: [Actionable change for next time]
- **Context**: [Stage/gate/task where this occurred]
```

See `agentic-development/skills/orchestrator/memory-template.md` for full documentation.

## Agent Types Reference

### Gates (Validators)
- **Gate 0**: requirements-refiner - Validates requirements clarity
- **Gate 3**: spec-kit-refiner - Validates spec completeness
- **Gate 6**: documentation-spec-alignment - Validates specs against latest library docs
- **Gate 7**: spec-alignment-reviewer - Validates code-spec alignment

### Process Agents (Orchestration)
- **Stage 1**: repo-setup-expert - Repository initialization
- **Stage 2**: spec-kit-expert - Specification generation
- **Stage 4**: development-manager - Roadmap creation
- **Stage 5**: team-lead - Task assignment, commits

### Pre-processor
- **Stage 6**: documentation-expert - Fetches latest docs from internet

### Specialists (Code Generation)
- **Stage 6**: python-expert, fastapi-expert, deployment-expert, security-expert, documentation-writer

## Common Patterns

### Agent Invocation (from Orchestrator)

```markdown
Task(
  subagent_type="agent-name",
  prompt="[Clear description of what agent should do with file references]"
)
```

### Context File Reading (for Stage 6 agents)

```markdown
Read `.agent-context/<task>-<timestamp>.md` which contains:
- Relevant spec sections
- Latest documentation (from documentation-expert)
- Dependencies
- Acceptance criteria
```

### Reporting Completion

```markdown
Report to orchestrator:
- PASS/FAIL (gates only)
- Files created
- Any issues encountered
- Brief summary
```

## Debugging Tips

### Agent Not Getting Invoked

1. Check Team Lead assignment logic
2. Verify agent in orchestrator's "Available Agents"
3. Check roadmap task descriptions match agent specialty

### Agent Producing Verbose Output

1. Review "Token Efficiency" section in agent definition
2. Add constraint: "No explanations"
3. Check examples - should be code/bullets only

### Gate Failing Unexpectedly

1. Review PASS criteria - may be too strict
2. Check FAIL examples - are they realistic?
3. Verify agent has necessary context

### Memory Files Not Updating

1. Check agent has "Memory Management" section
2. Verify `.agent-memory/` directory exists
3. Check timestamp format (ISO 8601)

## File Naming Conventions

- Agent files: lowercase, hyphens, descriptive (`python-expert.md`, not `python.md`)
- Context files: `<task-name>-<timestamp>.md`
- Memory files: `<agent-name>.md` (matches agent name exactly)

## Commit Conventions

When modifying this plugin:

- `feat(agent): add rust-expert for Rust code generation`
- `fix(orchestrator): correct gate routing logic`
- `docs(readme): update Stage 6 agent list`
- `refactor(team-lead): improve task assignment clarity`

Always use conventional commits format.

## Questions?

This plugin is self-documenting. If unclear:
1. Read agent definitions in `agentic-development/agents/`
2. Check `agentic-development/skills/orchestrator/SKILL.md` for flow
3. Review `agentic-development/skills/orchestrator/memory-template.md` for memory format
